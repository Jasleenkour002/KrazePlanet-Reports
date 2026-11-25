## Title
Reflected XSS - Category Filter

## Description
**Affected URL:** https://shop.pwnedbox.site/
A Reflected Cross-Site Scripting (XSS) vulnerability was identified in the Shop application through the `sort` GET parameter. The application reflects the supplied value in the sorting label and category navigation links without adequate output encoding. During testing, a crafted payload containing a mixed-case script tag and a Unicode escape sequence successfully executed JavaScript in the browser, triggering an alert displaying `1`. This confirms that the application does not properly handle the reflected input, allowing JavaScript to execute when a specially crafted URL is accessed.

## Steps to Reproduce
1. Open the following URL in a browser: https://shop.pwnedbox.site/.
2. Wait for the page to load.
3. Enter the JavaScript payload displaying `1`.
4. Verify that the supplied `sort` value is reflected in these locations without proper output encoding.

## POC (Proof of Concept)
Screenshot 1 – Shop page showing the original category and sorting options.

<img width="1918" height="1017" alt="shop 1" src="https://github.com/user-attachments/assets/88cc2322-ff09-4c08-a62e-8ab0f5130900" />

Screenshot 2 – JavaScript alert displaying 1 after opening the crafted URL.

<img width="1917" height="1024" alt="shop 2" src="https://github.com/user-attachments/assets/b651504c-1ea6-4cd5-af0e-5d7da4c89628" />

<img width="1918" height="988" alt="shop 23" src="https://github.com/user-attachments/assets/9744e854-f9ad-4c04-b677-0deafe651b27" />

## Impact
- It allows attackers to hijack user session
- It potentially leads to full account takeover
- It allows to perform  unauthorized actions within the vulnerable application

## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
