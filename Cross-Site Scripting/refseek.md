## Title
Reflected XSS - RefSeek: Academic & Scientific Search Engine

## Description
**Affected URL:** https://refseek.pwnedbox.site/

The search feature has Reflected Cross-Site Scripting (XSS) due to unsafe handling of the value provided by user. The submitted input is returned in the search page without adequate encoding, which may cause injected JavaScript to run in a user's browser when they access a crafted link.

## Steps to Reproduce

1. Access the search page through a URL: https://refseek.pwnedbox.site/.
2. Wait for the response to load and check the browser's behavior.
3. Note whether a JavaScript alert is triggered by the supplied input. Confirm the vulnerability if the script executes successfully

## POC (Proof of Concept)
Screenshot 1 – XSS payload entered in the search box.

<img width="1918" height="879" alt="3 Refseek 1" src="https://github.com/user-attachments/assets/74138df9-d806-4ee0-a7e9-af4404c3d6c3" />

Screenshot 2 – JavaScript alert successfully executed.

<img width="1918" height="886" alt="3 Refseek 2" src="https://github.com/user-attachments/assets/fbd2e580-171d-421d-b49d-f3163200efa1" />

## Impact

- Malicious scripts could be used to access sensitive session data if the application does not have adequate protections.
- An attacker may be able to compromise a user's account under certain conditions.
- The flaw could allow unauthorized operations to be carried out through an affected user's browser.

## Remediation

- Ensure that all search input is handled safely and encoded correctly before being displayed in the response page.
- In PHP, apply `htmlspecialchars()` with suitable flags and character encoding when outputting user-controlled values into HTML.
- Consider deploying a Web Application Firewall (WAF), such as Cloudflare, to help detect and block common XSS attempts. Treat this as an additional security measure, not a substitute for fixing the underlying application code.
