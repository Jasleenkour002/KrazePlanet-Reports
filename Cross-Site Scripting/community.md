## Title
Stored XSS - Forum Discussions (Reddit Clone)

## Description
**Affected URL:** https://community.pwnedbox.site/
A Stored Cross-Site Scripting (XSS) vulnerability was found in the Community Hub application. The issue occurs when user-provided data from registration fields, profile details, and discussion comments is saved and displayed without proper output encoding. As a result, specially crafted HTML can execute JavaScript when the stored content is viewed by another user. This could affect users who visit profiles or discussion pages containing the injected content.

## Steps to Reproduce
1. Open https://community.pwnedbox.site/ and go to the **Register** page.
2. Enter a valid email address and fill in the required registration details.
3. In a profile field such as **First Name**, **About Me**, or **Signature**, enter the following payload: `ABC'"><><img src=x onerror=confirm(1)>`
4. Submit the form and check whether the account is created successfully.
5. Visit the profile page and check whether a JavaScript confirmation dialog appears.

## POC (Proof of Concept)
Screenshot 1: Registration form containing HTML injection payloads in multiple user-controlled fields.

<img width="1918" height="1014" alt="image" src="https://github.com/user-attachments/assets/0ae50711-6509-409d-885f-a4c948bcd212" />

Screenshot 2: Registration completed successfully, confirming the submitted profile data was accepted.

<img width="1918" height="1023" alt="image" src="https://github.com/user-attachments/assets/b59c46ce-ccfe-421b-8cbc-d5098f6654d4" />

Screenshot 3: JavaScript confirmation dialog triggered while viewing the stored user profile.

<img width="1918" height="1023" alt="image" src="https://github.com/user-attachments/assets/e2b9cd2e-e717-47b0-aeaa-8f6344a5dc20" />

## Impact
- It allows attackers to execute arbitrary JavaScript in the victim's browser within the context of the vulnerable application.
- It may allow attackers to hijack user sessions, steal sensitive information accessible to the application, or potentially lead to account takeover.
- It allows attackers to perform unauthorized actions on behalf of affected users, including modifying application data or abusing available account privileges.

## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
