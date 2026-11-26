## Title
Stored XSS - Twitter

## Description
**Affected URL:** https://ads.pwnedbox.site/
A Stored Cross-Site Scripting (XSS) vulnerability was found in the AdPulse Network Reports feature. The **Report Name** field allows HTML content to be entered without proper sanitization or output encoding. After the report is saved, the supplied input is stored and displayed in the reports table. This can cause the injected JavaScript to execute when a user opens the affected page, potentially impacting other authenticated users who have access to network reports.

## Steps to Reproduce

1. Open https://ads.pwnedbox.site/ and log in.
2. Navigate to the **Reports** section.
3. Click **New Network Report**.
4. Enter the following payload in the **Report Name** field: `ABC"><><Img SrC=x OnErRoR=confirm(1)>`
5. Select an Ad Network and fill in the required report details.
6. Click **Run & Save Report**.
7. Check whether a JavaScript confirmation dialog displaying `1` appears.
8. Close the dialog and confirm that the report has been saved.

## POC (Proof of Concept)
Screenshot 1: Payload entered in the authenticated user's profile area.

<img width="1918" height="1021" alt="image" src="https://github.com/user-attachments/assets/170759a0-10a9-425e-9ea0-543a3a7ee1a5" />

Screenshot 2: Payload successfully executed in the user's profile.

<img width="1918" height="1021" alt="image" src="https://github.com/user-attachments/assets/a59e7745-5a6e-4435-82d5-04ac5a955cba" />

## Impact
- It allows attackers to execute arbitrary JavaScript in the victim's browser within the context of the vulnerable application.
- It may allow attackers to hijack user sessions, steal sensitive information accessible to the application, or potentially lead to account takeover.
- It allows attackers to perform unauthorized actions on behalf of affected users, including modifying application data or abusing available account privileges.

## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
