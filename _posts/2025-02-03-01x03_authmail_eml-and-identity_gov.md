---
layout: post
title: "01x03_authmail.eml-and-identity.gov"
date: 2025-02-03 10:00:00 +1100
categories: episodes
image:
    path: assets/img/01x03.jpg
    width: 1400
    height: 1400
cover: 'assets/images/cover/cover-01x03.jpg'
navigation: True
subclass: 'post'
logo:
---
In this episode...

* Chris takes a look at email security and digs into SPF, DMARC and all the other acronyms.
* Koos recently had some experience with Entra ID Entitlement Management he wants to share. What are Access Packages? And why should you look into it?

<iframe src="https://player.rss.com/df3ndr/1877948?theme=dark" style="width: 100%; height: 150px;" title="01x03_authmail.eml-and-identity.gov" frameBorder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"><a href="https://rss.com/podcasts/df3ndr/1877948/">01x03_authmail.eml-and-identity.gov | RSS.com</a></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/6DkHfcB-898?si=NPuVgnH2Ez933jpG" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## E-mail Security

When I working with customers doing security assessments  - one of the first things I look at is how they have configured email authentication. I pretty much never find these to be optimized and thought it would be a good idea to break them down for our listeners. Email authentication is a group of standards to identify and prevent spoofing, these standards include:

* Sender Policy Framework (SPF): Specifies the source email servers that are authorized to send mail for the domain.
* DomainKeys Identified Mail (DKIM): Uses a domain to digitally sign important elements of the message to ensure the message hasn't been altered in transit.
* Domain-based Message Authentication, Reporting and Conformance (DMARC): Specifies the action for messages that fail SPF or DKIM checks for senders in the domain, and specifies where to send the DMARC results (reporting).

Without too much of a deep dive, it's important to understand that email authentication protocols validate your outbound email - the theory is that if everyone validates the email they send, it becomes easier to identify email sent by bad actors.

### SPF

SPF is used to validate the 'RFC5321.MailFrom' sender to ensure that the email is being sent by someone authorized for that domain. This validation is done in the form of a SPF record - a TXT record in DNS - you've probably seen these, they start with a string: "v=spf1"

When you add a new domain to M365, it will automatically generate a 'base' SPF record for you - it's important to  understand that it is the minimum required and does not take into account any other services you may use (payroll, etc) or on-prem email relays you may have so the record should be optimized for your exact needs.

Some tips for optimizing your SPF record:

* Use a hardfail '-all'
* Align SPF and DMARC (more on DMARC shortly)
* Monitor your SPF record and clean it up regularly
* Limit your records to 10 DNS Lookups - more on SPF clean-up and flattening in future episode

For more info about SPF, [check out the official documentation](http://www.open-spf.org/Introduction/)

### DKIM

When using DKIM, the receiving server makes a DNS request using the sender’s domain name (RFC5322.From) and obtains the public key from a DNS record in the DNS zone of the sending domain and compares it to the private key in the message from the sending server.

DKIM is easy enough to configure, but it is important to know that it is not configured automatically in M365 for custom domains - you need  to create your CNAME records and enabled it yourself. The records are in the format:

* selector1._domainkey.domain.com
* selector2._domainkey.domain.com

Some tips for implementing DKIM:

* Pay attention to your mail routing and where your email is being sent from, i.e, third party providers like Mimecast etc or third party services like Salesforce.
* Plan to rotate your DKIM keys every 3-6 months.

### DMARC

DMARC essentially ties SPF and DKIM together where the sender specifies what to do with email on behalf of the domain if it does not meet the requirements of SPF and DKIM.

DMARC is implemented as another TXT record starting with the string: "v=DMARC1" - Once you have implemented a record, receiving servers can verify the incoming email based on the DMARC policy. If the email fails the check, the email can be delivered, quarantined, or rejected - based on the instructions in the DMARC record. DMARC will pass if the RFC5321.MailFrom and RFC5322.From are equal, and/or SPF and DKIM are aligned.

Some tips for implementing DMARC:

* You should always use p=reject - only time to use anything else is when first implementing a DMARC policy
* In M365, setup a DMARC policy for you Microsoft Online Email Routing Address (MOERA) , aka 'onmicrosoft.com' domain
* Monitor and update your policy regularly

It's also really important to monitor your DMARC reports - they are no good just sitting in a shared mailbox. These reports help you gain visibility into  your email traffic and are useful to:

* Detect and prevent domain spoofing
* Ensure legitimate emails are getting delivered
* Protect your brand/reputation

There are many DMARC reporting services available - some are free, the good ones cost money and you can even roll your own. Either way, I'd encourage everyone to have something in place.

Check out [learndmarc.com](https://learndmarc.com) to help you validate your configuration.

Official documentation [can be found here](https://dmarc.org)

In our next episode, I'll dive a little deeper into SPF clean-up and flattening and spend some time looking at some newer email security protocols:

* Authenticated Received Chain (ARC)
* MTA-STS (Mail Transfer Agent Strict Transport Security)

## Entra ID Entitlement Management

Microsoft: *"Manage access (and lifecycle) for your users at scale, by automating access request workflows, access assignments, reviews, and expirations."*

Help with scenario's for people insider your org:

* People in your organizations need access to various groups and applications to perform their jobs. Users might not know what access they should have, and even if they do, they could have difficulty locating the right individuals to approve their access.
* Once users find and receive access to a resource, they could hold on to access longer than is required for business purposes. Also when they move into different roles in the future, you might want to strip them of previous permissions.

But also outside your org:

* These scenarios gets more complicated when you collaborate with people outside your org. You might not know who in the other organization needs access to your resources, and they won't know what applications and groups your're using.
* And you also need to invite these users as guests inside your directory, and clean them up once they no longer touch your resources.

Entitlement Management will make all this much easier by creating Access Packages 📦.

### Access Packages

Grant access to

* Entra Security Groups
* Microsoft 365 Groups and Teams
* Entra Enterprise Applications (SaaS applications and custom-integrated applications with federation/SSO)
* Sharepoint Online sites

Users can visit myaccess.microsoft.com and select an Access Package that's available to them.

Lot of different approval steps inside AP policy.

* Different approvals steps for people inside and outside your org
* Scope to specific external org(s)
* Enable Access Reviews to make sure people are actually using their permissions
* What's great is that external users are automatically invited AND disabled/cleaned upon unassignment of an Access Package.

### Licensing

* Entitlement Management is part of Entra ID Governance
* When you think you have all Entra ID features because you have Entra ID Premium P2 licenses, you're wrong. ;-)
* Luckily Access Packages are still part of P2, but some specific features might now. For example PIM-enabled Group with Access Reviews.

#### How we use this as an MSSP

* Security Analists
* External sponsors for approvals
* Conditional Access for highest MFA strength
* Trust MFA Claim

### Entitlement Management links

[What is entitlement management?](https://learn.microsoft.com/en-us/entra/id-governance/entitlement-management-overview)

[More on Entra ID Governance features](https://learn.microsoft.com/en-us/entra/id-governance/identity-governance-overview)

[Detailed Governance feature per license](https://learn.microsoft.com/en-us/entra/id-governance/licensing-fundamentals#features-by-license)

## Community Project

### Yellowhat

We already have Blackhat and Bluehat, but now there's Yellowhat! 👷🏻‍♂️

A couple of Security MVPs came together to organize a 100% Microsoft Security conference on March 6th 2025.
Only deep-dive sessions (Level 400+) led by world-class experts, including Raviv Tamir (Microsoft ILDC), Roberto Rodriguez (Microsoft Redmond), Dirk-jan Mollema, and more announcements soon.
All sessions will be broadcast live between 3pm and 9pm CET.

But there are also a few last VIP tickets for in-person visit. Hosted at Microsoft HQ @ Amsterdam and made possible with some great sponsors!

Register your (livestream) ticket now at [yellowhat.live](https://yellowhat.live)
