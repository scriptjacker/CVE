<h1>Stored Cross-Site Scripting (XSS) in Computer Base Test V1.0 Vulnerability Report</h1>


<h2>Vulnerability Summary</h2>
A critical Stored Cross-Site Scripting (XSS) vulnerability was discovered in the <code>/users/adminpanel/admin/home.php</code> file of Kashipara's Computer Base Test (v1.0). Attackers can inject malicious scripts via the smyFeedbacks POST parameter, which are then persistently stored and executed when the search functionality is used.

---

| Field               | Details                                                                 |
|---------------------|-------------------------------------------------------------------------|
| Product Name        | Computer Base Test project in PHP                                       |
| Affected Version    | v1.0                                                                    |
| Vendor              | Kashipara                                                               |
| Affected File       | `home.php`                                                              |
| Affected Parameter  | `smyFeedbacks`                                                          |
| Request Method      | POST                                                                    |
| Vulnerability Type  | Stored Cross-Site Scripting (XSS)                                       |
| Official Website    | [Kashipara Computer Base Test](https://www.kashipara.com/project/php/13235/computer-base-test-php-project-source-code) |

---

<h2>Steps to Reproduce</h2>
<ol>
  <li></li>
  <li></li>
</ol>

---

<h2>Impact</h2>
<ul>
  <li></li>
  <li></li>
</ul>

---

<h2>Remediation</h2>
<ul>
  <li></li>
  <li></li>
</ul>

---

<h2>Reference</h2>
<a href="https://portswigger.net/web-security/cross-site-scripting">PortSwigger XSS Guide</a>
--
<a href="https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html">OWASP XSS Prevention Cheat Sheet</a>

---

**Urgency**: Patch immediately to prevent exploitation in production environments.  
