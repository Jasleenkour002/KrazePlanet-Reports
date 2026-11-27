## Title
Open Redirect - Omise

## Description
**Affected URL:** https://links.pwnedbox.site/
The website appears to have an Open Redirect vulnerability that allows users to modify the URL and navigate to an external website. During testing, opening a specially crafted URL redirected the browser away from the original Omise Developer Blog to Bing.

## Steps to Reproduce

1. Open the following URL in your browser: 
2. Replace the URL with the following modified address: 
3. Press **Enter** to load the URL.
4. Observe whether the browser navigates to Bing instead of remaining on the original website.

## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->

## Impact
- Attackers could misuse the trusted domain to create misleading links.
- Users may be redirected to fake websites designed to steal their information.
- This could lead to stolen login credentials and other social engineering attacks.

## Remediation
- Check all redirect destinations against a predefined list of trusted URLs.
- Block requests that attempt to redirect users to unauthorized external websites.
- Normalize the URL before validating it to prevent bypasses.
- Reject malformed URLs and protocol-relative redirect destinations.
- Use predefined destinations on the server instead of accepting arbitrary URLs from users.
- Redirect invalid requests to a safe page within the application.
- Keep logs of suspicious redirect attempts for security monitoring.
