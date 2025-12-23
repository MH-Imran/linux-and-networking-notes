# Wireshark Lab 7 — Retransmitted SYNs, ICMP “Port Unreachable”, and a DNS Answer

Today’s capture was more interesting than usual because I saw a few patterns that felt very “real world”:
- repeated TCP SYN packets (retransmissions)
- ICMP messages saying “port unreachable”
- and later, a clean DNS response for a domain I’ve been seeing (chromium.dns.nextdns.io)

I’m writing this like a learning log — not perfect, just what I understood.

---

## 1) Repeated TCP SYN retransmissions (same destination + same port)

These lines caught my attention:

- `192.168.0.113 → 192.168.0.102`  
- `53794 → 8009`  
- `[SYN]` with **TCP Retransmission**

I saw the SYN being sent multiple times:

- **344.514s** — SYN retransmission  
- **345.538s** — SYN retransmission  
- **348.614s** — SYN retransmission  

### What I think this means
My device (`192.168.0.113`) was trying to start a TCP connection to a local device (`192.168.0.102`) on **port 8009**, but it wasn’t getting a response back (no SYN/ACK), so it retried.

This is not automatically malicious. It can happen when:
- the destination device is offline
- the service/port is closed
- a firewall blocks it
- something on my system is probing a local service (sometimes normal discovery behavior)

The key detail for me is: **it’s repeated attempts to the same host + same port**, which is exactly the kind of “pattern” a SOC analyst would notice.

---

## 2) ICMP “Destination unreachable (Port unreachable)”

I also saw:

- `192.168.0.113 → 192.168.0.1`  
- ICMP: **Destination unreachable (Port unreachable)**

It appeared at least twice:
- **531.949s**
- **749.581s**

### How I understand it
“Port unreachable” usually means something tried to send traffic to a port that wasn’t open, and the system responded saying:  
> “Nothing is listening there.”

The part I still need to confirm is *why my device is sending this to my router (192.168.0.1)*. My guess is that some UDP traffic hit a closed port and triggered an ICMP message, but I need more context to be sure.

For now, I’m just noting the fact that ICMP “port unreachable” appeared, because it’s a useful troubleshooting clue.

---

## 3) DNS response for chromium.dns.nextdns.io

Later I got a proper DNS response from the router:

- `192.168.0.1 → 192.168.0.113`
- Response includes:
  - `chromium.dns.nextdns.io`
  - CNAME: `steering.nextdns.io`
  - A records:
    - `217.146.10.59`
    - `139.84.177.196`

And I saw it more than once, with different query IDs:
- `0xfb6a`
- `0x0b00`

### What this tells me
This looks like normal DNS behavior:
- my router responded
- the domain resolves through a CNAME
- it gave multiple IP addresses (which is common)

This DNS part feels “clean” and normal compared to the TCP retransmissions.

---

## My simple SOC-style takeaway
Today’s logs made me practice a basic investigation mindset:

- **DNS shows intent** (what domain was requested)
- **TCP SYN retransmissions show connection trouble** (something tried repeatedly and failed)
- **ICMP port unreachable is a clue** (a port was closed somewhere)

Nothing here screams “attack”, but these patterns are exactly the kind of things I should be able to explain during SOC work.

---

## What I want to check next time
- Identify what device `192.168.0.102` really is (phone? TV? another machine?)
- Confirm whether port **8009** is related to something normal in my home network
- Look for any **SYN/ACK** or RST responses around the retransmissions
- If possible, map the traffic to a specific app/process (later step)

Learning in progress.
