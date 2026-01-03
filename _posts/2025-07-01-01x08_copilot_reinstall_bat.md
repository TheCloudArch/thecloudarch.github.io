---
layout: post
title: "01x08 | copilot_reinstall.bat"
date: 2025-07-01 16:45:00 +1000
categories: episodes
image:
    path: assets/img/01x08.jpg
    width: 1400
    height: 1400
cover: 'assets/images/cover/cover-01x08.jpg'
navigation: True
subclass: 'post'
logo:
---
In this episode Koos returns to a somewhat controversial topic - **Security Copilot**, but this time without the hype and of course with some practical tips. Chris introduces **Microsoft Entra Suite** in the first of two episodes taking a look at these capabilities.

<iframe src="https://player.rss.com/df3ndr/2096210?theme=dark&v=2" width="100%" height="202px" title="01x08 | copilot_reinstall.bat" frameBorder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen scrolling="no"><a href="https://rss.com/podcasts/df3ndr/2096210/">01x08 | copilot_reinstall.bat | RSS.com</a></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/EY-leVFZsOs?si=zBY-GzZWun_8sMl6" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Security Copilot

### Why did I say it's controversial?

* It seemed to burn through SCUs like candy. And these are expensive!
* I think there were some design flaws like lack of caching.

But there are some nice improvements!

* **Caching is here!** Previously, Copilot regenerated incident summaries every time—burning SCUs with each refresh. Also "incident report" stays in place as well as branched out investigations in dedicate Security Copilot portal.
* New "overage" SCUs make it more "pay-as-you-go".
* There are better visual insights into usage and costs.

### Cost structure

> Billing is calculated on hourly blocks based on provisioned capacity rather than by 60-minute increments and has a minimum of one hour. Any usage consumed within the same hour is billed as a full SCU for provisioned capacity, regardless of start or end times within that hour. For overage units, SCUs are billed up to one decimal increments for the exact consumed units. Consumed units are not rounded up to whole numbers. This means that you're charged precisely based on your usage (to one decimal place).
> For instance, if you provision an SCU at 9:05 a.m., then deprovision it at 9:35 am, and then provision another SCU at 9:45 am, you'll be charged for two units within the 9:00 a.m. to 10:00 a.m. hour. To maximize usage, make SCU provisioning changes at the beginning of the hour. For more information, see Manage usage.

<img src="/assets/images/01x08/01x08_usage-monitor-filter.png" alt="summary_rules" width="100%">

### Practical tips

* You don’t have to keep it running 24/7.  
* Activate **Security Copilot temporarily** in your tenant and benefit from it across:
  * Microsoft Defender XDR
  * Microsoft Intune
  * Entra ID
* Once you’ve gotten what you need—offboard it again. No shame. Smart use of your SCU.
* Use my PowerShell script for easy on-/off-boarding. (since there are ZERO docs out there :-( ))

### Real-world scenarios

* **Defender XDR**

  Use Security Copilot directly inside an incident to:

  * *“Show me the exact command line that was executed by this user in this incident.”*
  Copilot pulls relevant `ProcessCreationEvents`, extracts the command line, and summarizes the behavior—without clicking through multiple pivots.

  * *“Summarize which lateral movement techniques were observed in this incident.”*
  Copilot parses incident evidence (like remote desktop usage, token theft, or SMB connections) and provides MITRE-aligned context.

* **Entra ID**

  Use natural queries to investigate identity-based activity:

  * *“Show me the 10 most recent sign-ins for this user and summarize the MFA methods used.”*
  Copilot fetches sign-in logs, filters and aggregates sign-in frequency, location anomalies, and which MFA methods were used.

  * *“Summarize all Conditional Access policies that apply to this user and describe their impact.”*
  No more digging through the CA policy blade—Copilot gives you a readable breakdown of each rule, its scope, and enforcement logic.

* **Microsoft Intune**

  Security Copilot can help surface endpoint insights fast:

  * *“Which devices are non-compliant due to missing disk encryption?”*
  Copilot summarizes compliance policies and instantly lists affected devices.

  * *“Summarize app risk posture across all Android BYOD devices.”*
  Copilot reviews app inventory and flags outdated, side-loaded, or unmanaged apps by device type.

### Final Thoughts

Security Copilot has come a long way. If you were burned by it early on—or avoided it due to cost—it might be time to **revisit and re-evaluate**.
But still be careful! SCU's are still burning down quickly and with an hourly rate of $4 ($6 for overage) this can quickly add up if you keep them running 24/7.

## Microsoft Entra Suite

Microsoft Entra Suite is a comprehensive identity and network access solution designed to support a Zero Trust security model. It integrates multiple security and access management tools to ensure secure and seamless authentication across applications and networks. Key Components of Microsoft Entra Suite are:

* Microsoft Entra Private Access – Provides secure access to private applications and corporate networks without requiring a VPN.
* Microsoft Entra Internet Access – Protects access to internet resources, including SaaS applications and Microsoft 365.
* Microsoft Entra ID Governance – Automates identity lifecycle management and access permissions.
* Microsoft Entra ID Protection – Detects and mitigates identity-based security risks.
* Microsoft Entra Verified ID Premium – Enables real-time identity verification while maintaining privacy

### More licenses?

Licensing Options for Microsoft Entra:

* Microsoft Entra ID P1 – Required as a base subscription for Entra Suite. Available as a standalone product or included with Microsoft 365 E3 and Business Premium.
* Microsoft Entra ID P2 – Includes advanced security features and is available as a standalone product or bundled with Microsoft 365 E5.
* Microsoft Entra Suite – Combines multiple Entra products, including Private Access, Internet Access, ID Governance, ID Protection, and Verified ID Premium. It requires an Entra ID P1 subscription and is priced at $12 (USD) per user/month

<img src="/assets/images/01x08/01x08_entraSuite.png" alt="summary_rules" width="100%">

### Real-world use cases

* Microsoft Entra Private Access
  * VPN Replacement – Securely connect users to private applications without exposing full network access.
  * Just-in-Time Access – Grant temporary access to sensitive resources only when needed.
  * Secure Remote Work – Enable employees to access corporate apps securely from any device or location.
  * Privileged Access Protection – Enforce additional security controls for administrators accessing critical systems2.
* Microsoft Entra Internet Access
  * Web Content Filtering – Block access to malicious or inappropriate websites.
  * Secure SaaS Access – Protect access to cloud applications like Microsoft 365.
  * Conditional Access Enforcement – Apply security policies based on user identity and device compliance.
  * Threat Protection – Prevent phishing and malware attacks targeting internet traffic4.
* Microsoft Entra ID Governance
  * Identity Lifecycle Management – Automate onboarding, role transitions, and offboarding.
  * Access Reviews – Periodically review user access to ensure compliance.
  * Guest Access Management – Control access for external users like partners and vendors.
  * Privileged Identity Management – Secure high-risk accounts with additional verification6.
* Microsoft Entra ID Protection
  * Risk-Based Conditional Access – Automatically block or challenge risky sign-ins.
  * Threat Detection – Identify compromised accounts using AI-driven risk analysis.
  * Automated Remediation – Require multi-factor authentication or password resets for suspicious activity.
  * Security Information and Event Management (SIEM) Integration – Export risk data for deeper analysis8.
* Microsoft Entra Verified ID Premium
  * Identity Verification – Confirm user identities using verifiable credentials.
  * Self-Service Enrollment – Allow users to verify their identity without manual intervention.
  * Know Your Customer (KYC) Compliance – Streamline identity verification for financial transactions.
  * Face Check Authentication – Use facial matching for high-assurance identity verification.

#### Links

* [Microsoft Entra Suite features and pricing model](https://www.microsoft.com/en-us/security/business/microsoft-entra-pricing)
* [The Zero Trust Zone - Episode 1: Decentralized Identities](https://www.youtube.com/watch?v=qSqC2HdkN8k)

## 🛠️ Community Project

### PIM Role Advisor by Morten Knudsen

Morten shows us that AI doesn't need to be expensive. And in a fun way by integrating Azure AI Foundry with your PowerShell session.
Then with help of his script, PowerShell is advise you which Entra/Azure role you need based on an explanation you provide of the work you want to get done formulated in natural language.

With 77 requests in his demo he burned through 2 million tokens but payed only $ 0.23

[Check it out here and try it yourself](https://mortenknudsen.net/?p=5098) There's also [a video](https://www.youtube.com/watch?v=QbUJInHLBDc) of his script in action.

<img src="/assets/images/01x08/01x08_PIMRoleAdvisor.png" alt="summary_rules" width="100%">

If you're not already following Morton, please do. He's probably the most hard-working community member I've ever met. He's cranking out different sessions at events, organizing Experts Live Denmark and still able to write blogs like these while also running a company.
