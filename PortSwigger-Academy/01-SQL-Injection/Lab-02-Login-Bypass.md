# Lab 02: SQL Injection - Login Bypass

## 🎯 Objective
The goal of this lab was to exploit a SQL injection vulnerability in the login form to bypass authentication and log in as the administrator user without knowing their password.

## 🔍 Vulnerability Description
The application uses unsanitized user input to construct the SQL query for user authentication. By injecting SQL syntax into the username field, an attacker can alter the query's logic to completely ignore the password check.

The original backend query likely looked like this: SELECT * FROM users WHERE username = 'USER_INPUT' AND password = 'PASSWORD_INPUT'

## 💥 Exploitation Steps
1. Navigated to the "My account" login page.
2. Entered administrator as the username and any random text (e.g., test) as the password.
3. Intercepted the POST /login request using Burp Suite and sent it to Repeater.
4. Modified the username parameter to inject a comment sequence, which comments out the password verification part of the SQL query.

Payload used: administrator'--

Resulting Query: SELECT * FROM users WHERE username = 'administrator'--' AND password = 'test'

## ⚠️ Impact
This is a critical vulnerability. An attacker can gain unauthorized access to any account, including administrative accounts, leading to complete system compromise and data theft.

## 🛡️ Remediation (Blue Team Perspective)
To fix this vulnerability, the application must use Parameterized Queries (Prepared Statements). This ensures that user input is treated strictly as data, not as executable SQL code. Additionally, ensure that error messages do not reveal SQL syntax errors to the user.
