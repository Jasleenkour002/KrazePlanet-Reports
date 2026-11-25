## Title
Reflected XSS - HTML Tag Filter Bypass

## Description
**Affected URL:** https://news.pwnedbox.site/
A Reflected Cross-Site Scripting (XSS) vulnerability was identified in the News application through the `title` GET parameter. During testing, a specially crafted HTML payload was supplied to the parameter and successfully triggered a JavaScript confirmation dialog in the browser. The application does not properly handle the supplied input before including it in the HTML response, allowing the injected markup to alter the document structure and execute JavaScript. This indicates that the filtering and output-handling mechanisms are insufficient to prevent script injection.

## Steps to Reproduce
1. Open the URL in browser: https://news.pwnedbox.site/.
2. Inspect the page's HTML source or server response.
3. Run the x8 scan to find hidden parameters.
4. Enter the URLencoded payload
5. Verify that the supplied value is not safely handled and that the injected markup can execute JavaScript.
6. Confirm the alert appears that confirm the successful execution of payload.
   
## POC (Proof of Concept)
Screenshot 1 – x8scan output showing the discovered GET parameters.

<img width="1263" height="183" alt="image" src="https://github.com/user-attachments/assets/3d94efc7-6ab9-4433-b792-6e7dfecd9cd9" />

Screenshot 2 – Crafted payload submitted through the `title` parameter.

<img width="1908" height="898" alt="18 News 3" src="https://github.com/user-attachments/assets/3c89fb10-8d17-45db-91ce-cc157f9d415b" />

Screenshot 3 – JavaScript confirmation dialog displaying `alert`.

<img width="1918" height="853" alt="18 News 4" src="https://github.com/user-attachments/assets/ac8605fd-ae90-41b2-8308-73677e1513ab" />

## Impact
- An attacker may execute malicious JavaScript in a victim's browser.
- Depending on the application's security controls, this could expose session-related data and increase the risk of account compromise.
- The vulnerability may allow unauthorized actions to be performed through an affected user's browser.

## Remediation
- Ensure the `lname` parameter is properly HTML-attribute encoded before its value is displayed in the input field.
- Avoid relying on blocklists for HTML tags such as `<script>`, `<img>`, and `<svg>`, or JavaScript functions such as `alert()`, `confirm()`, and `prompt()`, as these can be bypassed.
- If the application uses PHP, apply `htmlspecialchars()` with appropriate flags and UTF-8 encoding before rendering user-controlled input into HTML.
- Configure Cloudflare WAF rules to help detect and block common XSS payloads as an additional layer of defense. WAF protection should complement, not replace, proper output encoding.
