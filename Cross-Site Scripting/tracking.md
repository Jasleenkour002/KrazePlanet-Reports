## Title
Reflected XSS - Equifax

## Description
**Affected URL:** https://tracking.pwnedbox.site/
A Reflected Cross-Site Scripting (XSS) vulnerability was identified in the Tracking application through the `search` GET parameter. The application uses the supplied search value in its page content and passes it to a JavaScript analytics tracking function without sufficient context-specific escaping. During testing, a specially crafted input caused a JavaScript confirmation dialog to appear in the browser. This indicates that the search value is not safely handled in the JavaScript context and may allow script execution when a crafted URL is opened.

## Steps to Reproduce
1. Open the following URL in a browser: https://tracking.pwnedbox.site/.
2. Allow the page to load with the crafted `search` value.
3. Observe whether a JavaScript confirmation dialog displaying `1` appears.
   
## POC (Proof of Concept)
Screenshot 1 – Original Equifax search page with the search parameter.

<img width="1918" height="1014" alt="tracking 1" src="https://github.com/user-attachments/assets/eddb5981-bd4e-46be-8b6a-c19cc674311b" />

Screenshot 2 – Entering the payload.

<img width="1918" height="994" alt="tracking 2" src="https://github.com/user-attachments/assets/b8415853-c385-4605-85d4-793a11ebe53d" />

Screenshot 3 – Crafted payload triggering the JavaScript confirmation dialog.

<img width="1918" height="1023" alt="tracking 3" src="https://github.com/user-attachments/assets/303b899d-db00-4b73-9b36-bc1ed4be5b0a" />

## Impact
- It allows attackers to hijack user session
- It potentially leads to full account takeover
- It allows to perform  unauthorized actions within the vulnerable application

## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
