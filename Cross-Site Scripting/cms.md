## Title
Stored XSS - Shopify

## Description
**Affected URL:** https://cms.pwnedbox.site/
The Quill CMS application is vulnerable to Stored Cross-Site Scripting (XSS) through its article publishing feature. The editor allows HTML content to be added to an article, but the submitted content is stored and later rendered on the articles page using `innerHTML` without proper sanitization. During testing, a crafted HTML payload containing a JavaScript event handler was saved in an article and executed when the published article was viewed. This could affect other users who access the page containing the malicious content.

## Steps to Reproduce
1. Log in to the CMS at `https://cms.pwnedbox.site/`.
2. Open the **Write** section to create a new article.
3. Enter the following test payload in the article title or HTML content field.
4. Publish the article.
5. Navigate to the **My Articles** page.
6. Observe the confirmation dialog displaying `alert`.
7. Close the dialog and inspect the published article using the browser's developer tools.
8. Confirm that the submitted payload is stored and rendered as HTML, allowing the JavaScript event handler to execute.
   
## POC (Proof of Concept)
Screenshot 1: The account created functionality is used and an account is created

<img width="1918" height="868" alt="1  cms 1" src="https://github.com/user-attachments/assets/7525d444-73b3-4a31-a3ba-1578c538015c" />

Screenshot 2: The XSS payload is entered into the article title, description, and tags fields.

<img width="1915" height="894" alt="1  cms 3" src="https://github.com/user-attachments/assets/c2842c68-9197-4a14-9971-156c04bbf735" />

Screenshot 3: After publishing the article, a JavaScript confirmation dialog displaying 8 appears.

<img width="1918" height="957" alt="1  cms 4" src="https://github.com/user-attachments/assets/3d558b0c-95c7-4ea4-82bd-52a65810d98d" />

## Impact
- An attacker may execute malicious JavaScript in a victim's browser.
- Depending on the application's security controls, this could expose session-related data and increase the risk of account compromise.
- The vulnerability may allow unauthorized actions to be performed through an affected user's browser.

## Remediation
- Ensure the `lname` parameter is properly HTML-attribute encoded before its value is displayed in the input field.
- Avoid relying on blocklists for HTML tags such as `<script>`, `<img>`, and `<svg>`, or JavaScript functions such as `alert()`, `confirm()`, and `prompt()`, as these can be bypassed.
- If the application uses PHP, apply `htmlspecialchars()` with appropriate flags and UTF-8 encoding before rendering user-controlled input into HTML.
- Configure Cloudflare WAF rules to help detect and block common XSS payloads as an additional layer of defense. WAF protection should complement, not replace, proper output encoding.
