# Lab 04: XSS - DOM XSS in innerHTML sink using source location.search

## 🎯 Objective
The goal of this lab was to exploit a DOM-based Cross-Site Scripting (DOM XSS) vulnerability in the search functionality. The objective was to execute an alert() function by injecting a payload into an innerHTML sink.

## 🔍 Vulnerability Description
The application uses the innerHTML property to write user-supplied data (from location.search) directly into a <span> element. Unlike document.write(), the innerHTML property does not execute <script> tags. Therefore, an attacker must use HTML elements that execute JavaScript via event handlers, such as onerror or onload.

## 💥 Exploitation Steps
1. Entered a random string into the search box and inspected the DOM.
2. Observed that the input was reflected inside a span tag: <span id="searchMessage">test123</span>.
3. Because innerHTML does not execute <script> tags, crafted a payload using an image tag with a broken source and an onerror event handler.
4. Injected the payload into the search box.

Payload used: <img src=1 onerror=alert(1)>

Resulting HTML: <span id="searchMessage"><img src=1 onerror=alert(1)></span>

## ⚠️ Impact
An attacker can craft a malicious URL containing the payload. When a victim clicks the link, the script executes in their browser, allowing the attacker to steal session cookies, perform unauthorized actions, or redirect the user to malicious sites. 

## 🛡️ Remediation (Blue Team Perspective)
Avoid using innerHTML with untrusted data. Instead, use safe alternatives like textContent or document.createElement() to insert data into the DOM. If HTML formatting is required, use a robust sanitization library (like DOMPurify) to strip out dangerous tags and attributes before rendering.
