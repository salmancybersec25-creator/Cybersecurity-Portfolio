# Lab 03: Business Logic - Inconsistent Security Controls

## 🎯 Objective
The goal of this lab was to exploit a flaw in the application's security controls to gain administrator access and delete the user carlos. The application attempts to protect the admin panel by restricting it to users with an internal company email domain.

## 🔍 Vulnerability Description
The application suffers from a logic flaw where it blindly trusts the email address the user provides during profile updates. It does not verify if the user is actually an employee before granting them internal privileges based solely on the email domain.

The intended flow is that only users with an email ending in @dontwannacry.com should be granted administrative rights.

## 💥 Exploitation Steps
1. Clicked the "Email client" button on the lab page to find the unique lab email domain.
2. Registered a new account using an email address on that domain (e.g., test@your-lab-id.web-security-academy.net).
3. Verified the registration by clicking the link in the email client.
4. Logged in and navigated to the "My account" page.
5. Updated the email address to test@dontwannacry.com.
6. Accessed the "Email client" again to verify the new internal email address.
7. Refreshed the main page, which now displayed the "Admin panel" link.
8. Accessed the Admin panel and deleted the user carlos.

## ⚠️ Impact
An attacker can escalate their privileges to administrator by simply spoofing an internal email domain. Once admin access is gained, the attacker can view sensitive internal data, manage users, and perform destructive actions like deleting accounts.

## 🛡️ Remediation (Blue Team Perspective)
The application must not rely on client-provided email domains for authorization. Admin roles must be assigned manually by a super-administrator or through a secure, out-of-band internal onboarding process. Additionally, the application should implement strict server-side validation to ensure that email verification actually ties back to the true identity of the user.
