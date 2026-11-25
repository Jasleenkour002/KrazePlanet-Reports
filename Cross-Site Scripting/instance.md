## Title
Reflected XSS - Cloud Instance Console

## Description
**Affected URL:** https://instance.pwnedbox.site/
The Cloud Instance Console application takes user input through the `fname` and `lname` GET parameters.
During source code review, it was observed that both parameters are handled using PHP's `htmlspecialchars()` function with the `ENT_QUOTES` flag.
This function converts special HTML characters and quotation marks into their corresponding HTML entities, which helps prevent user input from being interpreted as executable HTML or JavaScript when rendered in the correct context.
Based on the source code reviewed, both parameters have output encoding in place. No exploitable Reflected Cross-Site Scripting (XSS) vulnerability was confirmed during the assessment.

## Steps to Reproduce
1. Open the application: https://instance.pwnedbox.site/
2. Identify the `fname` and `lname` parameters in the URL.
3. Enter sample values through these GET parameters.
4. Observe how the application handles the supplied input.
5. Inspect the page source or HTTP response to check how the values are displayed.
6. Verify that special HTML characters and quotation marks are converted into HTML entities before being reflected in the response.

## POC (Proof of Concept)
Screenshot 1: Payloads entered into input fields.

<img width="1918" height="1006" alt="image" src="https://github.com/user-attachments/assets/6e2bc9ed-a6af-40f5-8e60-f6c8862e85c2" />

## Impact
- It allows attackers to execute arbitrary JavaScript in the victim's browser within the context of the vulnerable application.
- It may allow attackers to hijack user sessions, steal sensitive information accessible to the application, or potentially lead to account takeover.
- It allows attackers to perform unauthorized actions on behalf of affected users, including modifying application data or abusing available account privileges.

## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
