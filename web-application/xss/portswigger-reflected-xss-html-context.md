# Reflected XSS into HTML Context (No Encoding)

**Platform:** PortSwigger Web Security Academy
**Category:** Cross-Site Scripting (XSS)
**Difficulty:** Apprentice

## Objective
Identify and exploit a reflected XSS vulnerability in a blog's search feature.

## Steps
1. Located the search parameter in the URL (`?search=`)
2. Submitted a test value to confirm it was reflected unescaped in the page
3. Injected the payload: `<script>alert(1)</script>`
4. Confirmed execution via a JavaScript alert popup

## Root Cause
User input from the search parameter was inserted directly into the HTML
response without any sanitization or output encoding, allowing injected
script tags to be parsed and executed by the browser.

## Impact
An attacker could craft a malicious link and, via social engineering,
trick a victim into clicking it — executing arbitrary JavaScript in the
victim's browser session, including cookie theft leading to account
takeover.
