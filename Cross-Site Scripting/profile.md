## Title
Reflected XSS - Function Name Filter
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://profile.pwnedbox.site/

The application handles the `fname` and `lname` GET parameters inconsistently. Testing revealed that the First Name value is properly HTML-encoded, while the Last Name value is returned without sufficient encoding. A crafted HTML event-handler input was able to bypass the existing filtering and execute JavaScript when the page loaded, indicating inadequate output handling for the `lname` parameter.

## Steps to Reproduce

1. Access the profile page using a URL containing a test payload.
2. Wait for the page to load and observe whether a JavaScript alert is triggered.
3. Inspect the page's HTML source or response.
5. Compare how the application renders the `fname` and `lname` values.
6. Check whether the First Name value is encoded correctly while the Last Name value is inserted into the HTML without suitable encoding.
7. Confirm the finding if the supplied event-handler payload executes successfully in the browser.

## POC (Proof of Concept)
Screenshot 1 – Crafted payload entered into the First Name and Last Name fields.

<img width="1918" height="883" alt="10 Profile 1" src="https://github.com/user-attachments/assets/4566e2ce-d017-4674-9f2c-39cfe2c083d0" />

Screenshot 2 – JavaScript alert displaying `alert` executes after form submission.

<img width="1918" height="883" alt="10 Profile 3" src="https://github.com/user-attachments/assets/f63d2b79-5c29-46d0-b303-7e1fe5ccb3de" />

Screenshot 3 – Page source shows HTML-encoded `fname` and unencoded payload reflection in `lname`.

<img width="1918" height="880" alt="10 Profile 2" src="https://github.com/user-attachments/assets/cfffe721-4aee-48f6-ab1a-918bc27064d6" />

<img width="1918" height="889" alt="10 Profile 4" src="https://github.com/user-attachments/assets/06a4c4f2-8395-450f-9d54-0f15bb72ddca" />

## Impact

- Malicious JavaScript may access sensitive information available within the victim's browser session.
- The vulnerability could increase the risk of account compromise if other security protections are insufficient.
- An attacker may be able to trigger unintended actions through a user's browser.

## Remediation

- Apply proper HTML attribute encoding to the `lname` parameter before displaying its value in the page.
- Do not rely on blocking specific HTML tags or JavaScript functions, as alternative payloads and event handlers may bypass these filters.
- For PHP applications, use `htmlspecialchars()` with appropriate flags and UTF-8 encoding when rendering user-controlled data into HTML.
- Consider using a Web Application Firewall (WAF), such as Cloudflare, to help detect and block common XSS attempts. This should supplement proper output encoding rather than replace it.
