## Title
Reflected XSS - Path Based

## Description
**Affected URL:** https://path-fetch.pwnedbox.site/
A Reflected Cross-Site Scripting (XSS) issue was found in the URL path of the KrazePlanetLabs lab page. The content provided in the URL is reflected into the form's `action` attribute without proper HTML encoding. By modifying the URL to include a crafted payload, it was possible to execute JavaScript in the browser when the page loaded. The vulnerability was verified when a JavaScript alert appeared and the injected content was visible in the page source.

## Steps to Reproduce
1. Open the following URL in your browser: https://path-fetch.pwnedbox.site.
2. Wait for the page to finish loading.
3. Check whether a JavaScript alert displaying `1` appears.
4. Verify that the value provided in the URL path appears in the attribute without proper encoding.
   
## POC (Proof of Concept)
Screenshot 1: Crafted URL containing the path-based XSS payload.

<img width="1917" height="1018" alt="Path fetch 1" src="https://github.com/user-attachments/assets/baa0cde6-f5bc-4df7-9218-bf4e861b4965" />

Screenshot 2: JavaScript alert displaying 1, confirming script execution.

<img width="1918" height="961" alt="Path fetch 12" src="https://github.com/user-attachments/assets/8253d273-a74f-451d-bf35-2c191138df8f" />

## Impact
- It allows attackers to hijack user session
- It potentially leads to full account takeover
- It allows to perform  unauthorized actions within the vulnerable application

## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
