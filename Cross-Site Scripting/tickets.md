## Title
Reflected XSS - Event Handler Filter
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://tickets.pwnedbox.site/

A Reflected Cross-Site Scripting (XSS) vulnerability was identified in the Tickets application through the `lname` GET parameter. During testing, a crafted event-handler payload was used, with string concatenation to construct the JavaScript function name. Inspection of the response showed that the `fname` parameter was HTML-encoded, while the `lname` value was inserted into the input element without proper encoding. The payload successfully executed JavaScript in the browser, confirming that the inconsistent output handling allows script injection through the `lname` parameter.

## Steps to Reproduce

1. Visit the URL in a browser and wait for the Tickets page to load.
3. Check whether a JavaScript alert displaying `alert` appears.
4. Inspect the HTML response and locate the reflected parameter values.
5. Enter the URLencoded payload.
6. Confirm that the `alert` appears and it proofs the successful execution of the payload.

## POC (Proof of Concept)

Screenshot 1 – Entered the payload.

<img width="1908" height="882" alt="16 Tickets 1" src="https://github.com/user-attachments/assets/e7d80b6f-0260-4847-92e9-79e7d36ddb81" />

Screenshot 2 – Page source showing encoded `fname` and unescaped event-handler payload reflection in `lname`.

<img width="1918" height="886" alt="16 Tickets 2" src="https://github.com/user-attachments/assets/83351081-6799-4584-b1b8-a66437a1952f" />

Screenshot 3 – Entered the URLencoded payload.

<img width="1878" height="889" alt="16 Tickets 3" src="https://github.com/user-attachments/assets/5ccec70e-3af8-4b2d-abf5-256042b4ff46" />

Screenshot 4 – JavaScript alert displaying `1`, confirming successful execution.

<img width="1918" height="886" alt="16 Tickets 4" src="https://github.com/user-attachments/assets/437065ec-3d56-4721-b618-b39e8456f6a9" />

## Impact
- An attacker may execute malicious JavaScript in a victim's browser.
- Depending on the application's security controls, this could expose session-related data and increase the risk of account compromise.
- The vulnerability may allow unauthorized actions to be performed through an affected user's browser.

## Remediation
- Ensure the `lname` parameter is properly HTML-attribute encoded before its value is displayed in the input field.
- Avoid relying on blocklists for HTML tags such as `<script>`, `<img>`, and `<svg>`, or JavaScript functions such as `alert()`, `confirm()`, and `prompt()`, as these can be bypassed.
- If the application uses PHP, apply `htmlspecialchars()` with appropriate flags and UTF-8 encoding before rendering user-controlled input into HTML.
- Configure Cloudflare WAF rules to help detect and block common XSS payloads as an additional layer of defense. WAF protection should complement, not replace, proper output encoding.
