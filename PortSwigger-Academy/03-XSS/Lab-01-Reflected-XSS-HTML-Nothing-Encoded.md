# Lab 01: XSS - Reflected XSS into HTML context with nothing encoded

## 🎯 Objective
The goal of this lab was to exploit a reflected Cross-Site Scripting (XSS) vulnerability in the search functionality. The objective was to execute an alert() function to prove that arbitrary JavaScript can be executed in the victim's browser.

## 🔍 Vulnerability Description
Reflected XSS occurs when an application receives data in an HTTP request and includes that data within the immediate response in an unsafe way. In this lab, the search term is directly embedded into the HTML response without any encoding or sanitization, allowing an attacker to inject executable script tags.

## 💥 Exploitation Steps
1. Navigated to the home page and located the search bar.
2. Tested for reflection by entering a benign string (e.g., 'test') and observing it reflected in the DOM.
3. Injected a malicious JavaScript payload into the search bar.
4. The server reflected the payload exactly as provided, resulting in the browser parsing the script tag and executing the code.

Payload used: <script>alert(1)</script>

## ⚠️ Impact
An attacker can craft a malicious URL containing the payload and trick a victim into clicking it. Once clicked, the script executes in the context of the victim's browser, allowing the attacker to steal session cookies, perform actions on behalf of the user (CSRF), or redirect the user to malicious websites.

## 🛡️ Remediation (Blue Team Perspective)
To fix this vulnerability, the application must perform context-aware output encoding. When reflecting user input into the HTML body, the application should encode special characters (like < and >) into their HTML entities (&lt; and &gt;). Additionally, implementing a strict Content Security Policy (CSP) can help mitigate the impact of XSS vulnerabilities.
