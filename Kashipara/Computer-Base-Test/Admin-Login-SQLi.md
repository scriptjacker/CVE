## 1. SQL Injection in Admin Login in Computer Base Test V1.0 Vulnerability Report

## Vulnerability Summary
A critical SQL Injection vulnerability exists in the `username` and `password` parameter of the **Computer Base Test Project v1.0**, allowing remote attackers to execute arbitrary SQL commands. By injecting boolean based payloads, attackers can determine the presence of a SQL Injection flaw by bypassing the login page.

# Affected Product

| **Attribute**        | **Details** |
|----------------------|------------|
| **Product Name**    | Computer Base Test project in PHP |
| **Vendor**          | Kashipara |
| **Version**         | v1.0 |
| **Affected File**   | `/users/adminpanel/admin/query/loginExe.php` |
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
    "SELECT * FROM admin_acc
     WHERE admin_user='$username' AND admin_pass='$pass'"
);
```

### Steps to Reproduce
1. Access Admin Login page 
<img width="1472" height="842" alt="1 - SQLi in admin panel" src="https://github.com/user-attachments/assets/39a99352-c202-4cfc-82bf-edc1a8ffd225" />

2. Enter malicious SQLi Payload like `' OR '1' = '1--+` in username field and anything in password field.
<img width="1449" height="857" alt="1 - SQLi in admin panel 2" src="https://github.com/user-attachments/assets/5c3b16c8-d2b8-45bd-a790-51e268cc97ef" />

3. A success message is shown and we are successfully authenticated into admin dashboard, futher confirm by seeing the error.
<img width="1449" height="831" alt="1 - SQLi in admin panel 3" src="https://github.com/user-attachments/assets/cf3bf37c-4c0f-41cf-99fb-3c099d4801e8" />


## Impact

- **Data Theft:** Unauthorized access to admin dashboard.  
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
