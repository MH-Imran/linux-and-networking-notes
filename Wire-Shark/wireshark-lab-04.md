# Wireshark Lab 4 — DNS → HTTP Flow (Correlation Practice)

## Why I did this lab
I’m trying to get better at connecting events together instead of looking at packets one by one. In SOC work, you usually don’t stare at every packet — you try to understand the sequence: **what happened first, what happened next, and why**.

So today I kept the capture short and focused on one simple question:

**Did DNS happen before the website loaded, and can I follow that into HTTP traffic?**

---

## What I did (simple setup)
- Started a fresh capture in Wireshark (short session)
- Opened my browser and visited 2–3 websites
- Stopped the capture quickly so it stayed manageable

I didn’t try to analyze everything. I focused only on DNS and HTTP.

---

## Step 1 — DNS check
Filter I used:

What I looked for:
- The **domain name** that was queried
- Source IP (my device) → Destination IP (DNS server)
- Whether I saw a response (so I knew the query actually completed)

What I noticed:
- A single website often triggers **multiple DNS queries**, not just one.
- DNS happens immediately before the browser does anything meaningful.
- Even normal browsing creates a lot of “noise”, so filtering is required.

---

## Step 2 — HTTP check
Filter I used:

What I looked for:
- **GET** requests
- Host header / requested resource (if visible)
- Source and destination IPs
- Any obvious status codes (200, 301, 404)

What I noticed:
- Some websites didn’t show much HTTP because they were mainly HTTPS.
- When HTTP was visible, it was easier to understand than encrypted traffic.
- The traffic felt much less confusing when I stopped trying to read every line.

---

## Correlation (the main point)
This is the part I’m training:

1) I see a DNS query for a domain  
2) Then I see the follow-up traffic that happens after that (HTTP if available)

Even when HTTP isn’t visible (because HTTPS is used), the DNS step is still useful:
- It shows *where the system wanted to go*
- It can help in investigations when content is encrypted

This made me understand why SOC analysts often start with DNS logs.

---

## What confused me (honestly)
- It’s not always obvious which HTTP request matches which DNS query, especially when multiple tabs or background services are active.
- I still get distracted by random protocols and extra packets.
- HTTPS makes traffic analysis harder because I can’t read the content.

Instead of getting stuck, I reminded myself: **my goal is the flow, not perfection**.

---

## Key takeaway
This lab helped me build a simple investigation habit:

**“Start with DNS, then follow the next step.”**

It’s a small skill, but I can see how this mindset becomes useful in real SOC investigations, especially when dealing with suspicious domain activity.

---

## Next time
- Do a longer capture but only for one website
- Try to reduce background noise (close extra tabs/apps)
- Keep practicing the DNS → connection flow
