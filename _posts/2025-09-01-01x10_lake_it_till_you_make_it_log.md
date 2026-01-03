---
layout: post
title: "01x10_lake_it_till_you_make_it.log"
date: 2025-09-01 10:00:00 +1000
categories: episodes
image:
    path: assets/img/01x10.jpg
    width: 1400
    height: 1400
cover: 'assets/images/cover/cover-01x10.jpg'
navigation: True
subclass: 'post'
logo:
---

In this episode Koos takes a look at the recent release of **Sentinel data lake** and, Chris shares 5 tips to help your **Entra Privileged Identity Management (PIM)** deployment.

<iframe src="https://player.rss.com/df3ndr/2194487?theme=dark&v=2" width="100%" height="202px" title="01x10_lake_it_till_you_make_it.log" frameBorder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen scrolling="no"><a href="https://rss.com/podcasts/df3ndr/2194487/">01x10_lake_it_till_you_make_it.log | RSS.com</a></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/QyumDYu18j4?si=mc7F3NN7kPWdR991" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Microsoft Sentinel data lake (yes, that's in lowercase Microsoft assured me)

Security departments have always struggled with the need for security data. How can they retain as much security data as possible? But with the pricing model — especially for Microsoft Sentinel — they always needed to be selective about what to ingest. That led many to explore (third-party) alternatives, which introduced their own challenges.

* _"We can't ingest that data because it's too expensive."_
* _"We can ingest that, but let's cut out these columns and hope we won't need them later."_
* _"We can ingest this data, but we can't retain it for very long due to cost."_

The result is often a patchwork of custom, bespoke solutions, with complex transformations through third-party platforms like Elastic and Azure Data Explorer. And all of this creates additional overhead.

While I’ve always been a big fan of running ADX alongside Sentinel (I talked about this back in episode 4), I’ve also acknowledged the extra complexity and overhead it introduces.

It’s always been frustrating that the technical capabilities for storing the data existed — but the data wasn’t there when you actually needed it.

With a data lake, the idea is to **ingest everything in raw format** and **apply transformations in place** rather than on ingest. And because it's fully integrated, you can query it from multiple angles — not only with KQL, but also using Power BI and Jupyter Notebooks.

Over time, customers tend to store data in different silos:

* Microsoft Sentinel  
* Azure Data Explorer  
* Azure Blob Storage  

But how do you join it all together?

[Mark Kendrick](https://www.linkedin.com/in/mark-kendrick-3731b11/) (Principal Product Manager @ Microsoft) described this beautifully on _The Azure Security Podcast_ — calling it "**Data Adjacency**." I think that’s a very fitting term.

The cool thing about **Sentinel data lake** is that it mirrors, by default, everything that comes into Sentinel. So, you can decide to ingest new logs exclusively into the data lake, and later choose to "promote" specific logs for analytics use. Hence today's episode title: **_lake it till you’ve decided to use (make) it later_** ;-)

### Data lake is essentially a combination of features

Sentinel **Auxiliary Logs** went GA on April 1st, 2025. This was the cheapest log storage option until now — but it was limited to **custom logs only**, and had some other limitations like lack of dynamic datatype support and a somewhat painful setup (DCR/DCE, API-based only).

Then we had **Basic logs**, which were arguably already superseded by Aux logs — except in a few scenarios where Aux wasn’t supported.

**Data lake** seems to sit one layer higher (or lower, depending on how you phrase it 😉), abstracting away much of that complexity — while supporting **many more tables**. When you configure a table to use the data lake tier, my guess is that it's still stored using Auxiliary under the hood — although this isn’t explicitly mentioned in the docs.

It’s a much more **refined and streamlined experience**, in my opinion.

<img src="/assets/images/01x10/01x10-datalake_meme.jpeg" alt="datalake_meme" width="100%">

> Although I appreciated the humor of this meme, I think it doesn’t do data lake enough justice. It's _much more_ than just Auxiliary logs with a new name.

### Jupyter Notebooks

A Jupyter Notebook contains an ordered list of input/output cells which can contain code, text (Markdown), mathematics, plots and other media.

Jupyter notebooks are an integral part of the Microsoft Sentinel data lake ecosystem, offering powerful tools for data analysis and visualization. The notebooks are provided by the Microsoft Sentinel Visual Studio Code extension (preview) that allows you to interact with the data lake using Python for Spark (PySpark). Notebooks enable you to perform complex data transformations, run machine learning models, and create visualizations directly within the notebook environment.

<img src="/assets/images/01x10/01x10-datalake_jupyter.jpg" alt="jupyternotebook" width="100%">

### Caveats

I get the feeling that whenever Microsoft ships a new feature, customers are happy for a few minutes… and then immediately want more. 😅

There are already a few things people are wishing for with data lake — like extending **XDR data** (e.g., MDE tables) into the data lake **natively**. That’s not possible yet.  
These “XDR-tiered” tables still have a **30-day retention limit**. You could already extend this via Sentinel, but that required ingesting logs into Sentinel first — and since these tables generate **huge volumes**, this was never a very cost-effective strategy.

I’ve seen community blog posts showing ways to work around this — like manually storing MDE data in custom auxiliary tables and then streaming that into the lake — but in my opinion, that defeats the whole idea of a _streamlined experience_.

#### Pro tip

Although the UI _says_ it’s possible to extend data retention into the lake for tables like `DeviceNetworkEvents`, **don’t enable it this way!**  
This will **first ingest** those logs into Sentinel (at full price), and _then_ mirror them to the data lake — defeating the purpose of having a low-cost solution.

A bigger warning label on this would’ve been appreciated.

### Closing notes

Remember: it’s called **Microsoft Sentinel data lake**, not **Defender XDR data lake**. So this is all about extending **Sentinel** data only! Keep that in mind.

My best guess? Microsoft will continue to extend data lake capabilities in the future. And since it's still in _preview_, who knows what we’ll see when it hits GA…

### Links

[Microsoft Sentinel data lake pricing (preview)](https://techcommunity.microsoft.com/blog/microsoft-security-blog/microsoft-sentinel-data-lake-pricing-preview/4433919)

[Plan costs and understand Microsoft Sentinel pricing and billing](https://learn.microsoft.com/en-us/azure/sentinel/billing?tabs=simplified%2Ccommitment-tiers#data-lake-tier/?wt.mc_id=SEC-MVP-5005030)

[Planning your move to Microsoft Defender portal for all Microsoft Sentinel customers](https://techcommunity.microsoft.com/blog/microsoft-security-blog/planning-your-move-to-microsoft-defender-portal-for-all-microsoft-sentinel-custo/4428613)

[Jupyter notebooks and the Microsoft Sentinel data lake (preview)](https://learn.microsoft.com/en-us/azure/sentinel/datalake/notebooks-overview)

[Project Jupyter on Wikipedia](https://en.wikipedia.org/wiki/Project_Jupyter)

---

## Entra PIM

Microsoft Entra Privileged Identity Management (PIM) is a security and governance feature that helps you manage, control, and monitor access to high-impact roles across Microsoft Entra ID, Azure, and other Microsoft 365 services. PIM is designed to reduce the risks of standing administrative access by offering just-in-time and time-bound role activation. Here's what it enables:

* Just-in-time access: Users can activate privileged roles only when needed, reducing exposure.
* Approval workflows: You can require approval before a role is activated.
* MFA enforcement: Activation can require multi-factor authentication.
* Access reviews: Periodic checks to ensure users still need their roles.
* Audit logging: Full visibility into who activated what, when, and why.
* Notifications: Alerts when privileged roles are activated.

<img src="/assets/images/01x10/01x10-pim.png" alt="PIM" width="100%">

### More Licenses?

As always, licensing is.. well, complicated. PIM is a Microsoft Entra ID P2 feature, Entra ID P2 is available as a standalone product or included with Microsoft 365 E5 for enterprise customers.

PIM is also included with Microsoft Entra ID Governance which is available as an add-on or part of Microsoft Entra Suite

<img src="/assets/images/01x10/01x10-pim-license.png" alt="PIM Licenses" width="100%">

### Deployment Tips

You can manage the following with PIM:

* Microsoft Entra roles (e.g., Global Admin, Security Admin)
* Azure resource roles (e.g., Owner, Contributor)
* Group membership (via PIM for Groups)

#### Tip 1 - Start with an audit

Before deploying PIM, its a good idea to start with an audit and review of all your existing role assignments.

#### Tip 2 - Limit highly privileged roles to 4 hours or less

I typically recommend limiting these roles to a 4 hour activation window:

* Global Administrator
* Privileged Role Administrator
* Security Administrator
* Compliance Administrator
* Exchange Administrator
* SharePoint Administrator
* Teams Administrator
* User Administrator
* Authentication Administrator
* Application Administrator
* Cloud App Administrator
* Intune Administrator
* Billing Administrator
* Directory Writers

All other roles are typically ok with 8 hour activations - your environment may differ so consider your risk profile etc.

#### Tip 3 - It's ok to mix direct and group-based assignments, but plan it carefully

I prefer to always directly assign roles to admin users, however there are use-cases where it doesn't make sense - for example you may have help desk users that perform several different tasks and these don't map directly to a specific built-in role in Entra. Expecting users to always know which is the best least privilege fit for a specific task isn't always viable. In these cases, creating a group that has the various roles assigned makes sense and allows users to activate group membership instead and as a member of that group they will inherit the relevant roles.

<img src="/assets/images/01x10/01x10-pim-groups.png" alt="PIM Groups" width="100%">

#### Tip 4 - Always MFA!

You may hear differing opinions on this one, but personally I always recommended requiring MFA for any role activation no matter if its Global Admin or Global Reader.

#### Tip 5 - Approval workflows can be painful

Approval workflows are great in highly-regulated environments, but approvals also add a lot of administrative overhead so I always recommend careful consideration here. If you are going to use approvals, start with highly-privileged roles like Compliance Admin or Global Admin first and gradually deploy to other roles as needed. Requiring approval for all roles will be no fun for anyone!

---

## Community Project

### EasyPIM

Created by Loïc Michel, a support engineer in the Azure identity team at Microsoft.

EasyPIM is a PowerShell module created to help you manage Microsof Privileged Identity Management (PIM) either working with Entra ID, Azure or groups. Packed with more than 30 cmdlets, EasyPIM leverages the ARM and Graph APIs complexity to let you configure PIM Azure Resources, Entra Roles and groups settings and assignments in a simple way.

Features:

* Support editing multiple roles at once
* Copy settings from one role to another
* Copy eligible assignments from one user to another
* Export role settings
* Import role settings
* Backup all roles

Check out EasyPIM [on Github](https://github.com/kayasax/EasyPIM)

---

## Experts Live US

Experts Live is a global network that brings together Microsoft executives, MVPs, subject matter experts, and community members through regional and country events to share knowledge and expertise about Microsoft technologies.

Held on October 10th for the very first time in the United States at the Microsoft office at Times Square in New York City. The lineup of speakers looks to be amazing and  tickets are only $ 15,- !! Will we see you there??

Experts Live US is proud to support STEM Kids NYC, helping them bring technical classes, materials and support to kids in the New York City area. All proceeds from our attendee registration will be donated to STEM Kids NYC!

[Check out the Experts Live US website for more information](https://www.expertslive.us)
