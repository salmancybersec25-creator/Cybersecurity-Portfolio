# Lab 06: DOM XSS in jQuery selector sink using a hashchange event

## 🎯 Objective
The goal of this lab was to exploit a DOM-based Cross-Site Scripting (DOM XSS) vulnerability in the homepage. The application uses a jQuery selector `$()` on the URL hash and listens for a `hashchange` event. The objective was to deliver a payload to a victim that triggers the `print()` function.

## 🔍 Vulnerability Description
The application uses the jQuery `$()` selector on the `location.hash` value. If an attacker can inject a malicious HTML string into the hash, the jQuery selector will parse it as HTML, creating a new element and executing its event handlers. Because the exploit relies on the `hashchange` event, it must be delivered to a victim in a way that changes the hash without requiring user interaction.

## 💥 Exploitation Steps
1. Identified that the homepage uses `$(location.hash)` inside a `hashchange` event listener.
2. Crafted an HTML payload containing an image with an `onerror` event handler: `<img src=x onerror=print()>`.
3. Accessed the Exploit Server to deliver the payload to the victim.
4. Used an `iframe` to load the victim's homepage and dynamically append the payload to the URL hash using the `onload` event.

Payload used in Exploit Server Body:
```html
<iframe src="https://YOUR-LAB-ID.web-security-academy.net/#" onload="this.src+='<img src=x onerror=print()>'"></iframe>
