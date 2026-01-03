---
layout: post
title: "01x07 | summarize harden(Tenant) | take 5"
date: 2025-06-02 16:45:00 +1000
categories: episodes
image:
    path: assets/img/01x07.jpg
    width: 1400
    height: 1400
cover: 'assets/images/cover/cover-01x07.jpg'
navigation: True
subclass: 'post'
logo:
---
In this episode Chris shares 5 things you should configure in Microsoft 365 to make your tenant more secure and Koos introduces "Summary Rules" in Microsoft Sentinel. What are "Summary Rules"? And what new opportunities might bring it to your logging strategies?

<iframe src="https://player.rss.com/df3ndr/2055091?theme=dark" width="100%" height="154px" title="01x07 | summarize harden(Tenant) | take 5" frameBorder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen scrolling="no"><a href="https://rss.com/podcasts/df3ndr/2055091/">01x07 | summarize harden(Tenant) | take 5 | RSS.com</a></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/05WUnTCZJ2s?si=_DDnfe3JmL_Se9IG" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## 5 ways to harden your Microsoft 365 tenant Security

There are many out-of-the-box configurations in M365 that are optimized for productivity and less than optimal from a security perspective. I thought it would be a good idea to go back to the basics today and talk about 5 things you can and should be doing to make your tenant more secure.

### Disable user app registration

Setting "Users can register applications" to "No" in Microsoft 365 is a security measure to prevent users from registering their own applications within the organization's environment. Here’s why this can be important:

* Prevent Unauthorized Access: By default, users can register applications that use Azure AD authentication. If misconfigured, these apps could introduce security risks or allow unintended access to sensitive data.
* Reduce Shadow IT: Without restrictions, users might create and integrate applications that bypass IT governance, potentially leading to security vulnerabilities or compliance issues.
* Enhance Governance and Control: This setting ensures that only IT administrators or designated personnel can register applications, maintaining oversight and control over app integrations.
* Minimize Data Exposure Risks: Some applications require extensive permissions to function, including access to organizational data. Disabling user registration prevents apps from accessing sensitive information without approval.

If your organization requires certain users to register applications, you can manage this through specific roles and policies rather than leaving it open to all users.

1. Navigate to Microsoft Entra admin center https://entra.microsoft.com/
2. Click to expand Identity > Users select Users settings.
3. Set Users can register applications to No.
4. Click Save.

### Disable User consent for applications

Setting "User consent for applications" to "Do not allow user consent" in Microsoft 365 enhances security and governance by ensuring only administrators control which applications can access organizational data. Here’s why it’s a recommended practice:

* Prevent Data Exposure: Users may unintentionally grant excessive permissions to third-party apps, risking sensitive data exposure.
* Reduce Security Vulnerabilities: Some apps request broad access scopes, which could lead to unauthorized data leaks or malicious exploitation.
* Maintain Compliance: Organizations handling regulated data need strict access controls to meet security and privacy standards.
* Ensure IT Oversight: Administrators can vet applications before approving access, reducing the risk of shadow IT and unmanaged integrations.

If you need flexibility, you can configure specific consent policies, allowing only trusted applications or designated users to request access. Tenant-wide admin consent
can be requested by users through an integrated administrator consent request workflow or through organizational support processes

1. Navigate to Microsoft Entra admin center https://entra.microsoft.com/
2. Click to expand Identity > Applications select Enterprise applications.
3. Under Security select Consent and permissions > User consent settings.
4. Under User consent for applications select Do not allow user consent.
5. Click the Save option at the top of the window.

### Allow collaboration invitations to trusted domains only

Restricting user invitations to specified domains in Entra ID is a security best practice that ensures external collaboration remains controlled and aligned with organizational policies. Here’s why it’s a good idea:

* Prevent Unauthorized Access: Users might unintentionally invite people from untrusted or personal domains, increasing security risks.
* Enhance Data Protection: Limiting invitations to approved domains ensures sensitive organizational data isn't exposed to unverified external users.
* Maintain Compliance: Certain industries require strict access controls to meet regulatory standards like GDPR or HIPAA.
* Reduce Risks from Shadow IT: Without restrictions, users might invite external collaborators without IT oversight, leading to unmanaged data sharing.
* Strengthen Identity Governance: Ensuring invitations align with approved domains prevents identity management inconsistencies and helps enforce security policies.

If your organization regularly collaborates with specific external partners, this policy ensures that only trusted domains are allowed. You should ensure that you have a process users can follow to request a trusted domain.

1. Navigate to Microsoft Entra admin center https://entra.microsoft.com/
2. Click to expand Identity > External Identities select External collaboration settings.
3. Under Collaboration restrictions, select Allow invitations only to the specified domains (most restrictive) is selected. Then specify the allowed domains under Target domains.

### Manage SharePoint external sharing through domain allow lists

Setting SharePoint to limit external sharing by domain is a strategic way to maintain security, control data access, and prevent unauthorized sharing. Here’s why it’s a good practice:

* Prevent Data Leaks: Without domain restrictions, users could accidentally share sensitive files with untrusted or personal email accounts.
* Enhance Security: Limiting sharing to specific domains ensures external collaboration only happens with verified partners.
* Maintain Compliance: If your organization handles regulated data, restricting external sharing helps meet privacy and security standards.
* Reduce Insider Risks: Prevents users from sharing data with competitors or unauthorized third parties, safeguarding intellectual property.
* Ensure IT Governance: Provides administrators visibility and control over external sharing, reducing shadow IT and unmanaged file access.

If your organization regularly collaborates with specific external entities, this policy allows seamless access while keeping security tight.

1. Navigate to SharePoint admin center https://admin.microsoft.com/sharepoint
2. Expand Policies then click Sharing.
3. Expand More external sharing settings and check Limit external sharing by domain.
4. Select Add domains to add a list of approved domains.
5. Click Save at the bottom of the page.

### Disable communication with unmanaged Teams users

Setting "People in my organization can communicate with unmanaged Teams accounts" to "Off" in Microsoft Teams is an important security measure to control communication and prevent unauthorized data sharing. Here’s why it matters:

* Prevent Unverified Communication: Unmanaged accounts may belong to individuals who aren’t formally part of your organization, increasing security risks.
* Enhance Data Protection: Prevents sensitive conversations, files, and messages from being exchanged with untrusted accounts.
* Reduce Insider Threats: Ensures that only verified, managed accounts can interact with internal users, lowering the risk of data leaks.
* Maintain Compliance: Certain regulations require organizations to manage and track external communications, and allowing unmanaged accounts may violate those policies.
* Improve IT Governance: Keeps communication within approved boundaries, reducing shadow IT risks and unmanaged collaboration.

If your organization needs to collaborate externally, setting up verified guest accounts or using controlled external access policies is a safer alternative.

1. Navigate to Microsoft Teams admin center https://admin.teams.microsoft.com/
2. Click to expand Users select External access.
3. Select the Policies tab
4. Click on the Global (Org-wide default) policy.
5. Set People in my organization can communicate with unmanaged Teams accounts to Off.
6. Click Save.

## Sentinel Summary Rules

In episode 4 back in March I spoke about the different table tiers in Sentinel. Auxiliary tier was still in preview back then, now it's GA. But one of the downsides to these lower-tiered table plans is that you can't use the data for real-time incident creation with your Sentinel Analytic Rules.
And as I eluded earlier; you might want to consider looking into Azure Data Explorer for this reason alone since the costs will even be lower there.

Well, with Summary Rules I think Microsoft took a nice step into the right direction for making sure customers keep their data in Sentinel by increasing the value of logs in Auxiliary and Basis tables.

### What is a Summary Rule?

* Aggregate large sets of data in the background
* Sort of “Scheduled KQL queries”
* Results are stored in separate `Analytics` table(s)

<img src="/assets/images/s01e07/everyday-defender-01x07_summaryrule.jpg" alt="summary_rules" width="100%">

### Example scenarios

* Quickly find potential malicious IPs in your network as part of triage
* Generate alerts on TI indicator matches
* Trigger alerts on baseline anomalies (i.e. `TotalBytesSent`)

<img src="/assets/images/s01e07/everyday-defender-01x07_anomalies.jpg" alt="anomalies" width="100%">

* Bring down retention cost by summarizing high-volume tables(i.e. `MicrosoftGraphActivityLogs`)

### But remember

* Still keep an eye on query performance!
* Enable monitoring and create an alert rule:

  ```kusto
  LASummaryLogs
  | where Status !in("Succeeded", "Started")
  ```

* Summary rule creation needs `Sentinel Contributor`, but tables creation needs at least `Log Analytics Contributor`
* SIEM-as-a-Code deployments should also take destination table creation into account

### Read more

* Microsoft provides [some example scenario's for Summary Rules on their Learn page](https://learn.microsoft.com/en-us/azure/sentinel/summary-rules).
* Dutch Security MVP Bert-Jan Pals (of course he's Dutch 😉) [gathered some useful scenario's and KQL queries](https://kqlquery.com/posts/sentinel-summary-rules/) to get you started with Summary Rules.
* [Time Series visualization of Palo Alto logs to detect data exfiltration](https://techcommunity.microsoft.com/blog/microsoftsentinelblog/time-series-visualization-of-palo-alto-logs-to-detect-data-exfiltration/666344)

## 🛠️ Community Project

### MDE Automator

Microsoft MVP [Eric Mannon](https://www.linkedin.com/in/emannon/) has created a very elaborate Toolkit for Defender for Endpoint! His experiences in the SecOps space led to the creation of a set of tools which can help with day-to-day incident response tasks in MDE environments.

It consists of:

* PowerShell module `MDEAutomator`

  Provides cmdlets for authentication, profile management, live response, response actions, custom detections, advanced hunting and threat indicator management in MDE.

  ```powershell
  # Install & Import from PowerShell Gallery
  Install-Module -Name MDEAutomator -AllowClobber -Force
  Import-Module -Name MDEAutomator -ErrorAction Stop -Force
  ```

* Several Azure Functions _(also built on PowerShell leveraging his `MDEAutomator` module)_

  * `MDEDispatcher`

    Automates bulk management of response actions delivered to endpoints.

  * `MDEOrchestrator`

    Automates bulk management of Live Response commands.

  * `MDEProfiles`

    Automates bulk delivery of custom PowerShell scripts to configure policy on MDE endpoints.

  * `MDETIManager`

    Automates management of Threat Indicators (IOCs) in Microsoft Defender for Endpoint.

  * `MDEAutoHunt`

    Automates bulk threat hunting and exports output to Azure Storage.

  * `MDECDManager`

    Automates synchronization of Custom Detections from a blob container.

Check it out on [Github](https://github.com/msdirtbag/MDEAutomator)

And make sure to follow Eric on LinkedIn! He not only has some useful insights for Incident Response challenges, SIEM and Microsoft Security products in general, his posts are also very enjoyable and funny to read.
