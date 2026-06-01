## 1. SQL Injection in Student Login in Computer Base Test V1.0 Vulnerability Report

## Vulnerability Summary
A critical SQL Injection vulnerability exists in the `username` and `password` parameter of the **Computer Base Test Project v1.0**, allowing remote attackers to execute arbitrary SQL commands. By injecting boolean based payloads, attackers can determine the presence of a SQL Injection flaw by bypassing the login page.

# Affected Product

| **Attribute**        | **Details** |
|----------------------|------------|
| **Product Name**    | Computer Base Test project in PHP |
| **Vendor**          | Kashipara |
| **Version**         | v1.0 |
| **Affected File**   | `/users/query/loginExe.php` |
| **Affected Parameter** | `username` & `password` |
| **Method**         | `POST` |
| **Vulnerability Type** | SQL Injection |
| **Vulnerability Type** |  Boolean Based SQL Injection |

## Official Website

[Kashipara Computer Base Test](https://www.kashipara.com/project/php/13235/computer-base-test-php-project-source-code)


### Vulnerable Code

```php
extract($_POST);

$selAcc = $conn->query(
    "SELECT * FROM examinee_tbl
     WHERE student_email='$username' AND student_password='$pass'"
);
```

### Steps to Reproduce
1. Access Student Login page 
<img width="1399" height="865" alt="Screenshot 2025-07-18 140411" src="https://github.com/user-attachments/assets/1ab654ec-626d-473c-8374-490cf79d46fa" />

2. Enter malicious SQLi Payload like `' OR '1' = '1--+` in username field and anything in password field.
<img width="1448" height="860" alt="2 - SQL in student 2" src="https://github.com/user-attachments/assets/e73c1443-7c54-4c15-a0d1-71983ba337b2" />


3. A success message is shown and we are successfully authenticated into student dashboard, futher confirm by seeing the error.
<img width="1445" height="863" alt="2 - SQL in student" src="https://github.com/user-attachments/assets/0bfcdba7-6bda-4131-9367-0a7722fbd7dd" />


## Impact

- **Data Theft:** Unauthorized access to student dashboard.  
- **Data Manipulation:** Modification or deletion of database records.  
- **Credential Exposure:** Extraction of usernames, passwords, or authentication details.  
- **Server Compromise:** Potential exploitation of underlying server systems.  
- **Reconnaissance:** Enumeration of database structures (tables, columns, schemas).  
- **Financial Loss:** Downtime and potential monetary losses.  
- **Loss of Reputation:** User trust degradation due to service disruption or data breaches.  

## Recommended Mitigations

- **Use Prepared Statements (Parameterized Queries).**  
- **Sanitize User Inputs:** Validate and filter all incoming data.  
- **Implement Web Application Firewall (WAF).**  
- **Use the Principle of Least Privilege (PoLP) for database users.**  
- **Regularly Update and Patch the Application.**  
- **Monitor Logs for Suspicious Activities.**  

For detailed guidelines, refer to: [OWASP SQL Injection Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html).
