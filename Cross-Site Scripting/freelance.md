## Title
Blind XSS - Freelance Full-Stack Developer

## Description
**Affected URL:** https://freelance.pwnedbox.site/
A Blind Cross-Site Scripting (XSS) vulnerability was identified in the project inquiry form. User-provided data entered in fields such as **Full Name**, **Phone**, and **Message** is saved and later displayed in the administrative inquiry panel without proper output encoding. A specially crafted payload can execute JavaScript when an administrator opens the submitted inquiry. Since the execution takes place in a separate user's browser, the issue is classified as Blind XSS. The execution can be verified through an external XSS callback service.

## Steps to Reproduce

1. Open the affected website: https://freelance.pwnedbox.site/ and navigate to the **Contact** section.
2. Enter an XSS test payload in the **Full Name**, **Phone**, or **Message** field. The payload used during testing was:
3. Fill in the remaining required fields with valid test data and submit the inquiry.
4. Verify that the website displays a successful submission message.
5. Open the administrative login page: https://freelance.pwnedbox.site/admin/login.php
6. Log in to the lab's administrative interface and locate the submitted inquiry.
7. Open the inquiry and check whether the stored payload executes in the administrator's browser.
8. Check the XSS callback dashboard for evidence of execution.

## POC (Proof of Concept)
Screenshot 1: 

Screenshot 2: 


Screenshot 3: 


Screenshot 4: 


Screenshot 5: 


Screenshot 6: 


## Impact
- An attacker may execute JavaScript in an administrator's authenticated browser session.
- Sensitive information accessible to the administrator's page, including session-related or browser storage data, may be exposed if accessible to the injected script.
- The attacker may perform unauthorized actions through the administrator's session, depending on the application's permissions and protections.

## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
