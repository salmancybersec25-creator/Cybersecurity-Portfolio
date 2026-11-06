# Lab 01: Authentication - Username enumeration via different responses

## 🎯 Objective
The goal of this lab was to exploit a username enumeration vulnerability in the login form to identify a valid username and then brute-force the associated password.

## 🔍 Vulnerability Description
The application returns a specific error message ("Invalid username") when a username does not exist, but a different message ("Incorrect password") when the username exists but the password is wrong. This difference in the response allows an attacker to enumerate valid users.

## 💥 Exploitation Steps
1. Intercepted the `POST /login` request.
2. Observed that submitting a non-existent user returned "Invalid username". 
3. Tested common usernames (e.g., admin, root, carlos). When a valid username was entered, the server returned "Incorrect password".
4. Once the valid username was identified, used a brute-force approach to test a list of common passwords. A successful login returned a `302 Found` redirect rather than a `200 OK` error.
5. Logged in with the discovered credentials.

## ⚠️ Impact
An attacker can harvest a list of valid usernames. This information is used for targeted phishing, password spraying, or brute-force attacks against specific accounts.

## 🛡️ Remediation (Blue Team Perspective)
The application should use a generic error message for all failed login attempts (e.g., "Invalid username or password") to prevent information leakage. Implement rate limiting or account lockout mechanisms to mitigate brute-force attacks.

