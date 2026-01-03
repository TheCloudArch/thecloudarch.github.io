---
layout: post
title: "01x02_Get-Ignite24Recap.ps1"
date: 2025-01-05 18:00:00 +1100
categories: episodes
image:
    path: assets/img/01x02.jpg
    width: 1400
    height: 1400
cover: 'assets/images/cover/cover-01x02.jpg'
navigation: True
subclass: 'post'
logo:
---
## Topics

* Chris takes a look at the Intune announcements from Ignite
* Koos has some thoughts on new capabilities within Defender XDR and some other Security-related updates.

<iframe src="https://player.rss.com/df3ndr/1828176?theme=dark" style="width: 100%; height: 150px;" title="01x02_Get-Ignite24Recap.ps1" frameBorder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"><a href="https://rss.com/podcasts/df3ndr/1828176/">01x02_Get-Ignite24Recap.ps1 | RSS.com</a></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/tpg4TjtCIdE?si=3tzPz8LeJv1IWh4U" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

### Ignite book of News

* [Ignite '24 Book of News](https://news.microsoft.com/ignite-2024-book-of-news/)
* The terms 'Copilot' and 'AI' were used 259 and 392 times respectively 😜
* Only two product/feature renames! 😳
  * **'Azure Virtual Desktop for Azure Stack HCI'** has been renamed **'Azure Virtual Desktop for Azure Local'**
  * **'Microsoft Purview Data Catalog'** has been renamed **'Microsoft Purview Unified Catalog'**.

## Intune & Entra

### Intune

Microsoft Security Copilot in Intune will expand to more platforms & scenarios

We'll see integration and start to surface in more places inside Intune and the greater Intune Suite:
* Intune Advanced Analytics - Natural language help with KQL queries, multi-device queries, etc.
* Endpoint Privilege Management (EPM) - Copilot insights into apps requesting elevation
* Policy Management - Easier to setting help, insights into settings in other policies, conflicts etc.
* Windows Autopatch - Natural language insights into Win 10/ Win 11 updates, issues with updates etc.

Microsoft Intune is expanding its core device hardware inventory capability for Windows to iOS, Android, macOS and Linux devices.

### Entra

New capabilities strengthen Microsoft’s Security Service Edge solution

If you recall, earlier this year Microsoft announced it's SSE solution that consists of:
* Microsoft Entra Private Access - Helps provide secure access to private resources
* Microsoft Entra Internet Access - Provides secure access to all internet and SaaS apps

Announced at Ignite, Entra Private Access will have some new features added:

* Quick access policies, generally available, make it easy to onboard private apps to Microsoft Entra.
* App Discovery, coming soon to public preview, makes it easy to discover all your private apps.
* Private DNS, in public preview, makes it easy to configure single label names or hostnames that users can use to access resources seamlessly.
* Private network connectors available in the Azure, AWS, and Google Cloud marketplaces, in public preview, improve the admin experience and simplify deployment of private network connectors.

Entra Internet Access is also getting some new features:

* Continuous access evaluation (CAE) support, in public preview, allows network access to be revoked in near real-time when it detects a critical event. It’s like gaining an automatic emergency switch that turns off the Internet until policy conditions are met. Because these controls operate at the network level, they work whether or not the application or client supports modern authentication and CAE natively.
* TLS inspection, in private preview, provides comprehensive visibility of encrypted traffic and enables enhanced URL web category filtering based on full URLs. 

Microsoft Security Copilot will be embedded directly into Microsoft Entra admin center, bringing the available identity skills from the standalone Security Copilot experience, along with new identity capabilities, directly to identity admin workflows

## Defender XDR

Very broad subject of course.

So when we think of "Defender" we might still think about Defender for Endpoint, right?

XDR = Cross Detection & Response

Defender XDR = Defense suite with "all the defenders" like:

* Microsoft Defender for Endpoint (Defender ATP)
* Microsoft Defender for Office 365 (Office 365 ATP)
* Microsoft Defender for Identity (formerly: Azure ATP)
* Microsoft Defender for Cloud Apps (formerly: Cloud App Security)
* Microsoft Defender Vulnerability Management
* Microsoft Defender for Cloud (Azure Defender)
* Microsoft Entra ID Protection (AAD Identity Protection)
* Microsoft Purview (Data Loss Prevention)

### Unification of portals

Different role for Sentinel.

First Sentinel as orchestrator if you will.

Nowadays all "Defenders" are unified in the "Defender portal" (security.microsoft.com).

Last year rebranded to "Unified Security Operations Platform" where also Sentinel (Azure resource) was added as well.

But there are still some features missing for which you needed to switch back to the Azure Portal.

* Workbooks are now available
* Sentinel will now also be available to customers who do not use Defender XDR. Customers will be able to access the embedded Security Copilot experience.

I hope other missing functionality like automation rules and playbooks management will be added soon as well.

#### Other new integrations

* Purview Insider Risk Management is now integrated in the incident page

This appears to be a big year for data security. More on this later...

### Most notable product now GA

* **Microsoft Security Exposure Management**

  Before I explain what it is, it's good to understand the different ways Attackers and Defender generally operate.

  [John Lambert (Microsoft Corporate Vice President Security) once said:](https://github.com/JohnLaTwC/Shared/blob/master/Defenders%20think%20in%20lists.%20Attackers%20think%20in%20graphs.%20As%20long%20as%20this%20is%20true%2C%20attackers%20win.md)

  * Windows Security team since 2000
  * Led the Microsoft Threat Intelligence Center (MSTIC)

  > *Defenders think in lists, attackers think in graphs. As long as this is true, attackers win.*

  Most defenders focus on protecting their assets, prioritizing them, and sorting them by workload and business function. They have plenty of lists of assets in system management services, in asset inventory databases and even in spreadsheets.
  There's one problem with all of this. Attackers don't have a list of assets, they have a graph.
  Assets are connected to each other by security relationships. Attackers breach a network by landing somewhere in the graph and start hacking their way to the next point, finding vulnerable systems by navigating the graph.
  Who creates this graph? Well, you do.

  According to Microsoft Security Exposure Management:
  * Helps you to: understand your attack surface
  * Helps you to: think like an attacker
  * Helps you to: prioritize actions to protect most critical assets.

  It does this by consolidating posture data like:

  * Endpoints
  * Cloud
  * Applications
  * Identities
  * Data
  * Vulnerabilities
  * Attack Surface

  From both Microsoft AND third-party solutions:

  * ServiceNow CMDB
  * Qualys VM
  * Rapid7 VM
  * Tenable
  * Wiz (coming soon)
  * Palo Alto (coming soon)

  And present the data with three three key components:

  * **Attack Surface Management**
    Visualize these relationships in a graph, that's what gives you the "attackers perspective" of your organization.
  * **Attack Path Analysis**
    Highlights how an attacker could potentially abuse your exposures and security gaps. It will show how they would be able to traverse their way through your organization, to your most critical assets.
  * **Unified Exposure Insights**
    Get dynamic metrics and scores for security initiatives like Cloud & Endpoint Security, Ransomware Protection and Zero Trust. This helps you prioritize efforts to close the most important gaps first.

Microsoft Security Exposure Management not only went GA, but it also received a couple of updates during Ignite

* **DACL Support**
  XSPM now includes Discretionary Access Control Lists (DACLs) in attack path analysis.
* **Hybrid Attack Paths**
  now identifies hybrid attack paths, capturing routes that originate on-premises.

[Read more details about Microsoft Security Exposure Management going GA here.](https://techcommunity.microsoft.com/blog/microsoftsecurityandcompliance/unlock-proactive-defense-microsoft-security-exposure-management-now-generally-av/4303219?WT.mc_id=AZ-MVP-5004291)

### Several other notable new additions

* Defender for Office 365 now provides AI-powered email and collaboration security. Using purpose-built Large Language Models. Initial rollout to select customers shown impressive results of a 99.995% attacker intent detection accuracy and filtering.

* Defender Cloud Security Posture Management (CSPM) received new additions. CSPM provides detailed visibility into the security state of your Cloud assets and workloads, and provides hardening guidance to improve your security posture. But not only Azure Cloud, AWS and GCP as well.

  * API security posture  
    Mapping APIs in Azure API Management’s to back-end to gain full context across the entire app, including compute and storage.
  * Container security posture
    Azure, AWS and Google Cloud Platform and third party/private registries like Docker Hub and JFrog Artifactory
  * AI security posture
    Azure OpenAI Service, Azure Machine Learning and Amazon Bedrock

* Microsoft launched "Zero Day Quest" - A $4 million AI and cloud security bug bounty program. "hacking event will be the largest of its kind". Started on November 19th and runs until my birthday on January 19th.
* New capabilities in Purview Data Loss Prevention will prevent oversharing of sensitive information and detect risky AI usage in Copilot.
* Microsoft Purview Data Security Posture Management (DSPM) (preview). Will provide centralized visibility from across Microsoft Purview data security solutions into one place.
  * Information Protection
  * Insider Risk Management
  * Data Loss Prevention

  Data Security Posture Management will help:
  * identify possible labeling and policy gaps
  * unusual patterns and activities that might indicate potential risks and opportunities.

* Security Copilot new features:
  * During incident investigations, analysts commonly review details about related evidence entities (like IP, accounts and devices) There already was a 'Device Summary' but now there's also a new 'Identity Summary', highlighting behavioral anomalies and potential misconfigurations.
  * MDTI (Threat Intelligence) indicator skills can leverage threat intelligence in MDTI to link any IoC (indicator of compromise) to all related data and content.
* Security Copilot enhancements:
  * 'Script and file analysis' simplifies complex investigations by translating what a script does into natural language and streamlining the analysis of multiple executable files.
  * 'Incident Report' compiles all response activities into a detailed report of the security incident. It now comes with third-party integration with ServiceNow
  * 'Incident Summary' in Copilot standalone experience is able to retrieve more details like entities and across both Sentinel and Defender
  * 'Guided Response' Enables security analysts to easily communicate with end users by dynamically generating text for analysts to use
* Security Copilot now in more products:
  * Purview
  * Entra
  * Intune
* 2025 will be the year of Microsoft Purview and Data Security.

## Community Project

### Entra ID Security Config Analyzer (EIDSCA)

Speaking of Workbooks; [Sami](https://www.linkedin.com/in/sami-lamppu/) 🇫🇮, [Thomas](https://www.linkedin.com/in/thomasnaunheim/) 🇩🇪 and [Markus](https://www.linkedin.com/in/markus-pitkaranta/) 🇫🇮 have created the ["Entra ID Security Config Analyzer (EIDSCA)"](https://github.com/Cloud-Architekt/AzureAD-Attack-Defense/blob/main/AADSecurityConfigAnalyzer.md) a while ago. And it contains a lot of great stuff but one of which is [a Workbook](https://github.com/Cloud-Architekt/AzureAD-Attack-Defense/blob/main/AADSecurityConfigAnalyzer.md#azure-workbook) which will evaluate your Entra ID tenant settings with several best practices.

[Check out the project on Github](https://github.com/Cloud-Architekt/AzureAD-Attack-Defense/blob/main/AADSecurityConfigAnalyzer.md).

Follow us on your favorite podcast platform or check us out on [YouTube](https://www.youtube.com/@CloudArchitects/podcasts)
