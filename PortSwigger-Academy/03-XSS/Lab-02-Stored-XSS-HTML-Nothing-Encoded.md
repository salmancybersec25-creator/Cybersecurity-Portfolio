# Lab 02: XSS - Stored XSS into HTML context with nothing encoded

## 🎯 Objective
The goal of this lab was to exploit a Stored Cross-Site Scripting (XSS) vulnerability in the comment functionality. The objective was to submit a comment containing a JavaScript payload that executes automatically when a user views the blog post.

## 🔍 Vulnerability Description
Stored XSS (also known as Persistent XSS) occurs when an application receives data from an untrusted source and saves it in its database. Later, when a different user visits the page, the stored data is rendered in their browser without any validation or encoding. 

In this lab, the comment field is saved to the database and directly embedded into the HTML of the page on every subsequent load.

## 💥 Exploitation Steps
1. Navigated to a specific blog post and scrolled down to the comment section.
2. Filled in the required fields (Name, Email, Website) with dummy data.
3. Injected the malicious JavaScript payload into the Comment field.
4. Submitted the comment, which was then stored in the application's database.
5. When the page reloaded to display the new comment, the browser parsed the script tag and executed the code.

Payload used: <script>alert(1)</script>

## ⚠️ Impact
Because the script is stored on the server, it executes for *every* user who visits that specific blog post. An attacker could use this to steal session cookies from multiple victims, perform unauthorized actions on their behalf, or redirect them to malicious sites. This is far more dangerous than Reflected XSS because the attacker does not need to trick the victim into clicking a unique link.

## 🛡️ Remediation (Blue Team Perspective)
To fix this vulnerability, the application must perform context-aware output encoding when rendering user-generated content. The comment should be encoded so that special characters (like < and >) are rendered as text rather than executable HTML. Additionally, implementing an HTML sanitization library (like DOMPurify) to strip dangerous tags before saving the data is highly recommended.
