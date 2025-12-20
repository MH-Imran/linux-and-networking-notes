# Wireshark Lab 1 — First Look at Network Traffic

## Why I did this lab
This was my first serious attempt at using Wireshark.  
My goal wasn’t to understand everything — it was to get comfortable seeing real network traffic and recognizing a few basic patterns.

At first, Wireshark felt overwhelming because it shows *a lot* of packets. This lab helped me realize that SOC analysts don’t look at everything — they focus on what matters.

---

## What I captured
I ran Wireshark on my main system and generated normal traffic by:
- Opening a web browser
- Visiting a few common websites

This helped me see what “normal” traffic looks like.

---

## DNS observations
Using the filter:


I noticed:
- DNS queries happening before websites loaded
- Domain names being resolved into IP addresses
- Clear source and destination IPs

This helped me understand that DNS activity is often the *first step* before any network connection.

---

## HTTP observations
Using the filter:


I observed:
- HTTP GET requests
- Host headers showing which site was being accessed
- Status codes like 200 indicating successful responses

Seeing this in real traffic made HTTP feel much more real than just reading about it.

---

## TCP handshake (basic)
Using the filter:


I identified:
- SYN packets starting connections
- The beginning of TCP sessions

I didn’t analyze every flag yet — the goal was just to recognize how connections start.

---

## Key takeaway
I learned that Wireshark is not about reading every packet.  
It’s about asking simple questions like:
- What domain was contacted?
- Which IP initiated the connection?
- What protocol was used?

This lab helped reduce my fear of Wireshark.
