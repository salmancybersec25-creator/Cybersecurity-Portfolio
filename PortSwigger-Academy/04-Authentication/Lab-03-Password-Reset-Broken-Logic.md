# Lab 03: Authentication - Password reset broken logic

## 🎯 Objective
The goal of this lab was to exploit a flaw in the password reset functionality to take over the `carlos` account. We did not have access to Carlos's email or password reset token.

## 🔍 Vulnerability Description
The application uses a secure token to verify that a password reset request is legitimate. However, when submitting the final request to change the password, the server blindly trusts the `username` parameter provided by the client to determine *whose* password to change. It fails to verify that the user requesting the reset is the actual owner of the account.

## 💥 Exploitation Steps
1. Initiated a password reset for our own account (`wiener`) to obtain a valid reset token.
2. Accessed the reset link and intercepted the final `POST` request using Burp Suite.
3. The request contained the parameter `username=wiener`.
4. Modified the `username` parameter to `username=carlos`, while keeping our own valid reset token.
5. Forwarded the request. The server processed the token and changed Carlos's password to the one we provided.
6. Successfully logged in as `carlos` with the new password.

Payload used in Request Body:
username=carlos&new-password=hacked123&temp-forgot-password-token=YOUR_TOKEN

## ⚠️ Impact
This is a Critical (P1) Account Takeover vulnerability. An attacker only needs a valid account to request a reset token for themselves. By tampering with the username parameter, they can reset the password of *any* user, including administrators.

## 🛡️ Remediation (Blue Team Perspective)
The server must tie the password reset token to the specific user account it was generated for. 
1. When the token is validated, the server should look up the user associated with that token in the database.
2. The server must ignore any `username` parameter sent in the request body and only update the password for the user tied to the token.
