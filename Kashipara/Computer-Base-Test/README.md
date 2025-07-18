<h1>Stored Cross-Site Scripting (XSS) in Computer Base Test V1.0 Vulnerability Report</h1>


<h2>Vulnerability Summary</h2>
A critical Stored Cross-Site Scripting (XSS) vulnerability was discovered in the <code>/users/adminpanel/admin/home.php?page=feedbacks</code> file of Kashipara's Computer Base Test (v1.0). Attackers can inject malicious scripts via the smyFeedbacks POST parameter in <code>/users/home.php</code>, which are then persistently stored and executed when the search functionality is used.

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
  <li><b>Student Login:</b> Authenticate to student login using valid credentials.</li>
  <img width="1399" height="865" alt="Screenshot 2025-07-18 140411" src="https://github.com/user-attachments/assets/1ab654ec-626d-473c-8374-490cf79d46fa" />

  <li><b>Navigate to Add Feedback:</b> Access the "Add Feedback" section/page from student profile.</li>
  <img width="1856" height="952" alt="Screenshot 2025-07-18 140504" src="https://github.com/user-attachments/assets/f1db99a3-c7bc-45d3-aea2-79a515827dc5" />

  <li><b>Intercept Request:</b> Configure Burp Suite (127.0.0.1:8080) to intercept the POST request. Enable the Interceptor in Burp's Proxy tab.</li>
  <img width="1853" height="941" alt="Screenshot 2025-07-18 140755" src="https://github.com/user-attachments/assets/6f9dfcb1-41cc-4c86-a1d2-f4cfc59cdd85" />

  <li><b>Insert Malicious Payload:</b> Capture the request and modify the smyFeedbacks parameter with: <code>&lt;script&gt;alert(1)&lt;/script&gt;</code></li>
  <li><b>Admin Login:</b> Authenticate to admin portal using valid credentials.</li>
  <img width="1461" height="879" alt="Screenshot 2025-07-18 140438" src="https://github.com/user-attachments/assets/081ed5ee-167b-4124-bfea-ff163257bc2c" />

  <li><b>Navigate to All Feedbacks:</b> Access the "All Feedbacks" page from admin account.</li>
  <img width="1855" height="926" alt="Screenshot 2025-07-18 140639" src="https://github.com/user-attachments/assets/6c86e462-9e6d-4ca5-bf61-ac87b5d3af5d" />


  <li><b>Observe Execution:</b> The injected script renders persistently, triggering an alert when the "All Feedbacks" page is reloaded.</li>
  <img width="1850" height="951" alt="Screenshot 2025-07-18 140920" src="https://github.com/user-attachments/assets/32826065-da29-4f61-8dec-d9aca374ec6e" />
  
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
