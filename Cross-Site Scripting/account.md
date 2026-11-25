## Title
Reflected XSS - Extended Function Filter

## Description
**Affected URL:** https://account.pwnedbox.site/
The application handles the `fname` and `lname` GET parameters differently when displaying user input. During testing, the `fname` value was properly HTML-encoded, but the `lname` value was returned directly inside its HTML input element without adequate encoding.
The test payload included an obfuscated JavaScript event handler that constructs the `alert` function name using string concatenation. This indicates that the output handling for `lname` may allow HTML injection and could lead to reflected Cross-Site Scripting (XSS) if the payload executes in the browser.

## Steps to Reproduce
1. Open the provided test URL in a browser within the authorized testing scope: https://account.pwnedbox.site/.
2. Allow the page to load with the supplied `fname` and `lname` parameters.
3. Inspect the page's HTML source or the server response.
4. Find the locations where the `fname` and `lname` values are displayed.
5. Compare how the two parameters are handled.
6. Verify that `fname` is HTML-encoded, while `lname` is reflected without appropriate encoding.
7. If execution has been confirmed in the browser, record the result as evidence of reflected XSS.

## POC (Proof of Concept)
Screenshot 1 – JavaScript function payload entered into both First Name and Last Name fields.

<img width="1911" height="885" alt="15 Account 1" src="https://github.com/user-attachments/assets/fe391aee-63bf-4d10-811c-0d2b4bded132" />

Screenshot 2 – Page source shows HTML-encoded `fname` and unescaped payload reflection in the `lname` input element.

<img width="1915" height="846" alt="15 Account 2" src="https://github.com/user-attachments/assets/38457ebb-b28a-41ef-8165-72e522c6a946" />

Screenshot 3 – Entering Unicoded payload.

<img width="1918" height="885" alt="15 Account 3" src="https://github.com/user-attachments/assets/8a1eb9ce-f59e-4e7a-b4ea-9b47e38e87ce" />

Screenshot 4 – JavaScript alert displaying `alert` executes successfully.

<img width="1918" height="892" alt="15 Account 4" src="https://github.com/user-attachments/assets/b2b7deaa-0e82-44f7-ab19-3c6ef5e2d41b" />

## Impact
- An attacker may be able to execute malicious JavaScript in a victim's browser.
- Depending on the application's security controls, this could put user sessions and account security at risk.
- The vulnerability may allow attackers to perform unauthorized actions in the context of an affected user's session.

## Remediation
- Apply proper HTML attribute encoding to the `lname` parameter before displaying it in the input field.
- Do not rely on blocking specific HTML tags or JavaScript functions, as attackers may use alternative techniques to bypass such filters.
- If the application uses PHP, apply `htmlspecialchars()` with appropriate flags and UTF-8 encoding before rendering user input into HTML.
- Consider using Cloudflare WAF rules as an additional layer of protection against common XSS payloads. This should complement, not replace, secure output encoding in the application.
