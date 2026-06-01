# Stored Cross-Site Scripting (XSS) via Examinee Registration in Computer Base Test V1.0 Vulnerability Report


## Vulnerability Summary
A critical Stored Cross-Site Scripting (XSS) vulnerability was discovered in the Examinee Registration Fields file of Kashipara's Computer Base Test (v1.0). Attackers can inject malicious scripts via the `fullname` POST parameter, which are then persistently stored and executed when visited by any authenticated user.

---

| Field               | Details                                                                 |
|---------------------|-------------------------------------------------------------------------|
| Product Name        | Computer Base Test project in PHP                                       |
| Affected Version    | v1.0                                                                    |
| Vendor              | Kashipara                                                               |
| Affected File       | `aaddExamineeExe.php`                                                              |
| Affected Parameter  | `fullname`                                                          |
| Request Method      | POST                                                                    |
| Vulnerability Type  | Stored Cross-Site Scripting (XSS)                                       |
| Official Website    | [Kashipara Computer Base Test](https://www.kashipara.com/project/php/13235/computer-base-test-php-project-source-code) |

---

### Vulnerable Code — Injection

```php
echo $selExmneRow['student_fullname'];
echo $selExmneRow['student_sex'];
echo $selExmneRow['student_email'];
echo $selExmneRow['student_password']; 
```

---

### Steps to Reproduce
1. Visit Admin login page and authenticate with valid credentials.
<img width="1472" height="842" alt="1 - SQLi in admin panel" src="https://github.com/user-attachments/assets/28d128cf-a46c-4434-9ccb-da3cfa5779da" />


2. Visit Add Examinee in manage students at `/users/adminpanel/admin/query/addExamineeExe.php`


3. In the `fullname` parameter, enter the XSS payload like `<script>alert(document.cookie)</script>` and intercept the request in burpsuite.
<img width="1815" height="894" alt="8 - Stored XSS in admin" src="https://github.com/user-attachments/assets/47662b6b-917b-43c2-b9a4-dc1b489096ed" /><img width="1829" height="884" alt="8 - Stored XSS in admin 2" src="https://github.com/user-attachments/assets/637ae79b-f30c-4838-8f7c-ed4f11b71864" />


4. Now, whenever admin visit manage student page, so it execute the XSS Payload.
<img width="1829" height="884" alt="8 - Stored XSS in admin 2" src="https://github.com/user-attachments/assets/14773eba-9e93-461a-b8c4-8daf65b82d5a" />


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
