# OTP Bypass in TaskBear

## OWASP Top 10
A07:2025 – Authentication Failures

## Summary

While checking the TaskBear sign-up page, I noticed that even after entering an incorrect OTP, I could move ahead by changing the response in Burp Suite. The application accepted the modified response and allowed me to complete the remaining registration steps.

Once I submitted the form, I was able to access the dashboard. This needs further investigation to understand whether email verification is actually being checked by the server or if the registration process can be completed without verifying the email.

## Affected Asset

- **Web Application:** TaskBear
- **Affected URL:** https://taskbear.com.au/auth/sign-up?redirect=%2F

## Steps to Reproduce (PoC)

1. Go to the TaskBear sign-up page and enter your email address to start the registration.
2. Enter a wrong OTP and intercept the response in Burp Suite, which should show `401 Unauthorized`.
3. Now enter the correct OTP and capture the response showing `201 Created`.
4. Open both responses in Burp Suite and Meld to compare them. Modify the response from the failed OTP attempt to match the successful one.
5. Send the modified response and continue with the registration form. After doing these steps the account was created successfully.

## Proof of Concept (PoC)

### Screenshot 1: Sign-Up Page

<img width="1918" height="1015" alt="image" src="https://github.com/user-attachments/assets/d753c8a6-79b3-4fec-9356-d9e1ea2130f7" />

### Screenshot 2: OTP Verification Request with Invalid OTP

<img width="1918" height="1072" alt="image" src="https://github.com/user-attachments/assets/b811b32a-1b5a-4a46-8b44-340b5f2ecc1b" />

### Screenshot 3: Valid OTP Verification Response

<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/28f513eb-02d0-464f-8184-09c5667eedf9" />

### Screenshot 4: Manual Modification of OTP Verification Response

<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/ace9012f-ee55-4460-8d1f-057afc24a8cd" />

### Screenshot 5: Successful Account Creation After OTP Bypass

<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/1a0a711a-63ae-4144-9986-83e817db36e8" />

## Impact

- A user may be able to complete the sign-up process without entering the correct email OTP.
- Accounts could be created with email addresses that have not been verified if the server does not properly check the OTP.
- The application's email verification process may be bypassed without confirming ownership of the email address.

## Remediation

1. **Verify OTP on the Server:** The backend should check whether the OTP is correct before marking the email address as verified.
2. **Improve OTP Security:** Link each OTP to the correct email address and registration session. OTPs should expire after a set time, and repeated incorrect attempts should be limited.
3. **Block Unverified Accounts:** Do not activate the account or allow access to protected features until the server confirms that email verification was successful.
