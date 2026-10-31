# Lab 09: SQL Injection - Retrieving Data From Other Tables

## 🎯 Objective
The goal of this lab was to exploit a SQL injection vulnerability in the product category filter to retrieve all usernames and passwords from the users table, and then use the administrator credentials to log in and solve the lab.

## 🔍 Vulnerability Description
The application is vulnerable to a UNION-based SQL injection. The results of the query are returned in the application's response, allowing an attacker to append a second SELECT statement and retrieve data from any table in the database. The lab provided the target table name (users) and its column names (username and password).

## 💥 Exploitation Steps
1. Intercepted the category filter request and sent it to Burp Suite Repeater.
2. Determined the query returns 2 columns using ORDER BY.
3. Verified both columns accept text data using the payload: ' UNION SELECT 'abc', 'def'--
4. Injected the final payload to extract the credentials from the users table.
Payload used: ' UNION SELECT username, password FROM users--
5. Located the administrator credentials in the response, logged in, and solved the lab.

Resulting Query: SELECT * FROM products WHERE category = '' UNION SELECT username, password FROM users--' AND released = 1

## ⚠️ Impact
An attacker can dump the entire contents of the users table, exposing all usernames and plaintext or hashed passwords. This leads to a complete account takeover of every user on the platform, including administrators.

## 🛡️ Remediation (Blue Team Perspective)
Use Parameterized Queries (Prepared Statements) to prevent user input from being interpreted as SQL code. Additionally, implement the principle of least privilege for database accounts and never display raw query results or database errors to the user.
