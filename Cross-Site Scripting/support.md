## Title
Reflected XSS - Script & Img Tag Filter

## Description
**Affected URL:** https://support.pwnedbox.site/
The application is susceptible to Reflected Cross-Site Scripting (XSS) through the `fname` and `lname` GET parameters. Testing showed that the supplied values were incorporated into the corresponding HTML input fields without adequate context-specific encoding. A crafted HTML event handler triggered JavaScript execution in the browser, indicating that user-controlled input is not being handled safely.

## Steps to Reproduce
1. Navigate to the target page using a URL: https://support.pwnedbox.site/.
2. Wait for the application to process the request and render the page.
3. Check whether a JavaScript alert appears in the browser.
4. Close the alert if it is triggered.
5. Examine the page's HTML response or source code.
6. Find the First Name and Last Name input elements and inspect how their values are rendered.
7. Confirm the issue if the submitted input is reflected without proper HTML attribute encoding and the injected JavaScript executes successfully.

## POC (Proof of Concept)
Screenshot 1 – Crafted payload entered into both First Name and Last Name fields.

<img width="1918" height="855" alt="13 Support 1" src="https://github.com/user-attachments/assets/338ab73a-d6e0-44ae-968d-fa228568959a" />

Screenshot 2 – JavaScript alert displaying `ALERT` executes successfully after submission.

<img width="1918" height="855" alt="13 Support 2" src="https://github.com/user-attachments/assets/c7439fd7-4e02-45cb-89b6-4d8601d76585" />

Screenshot 3 – Page confirms script execution.

<img width="1918" height="894" alt="13 Support 3" src="https://github.com/user-attachments/assets/5c611c70-377b-4314-bd24-aa697b843d88" />

## Impact
- Sensitive information accessible to browser-based scripts may be exposed if session protections are insufficient.
- Successful exploitation could put user accounts at risk of unauthorized access.
- An attacker may be able to trigger unintended operations through the affected user's browser.

## Remediation
- Encode the `fname` and `lname` values according to their HTML attribute context before inserting them into the input fields.
- Avoid relying on blacklisting individual HTML tags or JavaScript functions, as attackers may bypass these restrictions using alternative payloads or event handlers.
- For PHP applications, use `htmlspecialchars()` with appropriate flags and UTF-8 encoding when rendering user-controlled values into HTML.
- Deploy a Web Application Firewall (WAF), such as Cloudflare, as an additional security layer to help detect and block common XSS attempts. This should complement proper output encoding and secure application logic.
