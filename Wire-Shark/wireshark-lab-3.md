# Wireshark Lab 3 — Thinking Like a SOC Analyst

## Purpose of this lab
This lab was less about tools and more about mindset.  
I tried to look at traffic the way a SOC analyst would — asking basic investigative questions instead of diving deep too fast.

---

## Capture approach
I kept the capture simple:
- Short capture duration
- Normal browsing behavior
- Focused analysis instead of long sessions

This made the traffic easier to reason about.

---

## DNS review
Filter:


SOC-style questions I asked myself:
- Which domains are being resolved?
- Do these domains look expected?
- Is the volume normal?

Even simple DNS traffic can tell a story.

---

## HTTP review
Filter:


What I focused on:
- Which site initiated the request
- What resource was being requested
- Whether the request looked normal

I started thinking about how suspicious URLs or strange user-agents might stand out.

---

## Connecting traffic to SOC work
This lab helped me understand:
- Why network alerts often start with DNS or HTTP logs
- How packet data supports SIEM alerts
- Why knowing “normal” behavior matters before detecting attacks

I didn’t detect an attack here — and that’s fine.  
Understanding baseline behavior is the first step.

---

## What I’m improving next
- Becoming faster with filters
- Connecting Wireshark observations to logs
- Learning which traffic patterns deserve attention

---

## Key takeaway
This lab helped me shift from “learning Wireshark” to “using Wireshark with purpose.”  
I’m still learning, but things are starting to make more sense.

