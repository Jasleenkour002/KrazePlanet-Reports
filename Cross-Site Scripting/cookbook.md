## Title
Reflected XSS - Multi-Parameter Script Filter Evasion

## Description
**Affected URL:** https://cookbook.pwnedbox.site/
The application has a Reflected Cross-Site Scripting (XSS) vulnerability associated with the `lname` GET parameter. Testing found that the submitted value was reflected in the Last Name field, whereas the `fname` parameter did not show the same behavior. A mixed-case script tag was able to bypass the existing filtering and trigger JavaScript execution, indicating that the application does not properly encode user input before displaying it.

## Steps to Reproduce
1. Navigate to the target page using a URL: https://cookbook.pwnedbox.site/.
2. Allow the application to process the request and display the page.
3. Check whether a JavaScript alert box appears.
4. Verify that the supplied input is reflected through the `lname` parameter without appropriate output encoding.

## POC (Proof of Concept)
Screenshot 1 – Payload entered in the First Name and Last Name fields.

<img width="1918" height="883" alt="8 Cookbook 1" src="https://github.com/user-attachments/assets/3b7641c6-95a9-4b28-ad4b-77c2aa16b894" />

Screenshot 2 – JavaScript alert successfully executed.

<img width="1918" height="892" alt="8 Cookbook 2" src="https://github.com/user-attachments/assets/9855350f-7658-453a-b952-e2f86147bc0b" />

Screenshot 3 – Reflected payload visible in the HTML page source.

<img width="1918" height="886" alt="8 Cookbook 3" src="https://github.com/user-attachments/assets/89cc033e-392a-457a-8cf4-f86318cd9999" />

## Impact
- The vulnerability could expose session-related information that may be accessible through the victim's browser.
- Successful exploitation may put user accounts at risk of unauthorized access.
- An attacker could potentially execute unwanted actions through a victim's browser.

## Remediation
- Properly encode the `lname` parameter value before inserting it into the Last Name input field.
- Avoid relying on filters that block specific HTML tags or JavaScript functions, as attackers may bypass them using alternative syntax or mixed-case tags.
- In PHP applications, use `htmlspecialchars()` with suitable flags and UTF-8 encoding to safely render user-supplied values in HTML.
- Consider deploying a Web Application Firewall (WAF), such as Cloudflare, to help identify and block common XSS attempts. This should be used alongside secure coding practices, not as the only defense.
