## Title
Reflected XSS - Page Heading
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://docs.pwnedbox.site/

The application contains a search-related reflection issue that can be triggered through an additional GET parameter named `q`. Although the visible form only includes First Name (`fname`) and Last Name (`lname`) fields, parameter enumeration identified `q` as another accepted input. Testing showed that data submitted through this parameter was inserted into the page heading without proper output encoding, allowing JavaScript to execute in the browser.

## Steps to Reproduce

1. Perform GET parameter discovery on the target application using `x8scan`.
2. Review the results and note the presence of the `q` parameter, which is not shown in the visible search form.
3. Send a request to the application with a test XSS payload in the `q` parameter.
4. Wait for the response and check whether a JavaScript alert appears.
5. Inspect the returned HTML and locate the page heading.
6. Verify that the submitted value appears in the heading without appropriate output encoding and that script execution is successful.

## POC (Proof of Concept)
Screenshot 1 – `x8scan` identifies the hidden `q` GET parameter alongside `fname` and `lname`.

<img width="772" height="172" alt="9 Docs 2" src="https://github.com/user-attachments/assets/2c3c89cc-e14c-43b2-b231-d07c660fbfda" />

Screenshot 2 – The crafted `q` parameter is supplied in the browser URL.

<img width="1918" height="895" alt="9 Docs 3" src="https://github.com/user-attachments/assets/2bdc6ac9-838f-4228-9e24-823374c109a1" />

Screenshot 3 – JavaScript alert displaying `XSS` executes successfully.

<img width="1918" height="894" alt="9 Docs 4" src="https://github.com/user-attachments/assets/d6f50627-28b3-4132-a775-06743b174714" />

## Impact

- The flaw may expose session-related data that could be accessed by malicious scripts running in a user's browser.
- Depending on the application's security mechanisms, exploitation could increase the risk of unauthorized account access.
- An attacker may be able to execute unintended actions through a user's browser without their consent.

## Remediation

- Ensure the `q` parameter is safely encoded before displaying its value in the page heading or any other HTML element.
- Avoid relying on tag or function blocklists, as attackers can use alternative HTML elements, casing variations, or other techniques to bypass them.
- If the application is built with PHP, use `htmlspecialchars()` with appropriate flags and UTF-8 encoding when rendering user-controlled values in HTML.
- Consider configuring a Web Application Firewall (WAF), such as Cloudflare, to provide additional protection against common XSS payloads while fixing the underlying output-encoding issue.
