---
layout: post
title: "01x05_DEVICEHIGH=MVPSUMMIT.SYS"
date: 2025-04-03 00:00:00 +0200
categories: episodes
image:
    path: assets/img/01x05.jpg
    width: 1400
    height: 1400
cover: 'assets/images/cover/cover-01x05.jpg'
navigation: True
subclass: 'post'
logo:
---
Booting high on community energy! In this special in-person episode, recorded on-site at the Microsoft Campus during the MVP Summit 2025, Koos and Chris share behind-the-scenes insights, tech trends, and reflections on the evolving security landscape—all while dodging T-shirt printers and Summit buzz.

<iframe src="https://player.rss.com/df3ndr/1970841?theme=dark" style="width: 100%; height: 150px;" title="01x05_DEVICEHIGH=MVPSUMMIT.SYS" frameBorder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"><a href="https://rss.com/podcasts/df3ndr/1970841/">01x05_DEVICEHIGH=MVPSUMMIT.SYS | RSS.com</a></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/GI7LRReHmGU?si=qMsl5SfwmyKLnHtW" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## 🏙️ MVP Summit Experience

- First time recording in person!
- Koos and Chris reflect on how special it is to collaborate live, normally split by time zones.
- Value of meeting with product teams and MVP peers from around the world.

## 🧩 Capture the Flag Challenge

- Microsoft’s hands-on CTF challenge provided deep XDR visibility.
- Realistic red-team simulation in Defender showed how challenging threat hunting can be.
- Koos & Chris share a new appreciation for SOC analysts sifting through complex telemetry.

## 🤖 Security Copilot + Agents

- Microsoft announced **Security Copilot Agents** at Summit.
- These "agentic AI" bots can handle tasks like phishing triage and vulnerability remediation.
- Koos went from skeptic to fan—Agents reduce the need for custom logic app workflows.
- Built-in dashboards now show **time saved per task**, helping justify ROI.

## 🕵️‍♂️ Threat Hunting with the GHOST Team

- Microsoft’s **GHOST Team**: *Global Hunting Oversight and Strategic Triage*.
- Proactive hunting using advanced logs and behavioral anomalies.
- Emphasis on **graph logs** and **assumed breach** strategies.
- Outputs include improved detection rules and real-world attacker insights.

## 🔐 Identity & MFA – Still the #1 Target

- Identity remains the primary attack vector.
- **Stop excluding MFA for office IPs or “trusted” users**.
- Embrace **phishing-resistant MFA** like **passkeys**.
- Avoid risky group-based MFA exclusions—opt for dedicated groups or per-user control.

## ⚙️ Conditional Access & workload Identities

- Time to **revisit and enrich** older CA policies.
- Add **device compliance** and **user risk signals** (especially with Entra P2).
- Use tools like **risk-based CA** and **sign-in risk** to block compromised accounts.
- Apps and service principals are still a weak link in many orgs.
- Add CA rules to Applications (Workload Identities) to further heighten security (e.g. IP filtering).
- Because **App secrets** often go unmanaged and unrotated.

## 🧭 Final Thoughts

- Many attacks still succeed due to weak fundamentals: open ports, unpatched systems, overly-permissive apps.
- Mastering the basics remains critical.
- In-person energy made this episode extra special! 🙏🏻

## 🛠️ Community Project

### Device Offboarding Manager

Microsoft MVP [Ugur Koc](https://www.linkedin.com/in/ugurkocde/) has created a great PowerShell GUI tool for efficiently managing and offboarding devices from Microsoft Intune, Autopilot, and Entra ID, featuring bulk operations and real-time analytics for streamlined device lifecycle management. At it's core, the tool features:

* Multi-Service Integration: Manage devices across Intune, Autopilot, and Entra ID
* Bulk Operations: Support for bulk device imports and operations
* Real-time Dashboard: View device statistics and distribution
* Secure Authentication: Multiple authentication methods including interactive, certificate, and client secret

I really like the playbooks feature that allows automation and support for specific custom scenarios.

Check it out on [Github](https://github.com/ugurkocde/DeviceOffboardingManager)
