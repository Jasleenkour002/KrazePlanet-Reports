## Title
Reflected XSS - PubMed: National Library of Medicine Search Builder
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://pubmed.pwnedbox.site/

The search feature is affected by Reflected Cross-Site Scripting (XSS). The value provided through the `term` parameter is returned in the page content without sufficient encoding, which may allow browser-side script execution when a user visits a specially prepared URL. The issue can be observed when the submitted search value appears in the warning message and is interpreted as executable content instead of being treated as plain text.

## Steps to Reproduce

1. Open the target website using a URL and enter the payload.
2. Wait for the page to finish loading and review any messages displayed.
3. Check whether a JavaScript alert appears as a result of the submitted input.
4. Confirm the injected JavaScript executes in the browser.

## POC (Proof of Concept)
Screenshot 1 – XSS payload entered in the search box.

<img width="1918" height="888" alt="4 Pubmed 1" src="https://github.com/user-attachments/assets/bc26e467-8862-47f0-b008-373a9d02964a" />

Screenshot 2 – JavaScript alert successfully executed.

<img width="1918" height="891" alt="4 Pubmed 2" src="https://github.com/user-attachments/assets/90ffa60e-c7a7-4122-9f0d-2ba9e3ff9293" />

The alert confirms that the browser executed the JavaScript.

## Impact

- An attacker may exploit the issue to access sensitive session information, depending on the application's security controls.
- User accounts could be at risk of compromise if the vulnerability can be chained with other weaknesses.
- Unauthorized actions may be performed through a victim's browser if the injected script executes successfully.

## Remediation

- Ensure that user-supplied search terms are treated as plain text and safely encoded before being included in the page response.
- Avoid using simple blocklists for HTML tags or JavaScript functions, as these can often be bypassed with alternative payloads.
- For PHP applications, use `htmlspecialchars()` with appropriate flags and UTF-8 encoding when displaying user-controlled data in HTML.
- Configure a Web Application Firewall (WAF), such as Cloudflare, to help identify and block common XSS attempts, while also correcting the underlying input-handling and output-encoding issues.
