---
layout: post
title: "01x09 | open_port_regrets.pcap"
date: 2025-08-01 18:00:00 +1000
categories: episodes
image:
    path: assets/img/01x09.jpg
    width: 1400
    height: 1400
cover: 'assets/images/cover/cover-01x09.jpg'
navigation: True
subclass: 'post'
logo:
---
Chris revisits Microsoft Entra Suite and takes a deep dive into **GSA - Global Secure Access**. Koos did a project recently where **Defender for External Attack Surface Management (EASM)** was also implemented. And he likes to share how awesome this product is, as well as share some practical tips and pitfalls you need to wary of.

<iframe src="https://player.rss.com/df3ndr/2140318?theme=dark&v=2" width="100%" height="202px" title="01x09 | open_port_regrets.pcap" frameBorder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen scrolling="no"><a href="https://rss.com/podcasts/df3ndr/2140318/">01x09 | open_port_regrets.pcap | RSS.com</a></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/7eCv4CydkqU?si=Mn3N-h8dVS825lSb" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Global Secure Access

Microsoft Global Secure Access (GSA) is a modern network access solution built on zero-trust principles, delivering secure and identity-aware connectivity to both internet-based and private applications:

* It replaces traditional VPNs with Microsoft Entra Private Access and
* Enhances protection for SaaS apps and Microsoft 365 services via Entra Internet Access.

### GSA Requirements

* Microsoft Entra ID P1 + Private/Internet Access or Microsoft Entra Suite
* Global Secure Access Client installed on supported platforms - Windows, macOS, iOS, Android - No Linux support
* Devices must be Microsoft Entra joined, hybrid joined, or registered for Conditional Access enforcement - limits BYOD scenarios
* Entra Private Access uses Private Network Connector which is Windows only
* Traffic forwarding profiles must be enabled in the Entra admin center:
  * Microsoft traffic
  * Private Access
  * Internet Access

<img src="/assets/images/s01e09/01x09-GSA-Profiles.png" alt="GSA Profiles" width="100%">

The Microsoft Traffic Profile is a specialized traffic forwarding configuration within GSA that focuses on securing and optimizing access to Microsoft 365 services like Exchange Online, SharePoint, OneDrive, Teams, and Office Online. It is included with Microsoft Entra ID P1 or P2, which is part of Microsoft 365 Business Premium and E3/E5 plans—no extra license needed beyond that

* Applies Conditional Access policies to ensure only trusted users and devices can access these services.
* When combined with Universal Tenant Restrictions (UTR) policies, it becomes a powerful tool to limit Microsoft 365 connectivity to only a specific tenant.

### Entra Private Access

Microsoft Entra Private Access is a modern, identity-centric alternative to traditional VPNs, built on Zero Trust Network Access (ZTNA) principles. It enables secure, conditional access to private apps and resources. It’s designed to replace legacy VPNs, reduce lateral movement risk, and simplify secure access for remote and hybrid users.

* Access is granted based on verified identity, device posture, and Conditional Access policies—not network location.
* Supports all TCP/UDP protocols (e.g. RDP, SMB, SSH), not just web apps.
* Configure broad IP/FQDN ranges or fine-grained app-level access with separate policies.
* Global Secure Access client is installed on endpoints to route traffic securely through Microsoft’s SSE infrastructure.
* Works with legacy and modern apps alike, enforcing MFA, SSO, and segmentation without modifying the app itself.
* Uses lightweight agents (Private Network Connectors) deployed near private resources to broker secure access.

#### Entra App Proxy vs. Private Access

| **Category**             | **Entra App Proxy**                                       | **Entra Private Access**                                            |
|--------------------------|-----------------------------------------------------------|---------------------------------------------------------------------|
| **Primary Purpose**      | Securely publish internal **web apps** to external users  | Provide **Zero Trust access** to **any private resource**           |
| **Protocol Support**     | **HTTP/HTTPS only**                                       | **All TCP/UDP protocols** (e.g. RDP, SMB, SSH, SQL)                 |
| **Ideal Scenarios**      | Legacy web apps, B2B partner access, browser-based usage  | VPN replacement, secure access to hybrid/multicloud private apps    |
| **Authentication Method**| Entra ID via browser SSO (SAML, KCD, headers)             | Entra ID authentication via **Global Secure Access client**         |

**Currently in Preview - Microsoft Entra Private Access for domain controllers** - It allows you to enforce Conditional Access, including MFA, for apps and services that authenticate via Kerberos, without exposing your domain controllers to broad network access.

* Private Access Sensor: Installed on domain controllers to intercept and evaluate Kerberos requests
* Private Network Connector: Routes traffic from GSA clients to internal resources
* SPN-based policy enforcement: You define which services (e.g. cifs/*, host/*) require enforcement
* Audit and Enforce modes: Start in report-only mode, then shift to enforcement once validated

### Entra Internet Access

Microsoft Entra Internet Access is an identity-centric secure web gateway that protects users, devices, and data as they access the public internet and SaaS applications. It’s part of Microsoft’s Security Service Edge (SSE) solution and deeply integrates with Microsoft Entra ID to enforce Conditional Access policies across all internet destinations. It’s ideal for organizations looking to unify identity and network security, reduce reliance on legacy proxies, and enforce Zero Trust principles across all internet-bound traffic.

* Web content filtering by category or FQDN
* Universal Conditional Access enforcement
* Source IP restoration for accurate logging
* Compliant Network checks to block token replay attacks

## 🛡️ Defender for External Attack Surface Management (EASM)

### What is Defender for EASM?

Defender for External Attack Surface Management (EASM) is a tool designed to help organizations discover, monitor, and secure their internet-facing assets—even the ones they didn’t know existed. Think of it as an automated reconnaissance engine that simulates what an attacker might see when scanning your external footprint. From DNS records and IP ranges to exposed services, forgotten domains, and shadow IT—EASM aims to surface it all.

### Why should organizations care?

You can’t protect what you don’t know you own. As companies grow, acquire others, move to the cloud, and spin up new environments, their external attack surface becomes harder to track. EASM helps regain visibility and control, identifying unknown, unmanaged, or misconfigured assets before attackers do. It's like shining a flashlight into all the corners of your digital presence.

### What makes it powerful?

EASM doesn't just dump raw data—it enriches findings with risk context, prioritizes issues, and ties into your existing Defender ecosystem. Whether you're trying to reduce attack surface, audit your digital estate, or comply with regulatory requirements, EASM brings structure to chaos. It's especially useful for security teams dealing with legacy sprawl, mergers & acquisitions, or hybrid cloud environments.

### Lessons learned

So, I helped a large online retailer recently to setup their EASM instance and configure their Discovery Groups (Seeds).
Here you'll provide their domain names, IP address ranges, ASNs and contact information. All this information is used to discover (crawl) across your public-facing estate and check for potential security risks.

I was interesting to see that once a primary domain got added, the WHOIS information was retrieved and additional domains registered by the same e-mail address were discovered as well!
Here also lies the first thing you need to check regularly. Microsoft might think all kinds of domains, hosts and IP blocks are associated with your organization while they're not. Since you'll be charged per asset in inventory per day, this is something to keep an eye on.

After assets are being discovered you'll see a detailed overview of services running behind that host/ip, which certificates are being used and this is helpful to assess vulnerabilities. These range from Low to Medium and High

While keeping an eye on assets you might need to exclude certain hosts, domains etc later to make sure they aren't automatically discovered any longer in the future.

It seemed kind of weird at first to me that you're able to add all sorts of domains which aren't yours. But then I figured, that these are public-facing entities. the whole world is able to connect to them and check for potential vulnerabilities. As long as you're willing to pay for these assets in your inventory, you're free to add whatever you want.

There might also be assets discovered by EASM where Microsoft wasn't 100% confident that these are yours. These assets will have a state of `Requires Investigation` and this is also something you should regularly check. Either remove them from your inventory (don't forget to exclude them as well, otherwise they'll probably come back) of mark them as `Approved`.

Although you initially create a Defender for EASM instance in Azure, you can also incorporate it into Defender XDR by:

* Visit [`security.microsoft.com`](https://security.microsoft.com)
* Go to `Exposure Management` --> `Exposure insights` --> `Initiatives`
* Click one the External Attack Surface banner on top and select your MDEASM instance.

Also make sure to enable Log Analytics integration for useful integration with Microsoft Sentinel! You can find these inside the Defender for EASM instance in the Azure Portal and selecting `Manage` --> `Data Integrations`

### Sentinel Detections

I also like to share a couple of detections we've been using:

#### Defender for EASM discovered asset(s) with a HIGH priority observation

Defender for External Attack Surface Management (EASM) continuously monitors and discovers new assets related to your organization s external attack surface, based on the provided "seeds".
This alert indicates that Defender for EASM has identified one or more newly discovered assets associated with high-criticality vulnerabilities or significant exposure. Please review these findings in Defender for EASM within the Azure Portal and assess their impact.

```kusto
EasmRisk_CL
| where CategoryName_s has "High"
| mv-expand Item = todynamic(AssetDiscoveryAuditTrail_s)
| extend
    AssetKey = tostring(Item.AssetName),
    AssetValue = tostring(Item.AssetType)
| summarize AssetsPivot = make_bag(pack(AssetKey, AssetValue))
    by
    TimeGenerated,
    Description = CategoryDescription_s,
    DisplayName = MetricDisplayName_s,
    AssetName = AssetName_s
| evaluate bag_unpack(AssetsPivot)
```

#### Defender for EASM discovered asset(s) with a MEDIUM priority observation

Defender for External Attack Surface Management (EASM) continuously monitors and discovers new assets related to your organization s external attack surface, based on the provided "seeds".
This alert indicates that Defender for EASM has identified one or more newly discovered assets associated with medium-criticality vulnerabilities or significant exposure. Please review these findings in Defender for EASM within the Azure Portal and assess their impact.

```kusto
EasmRisk_CL
| where CategoryName_s has "Medium"
| mv-expand Item = todynamic(AssetDiscoveryAuditTrail_s)
| extend
    AssetKey = tostring(Item.AssetName),
    AssetValue = tostring(Item.AssetType)
| summarize AssetsPivot = make_bag(pack(AssetKey, AssetValue))
    by
    TimeGenerated,
    Description = CategoryDescription_s,
    DisplayName = MetricDisplayName_s,
    AssetName = AssetName_s
| evaluate bag_unpack(AssetsPivot)
```

#### Defender for EASM total assets increased significantly

Defender for External Attack Surface Management (EASM) continuously monitors and discovers new assets related to your organization s external attack surface, based on the provided "seeds".
Assets are charged as part of Azure billing and to help keep costs somewhat under control, this detection will compare the number of assets this week with the previous week. If an unexpectedly large increase (10%) is observed, this could indicate incorrect assumptions in the discovery process but, more importantly, could result in an unexpectedly high invoice at the end of the month. This way, we can potentially take timely corrective action.

```kusto
// Define the Defender for EASM daily price per asset in Euros (West Europe region)
let AssetPriceDayEur = 0.010;
// Retrieve a 7-day historic baseline window (15-8 days ago)
let AssetCountHistoric = workspace('').EasmAsset_CL
| where TimeGenerated between (ago(15d) .. ago(8d))
| summarize CountHistoric = dcount(AssetName_s) by AssetType_s;
// Retrieve a 7-day recent window (last 7 days)
let AssetCountLastWeek = workspace('').EasmAsset_CL
| where TimeGenerated between (ago(7d) .. now())
| summarize CountLastWeek = dcount(AssetName_s) by AssetType_s
// Calculate the projected monthly cost for each asset type
| extend ProjectedMonthlyCostPerAssetTypeEur = round(CountLastWeek * AssetPriceDayEur * 30,2);
// Calculate the total projected monthly cost across all asset types
let TotalProjectedCost = AssetCountLastWeek
| summarize TotalProjectedMonthlyCostEur = sum(ProjectedMonthlyCostPerAssetTypeEur);
// Join historic and recent counts on AssetType
AssetCountHistoric
| join kind=fullouter (AssetCountLastWeek) on AssetType_s
// Replace null counts with 0
| extend CountHistoric = coalesce(CountHistoric, 0)
| extend CountLastWeek = coalesce(CountLastWeek, 0)
// Calculate percentage change between historic and recent counts
| extend AssetCountDeltaPercent = iff(
    CountHistoric == 0 and CountLastWeek > 0,
    100, // If no historic count and new count >0, consider as 100% increase
    iff(
        CountHistoric == 0 and CountLastWeek == 0,
        0, // No change if both are zero
        tolong((CountLastWeek - CountHistoric) * 100 / CountHistoric) // Standard % delta
    )
)
// Add the total projected monthly cost to each row
| extend TotalProjectedMonthlyCostEur = toscalar(TotalProjectedCost)
// Final output columns
| project
    AssetType = AssetType_s,
    CountHistoric,
    CountLastWeek,
    AssetCountDeltaPercent,
    ProjectedMonthlyCostPerAssetTypeEur,
    TotalProjectedMonthlyCostEur
// Sort by the absolute delta percentage
| order by abs(AssetCountDeltaPercent) desc
// Only include rows where there was historic data (EASM must be enabled >2 weeks)
| where isnotempty(CountHistoric) and CountHistoric != 0
// Only output if there's more than 10% growth in assets
| where AssetCountDeltaPercent > 10
```

### Feature requests

I'm currently in touch with the engineering team of MDEASM because I think two things could be improved on currently. I've requested additional features to improve:

1. The asset `state` is currently not logged to Log Analytics. Therefore I'm unable to alert on new assets discovered which `Requires investigation`. Assets with this state are currently nog even visible in Log Analytics at all! Hopefully we'll get more controls on this.
2. Log ingestion into Log Analytics seems to be updated only once per 24 hours. I want to be notified sooner if for example a new assets with a **HIGH** vulnerability is discovered by MDEASM. In theory this can add an additional day, and I think this should be shorter.

## 🛠️ Community Project

### IntuneManagement

Mikael Karlsson has created IntuneManagement - A PowerShell tool to Copy, export, import, delete, document and compare policies and profiles in Intune and Azure with a nice WPF UI. This tool makes it easy to backup or clone a complete Intune environment. The scripts can export and import objects including assignments and support import/export between tenants.

<img src="/assets/images/s01e09/IntuneManagement.PNG" alt="IntuneManagement" width="100%">

Check out IntuneManagement [on Github](https://github.com/Micke-K/IntuneManagement)
