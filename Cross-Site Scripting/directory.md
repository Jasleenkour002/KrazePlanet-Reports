## Title
Reflected XSS - Case-Insensitive Filter Bypass
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://directory.pwnedbox.site/

The application has a Reflected Cross-Site Scripting (XSS) vulnerability involving the `fname` and `lname` GET parameters. The values submitted through these parameters are returned within the HTML input fields without adequate output encoding. During testing, a script tag with mixed capitalization was able to execute in the browser, showing that the application's existing input-handling controls do not adequately prevent script injection.

## Steps to Reproduce

1. Open the target page using a URL containing test input in the `fname` and `lname` parameters.
2. Wait for the application to render the page using the supplied values.
3. Check whether a JavaScript alert box appears.
4. Review the HTML source or response returned by the application.
5. Locate the First Name and Last Name input fields and examine their values.
6. Verify whether the submitted data is reflected without appropriate HTML attribute encoding and confirm the finding if JavaScript execution succeeds.

## POC (Proof of Concept)
Screenshot 1 – Mixed-case XSS payload entered into both First Name and Last Name fields.

<img width="1917" height="838" alt="14 Directory 1" src="https://github.com/user-attachments/assets/e69cc65e-5005-45ef-b8dd-44e6588b2ca7" />

Screenshot 2 – Page source confirms unescaped payload reflection in both `fname` and `lname` input elements.

<img width="1918" height="861" alt="14 Directory 2" src="https://github.com/user-attachments/assets/3b122b56-0b57-4770-8304-45f453183226" />

Screenshot 3 – JavaScript alert displaying `1` executes successfully.

<img width="1918" height="840" alt="14 Directory 3" src="https://github.com/user-attachments/assets/3c70a82b-2481-4399-84be-8b5aeb3c8f5c" />
<img width="1918" height="846" alt="14 Directory 4" src="https://github.com/user-attachments/assets/ba792958-9bb0-427c-907c-fdf5b2c23f35" />
<img width="1918" height="880" alt="14 Directory 5" src="https://github.com/user-attachments/assets/2bea8df1-3d6b-417d-8431-620e9502ad8e" />


## Impact

- The vulnerability may expose session-related information if browser security protections are insufficient.
- Successful exploitation could put user accounts at risk of unauthorized access.
- Malicious scripts may allow an attacker to trigger unwanted actions through a victim's browser.

## Remediation

- Apply proper HTML attribute encoding to the `fname` and `lname` values before displaying them in the corresponding input fields.
- Do not rely on blocking specific HTML tags or JavaScript functions, as mixed-case tags and alternative payloads may bypass these restrictions.
- If the application uses PHP, encode user-controlled values with `htmlspecialchars()` using appropriate flags and UTF-8 encoding before rendering them in HTML.
- Consider using a Web Application Firewall (WAF), such as Cloudflare, to help identify and block common XSS attempts. This should be an additional layer of defense alongside fixing the underlying encoding issue.
