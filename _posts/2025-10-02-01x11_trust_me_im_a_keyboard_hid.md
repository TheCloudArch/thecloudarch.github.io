---
layout: post
title: "01x11_trust_me_im_a_keyboard.hid"
date: 2025-10-02 08:00:00 +1000
categories: episodes
image:
    path: assets/img/01x11.jpg
    width: 1400
    height: 1400
cover: 'assets/images/cover/cover-01x11.jpg'
navigation: True
subclass: 'post'
logo:
---

In this episode Chris asks "To block or not to block?" as he looks at geo blocking in Conditional Access, while Koos explores the human element in cybersecurity.

<iframe src="https://player.rss.com/df3ndr/2248569?theme=dark&v=2" width="100%" height="202px" title="01x11_trust_me_im_a_keyboard.hid" frameBorder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen scrolling="no"><a href="https://rss.com/podcasts/df3ndr/2248569/">01x11_trust_me_im_a_keyboard.hid | RSS.com</a></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/k4UWCSnpu-U?si=K0vgeK-0JLzPqjRp" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Geo Blocking in Conditional Access

Geo blocking in Entra Conditional Access is all about controlling access to Microsoft 365 or Entra-integrated apps based on the geographic location of a sign-in attempt. Geo blocking helps reduce risk from regions where an organization doesn't operate or where malicious activity is common. If you've ever looked at your sign-in logs in Entra, you've likely seen something like this:

<img src="/assets/images/01x11/01x11-logs.png" alt="failed" width="100%">

Many organizations are familiar with network locations in Entra Conditional Access as a way to relax security requirements for connections from 'trusted' locations such as local office IPs, etc. With the geo blocking approach you can completely block geographic regions. Geo blocking is particularly effective at reducing the attack surface and enforcing compliance requirements, but as always there are some important considerations:

* Accuracy: IP-based geo-location isn’t perfect (VPNs, proxies, mobile carriers can obscure true location).
* Legitimate travel: A user traveling overseas may get blocked unless you allow certain exceptions - you'd want to implement an exception policy etc.
* Break glass accounts: Always exempt at least one emergency admin account from geo-blocking rules.
* Service traffic: Some Microsoft services may appear to originate from outside your “allowed” regions.

It's Conditional Access so you have a lot of flexibility - for example: Only allow access from trusted devices inside your approved geographies, otherwise block or enforce MFA. Personally, I usually advise customers to block access from what we deem to be 'high-risk' countries - this list will usually different from organization to organization and may even be different by industry. A good place to start is to look at your sign-in logs and build a list from there. Another strategy is to look at the list of Office of Foreign Assets Control (OFAC) or similar sanctioned countries or the Top 10 cyber threat countries:

* Russia
* China
* Ukraine
* Nigeria
* Romania
* North Korea
* Brazil
* India
* Pakistan
* Vietnam

It is important to understand that geo blocking by itself isn't a comprehensive strategy, it will reduce your attack surface and keep script kiddies away but it should form part of a layered security approach that makes it more difficult and/or expensive for bad actors to target you.

The simplest way to implement a geo blocking policy:

1. Go to Entra admin center > Security > Conditional Access > Named locations.
2. Create a Named location and select the countries/regions to allow or block - call it 'Risky Countries'
3. Create a new Conditional Access policy:

  * Users - Include: All Users
  * Target - Include: All Resources
  * Network - Include: Risky Countries
  * Grant: Block access

### Links

* Microsoft Learn [Block access by location](https://learn.microsoft.com/en-us/entra/identity/conditional-access/policy-block-by-location)

---

## The Human element in cybersecurity

So yes, the human factor in cybersecurity. "The human is usually the weakest link" a statement that is often thrown around casually. But I don't agree with it.

This topic came to me after I visited the hak5 booth at DEFCON actually. I picked up an **O.MG cable** there and starting tinkering with it and tried to answer the question "Could I protect end users from this?". And I think we can't.

For people who don't know an O.MG cable looks like a regular USB cable but can inject keystrokes, exfiltrate data, and/or create backdoors. If you want to know more go listen one of the recent episodes of Darknet Diaries.
This episode was actually the first time I heard about this. I feel a bit stupid for saying this but I'm not into red teaming, I'm no pentester, I wasn't aware of half the catalog hak5 was selling at DEFCON. And it made me think: if I wasn't aware of this cable, how can I expect my colleagues from HR or Marketing to know about it?!

Because this malicious USB device acts like a keyboard, I'm not sure how to prevent it from working except for blocking all USB ports altogether.

So, "Could I protect end users from this?" well perhaps, but not with tools but by training people. Humans are not the weakest link against these types of attacks, they might actually be the last line of defense.

This was also made clear to me when visiting the **Social Engineering Village** at DEFCON where I watched a *"vishing"* contest. A vishing contest is a live competition where participants use phone calls and social engineering tactics to trick real companies into revealing sensitive information.
I was both pleasantly surprised to hear the people who picked up the phone mostly had some form of security awareness training, and weren't handing out information for the get go. But unfortunately the skills of the social engineering experts were better in a lot of cases and with all kinds of flair and persuasion they still managed to get some valuable information about the systems they were using, how people enter the building etc.

### Why Humans Matter in Modern Security

* Humans (when trained properly) could detect social engineering attempts when tech can’t (e.g. gut feeling / instinct / intuition).
* Humans can report phishing links to the cybersecurity team whenever Defender for Office failed to remove it.
* Humans can spot MFA fatigue attacks by reporting repeated prompts instead of blindly accepting them.
* Humans question strange behavior like “Why is this random guy plugging in this cable in that PC under the desk?" ;-)

### Ways for building a "Human Firewall"

So how do we strengthen that human layer? Not with simulated phishing campaigns alone I think.
Whenever I'm at a party and explain to people what I do for a living, most people get started about the ridiculously fake phishing e-mails they receive within the company, and the boring video training they have to complete.

* **Security awareness that resonates** – Customize your phishing exercises and tailor them to your organization. I think not enough companies take advantage of this.
* **Realistic scenarios** – Simulate believable attacks and don't stick to e-mails only. Drop some phishing USB drives on the parking lot, have mystery guests visit, install rogue hardware in that one meeting room with all those exposed ports alongside the wall.
* **Hands-on demos** – Show users a real O.MG cable or Rubber Ducky and explain what they do. If I wasn't aware of this cable, how can I expect my colleague from HR or Marketing to know about it?!
* **Instruct about Public WiFi** - Explain the risks and teach them how to remove those stored networks from when they visited that hotel 5 years ago. DEFCON also led me to learn that this is possible on my iPhone ;-)  

For those unaware; a rogue access point can passively listen for “probe requests” that your device sends out when looking to reconnect to known networks. These probes contain the SSIDs of networks your device has previously connected to. By mimicking a SSID your devices recognizes it tricks your device into auto-connecting (without any user interaction) and enables for various man-in-the-middle attacks.

* **Encourage questions** – Employees might be hesitant to report weird behavior or phishing e-mails becaus they don't want to look stupid or are afraid to get into trouble if they clocked/opened something. I think it's best to remove the fear of reporting false alarms before this happens. Make employees feel empowered rather than intimidated.

### But Technology can still help as well!

* Use **Defender for Endpoint's device control** for unapproved USB HID class devices like unapproved keyboards. Apply allow lists based on vendor ID (VID) and product ID (PID) to only permit known hardware. Also remember to educate employees why personal USBs for example are blocked. Turn frustration into awareness.
* Use **"Additional context in Authenticator notifications"** to ensure that additional data is shown in the popup like location of origin, application, IP address etc.
* Enable **"Report suspicious activity"** for MFA so that users can report malicious MFA request.
* Make people aware of the **“Report Message” option in Outlook**.
* Far too few companies **incorporate 802.1X** if you'd ask me. This is a network access control protocol for devices connecting to your LAN. It enforces authentication at the network switch port level before granting access to the network. You typically need a RADIUS server and authentication can be done with a client certificate to ensure that unknown devices don't even receive an IP address on your trusted network.
* And even without this, know that Defender for Endpoint will "snitch" on potentially rogue devices if **Device Discovery** is turned on. Go to Microsoft 365 Defender portal --> Settings --> Endpoints --> Discovery and select **'Standard'**. ('Standard' will provide richer device info than 'Basic') View discovered devices at “Device Inventory” --> “Discovered devices” or create a custom detection with:

```kusto
DeviceInfo
| where OnboardingStatus != "Onboarded"
| summarize count() by DeviceName, DeviceType, IPAddress
```

### Links

* [Darknet Diaries episode about the O.MG Cable](https://darknetdiaries.com/episode/161/)
* [Buy pentesting gear at hak5](https://hak5.org)
* [How to forget a Wi-Fi network on iPhone, iPad or Mac](https://support.apple.com/en-us/102480)
* [How do I delete unwanted wifi network from my pixel phone](https://support.google.com/fi/thread/207045756/how-do-i-delete-unwanted-wifi-network-from-my-pixel-7-phone?hl=en)
* [How to forget a network on Android Mobile Device](https://www.samsung.com/sg/support/mobile-devices/how-to-forget-a-network-on-samsung-mobile-device/)
* [How to forget wifi networks in Microsoft Windows](https://learn.microsoft.com/en-us/answers/questions/4139279/how-to-forget-wifi-network-in-microsoft-windows-11)
* [Simulate a phishing attack with Attack simulation training](https://learn.microsoft.com/en-us/defender-office-365/attack-simulation-training-simulations)
* [Use additional context in Authenticator notifications](https://learn.microsoft.com/en-us/entra/identity/authentication/how-to-mfa-additional-context)
* [Configure Microsoft Entra multifactor authentication settings - Report suspicious activity](https://learn.microsoft.com/en-us/entra/identity/authentication/howto-mfa-mfasettings#report-suspicious-activity)
* [Use the built-in Report button in Outlook](https://learn.microsoft.com/en-us/defender-office-365/submissions-outlook-report-messages#use-the-built-in-report-button-in-outlook)
* [Microsoft Defender for Endpoint Device Discovery Overview](https://learn.microsoft.com/en-us/defender-endpoint/device-discovery)

---

## Community Project

### EntraOps and SentinelEnrichment

EntraOps was made by German Security MVP Thomas Naunheim and he together with Fabian Bader (also Security MVP from Germany) worked together on SentinelEnrichment.

SentinelEnrichment caught my eye first because this PowerShell module will make your life creating/updating Microsoft Sentinel Watchlists a lot easier!

- Supports large MicrosoftSentinel Watchlist uploads without requiring blob storage - by using file batching.
- Improved Deletion Handling, enhanced asynchronous operations make Watchlist cleanup smoother and more reliable.

[Download the module from PSGallery](https://www.powershellgallery.com/packages/SentinelEnrichment/0.2.0)

Only then I've noticed that Thomas incorporated SentinelEnrichment into EntraOps:

[EntraOps](https://github.com/Cloud-Architekt/EntraOps) is a research project to show capabilities for automated management of Microsoft Entra ID tenant at scale by using DevOps-approach

#### Key features

* Track changes and history of privileged principals and their assignments "as code"
* Identify privileged assets based on automated and full customizable classifications
* Build reports (Workbooks) on your classified privileges
* Automated assignment of privileged assets in Conditional Access Groups to protect high-privileged assets from lower privileges and apply strong Zero Trust policies.
* And much more!

---

## Experts Live US

Experts Live is a global network that brings together Microsoft executives, MVPs, subject matter experts, and community members through regional and country events to share knowledge and expertise about Microsoft technologies.

Held on October 10th for the very first time in the United States at the Microsoft office at Times Square in New York City. The lineup of speakers looks to be amazing and  tickets are only $ 15,- !! Will we see you there??

Experts Live US is proud to support STEM Kids NYC, helping them bring technical classes, materials and support to kids in the New York City area. All proceeds from our attendee registration will be donated to STEM Kids NYC!

[Check out the Experts Live US website for more information](https://www.expertslive.us)
