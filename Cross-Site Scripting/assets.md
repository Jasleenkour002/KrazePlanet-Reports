## Title
Reflected XSS - PUBG
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://assets.pwnedbox.site/

The application's community filtering feature is vulnerable to Reflected Cross-Site Scripting (XSS) through the `p` GET parameter. Testing identified that the submitted value was inserted into the `data-query` HTML attribute without sufficient encoding. This allowed the injected content to be interpreted by the browser, and a JavaScript confirmation dialog appeared during testing, confirming the issue.

## Steps to Reproduce

1. Navigate to the application's Community page.
2. Find the Filter field used to search or narrow down community content.
3. Submit a test string containing HTML and JavaScript through the filter.
4. Check whether a JavaScript confirmation popup appears after the request is processed.
5. Close the dialog and inspect the page's HTML response.
6. Locate the `data-query` attribute and review how the submitted value is rendered.
7. Confirm the vulnerability if the supplied input is reflected without proper attribute encoding and script execution is successful.

## POC (Proof of Concept)

Screenshot 1 – XSS payload entered into the PUBG Community Filter field and reflected in the results.

<img width="1914" height="852" alt="11 Assets 1" src="https://github.com/user-attachments/assets/f4cae504-3a9e-4111-bf98-44b27e027666" />

Screenshot 2 – Page source shows the injected payload inside the `data-query` attribute.

<img width="1917" height="882" alt="11 Assets 2" src="https://github.com/user-attachments/assets/dc1264bb-599b-4918-9f66-ffef087bdd1a" />

Screenshot 3 – JavaScript payload entered in the search field.

<img width="1918" height="889" alt="11 Assets 3" src="https://github.com/user-attachments/assets/b0dac569-13d5-4448-b4a1-ee9bd0ec8813" />

Screenshot 4 – JavaScript confirmation dialog displaying `XSS` executes successfully.

<img width="1918" height="855" alt="11 Assets 4" src="https://github.com/user-attachments/assets/efa31d86-d9c4-4163-b32f-fecf4bfa99cf" />

## Impact

- An attacker may be able to access sensitive information available to scripts running in a user's browser.
- Depending on the application's security controls, the issue could increase the risk of unauthorized account access.
- Malicious code may cause actions to be performed through the affected user's browser without their knowledge.

## Remediation

- Properly encode the `p` parameter before inserting its value into the `data-query` HTML attribute.
- Avoid relying on blocklists for specific HTML tags or JavaScript functions, as alternative payloads may bypass these restrictions.
- If the application uses PHP, apply `htmlspecialchars()` with appropriate flags and UTF-8 encoding when rendering user-controlled values into HTML attributes.
- Consider deploying a Web Application Firewall (WAF), such as Cloudflare, to provide additional protection against common XSS attempts. The primary fix should remain secure input handling and context-aware output encoding.
