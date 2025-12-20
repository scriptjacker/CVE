# SQL Injection Vulnerability Report

## Affected Product

| **Attribute**        | **Details** |
|----------------------|------------|
| **Product Name**    | Online Shopping Portal Project |
| **Vendor**          | PHPGurukul |
| **Version**         | v2.1 |
| **Affected File**   | `Online_Shopping_Portal_project/shopping/track-orders.php` |
| **Affected Parameter** | `orderid` |
| **Method**         | `POST` |
| **Vulnerability Type** | Time-Based Blind SQL Injection |

## Official Website

[PHPGurukul - Online Shopping Portal](https://phpgurukul.com/shopping-portal-free-download/)

## Vulnerability Overview

A SQL Injection vulnerability exists in the `orderid` parameter of the **Online Shopping Portal Project v2.1**, allowing remote attackers to execute arbitrary SQL commands. By injecting time-delay payloads, attackers can determine the presence of a SQL Injection flaw by observing server response delays.

## Steps to Reproduce

1. **Access the Vulnerable URL:**  
   ```
   http://localhost/Online_Shopping_Portal_project/shopping/track-orders.php
   ```
      ![image](https://github.com/user-attachments/assets/966e98cb-98db-46b7-9c5a-ee50e7aba11f)

2. **Intercept the Request:**  
   - Enable Burp Suite and set up the browser to route traffic through it.
    ![image](https://github.com/user-attachments/assets/b6b256c2-6150-40bc-8d42-137af9ab36bf)

3. **Modify the Parameter:**  
   - Send the request to Burp Suite **Repeater** and modify the `orderid` parameter with the following payload:
     ```
     '%2b(select*from(select(sleep(20)))a)%2b'
     ```
     ![image](https://github.com/user-attachments/assets/ca84693f-1f0a-4ef7-9307-be3abb7e2ffa)

    
4. **Send the Modified Request:**  
   - Forward the modified request in Burp Suite Repeater.  
   - Observe the delay in the response time.  
   - The server will delay its response by 20 seconds, confirming successful execution of the `SLEEP()` function, indicating a **time-based SQL injection vulnerability**.
   ![image](https://github.com/user-attachments/assets/bcfd4144-f7fa-477f-81be-af3d1b861462)


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
- **Sanitize User Inputs:** Validate and filter all incoming data.  
- **Implement Web Application Firewall (WAF).**  
- **Use the Principle of Least Privilege (PoLP) for database users.**  
- **Regularly Update and Patch the Application.**  
- **Monitor Logs for Suspicious Activities.**  

For detailed guidelines, refer to: [OWASP SQL Injection Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html).
