## Title
Stored XSS - Video Streaming Service (Netflix Clone)

## Description
**Affected URL:** https://netflix.pwnedbox.site/
The Netflix clone has a Stored Cross-Site Scripting (XSS) vulnerability in user-provided content. The username entered during registration is saved and later displayed in the navigation bar without proper HTML encoding. A similar issue exists in the movie reaction comments, where submitted content is stored and displayed in the viewer reactions section without proper sanitization.
During testing, the injected HTML payloads executed JavaScript when the stored content was displayed. This could affect users who visit pages containing the malicious username or movie comments.

## Steps to Reproduce
1. Open https://netflix.pwnedbox.site/index.php.
2. Open the **Sign Up** form.
3. Enter the following payload in the **Username** field:`ABC"><><Img SrC=x OnErRoR=confirm(9)>`
4. Fill in the remaining registration details and submit the form.
5. Check whether a JavaScript confirmation dialog displaying `9` appears.

## POC (Proof of Concept)
**Screenshot 1:** Registration form containing the injected payloads in the Full Name and Username fields.

<img width="1906" height="1017" alt="NETFLIX 1" src="https://github.com/user-attachments/assets/bf47f3be-4f7a-4216-aaf3-e7aa379ddc2f" />

**Screenshot 2:** JavaScript alert displaying `9` after registration, demonstrating execution of the stored username payload.

<img width="1918" height="1026" alt="NETFLIX 2" src="https://github.com/user-attachments/assets/54284fb3-01a7-4a19-a447-186010464516" />

## Impact
- It allows attackers to execute arbitrary JavaScript in the victim's browser within the context of the vulnerable application.
- It may allow attackers to hijack user sessions, steal sensitive information accessible to the application, or potentially lead to account takeover.
- It allows attackers to perform unauthorized actions on behalf of affected users, including modifying application data or abusing available account privileges.

## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
