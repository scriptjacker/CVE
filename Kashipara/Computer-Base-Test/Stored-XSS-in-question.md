# Stored Cross-Site Scripting (XSS) via Exam Question Fields in Computer Base Test V1.0 Vulnerability Report


## Vulnerability Summary
A critical Stored Cross-Site Scripting (XSS) vulnerability was discovered in the Exam Question Fields file of Kashipara's Computer Base Test (v1.0). Attackers can inject malicious scripts via the `question` POST parameter, which are then persistently stored and executed when exam is visited by student.

---

| Field               | Details                                                                 |
|---------------------|-------------------------------------------------------------------------|
| Product Name        | Computer Base Test project in PHP                                       |
| Affected Version    | v1.0                                                                    |
| Vendor              | Kashipara                                                               |
| Affected File       | `addQuestionExe.php`                                                              |
| Affected Parameter  | `question`, `choice_A`, `choice_B`, `choice_C`, `choice_D`, `correctAnswer`                                                          |
| Request Method      | POST                                                                    |
| Vulnerability Type  | Stored Cross-Site Scripting (XSS)                                       |
| Official Website    | [Kashipara Computer Base Test](https://www.kashipara.com/project/php/13235/computer-base-test-php-project-source-code) |

---

### Vulnerable Code — Injection

```php
// addQuestionExe.php
extract($_POST);
$insQuest = $conn->query(
    "INSERT INTO exam_question_tbl(…,exam_question,exam_ch1,…)
     VALUES('$examId','$question','$choice_A',…)"
);
```

---

### Steps to Reproduce
1. Visit Admin login page and authenticate with valid credentials.
<img width="1472" height="842" alt="1 - SQLi in admin panel" src="https://github.com/user-attachments/assets/28d128cf-a46c-4434-9ccb-da3cfa5779da" />


2. Visit Add Question option in any exam at users/adminpanel/admin/query/addQuestionExe.php
<img width="1838" height="894" alt="5 - Stored XSS in admin" src="https://github.com/user-attachments/assets/99d9ff4b-87a4-4091-9d8d-ade1d7136bb5" />


3. In the `quesiton` parameter, enter the XSS payload like `<script>alert(document.cookie)</script>` and intercept the request in burpsuite.
<img width="1251" height="636" alt="5 - Stored XSS in admin 2" src="https://github.com/user-attachments/assets/f46c6ede-82bd-432f-a59f-95c9833edd7a" />

4. In response it can be seen that the payload is injected successfully.


5. Now, whenever admin visit manage exam page or student visit All exam page so it execute the XSS Payload.
<img width="1836" height="887" alt="5 - Stored XSS in admin 4" src="https://github.com/user-attachments/assets/1485236b-2c25-475d-9445-d26599b71273" />
<img width="1825" height="865" alt="5 - Stored XSS in admin 3" src="https://github.com/user-attachments/assets/af72fd4e-d2ba-46d8-9ccc-c5c7923361bb" />


---

<h2>Impact</h2>
<ul>
  <li><b>Session Hijacking:</b> Steal admin cookies or session tokens.</li>
  <li><b>Phishing Attacks:</b> Inject fake login forms to harvest credentials.</li>
  <li><b>Defacement:</b> Modify displayed content to spread misinformation.</li>
  <li><b>Malware Distribution:</b> Redirect users to malicious sites.</li>
</ul>

---

<h2>Remediation</h2>
<ul>
  <li><b>Input Sanitization:</b> Use libraries like `htmlspecialchars()` or DOMPurify to neutralize HTML/JS in user inputs.</li>
  <li><b>Content Security Policy (CSP):</b> Implement CSP headers to restrict inline scripts and unauthorized sources.</li>
  <li><b>Output Encoding:</b> Encode dynamic content before rendering (e.g., via OWASP’s ESAPI).</li>
  <li><b>Framework Protections:</b> Leverage modern PHP frameworks (e.g., Laravel, Symfony) with built-in XSS protections.</li>
</ul>

---

<h2>Reference</h2>
<a href="https://portswigger.net/web-security/cross-site-scripting">PortSwigger XSS Guide</a>


<a href="https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html">OWASP XSS Prevention Cheat Sheet</a>

---

**Urgency**: Patch immediately to prevent exploitation in production environments.  
