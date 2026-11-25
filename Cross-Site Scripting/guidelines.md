## Title
Reflected XSS - GIGW: Government Website Guidelines Portal

## Description
**Affected URL:** https://guidelines.pwnedbox.site/

The search feature appears to be vulnerable to Reflected Cross-Site Scripting (XSS) because the value submitted through the s parameter is returned in the page response without being safely encoded. This may allow an attacker to craft a malicious link that executes JavaScript in the browser of a user who opens it. The issue was verified by observing a JavaScript alert triggered by the supplied input.

## Steps to Reproduce

1. Visit the target search page using a URL:
https://guidelines.pwnedbox.site/.
2. Submit the crafted URL and wait for the page to load.
3. Check whether a Script alert appears in the browser.  

## POC (Proof of Concept)
Screenshot 1 – Search results page displaying the submitted input.

<img width="1918" height="886" alt="2 Guidelines 1" src="https://github.com/user-attachments/assets/2dda7d3d-bf44-4c0a-ac66-28e67d7d20fe" />

Screenshot 2 – JavaScript alert successfully executed.

<img width="1918" height="894" alt="2 Guidelines  2" src="https://github.com/user-attachments/assets/3c10dfbb-a507-4f9e-ab7f-2ae7841de897" />

## Impact

- An attacker may gain unauthorized access to a user's session.
- The vulnerability could potentially result in account compromise.
- Malicious scripts may perform actions within the application using the victim's access.

## Remediation

- Properly validate user input and apply context-appropriate output encoding before displaying search values in the HTML response.
- Avoid relying on blocking specific tags such as `<script>`, `<img>`, or `<svg>` alone, as attackers may use alternative payloads to bypass filters.
- For PHP applications, use `htmlspecialchars()` with appropriate flags when rendering user-controlled data into HTML to prevent it from being interpreted as executable code.
