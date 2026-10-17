+++
title = "Probabilistic Attackers, Deterministic Rules: Why AI Scam Detection is Hard"
date = 2026-05-20T10:00:00+10:00
draft = true
tags = ["scams", "spf", "ai", "machine-learning", "telcos", "regulation"]
+++

**TL;DR:** Everyone wants AI to solve scam detection with a magic wand. But there is a fundamental clash: regulations and customer trust demand 100% deterministic guarantees, while AI models operate in probabilities. Here is why stopping scams across telcos and banks is an architectural challenge, not just an algorithmic one.

---

In my [previous post](/posts/post9-five-stages-of-telco-scam-defence/), I broke down the **5 stages of telco scam defence** under Australia's new Scam Prevention Framework (SPF). 

At the end of that post, I posed a question that comes up in almost every architectural discussion:

> *"If modern AI can write code, pass medical exams, and compose symphonies, why can't telcos and banks just feed all network traffic into an AI model and block scams automatically?"*

It sounds straightforward. We have massive Large Language Models (LLMs), transformer-based anomaly detectors, and real-time streaming pipelines. 

Why can't we just let AI protect us?

The short answer: **Scammers are probabilistic, regulations are deterministic, and customers have zero tolerance for false positives.**

---

### The Allure of the Magic Filter

On paper, building an AI scam detector sounds like a standard data science assignment:

1. Ingest SMS text, Call Data Records (CDRs), and Event Data Records (EDRs).
2. Train a classifier on known scam patterns and malicious phone numbers.
3. If `is_scam_probability > 0.90`, drop the call or block the message.

In a laboratory environment or a Kaggle competition, a model with **99% accuracy** gets you a gold medal.

In telecom and banking networks handling tens of millions of events every single day, a 99% accurate model is a catastrophe.

---

### The Mathematics of Friction (False Positives vs. False Negatives)

Let's look at what happens when you run probabilistic models at network scale.

```
                    ┌────────────────────────┐
                    │ 10,000,000 Events/Day  │
                    └───────────┬────────────┘
                                │
                    ┌───────────▼────────────┐
                    │    AI Filter (99.0%)   │
                    └─────┬────────────┬─────┘
                          │            │
            ┌─────────────▼────┐  ┌────▼─────────────┐
            │ 1% False Positive│  │ 1% False Negative│
            │  (100,000 Legitimate│ │ (Scams Slip    │
            │   Actions Blocked) │ │  Through)       │
            └──────────────────┘  └──────────────────┘
```

#### 1. The False Positive Nightmare (Customer Impact)
If an Australian telco processes 20 million SMS messages a day and your AI classifier has a tiny **0.1% false positive rate**:
- That is **20,000 legitimate messages blocked every single day**.
- These aren't just spam; they include bank 2FA login codes, hospital appointment reminders, flight cancellation alerts, and urgent family messages.
- The cost isn't just angry customer support calls—it breaks critical societal infrastructure.

#### 2. The False Negative Catastrophe (Regulatory & Victim Impact)
On the flip side, what happens to the 0.5% or 1% of scams that slip past the threshold?
- A victim loses their $50,000 life savings.
- Under the SPF, regulators (ACMA, ASIC, ACCC) and dispute resolution bodies (AFCA) ask a simple question: *Why wasn't this blocked?*

Saying *"our neural network had a 0.84 confidence score, which was below our 0.85 decision boundary"* will not satisfy a grieving victim or a compliance auditor.

---

### The Clash: Probabilistic AI vs. Deterministic Law

In **Post #8**, I wrote about using an AI agent to verify my tax returns, where I noted:

> *How do we trust something which by design is probabilistic in a task that requires strict deterministic compliance?*

The SPF places legal obligations on institutions across five key pillars: **Prevent, Detect, Disrupt, Respond, and Report**.

The law is fundamentally deterministic:
- Did you verify the Sender ID against the registry? *(Yes/No)*
- Did you check if the recipient account was newly created? *(Yes/No)*
- Did you notify the consumer of high-risk indicators? *(Yes/No)*

Machine learning models, however, are inherently statistical approximations. They do not operate on fixed rules; they operate on weighted probabilities. 

When you connect a probabilistic engine directly to a deterministic compliance mandate without strict guardrails, you risk catastrophic failures on both sides of the balance.

---

### Adversaries Adapt Faster Than Training Loops

There is another reality: **scammers do not stay static**.

Traditional spam filters relied on keyword blacklists and static rule engines. Scammers responded with character substitutions (`b@nk`, `A.T.O`).

When defenders moved to semantic embeddings and NLP classifiers, scammers shifted to:
- **Ecosystem jumping:** Starting with an SMS, moving to WhatsApp or Telegram, and concluding on peer-to-peer crypto rails.
- **Context dilution:** Sending normal-looking introductory messages ("Hi Mum, I dropped my phone") that contain zero malicious keywords or suspicious links.
- **Adversarial prompting:** Using AI themselves to generate polymorphic SMS scripts specifically tuned to evade common telco classifiers.

An AI model trained on last month's scam data is always fighting yesterday's war.

---

### How to Actually Solve This: Friction, Not Magic

If pure AI automation is too risky, and pure manual review is too slow, what is the architectural answer?

The solution lies in **layered architecture** and **intelligent friction**:

```
 ┌────────────────────────────────────────────────────────┐
 │ Layer 1: Deterministic Gates (Registry / KYC / KYT)    │ -> Fast, rule-based
 ├────────────────────────────────────────────────────────┤
 │ Layer 2: Probabilistic AI Scoring (Risk Anomaly)       │ -> Flags, does not drop
 ├────────────────────────────────────────────────────────┤
 │ Layer 3: Adaptive Friction (Step-up Verification / Hold)│ -> Delays suspicious actions
 ├────────────────────────────────────────────────────────┤
 │ Layer 4: Human-in-the-Loop & Shared Ecosystem Feeds   │ -> SPF / NASC intelligence
 └────────────────────────────────────────────────────────┘
```

1. **AI as an Anomaly Radar, Not an Executioner:**
   AI should rarely make unilateral binary decisions (like silently dropping a text or terminating a SIM). Instead, it assigns dynamic risk scores that feed into downstream workflows.

2. **Step-Up Friction Instead of Hard Blocks:**
   If a payment or message looks suspicious, introduce a 2-hour cooldown window, a confirmation callback, or a prominent warning banner.

3. **Deterministic Guardrails Around Probabilistic Cores:**
   Just like the rules file I built for my coding agent in **Post #5**, anti-scam AI pipelines require hard perimeter rules: never block certified emergency numbers, enforce verified sender whitelists, and require dual-key verification before blacklisting an entire IP range.

---

### Final Thoughts

AI will play an indispensable role in fighting scams. Without machine learning, detecting modern multi-vector attacks across millions of transactions is simply impossible.

But AI is not a silver bullet. It is an engine that must sit inside a carefully designed chassis of deterministic rules, ecosystem intelligence sharing, and calibrated human friction.

As Australia rolls out the Scam Prevention Framework, the winners won't be the organizations with the biggest AI models.

They will be the ones who figure out how to balance statistical confidence with deterministic trust.

---
*What do you think? Have you experienced a false-positive bank block or an SMS scam that bypassed filters? Let me know in the comments below!*
