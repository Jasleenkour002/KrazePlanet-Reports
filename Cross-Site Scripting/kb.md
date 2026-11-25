## Title
Reflected XSS - Search Filter Bypass

## Description
**Affected URL:** https://kb.pwnedbox.site/
A Reflected Cross-Site Scripting (XSS) issue was found in the Knowledge Base application through the `search` GET parameter. The value provided by the user is displayed in both the search results heading and the empty-state message without sufficient output encoding. During testing, a specially crafted payload containing a mixed-case script tag and a Unicode escape sequence executed successfully in the browser and triggered an alert displaying `1`. This demonstrates that the application does not safely handle the reflected search input, allowing injected JavaScript to run in the page's context.

## Steps to Reproduce
1. Open the URL: https://kb.pwnedbox.site/.
2. Locate the `search` parameter in the resulting URL.
3. Replace its value with the following test payload:

   `ABC'"><><ScRipT>a\u006cert(1)</ScRipT>`

4. Open the updated URL in the browser.
5. Observe the JavaScript alert displaying `1`.

## POC (Proof of Concept)
Screenshot 1 – Search parameter populated with the normal test value.

<img width="1918" height="1026" alt="kb 1" src="https://github.com/user-attachments/assets/50e328ab-f761-4211-8da1-efaf55124a86" />

Screenshot 2 – Crafted payload in the URL and JavaScript alert displaying 1.

<img width="1918" height="1017" alt="kb 2" src="https://github.com/user-attachments/assets/184077fc-112e-4ee6-bcce-b84e38b76ea9" />

## Impact
- It allows attackers to hijack user session
- It potentially leads to full account takeover
- It allows to perform  unauthorized actions within the vulnerable application

## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
