## Title
Basic URL Parameter Redirect

## Description
**Affected URL:** https://redirect.pwnedbox.site/
The application has an Open Redirect vulnerability in the `url` parameter. By changing this parameter, a user can be redirected from the trusted website to an external URL. Attackers could misuse this behavior to create links that appear to come from a legitimate website but take users to potentially malicious destinations. The redirect can be triggered without logging in.

## Steps to Reproduce

1. Open https://redirect.pwnedbox.site/.
2. Modify the `url` parameter to point to an external website using the following URL: `https://redirect.pwnedbox.site/?url=https://google.com%2F`
3. Open the modified URL in your browser.
4. Observe that the browser redirects to the external website, google.

## POC (Proof of Concept)
Screenshot 1 – Open the stories.

<img width="1918" height="838" alt="10  redirect 1" src="https://github.com/user-attachments/assets/9e37e5fb-ec34-4023-910b-e76e050a54b8" />

Screenshot 2 – Sended the intercepted request to burp repeater.

<img width="1918" height="889" alt="10  redirect 2" src="https://github.com/user-attachments/assets/697d510d-a767-4bc4-82d7-d41eed1e2a82" />

Screenshot 3 – Edit the value for redirection and there is 200 OK.

<img width="1918" height="849" alt="10  redirect 3" src="https://github.com/user-attachments/assets/76a150e1-8902-4b5d-b1ff-53575ced57b8" />

Screenshot 4 – The website is redirected successfully.

<img width="1918" height="883" alt="10  redirect 4" src="https://github.com/user-attachments/assets/7219c97d-207f-4a47-8d2f-23d42edd79f1" />

## Impact
- Users may be redirected to malicious websites that could harm their security.
- Attackers can create fake links that look like they belong to a trusted website.
- It may help attackers steal login credentials through phishing.
- It can be used to support social engineering attacks and trick users into sharing sensitive information.

## Remediation
- Allow redirects only to approved and trusted domains.
- Check the destination URL against a predefined list of allowed domains.
- Block redirect requests that point to unauthorized external websites.
- Allow only safe and expected URL schemes, such as `https`.
- Decode and normalize URLs before validating them to prevent validation bypasses.
- Use relative paths for redirects instead of accepting complete URLs from users.
