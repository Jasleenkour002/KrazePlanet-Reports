## Title
Reflected XSS - Encoding Bypass Attempts

## Description
**Affected URL:** https://mail.pwnedbox.site/
A Reflected Cross-Site Scripting (XSS) vulnerability was identified in the application through the `cat` GET parameter. During testing, a payload containing a mixed-case script tag and a Unicode escape sequence was submitted to assess the application's input-handling controls. The payload successfully executed JavaScript in the browser, displaying an alert with the value `1`. This indicates that the application does not properly encode or handle the reflected input, allowing injected script content to execute in the context of the application.

## Steps to Reproduce
1. Open the URL in the browser: https://mail.pwnedbox.site/.
2. Use x8scan to identify the GET parameters accepted by the application.
3. Select the `cat` parameter for testing.
4. Wait for the page to load with the supplied parameter value.
5. Observe the JavaScript alert displaying `alert`.
6. Verify that the supplied input is not handled with sufficient output encoding, allowing JavaScript execution.
   
## POC (Proof of Concept)
Screenshot 1 – x8scan identifies the available GET parameters.

<img width="1276" height="171" alt="image" src="https://github.com/user-attachments/assets/efd36d1b-b90d-4f21-a669-04c5ca7ad3a0" />

Screenshot 2 – Crafted payload submitted through the cat parameter.

<img width="1918" height="1018" alt="2  mail 2" src="https://github.com/user-attachments/assets/91d40628-3192-48fa-b435-a8a56d72e2c9" />

Screenshot 3 – JavaScript alert displaying alert, confirming successful execution.

<img width="1915" height="1014" alt="2  mail 3" src="https://github.com/user-attachments/assets/de2b69a9-c05e-43a6-a8a1-923f51129b35" />

## Impact
- An attacker may execute JavaScript in a user's browser by getting them to open a specially crafted URL.
- Depending on the application's security controls, this could put user sessions and account information at risk.
- The injected script may be able to perform unauthorized actions through the affected user's browser.

## Remediation
- Properly encode the `lname` parameter for HTML attribute context before displaying it in the input field.
- Do not depend on filtering specific HTML tags or JavaScript function names, as obfuscated payloads may bypass these restrictions.
- If the application uses PHP, apply `htmlspecialchars()` with appropriate flags and UTF-8 encoding before rendering user-controlled values in HTML.
- Use a Web Application Firewall (WAF), such as Cloudflare, to help detect and block common XSS attempts. Treat it as an additional layer of protection rather than a replacement for secure output encoding.
