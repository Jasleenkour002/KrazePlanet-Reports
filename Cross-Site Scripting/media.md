## Title
Reflected XSS - Imgur Mobile

## Description
**Affected URL:** https://media.pwnedbox.site/
A Reflected Cross-Site Scripting (XSS) vulnerability was identified in the application through the `username` GET parameter. Testing showed that a crafted JavaScript payload supplied through this parameter was reflected in the page and executed in the browser. Further inspection revealed that the supplied username was inserted into the profile link's `href` attribute without proper HTML attribute encoding. This improper handling of user-controlled input allows injected content to be interpreted by the browser, confirming the XSS vulnerability.

## Steps to Reproduce
1. Open the URL in browser and wait for the page to load with the crafted `username` value: https://media.pwnedbox.site/.
2. Inspect the page source or HTML response and locate the profile link.
3. Run `x8` scan to find hidden parameters.
4. Entered the payload within the discovered parameter.
5. Confirm that the input is not handled with appropriate HTML attribute encoding.
6. Confirm the  `alert` appears and this proofs the successful execution of the payload.
   
## POC (Proof of Concept)
Screenshot 1 – x8scan output showing the transformed URL and `username` parameter.

<img width="1209" height="175" alt="2026-11-23_16-13" src="https://github.com/user-attachments/assets/2eba23c7-4c53-42db-a47d-e8f8455b276b" />

Screenshot 2 – Page source showing the injected payload reflected inside the profile link's `href` attribute.

<img width="1918" height="882" alt="17 Media 2" src="https://github.com/user-attachments/assets/0fc1872d-565e-491b-b7ff-f7b3f478e5a5" />

Screenshot 3 – Application page displaying the reflected username.

<img width="1917" height="886" alt="17 Media 3" src="https://github.com/user-attachments/assets/7ad93a8b-f25a-45b7-8945-3fc119a9dadb" />

Screenshot 4 – JavaScript alert displaying `1`, confirming successful execution.

<img width="1918" height="889" alt="17 Media 4" src="https://github.com/user-attachments/assets/7bd0c3da-4efb-409f-84a6-3fc8e9532357" />

## Impact
- An attacker may execute malicious JavaScript in a victim's browser.
- Depending on the application's security controls, this could expose session-related data and increase the risk of account compromise.
- The vulnerability may allow unauthorized actions to be performed through an affected user's browser.

## Remediation
- Ensure the `lname` parameter is properly HTML-attribute encoded before its value is displayed in the input field.
- Avoid relying on blocklists for HTML tags such as `<script>`, `<img>`, and `<svg>`, or JavaScript functions such as `alert()`, `confirm()`, and `prompt()`, as these can be bypassed.
- If the application uses PHP, apply `htmlspecialchars()` with appropriate flags and UTF-8 encoding before rendering user-controlled input into HTML.
- Configure Cloudflare WAF rules to help detect and block common XSS payloads as an additional layer of defense. WAF protection should complement, not replace, proper output encoding.
