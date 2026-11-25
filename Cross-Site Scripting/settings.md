## Title
Reflected XSS - Mixed Security Parameters

## Description
**Affected URL:** https://settings.pwnedbox.site/
A Reflected Cross-Site Scripting (XSS) vulnerability was identified on the Settings application through the `academy` GET parameter. The available parameters were enumerated using x8scan, after which the `academy` parameter was tested with specially crafted input containing a mixed-case script tag and a Unicode escape sequence. The application reflected the input without sufficient output encoding, and the injected JavaScript executed successfully in the browser, displaying an alert with the value `1`. This confirms that the parameter can be used to execute injected JavaScript in the application's context.

## Steps to Reproduce
1. Open the following URL in a browser: https://settings.pwnedbox.site/
2. Use x8scan to discover the GET parameters accepted by the application.
3. Locate the `academy` parameter in the results.
4. Allow the page to load with the crafted `academy` value.
5. Observe the page response for the reflected input.
6. Confirm that a JavaScript alert displaying `alert` appears.

## POC (Proof of Concept)
Screenshot 1 – x8scan identifies multiple GET parameters on the application.

<img width="1260" height="217" alt="image" src="https://github.com/user-attachments/assets/da388a79-8cc7-4323-b44e-1205ba1a9320" />

Screenshot 2 – Crafted payload submitted through the academy parameter and reflected on the webpage.

<img width="1918" height="1021" alt="settings 2" src="https://github.com/user-attachments/assets/512f2a87-2b83-4a60-8a91-3b31f05dd4d5" />

Screenshot 3 – JavaScript alert displaying alert, confirming successful XSS execution.

<img width="1918" height="1014" alt="settings 3" src="https://github.com/user-attachments/assets/90d0e522-2506-4638-9b60-c6b762392632" />

## Impact
- An attacker may be able to execute malicious JavaScript in a victim's browser.
- Depending on the application's security controls, this could put user sessions and account security at risk.
- The vulnerability may allow attackers to perform unauthorized actions in the context of an affected user's session.

## Remediation
- Apply proper HTML attribute encoding to the `lname` parameter before displaying it in the input field.
- Do not rely on blocking specific HTML tags or JavaScript functions, as attackers may use alternative techniques to bypass such filters.
- If the application uses PHP, apply `htmlspecialchars()` with appropriate flags and UTF-8 encoding before rendering user input into HTML.
- Consider using Cloudflare WAF rules as an additional layer of protection against common XSS payloads. This should complement, not replace, secure output encoding in the application.
