## Title
Stored XSS - Acronis

## Description
**Affected URL:** https://forum.pwnedbox.site/
A Stored Cross-Site Scripting (XSS) vulnerability was found in the Profile Signature field of the Acronis Forum. The signature entered by a user is saved in the database and displayed in forum posts without proper sanitization. An attacker can add specially crafted HTML with a JavaScript event handler to the signature. When another user visits a forum post displaying that signature, the injected JavaScript runs in their browser under the forum's context.

## Steps to Reproduce

1. Visit https://forum.pwnedbox.site/ and log in.
2. Go to **My Profile** and click **Edit Profile**.
3. Find the **Signature** field in the Profile Signature section.
4. Enter the following payload:

   `ABC"><><Img SrC=x OnErRoR=confirm(9)>`

5. Click **Save Profile** to save the changes.
6. Visit a forum post where your signature is visible.
7. Check whether a JavaScript confirmation dialog appears.

## POC (Proof of Concept)
Screenshot 1: Payloads entered into Display Name, About Me, and Profile Signature fields.

<img width="1918" height="1017" alt="FORUM" src="https://github.com/user-attachments/assets/44914306-5b6b-4482-a2a5-fcb2e6b588e5" />

Screenshot 2: Profile saved successfully and the confirmation dialog executes.

<img width="1917" height="1029" alt="FORUM 2" src="https://github.com/user-attachments/assets/b14d434e-7605-4d01-a608-ad0fe46fd6d0" />

## Impact
- It allows attackers to execute arbitrary JavaScript in the victim's browser within the context of the vulnerable application.
- It may allow attackers to hijack user sessions, steal sensitive information accessible to the application, or potentially lead to account takeover.
- It allows attackers to perform unauthorized actions on behalf of affected users, including modifying application data or abusing available account privileges.

## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
