# Lab 02: Authentication - 2FA simple bypass

## 🎯 Objective
The goal of this lab was to bypass the Two-Factor Authentication (2FA) mechanism. We were provided with a victim's username and password, but did not have access to their 4-digit SMS code. The objective was to access the victim's account page without entering the 2FA code.

## 🔍 Vulnerability Description
The application fails to properly enforce the 2FA step server-side. After the initial login (username and password), the user is assigned a session that is "partially authenticated." The application assumes the user will complete the 2FA step, but it does not verify that the step was actually completed before allowing access to authenticated endpoints (like `/my-account`). 

By intercepting the `GET /login2` request and changing the path to `/my-account` in Burp Suite, we bypass the 2FA checkpoint entirely.

## 💥 Exploitation Steps
1. Logged in with the victim's valid credentials (username: carlos, password: montoya).
2. The application redirected to the `/login2` endpoint to prompt for a 4-digit security code.
3. Intercepted the `GET /login2` request using Burp Suite and sent it to Repeater.
4. Modified the request path from `GET /login2` to `GET /my-account`.
5. Sent the request. The server recognized the partially authenticated session cookie and returned the account dashboard (200 OK), completely bypassing the 2FA requirement.

## ⚠️ Impact
This is a Critical (P1) vulnerability. If an attacker obtains a user's password (via phishing or a data breach), 2FA acts as the final layer of defense. By bypassing 2FA, an attacker achieves a full Account Takeover.

## 🛡️ Remediation (Blue Team Perspective)
The server must enforce strict session state management. 
1. When a user logs in with a password, create a session state that indicates `2FA_PENDING` or `is_authenticated: false`.
2. The server must explicitly check this state on every single authenticated endpoint (like `/my-account`).
3. Accessing `/my-account` while in the `2FA_PENDING` state must result in a forced redirect back to `/login2` or a `403 Forbidden` error.
