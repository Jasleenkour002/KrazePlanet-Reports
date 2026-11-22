## Title
Reflected XSS - FreshCart: Online Grocery Store
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://freshcart.pwnedbox.site/

The application's search feature is susceptible to Reflected Cross-Site Scripting (XSS). The search value supplied through the search box and is returned in the page response without adequate output encoding. This may allow an attacker to execute JavaScript in a user's browser by sharing a specially crafted URL. The finding was validated by observing a JavaScript alert triggered by the input.

## Steps to Reproduce

1. Navigate to the target website using a URL containing a payload.
2. Wait for the search page to load and process the submitted value.
3. Check whether a JavaScript alert box appears in the browser.
4. Verify whether the input is reflected without proper encoding and whether the supplied script executes successfully.

## POC (Proof of Concept)
Screenshot 1 – XSS payload entered in the product search box.

<img width="1918" height="886" alt="5 Freshcart 1" src="https://github.com/user-attachments/assets/08b9b7af-ceda-4073-89db-d53bb66e039e" />


Screenshot 2 – JavaScript alert successfully executed.

<img width="1918" height="883" alt="5 Freshcart 2" src="https://github.com/user-attachments/assets/5fa8d7ff-afcf-472a-8dcb-52eab47e261d" />



## Impact

- Session-related information may be exposed if the injected script can access data available to the browser.
- The vulnerability could contribute to account compromise, depending on the application's session protections.
- An attacker may be able to trigger unwanted actions through a user's browser without their knowledge.

## Remediation

- Encode user-provided values before displaying them in search status messages or other parts of the HTML page.
- Do not rely on removing individual HTML tags or JavaScript functions, as this approach may fail to prevent alternative XSS techniques.
- For PHP-based applications, use `htmlspecialchars()` with appropriate flags and UTF-8 encoding when inserting user-controlled data into HTML output.
- Use a Web Application Firewall (WAF), such as Cloudflare, as an additional defensive layer to help block common XSS payloads, while addressing the root cause within the application.
