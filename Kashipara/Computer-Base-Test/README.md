<h1>Stored Cross-Site Scripting (XSS) in Computer Base Test V1.0 Vulnerability Report</h1>

<h2>Vulnerability Summary</h2>
A critical Stored Cross-Site Scripting (XSS) vulnerability was discovered in the /users/adminpanel/admin/home.php file of Kashipara's Computer Base Test (v1.0). Attackers can inject malicious scripts via the smyFeedbacks POST parameter, which are then persistently stored and executed when the search functionality is used.

<h2>Official Website</h2>
https://www.kashipara.com/project/php/13235/computer-base-test-php-project-source-code


| Field               | Details                                                                 |
|---------------------|-------------------------------------------------------------------------|
| Product Name        | Computer Base Test project in PHP                                       |
| Version             | v1.0                                                                    |
| Vendor              | kashipara                                                               |
| Affected File       | `home.php`                                                              |
| Affected Parameter  | `smyFeedbacks`                                                          |
| Request Method      | POST                                                                    |
| Vulnerability Type  | Stored Cross-Site Scripting (XSS)                                       |
