## Title
Stored XSS - Bug Bounty Platform (HackerOne Clone)

## Description
**Affected URL:** https://hackerone.pwnedbox.site/
A Stored Cross-Site Scripting (XSS) vulnerability was identified in the vulnerability report submission feature. The content entered in the **Title**, **Description**, and **Impact** fields is saved and later displayed without proper HTML encoding. This allows specially crafted HTML containing JavaScript event handlers to execute when the submitted report is viewed. As a result, users who open a report containing the injected content may be affected.

## Steps to Reproduce

1. Open the following URL: https://hackerone.pwnedbox.site/index.php?view=submit
2. Enter the following payload in the **Title** field: `ABC"><><ImG SrC=x OnErRoR=confirm(1)>`
3. Enter the following payload in the **Description** field: `ABC"><><ImG SrC=x OnErRoR=confirm(2)>`
4. Enter the following payload in the **Impact** field: `ABC"><><ImG SrC=x OnErRoR=confirm(3)>`
5. Select the target program and severity, then click **Submit Report**.
6. Check whether a JavaScript confirmation dialog appears.
7. Close the dialog and open the submitted report from the dashboard.
    
## POC (Proof of Concept)
Screenshot 1: The report submission form containing the injected payloads in the Title, Description, and Impact fields.

<img width="1918" height="1021" alt="image" src="https://github.com/user-attachments/assets/0918cefd-73b8-4dff-b928-74d317e2b875" />

Screenshot 2: JavaScript confirmation dialog triggered by the stored payload when the submitted report is viewed.

<img width="1918" height="1024" alt="image" src="https://github.com/user-attachments/assets/eb078619-0ed7-4bf7-929c-7eff9b108451" />

## Impact
- It allows attackers to execute arbitrary JavaScript in the victim's browser within the context of the vulnerable application.
- It may allow attackers to hijack user sessions, steal sensitive information accessible to the application, or potentially lead to account takeover.
- It allows attackers to perform unauthorized actions on behalf of affected users, including modifying application data or abusing available account privileges.

## Remediation
- Filter out HTML tags like: `<script>`, `<img>`, `<svg>` from the Report Name field before saving anything to the database
- Filter out JavaScript methods like: `alert()`, `confirm()`, `prompt()` so even if a tag slips through the method won't execute
- If you're using PHP then use `htmlspecialchars()` function before rendering any user input back to the page
- Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
