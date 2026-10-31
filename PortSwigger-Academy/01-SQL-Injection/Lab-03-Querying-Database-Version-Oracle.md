# Lab 03: SQL Injection - Querying Database Version on Oracle

## 🎯 Objective
The goal of this lab was to exploit a SQL injection vulnerability in the product category filter to retrieve the database type and version string on an Oracle database.

## 🔍 Vulnerability Description
The application is vulnerable to a UNION-based SQL injection. Because Oracle databases require a valid table name in every SELECT statement, a built-in table called "dual" is used when testing for column count and data types. The system view "v$version" contains the database version information.

## 💥 Exploitation Steps
1. Confirmed the injection point by adding a single quote (') to the category parameter, which caused a server error.
2. Determined the number of columns using ORDER BY, which revealed the query returns 2 columns.
3. Verified that both columns accept text data using the payload: ' UNION SELECT 'abc', 'def' FROM dual--
4. Injected the final payload to retrieve the version string from the v$version system table.

Payload used: ' UNION SELECT BANNER, NULL FROM v$version--

Resulting Query: SELECT * FROM products WHERE category = '' UNION SELECT BANNER, NULL FROM v$version--' AND released = 1

## ⚠️ Impact
An attacker can fingerprint the exact database version. This information allows the attacker to search for known vulnerabilities (CVEs) specific to that version and tailor further exploits, significantly increasing the risk of a full system compromise.

## 🛡️ Remediation (Blue Team Perspective)
Use Parameterized Queries (Prepared Statements) to prevent user input from being interpreted as SQL code. Additionally, implement strict input validation and avoid displaying detailed database error messages to users.
