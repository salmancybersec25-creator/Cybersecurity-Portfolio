# Lab 01: SQL Injection - Retrieving Hidden Data

## 🎯 Objective
The goal of this lab was to exploit a SQL injection vulnerability in the product category filter to retrieve unreleased (hidden) products from the database.

## 🔍 Vulnerability Description
The application constructs SQL queries by concatenating user input directly into the query string without sanitization. This allows an attacker to manipulate the query logic.

**The original backend query likely looked like this:**
SELECT * FROM products WHERE category = 'Gifts' AND released = 1

## 💥 Exploitation Steps
1. Intercepted the GET /filter?category=Gifts request using Burp Suite.
2. Injected a single quote (') to test for syntax errors, confirming the injection point.
3. Modified the payload to force the WHERE clause to evaluate as TRUE, bypassing the released = 1 restriction.

**Payload used:**
' OR 1=1--

**Resulting Query:**
SELECT * FROM products WHERE category = '' OR 1=1--' AND released = 1

## ⚠️ Impact
An attacker can bypass application logic to view unauthorized data. If this occurred on a login page, it could lead to complete authentication bypass.

## 🛡️ Remediation (Blue Team Perspective)
To fix this vulnerability, the application must use **Parameterized Queries (Prepared Statements)**. This ensures that user input is treated strictly as data, not as executable SQL code.
