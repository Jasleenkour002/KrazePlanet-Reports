## Title
Stored XSS - Events & Polls Activity Center

## Description
**Affected URL:** https://articles.pwnedbox.site/
A Stored Cross-Site Scripting (XSS) vulnerability was found in the Events & Polls Activity Center. The application saves user-provided information for events and polls and later displays it without proper HTML output encoding. As a result, specially crafted HTML containing JavaScript event handlers can execute when the saved event or poll is viewed. This could affect other users who access the content containing the injected payload.

## Steps to Reproduce
1. Open https://articles.pwnedbox.site/ and log in to the lab account.
2. Navigate to the **Events & Polls** section.
3. In the **Create Community Event** form, enter the following payloads in the respective fields:

   - **Event Title:** `ABC"><><Img SrC=x OnErRoR=confirm(0)>`
   - **Event Date & Time:** `ABC"><><Img SrC=x OnErRoR=confirm(1)>`
   - **Event Description:** `ABC"><><Img SrC=x OnErRoR=confirm(2)>`

4. Click **Publish Event** to submit the event.
5. Check whether a JavaScript confirmation dialog appears and dismiss it.
6. Open the **Interactive Polls** section and enter similar test payloads in the Poll Question, Poll Description, and Poll Options fields.
7. Click **Publish Poll** to submit the poll and check whether a confirmation dialog appears.

## POC (Proof of Concept)
Screenshot 1: Event form populated with payloads in the title, date/time, and description fields.

<img width="1915" height="1024" alt="articles 1" src="https://github.com/user-attachments/assets/4789d00c-0326-44f6-82ab-3144221eb7f6" />

Screenshot 2: Confirmation dialog appears after publishing the event; the event is stored and displayed.

<img width="1918" height="1026" alt="image" src="https://github.com/user-attachments/assets/84eef771-5f82-4968-89ae-941f45ba8fb0" />

Screenshot 3: Poll form populated with payloads in the question, description, and options.

<img width="1918" height="1021" alt="image" src="https://github.com/user-attachments/assets/fc74a403-4089-4970-9b95-994da53a982f" />

Screenshot 4: Confirmation dialog appears after publishing the poll.

<img width="1918" height="1020" alt="image" src="https://github.com/user-attachments/assets/aafa79a5-7640-41dc-88fd-6cdc5a99356c" />


## Impact
- It allows attackers to execute arbitrary JavaScript in the victim's browser within the context of the vulnerable application.
- It may allow attackers to hijack user sessions, steal sensitive information accessible to the application, or potentially lead to account takeover.
- It allows attackers to perform unauthorized actions on behalf of affected users, including modifying application data or abusing available account privileges.

## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
