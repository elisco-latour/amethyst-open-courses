---
id: "finguard_00_03"
title: "Data in Motion: How Information Travels"
type: "reading"
xp: 75
---

# Data in Motion

In the last chapter, you learned about **data at rest** — information saved to files. But saved data, by itself, doesn't *do* anything.

To check your balance, detect fraud, or send money, data must **move**. We call this **data in motion**.

## Three Ways Data Travels

Think about how physical mail works. You have different options depending on urgency:

### 1. The Bank Teller (Ask and Wait)

You walk up to a bank teller and ask: *"What's my balance?"*

The teller looks it up, and you **wait** until they give you an answer. You can't do anything else until they respond.

<div class="key-concept">
<h4>🔑 Key Concept: Synchronous Communication</h4>

When you ask for data and must **wait for the response** before continuing, this is called **synchronous** (or request/response) communication.

**Banking Example:** Checking your balance in a mobile app. The app asks the server and waits for the answer.

**Trade-off:** Simple and predictable, but if the server is slow, you're stuck waiting.
</div>

### 2. The Armored Truck (Collect and Deliver)

Imagine a bank that collects all the day's deposit slips in a box. At midnight, an armored truck picks up the entire box and delivers it to the central vault.

This is **batch processing**. We gather many pieces of data together and move them all at once.

<div class="key-concept">
<h4>🔑 Key Concept: Batch Processing</h4>

When we collect data over time and process it **all at once** on a schedule, this is called **batch** processing.

**Banking Example:** Monthly account statements. The bank doesn't generate your statement every second — it waits until month-end and creates it from all your transactions.

**Trade-off:** Very efficient for large amounts of data, but the information is always slightly "old" (we call this **latency**).
</div>

### 3. The Ticker Tape (Instant Alerts)

In old stock exchanges, prices were printed on a continuous paper tape the moment they changed. Traders could react instantly.

This is **stream processing**. Each piece of data is processed the moment it arrives.

<div class="key-concept">
<h4>🔑 Key Concept: Stream Processing</h4>

When we process data **immediately as it arrives**, one piece at a time, this is called **stream** (or real-time) processing.

**Banking Example:** Fraud detection. If someone steals your card and tries to buy a $5,000 TV, the bank needs to block it *now* — not at midnight when the batch runs.

**Trade-off:** Very fast, but complex and expensive to build.
</div>

## Why This Matters for FinGuard

In this residency, you'll learn to use **both** approaches:

| Task | Approach | Why |
|------|----------|-----|
| Fraud Detection | Stream | Speed is safety. We must react in milliseconds. |
| Regulatory Reports | Batch | Accuracy is safety. We need complete, verified data. |

The mark of a professional is knowing **which tool fits which problem**.

## Looking Ahead

Don't worry if these concepts feel abstract right now. Over the coming courses, you'll write actual code that implements each pattern. For now, just remember:

- **Data at rest** = Information saved to storage
- **Data in motion** = Information traveling between systems
- **Batch** = Move lots of data on a schedule
- **Stream** = Move data instantly, piece by piece

