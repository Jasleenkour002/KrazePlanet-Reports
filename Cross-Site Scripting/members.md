## Title
Stored XSS - Product Reviews (Flipkart Clone)

## Description
**Affected URL:** https://members.pwnedbox.site/
A Stored Cross-Site Scripting (XSS) vulnerability was found in the product review feature. The review content submitted by users is saved and later displayed without proper output encoding. An attacker can submit a review containing specially crafted HTML with JavaScript event handlers. When the review is displayed on the product page, the injected JavaScript executes in the browser.

## Steps to Reproduce
1. Log in to https://members.pwnedbox.site/.
2. Open the product page: https://members.pwnedbox.site/index.php?product_id=1
3. Find the **Write a Customer Review** section.
4. Select a star rating.
5. In the **Headline / Short Title** field, enter the following payload: `ABC"><><Img SrC=x OnErRoR=confirm('0')>`
6. In the **Detailed Review Comments** field, enter the following payload: `ABC"><><Img SrC=x OnErRoR=confirm('4')>`
7. Click **Submit Product Review** to submit the review.
8. Check whether a JavaScript confirmation dialog appears.

## POC (Proof of Concept)
Screenshot 1: XSS payloads entered in the review title and detailed comments fields.

<img width="1918" height="1030" alt="Flipkart 1" src="https://github.com/user-attachments/assets/730e4770-d987-4401-98a9-357fcfcc2e9c" />

Screenshot 2: JavaScript confirmation dialog displaying Des after the review is submitted.

<img width="1918" height="1024" alt="Flipkart 2" src="https://github.com/user-attachments/assets/eeed4ef1-2436-4285-a567-44ad322cb952" />


## Impact
- It allows attackers to execute arbitrary JavaScript in the victim's browser within the context of the vulnerable application.
- It may allow attackers to hijack user sessions, steal sensitive information accessible to the application, or potentially lead to account takeover.
- It allows attackers to perform unauthorized actions on behalf of affected users, including modifying application data or abusing available account privileges.


## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
