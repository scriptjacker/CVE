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
  <li><b>Student Login:</b> Authenticate to student login using avlid credentials.</li>
  <li><b>Navigate to Add Feedback:</b> Access the "Add Feedback" section/page from student profile.</li>
  <li><b>Intercept Request:</b> Configure Burp Suite (127.0.0.1:8080) to intercept the POST request. Enable the Interceptor in Burp's Proxy tab.</li>
  <li><b>Insert Malicious Payload:</b> Capture the request and modify the `smyFeedbacks` parameter with: </li>
  <li><b>Admin Login:</b> Redirect users to malicious sites.</li>
  <li><b>Navigate to All Feedbacks:</b> Redirect users to malicious sites.</li>
  <li><b>Observe Execution:</b> Redirect users to malicious sites.</li>
</ol>

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
