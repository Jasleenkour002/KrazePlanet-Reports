## Title
Reflected XSS - Global Site Search Portal

## Description
**Affected URL:** https://search.pwnedbox.site/
The application exhibits a Reflected Cross-Site Scripting (XSS) issue in the `fname` and `lname` parameters. Values provided through these parameters are inserted into the First Name and Last Name fields without sufficient HTML attribute encoding. This may allow crafted input to be interpreted as executable script in a visitor's browser.

## Steps to Reproduce
1. Access the application using a URL: https://search.pwnedbox.site/.
2. Wait for the page to process the supplied values and render the form.
3. Check whether a JavaScript alert is triggered by either input.
4. Close the alert dialog if it appears.
5. Review the URL to confirm that both parameters contain the test strings.
6. Examine the HTML source and locate the First Name and Last Name input elements.
7. Determine whether the submitted values are safely encoded within the input attributes. Confirm the finding if the supplied script executes successfully.

## POC (Proof of Concept)
Screenshot 1 – Payload entered in the First Name and Last Name fields.

<img width="1918" height="889" alt="6 Search 1" src="https://github.com/user-attachments/assets/5a3971ba-4c1b-4208-9b9b-6346b3428576" />

Screenshot 2 – JavaScript doesnot executed.

<img width="1918" height="895" alt="6 Search 2" src="https://github.com/user-attachments/assets/d9e06872-b7a0-4741-b7d6-6870d3d85fa3" />

Screenshot 3 – Reflected payload visible in both HTML input fields in page source.

<img width="1918" height="891" alt="6 Search 3" src="https://github.com/user-attachments/assets/b4511679-c166-4699-ac0a-d24b7e519a64" />

Screenshot 4 – Payload entered in the First Name and Last Name fields.

<img width="1918" height="837" alt="6 Search 4" src="https://github.com/user-attachments/assets/e4ea2e08-f932-4147-853a-d9c9685ee130" />

Screenshot 5 – JavaScript alert successfully executed.

<img width="1918" height="882" alt="6 Search 5" src="https://github.com/user-attachments/assets/f689dfef-01d4-4017-a1a2-8b338df3122e" />

Screenshot 6 – Reflected payload visible in both HTML input fields in page source.

<img width="1918" height="843" alt="6 Search 6" src="https://github.com/user-attachments/assets/0a116aa0-c639-47b8-ab5c-d7d3bb3e8400" />

## Impact
- Malicious scripts may expose session-related data accessible through the victim's browser.
- The issue could put user accounts at risk if an attacker is able to exploit the vulnerability further.
- An attacker may trigger unauthorized operations through the affected user's browser.

## Remediation
- Apply proper HTML attribute encoding to values inserted into the First Name and Last Name fields to ensure user input is displayed as text rather than interpreted as code.
- Avoid relying on blacklisting specific HTML elements or JavaScript functions, as attackers may find alternative ways to bypass these filters.
- If the application uses PHP, encode user-controlled values with `htmlspecialchars()` and appropriate flags before inserting them into HTML attributes.
- Consider using a Web Application Firewall (WAF), such as Cloudflare, to provide an additional layer of protection against common XSS attempts. Ensure the application's output-handling logic is fixed rather than relying solely on the WAF.
