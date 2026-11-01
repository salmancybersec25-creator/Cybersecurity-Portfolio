# Lab 05: DOM XSS in jQuery anchor href attribute sink using location.search source

## 🎯 Objective
The goal of this lab was to exploit a DOM-based Cross-Site Scripting (DOM XSS) vulnerability in the "Submit feedback" page. The application uses jQuery to read the `returnPath` parameter from the URL and set it as the `href` attribute of the "Back" button. The objective was to inject a `javascript:` payload to execute an alert.

## 🔍 Vulnerability Description
DOM XSS occurs when client-side JavaScript takes untrusted data (like the URL) and passes it into a dangerous "sink". In this lab, the jQuery `attr()` function was used to set the `href` property of an anchor tag without validation. Because the `href` attribute accepts the `javascript:` pseudo-protocol, an attacker can execute arbitrary code when the link is clicked.

## 💥 Exploitation Steps
1. Navigated to the "Submit feedback" page and observed the `returnPath` parameter in the URL.
2. Modified the URL parameter to inject a JavaScript URI.
3. Reloaded the page. The jQuery script read the parameter and updated the "Back" button's `href` attribute to match our payload.
4. Clicked the "Back" button, which triggered the execution of the JavaScript.

Payload used in URL: ?returnPath=javascript:alert(document.domain)

## ⚠️ Impact
An attacker can craft a malicious link and trick a victim into visiting it. When the victim clicks the compromised "Back" button, the JavaScript executes in the context of the victim's browser, allowing the attacker to steal session cookies or perform actions on their behalf.

## 🛡️ Remediation (Blue Team Perspective)
Avoid allowing user input to control the `href` attribute, especially if it can accept `javascript:` URIs. Developers should validate the input to ensure it is a safe, relative URL or an HTTPS link. Using a library like DOMPurify or encoding the input before passing it to jQuery's `.attr()` function can also prevent execution.
