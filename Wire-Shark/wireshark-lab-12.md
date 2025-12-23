# Wireshark Lab 11 + Lab 12 (Combined Notes)

**Host:** `192.168.0.113`  
**Router/Gateway:** `192.168.0.1`  
**Theme:** Learning to read the “story” in DNS → TCP → TLS, then comparing it with a refused connection (RST/ACK)



## Lab 11 — Finally making sense of the noise (DNS → TCP → TLS)

I used to open Wireshark and instantly feel overwhelmed. Too many packets, too many colors, and it’s easy to scroll without understanding anything.

So I changed the approach: I kept the capture very short and tried to prove one simple thing.

**Can I see the exact moment my browser connects to a site, step-by-step?**

## Setup (keeping it clean)
- **Action:** Trigger traffic to a specific domain: `chromium.dns.nextdns.io`
- **Goal:** Capture a clean chain of events I can explain without guessing
- **Method:** Stop capture as soon as the page loads (avoid “packet noise”)



## Step 1 — DNS: “I need an address before I can talk”

**The Query (my machine asking)**
- **Time:** ~9.941s  
- **Traffic:** `192.168.0.113` → `192.168.0.1`  
- **Meaning:** “What is the IP for `chromium.dns.nextdns.io`?”

**The Response (router answering)**
- **Time:** ~9.953s  
- **What I saw:** CNAME (`steering.nextdns.io`) followed by destination IPs (example: `217.146.10.59`, etc.)

**My takeaway**
DNS worked normally. The name resolution followed a CNAME path and then returned the final IPs. That’s a healthy baseline.



## Step 2 — TCP: “Knocking on the door”

Once I had the IP, my computer had to build the connection first (before sending TLS or application data).

**SYN (Hello?)**
- My PC sent a SYN to the destination on **port 443**
- Translation: “Are you available?”

**SYN/ACK (Yes, come in)**
- Server replied with SYN/ACK
- Translation: “I hear you, let’s connect.”

**ACK (Deal)**
- My PC sent ACK
- Translation: “Connection established.”

**My takeaway**
This is the classic TCP 3-way handshake. Seeing it clean (no retransmissions/resets) tells me the network path is stable.



## Step 3 — TLS: “Let’s switch to privacy mode”

After TCP is established, the browser tries to secure the session.

**TLS ClientHello**
- **Protocol:** TLSv1.3  
- **Info:** `Client Hello (SNI = chromium.dns.nextdns.io)`

**Why this matters (SOC note)**
ClientHello is a strong indicator of a legit TLS session starting. The **SNI** shows the domain name before encryption fully takes over, which is useful for visibility (especially when DNS is hidden by DoH).



## The full story (one clean chain)
- DNS found the location  
- TCP built the bridge  
- TLS locked the door  

This is the baseline I wanted.



## Why I’m saving this capture
You can’t confidently spot “bad” traffic if you don’t understand what “good” traffic looks like.

This clean capture becomes my reference point. If I later see:
- SYN packets with no reply
- DNS queries that never resolve
- TLS handshakes failing

…I’ll have a healthy example to compare against.



## What’s next (breaking it on purpose)
Next time I want to intentionally break the flow:
- block the destination IP, or
- break DNS resolution,
and compare how the capture changes.



## Lab 12 — Understanding a TCP RST/ACK (Connection Refused)

This lab is about recognizing what it looks like when a connection attempt is rejected quickly.

###Evidence (packet captured)

6795 166.399435089 192.168.0.1 → 192.168.0.113 TCP 60
8888 → 41766 [RST, ACK] Seq=1 Ack=1 Win=0 Len=0 [Malformed Packet]



## What this means (plain language)
- A **RST (Reset)** is basically: “Stop. I’m not accepting this connection.”
- **RST,ACK** usually means **connection refused** / **port closed**, not a timeout.

**SOC translation**
The destination is reachable and responding, but the service on that port is not available (or is intentionally rejecting connections).

## Why the ports look like this
- `8888` = the destination service port being contacted (often used by dev tools, proxies, test servers)
- `41766` = a random high client port chosen temporarily by my machine

## Closed vs Filtered (how I separate them)
- **RST,ACK** (what I saw) → closed/rejected quickly  
- **No response** → timeout, often filtered (firewall drops packets)  
- **ICMP Port Unreachable** → common with UDP, not TCP

### About the “[Malformed Packet]” label
Wireshark sometimes labels packets “malformed” because of capture/offload behavior (checksum/segmentation offload), not because the traffic is truly broken.

Here, the flags (**RST,ACK**) and empty payload (**Len=0**) look normal for a reset response. I’d treat “malformed” as a warning only if I see it repeatedly with other odd behavior.

## Useful Wireshark filters for Lab 12
- Show resets:
tcp.flags.reset == 1

- Focus on the host pair:
ip.addr == 192.168.0.1 && ip.addr == 192.168.0.113

- Focus on the port:
tcp.port == 8888




## Final takeaway (SOC mindset)
- Lab 11 taught me the “healthy” chain: **DNS → TCP handshake → TLS ClientHello**
- Lab 12 taught me a fast failure signature: **RST,ACK = connection refused**

Next step: capture a real **timeout** example (SYN with no reply) so I can recognize filtered traffic just as quickly.

