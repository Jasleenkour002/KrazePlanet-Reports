## Title
Reflected XSS - Multi-Parameter Filter Evasion

## Description
**Affected URL:** https://checkout.pwnedbox.site/
During testing, the `cat` GET parameter was found to reflect user-supplied input in the HTTP response. A test value containing HTML and JavaScript markup was submitted to check how the application handles potentially unsafe input. Parts of the supplied value appeared in the response, suggesting that the parameter requires further review for proper output encoding and filtering. However, JavaScript execution has not yet been confirmed, so additional validation is required before classifying this as a confirmed XSS vulnerability.

## Steps to Reproduce
1. Identify the GET parameters accepted by `https://checkout.pwnedbox.site/` using x8scan.
2. Select the `cat` parameter for further testing.
3. Open the following test URL in a browser:
4. Allow the page to load with the supplied `cat` value.
5. Observe whether any part of the submitted input appears in the response.
6. Inspect the HTTP response or page source to determine how the parameter value is encoded and where it is reflected.
7. Record whether the input is safely handled and perform further validation to determine if it can lead to JavaScript execution.
   
## POC (Proof of Concept)
Screenshot 1 – x8scan identifies the available GET parameters.

<img width="1918" height="843" alt="1  checkout 1" src="https://github.com/user-attachments/assets/576cfec8-7941-4ff3-b3f0-7e687a7b7b74" />

Screenshot 2 – Page source showing the reflected input.

<img width="1206" height="178" alt="image" src="https://github.com/user-attachments/assets/a3862fdb-2689-45d0-8a80-4e2ededf3664" />

Screenshot 3 – Reflected input visible in the browser response.

<img width="1917" height="856" alt="1  checkout 3" src="https://github.com/user-attachments/assets/9461a075-484c-4253-8fc0-601008b63d85" />

Screenshot 4 – Crafted input submitted through the `cat` parameter.

<img width="1918" height="855" alt="1  checkout 4" src="https://github.com/user-attachments/assets/7ac85981-45c8-4841-a76e-dfc9975cb8a2" />

## Impact
- An attacker may execute JavaScript in a user's browser by getting them to open a specially crafted URL.
- Depending on the application's security controls, this could put user sessions and account information at risk.
- The injected script may be able to perform unauthorized actions through the affected user's browser.

## Remediation
- Properly encode the `lname` parameter for HTML attribute context before displaying it in the input field.
- Do not depend on filtering specific HTML tags or JavaScript function names, as obfuscated payloads may bypass these restrictions.
- If the application uses PHP, apply `htmlspecialchars()` with appropriate flags and UTF-8 encoding before rendering user-controlled values in HTML.
- Use a Web Application Firewall (WAF), such as Cloudflare, to help detect and block common XSS attempts. Treat it as an additional layer of protection rather than a replacement for secure output encoding.
