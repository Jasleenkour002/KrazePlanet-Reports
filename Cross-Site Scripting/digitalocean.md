## Title
Stored XSS - Support Center (DigitalOcean Clone)

## Description
**Affected URL:** https://digitalocean.pwnedbox.site/
A Stored Cross-Site Scripting (XSS) vulnerability was found in the Support Center ticket creation feature. The content entered in the **Subject** and **Description** fields is saved and later displayed in the Support Tickets list without proper output encoding. This causes the browser to interpret the injected HTML instead of treating it as plain text, which can lead to JavaScript execution when the saved ticket is displayed. During testing, a JavaScript confirmation dialog appeared after submitting a ticket, confirming the issue.

## Steps to Reproduce
1. Open the affected website: https://digitalocean.pwnedbox.site/
2. Go to **Support Center** and click **Create Ticket**.
3. Enter the following payload in the **Subject** field: `ABC"'><><Img SrC=x OnErRoR=confirm(1)>`
4. Enter the following payload in the **Description** field: `ABC"'><><Img SrC=x OnErRoR=confirm(2)>`
5. Select the required priority and submit the ticket.
6. Check whether a JavaScript confirmation dialog displaying `2` appears.
7. Close the dialog and locate the saved ticket in the **Support Tickets** list.

## POC (Proof of Concept)
Screenshot 1: Malicious HTML payloads entered into the Subject and Description fields.

<img width="1918" height="1018" alt="digital 1" src="https://github.com/user-attachments/assets/8d27faf8-e158-44ab-bdee-e0cde635fede" />

Screenshot 2: JavaScript confirmation dialog displaying 2 after ticket submission; the stored ticket is visible in the listing.

<img width="1918" height="1021" alt="digital 2" src="https://github.com/user-attachments/assets/410ba22c-9a17-4c19-95f6-a8c2a70cc87a" />

<img width="1918" height="1024" alt="digital 3" src="https://github.com/user-attachments/assets/eb43cc03-5f8d-43e0-afbf-b2c50ae8cbb0" />

## Impact
- It allows attackers to execute arbitrary JavaScript in the victim's browser within the context of the vulnerable application.
- It may allow attackers to hijack user sessions, steal sensitive information accessible to the application, or potentially lead to account takeover.
- It allows attackers to perform unauthorized actions on behalf of affected users, including modifying application data or abusing available account privileges.

## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
