+++
title = "The 6 Stages of Telco Scam Defence"
date = 2026-05-18T10:00:00+10:00
draft = false
tags = ["scams", "spf", "telcos", "architecture", "consumer-protection"]
+++

**TL;DR:** When my family and friends picture scam prevention, they assume a single magic box labelled *"Filter"*. But as technologists working under Australia's new Scam Prevention Framework (SPF), our job is breaking that assumption down into 6 messy, connected boxes—from the customer journey at a retail counter to cross-sector intelligence and victim restitution.

---

I haven’t written a post in a few weeks. 

The truth is, the last month has been completely all-consuming. I've been deep in the trenches, whiteboarding and dissecting the latest [SPF Codes and Rules published by Treasury](https://consult.treasury.gov.au/c2026-765133). 

Whenever I talk to my friends outside of telecom, they assume our job is simple: 
> *"Why don't telcos just block the scam calls and texts?"*

In their minds, there is one giant box in the middle of our network. Good traffic goes in, bad traffic gets dropped, and everyone moves on with their day.

If only it were that easy.

In **Post #1**, I wrote that my career boils down to asking three simple questions: *What goes in the box? What stays out? And how do we draw lines so everything actually talks to each other?*

When you look at scam defence through an architect's lens, that single "magic filter" actually splits into **6 distinct, messy stages**:

```
 ┌────────────────────────────────────────────────────────────────────────┐
 │ Stage 1: The Front Door (Prevent Bad Actors from Signing Up)          │
 ├────────────────────────────────────────────────────────────────────────┤
 │ Stage 2: In-Life Analytics (Finding Needles in Billions of Events)    │
 ├────────────────────────────────────────────────────────────────────────┤
 │ Stage 3: Blast Radius Assessment (Confirming Before Disconnecting)     │
 ├────────────────────────────────────────────────────────────────────────┤
 │ Stage 4: Traffic Interception (Filtering Calls, SMS, and MMS)         │
 ├────────────────────────────────────────────────────────────────────────┤
 │ Stage 5: Cross-Sector Intelligence (Breaking Down Industry Silos)      │
 ├────────────────────────────────────────────────────────────────────────┤
 │ Stage 6: Victim Restitution (The Messy Human Aftermath)                │
 └────────────────────────────────────────────────────────────────────────┘
```

Here is what is actually happening inside each of those boxes, and the architectural trade-offs we grapple with every day.

---

### Stage 1: Stop Scammers from Becoming Customers (The Front Door)

Scam defence doesn't start at the cellular tower. It starts at the retail counter and on the web checkout page.

To an everyday customer, buying a prepaid SIM card is a 3-minute errand. To a scam syndicate, that SIM card is **disposable ammunition**. They want bulk, legitimate Australian mobile numbers to blast out phishing campaigns before the numbers get flagged.

Telcos throw serious friction at the front door:
- **Digital ID Verification:** Checking passports and driver licenses against government document verification services.
- **Biometric Liveness Checks:** Forcing selfie video scans to stop synthetic identity bots.
- **Device & Order Fingerprinting:** Catching repeat offenders using VPNs or suspicious bulk payment methods.

Here is the architectural tension: **Every lock you put on the front door also slows down honest people.**

Imagine a 19-year-old casual retail worker in a suburban shopping centre having to tell an innocent, frustrated customer: *"I'm sorry, our fraud risk scoring system won't let me sell you this phone plan today."* 

False rejections ruin customer trust. Scammers slip through anyway. It is an imperfect, highly delicate balancing act.

---

### Stage 2: Identify Scammers Who Got Through the Front Door (In-Life Analytics)

What happens when a bad actor passes the identity checks and gets an active SIM?

Now we are in the realm of **behavioural discovery**. A Tier-1 telco processes hundreds of millions of events every 24 hours:
- **Call Data Records (CDRs)**
- **Event Data Records (EDRs)**
- Rapid SIM-swap signals
- Sudden changes in handset IMEI devices

Finding a scammer in this firehose is like trying to spot a single bad grain of sand on Bondi Beach while dump trucks keep dropping more sand on your head.

You look for anomaly clusters: a newly activated prepaid number that instantly makes 300 short-duration outbound calls across 50 area codes, or a sudden spike in automated SMS blasts.

> *Could we just throw a generative AI model at this telemetry and let it auto-terminate bad actors?* 
> 
> That sounds tempting. But in real life, automating disconnections on live network traffic without human oversight is a terrifying proposition.

---

### Stage 3: Supervised Investigation & Blast Radius

Data gives you signals; it rarely gives you absolute certainty.

Before a Scam Analyst or an automated system pulls the plug on a mobile phone, we have to pause and calculate the **blast radius**:

1. **Is this a dedicated scammer rig, or a compromised handset belonging to a real person?** If malware hijacked an elderly grandfather's phone to blast spam, cutting off his mobile line cuts off his connection to family and emergency services.
2. **What if our anomaly rule had a bug?** If an automated rule accidentally terminates 500 active lines at 2:00 AM, who answers for the people who needed to dial **Triple Zero (000)**?

This is why Stage 3 demands supervised investigation. We need repeatable, auditable workflows where human Scam Analysts review the telemetry, evaluate the evidence, and confirm the blast radius before taking any action.

---

### Stage 4: Blocking Incoming Scam Traffic (Voice, SMS, MMS)

This is the frontline defence that customers notice most: stopping malicious calls and phishing texts before their phone ever rings.

We do this via two deterministic tools:

1. **Deterministic Rules:** Hard behavioural logic. If an unverified account attempts to send 500 SMS messages in 60 seconds, cut the circuit immediately.
2. **Dynamic Threat Lists:** Verified indicators of bad actors—known malicious URL domains, flagged caller IDs, and compromised international gateway routes.

Under the SPF, telcos are tightening controls around alphanumeric Sender IDs (e.g., ensuring nobody can spoof "myGov" or "CommBank" unless registered on the official SMS Sender ID Register). 

It raises the financial cost for scammers, but they constantly rotate their numbers and rewrite their phrasing to find cracks in the filter.

---

### Stage 5: Acting on and Sharing Cross-Sector Intelligence

Scams do not happen in a silo. 

A typical scam campaign:
- **Starts** on a digital social platform (a fake investment ad or impersonation profile),
- **Moves** across a telco network (SMS, WhatsApp, or phone calls), and
- **Extracts money** through a bank (authorized push payment or crypto transfer).

Historically, these three industries lived on separate islands. Banks blamed telcos for passing the SMS; telcos blamed banks for facilitating the payment; digital platforms looked the other way.

Under the SPF, that changes. 

Through bodies like the **National Anti-Scam Centre (NASC)** and intelligence exchanges like **AFCX**, intelligence must flow across boundaries:
- When a bank confirms a mule account or a fraudulent URL, they push that signal downstream so telcos can immediately blacklist the domain.
- When a telco detects a massive spoofing campaign, we push the originating indicators upstream so banks and platforms can flag high-risk transactions.

Getting competitors and different industries to share real-time threat telemetry safely—without falling foul of privacy regulations—is one of the most complex architectural integration projects happening in Australia today.

---

### Stage 6: Customer Care, Dispute Resolution & Restitution

Even with 5 layers of active defence, some scams will inevitably succeed. Scammers are cunning, and social engineering exploits human vulnerability, not just technical bugs.

When a customer loses money, Stage 6 kicks in:

- **Incident Triage:** Immediately securing the victim's account to stop secondary credential takeovers.
- **Forensic Audit:** Tracing the root cause - did the scam succeed due to a spoofed SMS, a fraudulent SIM swap, or direct credential harvesting?
- **Dispute Resolution & Restitution:** Under the SPF, the Australian Financial Complaints Authority (**AFCA**) acts as the single-door dispute resolver. If a regulated entity failed to take reasonable steps under the SPF codes, liability and financial compensation are apportioned fairly.

Stage 6 is where the technical architecture meets raw human consequence. It forces organisations to treat scam prevention as an enterprise-wide responsibility, not just an IT checkbox.

---

### The Big Question

Looking across these 6 stages, one question inevitably comes up:

> *"If modern AI is so powerful, why can't we replace these manual reviews, threat lists, and complex rules with an end-to-end AI detection engine?"*

It is a fair question—and in my next post, I will break down why connecting probabilistic AI to deterministic telecom compliance is one of the hardest architectural challenges we face.

---

I hope you enjoyed reading this post. I'd really appreciate your views and feedback.