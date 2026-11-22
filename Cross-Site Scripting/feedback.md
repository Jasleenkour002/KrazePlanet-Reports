## Title
Reflected XSS - Script Tag Filter Evasion
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://feedback.pwnedbox.site/

The application has a Reflected Cross-Site Scripting (XSS) vulnerability affecting the `fname` and `lname` GET parameters. The values provided through these parameters are inserted into HTML input fields without sufficient encoding. Testing showed that a script tag with mixed uppercase and lowercase characters was able to execute JavaScript, indicating that the existing input filtering can be bypassed.

## Steps to Reproduce

1. Visit the target page using a URL containing test input in the `fname` and `lname` parameters.
2. Let the application load the page with the supplied values.
3. Check whether a JavaScript alert box appears.
4. Select **OK** to close the alert.


## POC (Proof of Concept)
Screenshot 1 – Payload entered into both name fields.

<img width="1918" height="870" alt="7 Feedback 1" src="https://github.com/user-attachments/assets/2d224e06-a0ff-4f64-8193-d72893d66971" />


Screenshot 2 – Reflected payload visible in the page.

<img width="1918" height="876" alt="7 Feedback 2" src="https://github.com/user-attachments/assets/8bd0c8ef-b12f-4395-a46f-02105e311636" />


Screenshot 3 – Payload Executed.

<img width="1918" height="886" alt="7 Feedback 3" src="https://github.com/user-attachments/assets/894c2f7c-5c35-44d1-9e09-fcfd8ae42156" />


## Impact
## Impact

- The vulnerability may expose sensitive information available within a user's browser session.
- Depending on the application's security controls, successful exploitation could put user accounts at risk.
- An attacker may be able to execute unwanted actions through a victim's browser.

## Remediation

- Apply context-specific HTML attribute encoding to the `fname` and `lname` values before displaying them in the input fields.
- Do not depend on blocking specific tags or JavaScript functions, as variations in casing and alternative payloads may bypass these filters.
- For PHP applications, use `htmlspecialchars()` with appropriate flags and UTF-8 encoding when rendering user-controlled values in HTML.
- Consider implementing a Web Application Firewall (WAF), such as Cloudflare, to help detect and block common XSS attempts. This should complement, not replace, proper output encoding and secure application logic.
