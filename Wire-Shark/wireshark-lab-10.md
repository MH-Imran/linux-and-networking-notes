# Wireshark Lab 10 — One Browser Action → DNS Evidence + TCP Handshake

Today I did a controlled Wireshark test instead of capturing random traffic.

My goal was simple:
1) Start capture  
2) Open Chrome  
3) Visit one website  
4) Stop capture  
5) Pull out clear evidence of what happened on the network

I focused on two things only:
- DNS (what domain my device resolved)
- TCP handshake (whether the HTTPS connection actually established)

---

## 1) DNS: domain resolution (query + response)

### DNS query
- **Time:** 59.073s  
- **From:** `192.168.0.113` (my device)  
- **To:** `192.168.0.1` (router / DNS)  
- **Query:** `A chromium.dns.nextdns.io`

This is my device asking: “What IP address is this domain?”

### DNS response
- **Time:** 59.085s  
- **From:** `192.168.0.1`  
- **To:** `192.168.0.113`  
- **Answer:**
  - `chromium.dns.nextdns.io`
  - **CNAME:** `steering.nextdns.io`
  - **A records:** `139.83.177.196`, `218.146.10.59`

What I take from this:
- DNS resolution worked normally.
- The domain resolves through a CNAME and returns multiple IPs (which is common).

---

## 2) TCP: a clean HTTPS connection setup (443)

After DNS, I looked for a complete TCP handshake to port **443**.

### SYN — my device starts the connection
- **Time:** 61.309s  
- **From:** `192.168.0.113`  
- **To:** `139.83.177.196`  
- **Ports:** `49480 → 443`  
- **Flags:** SYN

This is basically my computer saying: “Can I connect?”

### SYN/ACK — server responds
- **Time:** 61.421s  
- **From:** `139.83.177.196`  
- **To:** `192.168.0.113`  
- **Ports:** `443 → 49480`  
- **Flags:** SYN, ACK

The server is reachable and agrees to establish a session.

### ACK — connection established
- **Time:** 61.421s  
- **From:** `192.168.0.113`  
- **To:** `139.83.177.196`  
- **Ports:** `49480 → 443`  
- **Flags:** ACK

Handshake completed. This means the TCP connection was successfully created and ready for HTTPS/TLS traffic.

---

## Why I’m saving this (SOC mindset)
This lab gave me a simple, clean “action → evidence” example:

- **DNS shows intent:** what domain my device tried to reach
- **TCP handshake shows success:** whether the connection actually established

It also gives me a baseline to compare against weird cases like:
- SYN retransmissions (timeouts / no reply)
- RST packets (connection reset/refused)
- ICMP errors (unreachable)

---

## Quick takeaway
I’m getting more comfortable treating Wireshark like a verification tool:
- Do one action
- Prove it with DNS + TCP evidence
- Write the story in simple words

---

## Next thing I want to try
- Follow the stream after the handshake to spot TLS/HTTPS traffic
- Repeat the same test and compare timings
- Capture a “failed” case (SYN retries or RST) to compare with this clean baseline
