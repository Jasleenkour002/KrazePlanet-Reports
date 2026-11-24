## Title
Reflected XSS - TutorialRepublic: Web Development Reference Search

## Description
**Affected URL:** https://tutorialrepublic.pwnedbox.site/

The search page does not properly handle user input received through the q parameter. This could allow an attacker to insert malicious script code into a crafted search URL, which may execute in the browser of anyone who visits the link. This issue could be used to affect users and compromise the security of their browsing session.

## Steps to Reproduce
1. Navigate to the search page using a URL.
2. Wait for the search results page to finish loading.
3. Check whether a JavaScript alert box appears automatically in the browser.
4. Close the alert and return to the search results.
5. Examine how the submitted search term is displayed within the page.
6. Review the relevant HTML to determine whether the input is rendered without proper output encoding.
7. Record the result as confirmed Reflected XSS if the injected script executes successfully.

## POC (Proof of Concept)
Screenshot 1 – Payload entered in the search box.

<img width="1918" height="888" alt="1 Tutorial rep 1" src="https://github.com/user-attachments/assets/62b93f2e-1918-4d39-a914-d35dab23adba" />

Screenshot 2 – Script alert executed and visible.

<img width="1918" height="879" alt="1 Tutorial rep  2" src="https://github.com/user-attachments/assets/5c70ddd5-3be8-4110-b756-4ab16bbd8edf" />

## Impact
1. An attacker may steal session information and gain unauthorized access to user accounts.
2. The vulnerability could potentially be exploited to take control of a victim's account.
3. Malicious scripts may perform unauthorized actions on behalf of users within the application.

## Remediation
1. Validate and sanitize the Report Name field on the server side, and safely encode user input before displaying it.
2. Implement context-aware output encoding to prevent user-supplied data from being interpreted as executable JavaScript.
3. For PHP applications, use htmlspecialchars() with appropriate flags when rendering user input into HTML.
4. Deploy a Web Application Firewall (WAF), such as Cloudflare, as an additional layer of protection against common XSS attempts. Do not rely on WAF rules as a replacement for secure coding practices.
