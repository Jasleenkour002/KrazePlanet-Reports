## Title
Reflected XSS - Reddit
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://widgets.pwnedbox.site/
The application has a Reflected Cross-Site Scripting (XSS) vulnerability in the `post_id` GET parameter. The value supplied by the user is added directly to the `id` attribute of the **See More Comments** button without being properly encoded.
By using a specially crafted input, it is possible to break out of the existing HTML attribute and insert code that the browser can execute. During testing, a browser alert appeared successfully, confirming that the injected JavaScript was executed.

## Steps to Reproduce

1. Open the following URL in your browser: https://widgets.pwnedbox.site.
2. Wait for the page to load with the modified `post_id` value.
3. Check whether a JavaScript alert displaying `1` appears.
4. Click **OK** to close the alert.

## POC (Proof of Concept)
Screenshot 1 – Parameter discovery using x8scan, identifying post_id.

<img width="1275" height="160" alt="image" src="https://github.com/user-attachments/assets/19cf320a-f90b-47dc-a64a-a66533c75fc5" />

Screenshot 2 – Injected payload inside the See More Comments button's id attribute.

<img width="1918" height="936" alt="widgets 2" src="https://github.com/user-attachments/assets/d22dba9a-b0d9-4872-a924-843f7d2f1345" />

Screenshot 3 – Crafted post_id payload in the URL and successful JavaScript alert execution.

<img width="1918" height="981" alt="widgets 3" src="https://github.com/user-attachments/assets/9c9dc563-fd69-413f-a0a7-d6874bf42c28" />

## Impact
- It allows attackers to hijack user session
- It potentially leads to full account takeover
- It allows to perform  unauthorized actions within the vulnerable application

## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
