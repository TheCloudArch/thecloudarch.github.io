---
layout: post
title: "01x04_RE:authmail.eml-and-siem.kql"
date: 2025-03-01 01:00:00 +0100
categories: episodes
image:
    path: assets/img/01x04.jpg
    width: 1400
    height: 1400
cover: 'assets/images/cover/cover-01x04.jpg'
navigation: True
subclass: 'post'
logo:
---
In this episode...

* Chris revisits his e-mail authentication and security from last time to dig a little deeper.
* Koos recently did some talks about SIEM migrations to Sentinel and keeping things as cost-efficient as possible. He also believes a company shouldn't focus solely on Microsoft Sentinel, and should consider looking into alternatives alongside it like Azure Data Explorer. And why are companies so focussed on collecting all those logs in a "legacy" matter?

<iframe src="https://player.rss.com/df3ndr/1916672?theme=dark" style="width: 100%; height: 150px;" title="01x04_RE:authmail.eml-and-siem.kql" frameBorder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"><a href="https://rss.com/podcasts/df3ndr/1916672/">01x04_RE:authmail.eml-and-siem.kql | RSS.com</a></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/rEqI7uyV91Y?si=pfyt-UBDuNdOWxrx" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## E-mail Security - Part II

Sequels aren't always better, but this is an exception. ;)

In this episode, I'll dive a little deeper into SPF clean-up and flattening and spend some time looking at some newer email security protocols:

* Authenticated Received Chain (ARC)
* MTA-STS (Mail Transfer Agent Strict Transport Security)

To follow on from our previous discussion on SPF, In an SPF record, there isn't a strict limit on the number of IPv4/6 blocks (ip4/6: mechanisms) you can include. However, there are practical constraints:

* DNS Lookup Limit – SPF allows a maximum of 10 DNS lookups (e.g., include, a, mx, ptr, exists) per evaluation. ip4 and ip6 do not count towards this limit because they are directly included in the record.
* DNS TXT Record Length – A single TXT record (which SPF uses) is limited to 255 characters per segment, but multiple segments can be concatenated. The practical SPF record size limit is about 450–512 characters to avoid truncation issues. If the SPF record exceeds 512 bytes, some email servers may reject it.

If you have too many DNS lookups, a "SPF PermError: too many DNS lookups" is returned during an SPF check, DMARC treats that as fail since it's a permanent error. There is one  solution to this problem that is recommended all over the interwebs called "flattening" an SPF record. Using this method, each of the DNS-querying mechanisms/modifiers is queried for the IP addresses and these then replace the original mechanism/modifier thus reducing the number of DNS lookups. This is great, right?

Danger, Will Robinson!

I don't personally recommend using SPF flattening unless absolutely necessary, with regular audit and cleanup you may not need it. It is also important to consider that:

* IP addresses do change and can break email delivery
* Flattening requires more administrative overhead to managed IP changes.

There are 'dynamic SPF' services available that market themselves as a solution to this - I haven't got any personal experience with these so YMMV. Please reach out if you have any good or bad experience you'd like to share.

### Authenticated Received Chain (ARC)

ARC is an email security mechanism that preserves authentication results (SPF, DKIM, and DMARC) when an email passes through forwarders, mailing lists, or intermediaries.

How ARC Works:

* The original sender sends an email → SPF, DKIM, and DMARC checks are performed.
* A forwarder (e.g., a mailing list, forwarding service) receives the email.
* The forwarder signs the email with ARC headers before sending it to the final recipient.
  * These headers include:
    * ARC-Authentication-Results → Records SPF/DKIM/DMARC results before forwarding.
    * ARC-Seal → Cryptographically signs the ARC chain to prevent tampering.
    * ARC-Message-Signature → Ensures integrity of the forwarded message.
* The recipient verifies the ARC chain to decide if it should trust the forwarded email.

Microsoft 365 already supports ARC, but if you run into issues with email delivery from a particular service, you can add their particular domain to the ARC trusted sealers list in the Microsoft 365 Defender portal

Microsoft Docs [Configure trusted ARC sealers](https://learn.microsoft.com/en-us/defender-office-365/email-authentication-arc-configure)

### MTA-STS (Mail Transfer Agent Strict Transport Security)

One of problems with SMTP is that encryption is entirely optional. When support for upgrading from plaintext to encryption in the form of the STARTTLS command was added to SMTP the specification explicitly specified that SMTP servers must accept plaintext connections. MTA-STS is a new standard that aims to improve the security of SMTP by enabling domain names to opt into strict transport layer security mode that requires authentication and TLS. MTA-STS is supported in Exchange Online.

MTA-STS requires two things to implement:

* A policy .txt file hosted at: [https://mta-sts.domain.com/.well-known/mta-sts.txt](https://mta-sts.domain.com/.well-known/mta-sts.txt)
* A DNS TXT record _mta-sts.example.com

Dutch MVP Michel de Rooij [has a great post on how to host you MTA-STS record using Github pages](https://eightwone.com/2023/10/05/hosting-mta-sts-policy-using-github-pages/)

Side note: You may have heard of DANE (DNS-Based Authentication of Named Entities) - DANE requires DNSSEC, which many domains do not implement and therefore kills adoption. MTA-STS is easier to deploy and works with standard DNS.

Check out the Microsoft Docs [Enhancing mail flow with MTA-STS](https://learn.microsoft.com/en-us/purview/enhancing-mail-flow-with-mta-sts)

## Microsoft Sentinel as a SIEM and how to re-think logging strategies

Microsoft Sentinel celebrated it's fifth anniversary already last September. But a lot has changed since:

* Sentinel's role as an orchestrator between all the different security products ("all your Defenders are belong to us")
* New log tiers, each with it's own pros and cons
  * Auxiliary
* Sentinel is now also part of the "Unified Security Operation Platform", but it's still an Azure resource?

During SIEM migrations the topic of Sentinel cost is a big topic.
Cloud-native SIEM works a bit different compared to a (dare I say) "legacy" SIEM solution.
"Sentinel is expensive" people might say, but you might be using it wrong.

* Consider a multi-tiered log strategy:
  * Real-time analytics
  * Triage & Hunting
  * Compliance & Forensics
* Consider NOT ingesting those logs any longer ;)
  * Please hear me out....
  * Be critical during SIEM migrations
    * Decide per log source what to move or what to drop

### Azure Data Explorer

* Very cost effective companion in my opinion to use alongside Sentinel.
* Very scalable and offers UNLIMITED data retention
* Query logs with KQL

## Community Project

### Yellowhat

We already have Blackhat and Bluehat, but now there's Yellowhat! 👷🏻‍♂️

A couple of Security MVPs came together to organize a 100% Microsoft Security conference on March 6th 2025.
Only deep-dive sessions (Level 400+) led by world-class experts, including Raviv Tamir (Microsoft ILDC), Roberto Rodriguez (Microsoft Redmond), Dirk-jan Mollema, and more announcements soon.
All sessions will be broadcast live between 3pm and 9pm CET.

But there are also a few last VIP tickets for in-person visit. Hosted at Microsoft HQ @ Amsterdam and made possible with some great sponsors!

Register your (livestream) ticket now at [yellowhat.live](https://yellowhat.live)

### Bluesky.ms

Another great project by Merill Fernando. Bluesky.ms is the authoritative source for Microsoft community-related activity on Bluesky. Its a crowdsourced database of anyone and everyone in the Microsoft community on Bluesky where you can connect with the Microsoft community and get your account verified. Here you can users categorized as:

* Microsoft Community Users
* Microsoft FTEs
* MVPs
* RDs

Check it out and join the community at [bluesky.ms](https://bluesky.ms)
