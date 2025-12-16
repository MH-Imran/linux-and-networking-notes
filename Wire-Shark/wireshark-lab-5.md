# Wireshark Lab 5 — TCP Handshake + “Who Started It?”

## Why I did this lab
In the previous labs I focused a lot on DNS and HTTP. Today I wanted to get more comfortable with TCP — mainly the part that matters most in investigations:

**Who initiated the connection, and how did the connection start?**

I kept the goal small on purpose. I’m not trying to understand every TCP flag or every packet. I just want to recognize the pattern and build confidence.

---

## What I did
- Started a short capture in Wireshark on my primary OS
- Did a little normal browsing (nothing fancy)
- Stopped the capture quickly so I wouldn’t drown in packets

---

## Filter I used
To find connection starts, I used:

This helped me focus on packets that begin new TCP connections.

---

## What I looked for (simple checklist)
When I clicked on SYN packets, I paid attention to:
- **Source IP** (who started it)
- **Destination IP** (who they tried to reach)
- **Source port** (usually a random high port)
- **Destination port** (like 80/443 or something else)

This alone already gives a lot of investigation value.

---

## What I noticed
- A single website visit can create **many TCP connections**, not just one.
- Usually my device (client) starts the connection with a **SYN**.
- The destination often responds with **SYN, ACK** (I could see it right after in many cases).
- After that, the **ACK** completes the handshake.

Even though I didn’t fully analyze every session, I could clearly see the handshake pattern repeating.

---

## “Who started it?” (SOC mindset practice)
This is the question I practiced today:

**Did my device initiate the connection, or did something connect to me?**

In normal browsing, it was almost always:
- My device → starts the connection → server responds

That’s expected behavior.

This is important because in real SOC investigations, an unexpected inbound connection attempt can be a bigger red flag.

---

## What confused me (being honest)
- Sometimes I couldn’t clearly follow one full handshake because the capture was busy.
- I still get distracted by a lot of background traffic.
- I’m not fully comfortable yet with all TCP fields in the details pane.

Instead of stressing about it, I stayed focused on the main objective: **recognize SYN packets and identify who initiated the connection**.

---

## Key takeaway
This lab helped me feel more confident about TCP connections.

I’m starting to see Wireshark less like a “wall of random packets” and more like a tool where I can ask one clear question and find the answer.

---

## Next time
- Capture traffic for one specific action (open one site, then stop)
- Follow a single TCP stream more carefully
- Practice spotting unusual ports or repeated connection attempts
