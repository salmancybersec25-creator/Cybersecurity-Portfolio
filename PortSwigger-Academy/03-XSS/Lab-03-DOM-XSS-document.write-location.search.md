# Lab 03: XSS - DOM XSS in document.write sink using source location.search

## 🎯 Objective
The goal of this lab was to exploit a DOM-based Cross-Site Scripting (DOM XSS) vulnerability in the search query tracking functionality. The objective was to execute an alert() function by breaking out of an HTML attribute.

## 🔍 Vulnerability Description
DOM-based XSS occurs when a client-side script processes data from an untrusted source (like the URL) and writes it to the DOM in an unsafe way. In this lab, the JavaScript function document.write() takes the search term from location.search and writes it directly into the page inside an img tag without sanitization.

The vulnerable code likely looks similar to: document.write('<img src="/resources/images/tracker.gif?searchTerms=' + query + '">');

## 💥 Exploitation Steps
1. Entered a random alphanumeric string (e.g., test123) into the search box.
2. Used the browser's "Inspect Element" tool to view the resulting HTML.
3. Observed that the string was embedded inside an img src attribute: <img src="/resources/images/tracker.gif?searchTerms=test123">
4. Crafted a payload to break out of the src attribute and the img tag, then injected a new HTML element with an event handler.
5. Entered the payload into the search box and pressed Enter.

Payload used: "><svg onload=alert(1)>

Resulting HTML: <img src="/resources/images/tracker.gif?searchTerms="><svg onload=alert(1)>">

## ⚠️ Impact
An attacker can craft a malicious URL containing the payload. When a victim clicks the link, the script executes in their browser, allowing the attacker to steal session cookies, perform unauthorized actions, or redirect the user to malicious sites. Because the attack is entirely client-side, it can bypass server-side security controls.

## 🛡️ Remediation (Blue Team Perspective)
Avoid using dangerous sinks like document.write(), innerHTML, or eval() with untrusted data. Instead, use safe methods like document.createElement() or textContent. If you must use document.write(), ensure the input is properly encoded for the specific context (HTML entity encoding). Additionally, implement a strict Content Security Policy (CSP) to block inline scripts.
