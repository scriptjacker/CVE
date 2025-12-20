# SQL Injection in Computer Base Test V1.0 Vulnerability Report

## Vulnerability Summary
A critical SQL Injection vulnerability exists in the `exam_id` parameter of the **Computer Base Test Project v1.0**, allowing remote attackers to execute arbitrary SQL commands. By injecting payloads which case SQL syntax to break or time delay, attackers can determine the presence of a SQL Injection flaw by observing SQL syntax error and server response delays.

## Affected Product

| **Attribute**        | **Details** |
|----------------------|------------|
| **Product Name**    | Computer Base Test project in PHP |
| **Vendor**          | Kashipara |
| **Version**         | v1.0 |
| **Affected File**   | `/users/query/submitAnswerExe.php` |
| **Affected Parameter** | `exam_id` |
| **Method**         | `POST` |
| **Vulnerability Type** | SQL Injection |
| **Vulnerability Type** | Error-Based SQL Injection |

## Official Website

[Kashipara Computer Base Test](https://www.kashipara.com/project/php/13235/computer-base-test-php-project-source-code)

## Steps to Reproduce

1. **Student Login: Authenticate to student login using valexam_id credentials**
   
<img wexam_idth="1399" height="865" alt="Screenshot 2025-07-18 140411" src="https://github.com/user-attachments/assets/1ab654ec-626d-473c-8374-490cf79d46fa" />

2. **Access the Vulnerable URL:**  
   ```
   http://127.0.0.1/cve_project_1/users/home.php?page=exam&exam_id=11
   ```
3. **Intercept the Request:**  
   - Enable Burp Suite and set up the browser to route traffic through it.
    <img wexam_idth="1429" height="796" alt="Screenshot 2025-12-20 161937" src="https://github.com/user-attachments/assets/c522ca15-9eed-4f9f-8b36-0489c1406bf4" />


4. **Modify the Parameter:**  
   - Send the request to Burp Suite **Repeater** and modify the `exam_id` parameter with the following payload:
     ```
     ' (single quote)
     ```
     <img wexam_idth="1433" height="763" alt="Screenshot 2025-12-20 162402" src="https://github.com/user-attachments/assets/79802312-217e-4012-8fdc-28446706e0f4" />

    

5. **Send the Modified Request:**  
   - Forward the modified request in Burp Suite Repeater.  
   - Observe the delay in the response Error.  
   - The server will delay its response by 5 seconds, confirming successful execution of the `SLEEP()` function, indicating a **Error-based SQL injection vulnerability**.
   <img wexam_idth="1914" height="810" alt="Screenshot 2025-12-20 162214" src="https://github.com/user-attachments/assets/f7298727-0909-489f-b8ad-4f5bd6b2b683" />


## Impact

- **Data Theft:** Unauthorized access to sensitive user or system data.  
- **Data Manipulation:** Modification or deletion of database records.  
- **Credential Exposure:** Extraction of usernames, passwords, or authentication details.  
- **Server Compromise:** Potential exploitation of underlying server systems.  
- **Reconnaissance:** Enumeration of database structures (tables, columns, schemas).  
- **Financial Loss:** Downtime and potential monetary losses.  
- **Loss of Reputation:** User trust degradation due to service disruption or data breaches.  

## Recommended Mitigations

- **Use Prepared Statements (Parameterized Queries).**  
- **Sanitize User Inputs:** Valexam_idate and filter all incoming data.  
- **Implement Web Application Firewall (WAF).**  
- **Use the Principle of Least Privilege (PoLP) for database users.**  
- **Regularly Update and Patch the Application.**  
- **Monitor Logs for Suspicious Activities.**  

For detailed guexam_idelines, refer to: [OWASP SQL Injection Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html).
