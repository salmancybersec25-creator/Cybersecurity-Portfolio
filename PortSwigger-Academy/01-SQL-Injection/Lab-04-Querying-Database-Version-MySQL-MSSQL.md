# Lab 04: SQL Injection - Querying Database Version on MySQL and Microsoft

## 🎯 Objective
The goal of this lab was to exploit a SQL injection vulnerability in the product category filter to retrieve the database type and version string on a MySQL or Microsoft SQL Server database.

## 🔍 Vulnerability Description
The application is vulnerable to a UNION-based SQL injection. Unlike Oracle, MySQL and Microsoft SQL Server do not require a FROM clause for simple SELECT statements. The global variable @@version can be used to retrieve the exact database version.

## 💥 Exploitation Steps
1. Confirmed the injection point by adding a single quote (') to the category parameter.
2. Determined the number of columns using ORDER BY, which revealed the query returns 2 columns.
3. Verified that both columns accept text data using the payload: ' UNION SELECT 'abc', 'def'--
4. Injected the final payload to retrieve the version string using the @@version variable.

Payload used: ' UNION SELECT NULL, @@version--

Resulting Query: SELECT * FROM products WHERE category = '' UNION SELECT NULL, @@version--' AND released = 1

## ⚠️ Impact
An attacker can fingerprint the exact database version. This information allows the attacker to search for known vulnerabilities (CVEs) specific to that version and tailor further exploits, significantly increasing the risk of a full system compromise.

## 🛡️ Remediation (Blue Team Perspective)
Use Parameterized Queries (Prepared Statements) to prevent user input from being interpreted as SQL code. Additionally, implement strict input validation and avoid displaying detailed database error messages to users.
