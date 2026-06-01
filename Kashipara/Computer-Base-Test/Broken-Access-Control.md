<h1>Broken Access Control (BAC) / Insecure Direct Object Reference (IDOR) in Computer Base Test V1.0 Vulnerability Report</h1>


<h2>Vulnerability Summary</h2>
A critical Broken Access Control (BAC) / Insecure Direct Object Reference (IDOR) vulnerability was discovered in the Update Examinee of Kashipara's Computer Base Test (v1.0). An authenticated attacker can enumerate student records including full names, email addresses, and plaintext passwords by manipulating the <code>id</code> GET parameter without any ownership or authorisation check.

---

| Field               | Details                                                                 |
|---------------------|-------------------------------------------------------------------------|
| Product Name        | Computer Base Test project in PHP                                       |
| Affected Version    | v1.0                                                                    |
| Vendor              | Kashipara                                                               |
| Affected File       | `updateExaminee.php`                                                    |
| Affected Parameter  | `id`                                                                    |
| Request Method      | GET                                                                     |
| Vulnerability Type  | Broken Access Control / Insecure Direct Object Reference (IDOR)        |
| Official Website    | [Kashipara Computer Base Test](https://www.kashipara.com/project/php/13235/computer-base-test-php-project-source-code) |

---

<h2>Steps to Reproduce</h2>
<ol>
  <li><b>Admin Login:</b> Authenticate to the admin portal using valid credentials.</li>
  <img width="1551" height="852" alt="admin-login" src="https://github.com/user-attachments/assets/96aeb060-2f41-4a5e-8434-2758398c8a2b" />


---

  <li><b>Navigate to Manage Examinee:</b> Access the "Manage Examinee" section from the admin dashboard. Observe that each student row has an Edit button that triggers a modal.</li>

---

  <li><b>Intercept the Edit Request:</b> Configure Burp Suite (127.0.0.1:8080) to intercept traffic. Click any Edit button on the Manage Examinee page and capture the GET request to <code>users/adminpanel/admin/facebox_modal/updateExaminee.php?id=</li>


---

  <li><b>Login student account & Visit same endpoint from student account:</b> In Burp Suite, forward the request to Repeater. Access student account and visit `users/adminpanel/admin/facebox_modal/updateExaminee.php?id=` Change the <code>id</code> value to any other integer (e.g., increment from <code>id=9114</code> to <code>id=9115</code>, <code>id=9116</code>, etc.).</li>
<img width="1863" height="830" alt="10 - BAC  IDOR 2" src="https://github.com/user-attachments/assets/77e148b2-2ef7-4b59-af71-be4d67a1be87" />



  <li><b>Observe Unauthorised Data Exposure:</b> The server responds with a pre-filled HTML form containing the target student's full name, email address, year level, and <b>plaintext password</b> with no authorisation check performed.</li>
<img width="1820" height="808" alt="10 - BAC  IDOR 3" src="https://github.com/user-attachments/assets/b0d5d960-c3db-4314-b7d8-51045074ff9f" />


---

  <li><b>Enumerate All Students:</b> Repeat the request for sequential <code>id</code> values. Every registered student's credentials are exposed without restriction and by low priviledge user (student).</li>

```
GET /users/adminpanel/admin/facebox_modal/updateExaminee.php?id=9114
GET /users/adminpanel/admin/facebox_modal/updateExaminee.php?id=9115
GET /users/adminpanel/admin/facebox_modal/updateExaminee.php?id=9116
...
```

  <!-- [SCREENSHOT: Multiple responses showing different students' data across sequential IDs] -->

</ol>

---

<h2>Vulnerable Code</h2>

```php
$id = $_GET['id'];
$selExmne = $conn->query(
    "SELECT * FROM examinee_tbl WHERE student_id='$id'"
)->fetch(PDO::FETCH_ASSOC);

echo $selExmneRow['student_fullname'];
echo $selExmneRow['student_email'];
echo $selExmneRow['student_password']; 
```

No session ownership check is performed. Any authenticated user who can reach the URL can read any student's record by changing the `id` value.

---

<h2>Impact</h2>
<ul>
  <li><b>Full Credential Exposure:</b> All student email addresses and plaintext passwords are accessible by iterating the <code>id</code> parameter.</li>
  <li><b>Account Takeover:</b> An attacker can use harvested credentials to log in as any student and submit exam answers, manipulate scores, or access results.</li>
  <li><b>Credential Stuffing:</b> Since passwords are stored and exposed in plaintext, they can be directly used against other services where students reuse the same password.</li>
  <li><b>Privacy Violation:</b> Exposure of personally identifiable information (PII) including full name, date of birth, gender, and year level for all registered students.</li>
</ul>

---

<h2>Remediation</h2>
<ul>
  <li><b>Authorisation Check:</b> Verify that the requesting session belongs to an admin and, where applicable, that the admin is authorised to manage the specific record before returning any data.</li>
  <li><b>Password Hashing:</b> Never store passwords in plaintext. Use <code>password_hash($password, PASSWORD_BCRYPT)</code> on registration and <code>password_verify()</code> on login. Remove the password column from all UI tables.</li>
  <li><b>Use Indirect References:</b> Replace sequential integer IDs in URLs with non-guessable tokens (e.g., UUID) so that object references cannot be iterated.</li>
  <li><b>Server-Side Access Control:</b> Implement a centralised access control layer that enforces role-based checks on every request rather than relying on UI-level visibility.</li>
</ul>

---

<h2>Reference</h2>
<a href="https://owasp.org/www-project-top-ten/2017/A5_2017-Broken_Access_Control">OWASP Top 10: Broken Access Control</a>

<a href="https://portswigger.net/web-security/access-control/idor">PortSwigger IDOR Guide</a>

<a href="https://cheatsheetseries.owasp.org/cheatsheets/Insecure_Direct_Object_Reference_Prevention_Cheat_Sheet.html">OWASP IDOR Prevention Cheat Sheet</a>

---

**Urgency**: Patch immediately to prevent exploitation in production environments.
