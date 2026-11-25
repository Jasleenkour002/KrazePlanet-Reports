## Title
Reflected XSS - Shopify

## Description
**Affected URL:** https://go.pwnedbox.site/
The account confirmation page has a Reflected Cross-Site Scripting (XSS) vulnerability associated with the `returnTo` GET parameter. This parameter controls the destination of the **Cancel & return** link. Testing found that the supplied value was inserted into the link's `href` attribute without sufficient encoding, allowing crafted input to be interpreted by the browser and trigger JavaScript execution.

## Steps to Reproduce
1. Open the URL: https://go.pwnedbox.site/.
2. Review the input fields.
3. Send a request with a test XSS payload.
4. Wait for the account confirmation page to load.
5. Check whether a JavaScript confirmation dialog appears.
6. Inspect the page's HTML source.
7. Locate the **Cancel & return** link and examine its `href` attribute.
8. Confirm the vulnerability if the submitted input is reflected without proper encoding and JavaScript execution is successful.

## POC (Proof of Concept)
Screenshot 1 – Entered the payload in the input field.

<img width="1918" height="883" alt="12 Go 1" src="https://github.com/user-attachments/assets/8a2c5e6d-1bfd-4178-973a-c19141a9a91b" />

Screenshot 2 – Page source confirms the injected payload is reflected inside the Cancel & return link's `href` attribute.

<img width="1918" height="895" alt="12 Go 5" src="https://github.com/user-attachments/assets/215f8ba2-a582-4b6e-a3fa-e81110449d8c" />

Screenshot 3 – Crafted payload supplied through the input field.

<img width="1918" height="885" alt="12 Go 3" src="https://github.com/user-attachments/assets/84ba9a5b-bafb-4c31-9b70-74759c90eee1" />

Screenshot 4 –  JavaScript confirmation dialog displaying `XSS` executes successfully.

<img width="1918" height="894" alt="12 Go 4" src="https://github.com/user-attachments/assets/3c68eaf1-27a1-4fb5-89c3-516e5ffe3cae" />

## Impact
- The flaw could expose sensitive information accessible through a user's browser session.
- Depending on the application's security protections, exploitation may increase the risk of account compromise.
- An attacker could potentially manipulate page behavior or trigger unintended actions in a victim's browser.

## Remediation
- Validate the `returnTo` parameter and allow only approved destination URLs to prevent untrusted input from being inserted into the link.
- Apply context-appropriate HTML attribute encoding when placing user-controlled values into the `href` attribute. Do not rely solely on blocking specific tags or JavaScript functions.
- For PHP applications, use `htmlspecialchars()` with suitable flags and UTF-8 encoding when rendering values into HTML attributes.
- Consider using a Web Application Firewall (WAF), such as Cloudflare, as an additional layer to help detect and block common XSS attempts. A WAF should complement the underlying code fix rather than replace it.
