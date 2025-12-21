# Wireshark Lab 11 — (DNS → TCP → TLS)

I used to open Wireshark and immediately feel overwhelmed. It’s easy to get lost in a sea of packets, protocols, and colors without really understanding what’s happening.

So today I tried a different approach.

Instead of trying to “analyze everything,” I kept the capture **very short** and focused on proving one simple thing:

**Can I clearly see the moment my browser connects to a site, step-by-step?**

---

## The Setup (Keeping it clean)

- **Action:** Trigger traffic to a specific domain: `chromium.dns.nextdns.io`
- **Goal:** Capture a clean **chain of events** I can explain without guessing
- **Result:** I stopped the capture right when the page loaded, so I didn’t have to dig through thousands of unrelated packets

What I got was a simple story the packets told me.

---

## Step 1 — DNS: “I need an address before I can talk”

### The Query (my machine asking)
- **Time:** 9.941s  
- **Traffic:** `192.168.0.113` → `192.168.0.1`  
- **Meaning:** “What is the IP address for `chromium.dns.nextdns.io`?”

### The Response (router answering)
- **Time:** 9.953s  
- **Meaning:** Router replied with a **CNAME** (`steering.nextdns.io`) and then returned the final destination IPs (example: `217.146.10.59`, etc.)

**My takeaway:**  
DNS is healthy. The domain didn’t just resolve instantly — it followed a CNAME path first, then landed on the final IP.

---

## Step 2 — TCP: “Knocking on the door”

Once my computer had the IP, it didn’t start sending application data immediately. It had to create the connection first.

### SYN (Hello?)
- **Packet 208:** My PC sends a **SYN** to the server on **port 443**
- **Translation:** “Are you open for business?”

### SYN/ACK (Yes, come in)
- **Packet 212:** Server responds with **SYN/ACK**
- **Translation:** “I hear you, and yes, let’s talk.”

### ACK (Deal)
- **Packet 217:** My PC sends the final **ACK**
- **Translation:** “Great — connection established.”

**My takeaway:**  
This is the clean TCP **3-way handshake**. No retransmissions, no resets — which suggests the network path is behaving normally.

---

## Step 3 — TLS: “Let’s switch to privacy mode”

Now that TCP is established, the browser immediately tries to secure the session.

### TLS ClientHello
- **Protocol:** TLSv1.3  
- **Info:** `Client Hello (SNI = chromium.dns.nextdns.io)`

**Why this matters (SOC note):**  
This packet is valuable because it proves encryption negotiation is starting, and the **SNI (Server Name Indication)** exposes the domain name before traffic becomes fully encrypted. Even if DNS is hidden later (like DoH), SNI can still help identify the destination.

---

## The Full Story (One clean chain)

- **DNS** found the location  
- **TCP** built the bridge  
- **TLS** locked the door  

That is exactly the flow I wanted to visualize.

---

## Why I’m saving this capture

You can’t spot “bad” traffic if you don’t know what “good” traffic looks like.

This capture is now my baseline. If I troubleshoot later and see:
- SYN packets with no response
- DNS queries that return nothing
- TLS handshakes failing

…I’ll have a healthy reference to compare against.

---

## What’s next?

Now that I understand what a healthy connection looks like, I want to break it on purpose.

**Next lab idea:**  
Block the destination IP or break DNS and compare how Wireshark looks when things go wrong.
