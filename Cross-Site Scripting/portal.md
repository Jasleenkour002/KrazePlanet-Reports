## Title
Reflected XSS - String Concatenation Bypass

## Description
**Affected URL:** https://portal.pwnedbox.site/
A Reflected Cross-Site Scripting (XSS) vulnerability was discovered in the Portal application through the `cat` GET parameter. The application's GET parameters were identified using x8scan, and further testing showed that the `cat` parameter returns user-supplied input in the response without adequate output encoding. A crafted payload containing a mixed-case script tag and a Unicode escape sequence was able to execute JavaScript in the browser, displaying an alert with the value `1`. This confirms that the parameter can be exploited to execute injected JavaScript in the application's context.

## Steps to Reproduce
1. UOpen the URL in the browser: https://portal.pwnedbox.site/.
2. Use x8scan to enumerate the application's available GET parameters.
3. Locate the `cat` parameter in the scan results.
4. Allow the page to load with the supplied `cat` value.
5. Observe how the input is reflected in the response.
6. Verify that a JavaScript alert displaying `1` appears.

## POC (Proof of Concept)
Screenshot 1 – x8scan identifies multiple GET parameters on the target application.

<img width="1309" height="162" alt="image" src="https://github.com/user-attachments/assets/810e6a2a-d607-4d89-8f9e-4e3c82f2fdd3" />

Screenshot 2 – Crafted payload supplied through the cat parameter and reflected on the webpage.

<img width="1918" height="1023" alt="portal 2" src="https://github.com/user-attachments/assets/abc8709a-62e5-41b9-af39-80d8687c4c65" />

Screenshot 3 – JavaScript alert displaying 1, confirming successful execution.

<img width="1918" height="1018" alt="portal 3" src="https://github.com/user-attachments/assets/de5ad6a3-01bf-46f0-82ad-aa3aa1c40c9c" />

## Impact
- It allows attackers to hijack user session
- It potentially leads to full account takeover
- It allows to perform  unauthorized actions within the vulnerable application

## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
