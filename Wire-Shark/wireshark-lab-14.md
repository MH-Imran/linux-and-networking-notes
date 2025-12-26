# Wireshark Lab 14 — DNS answer → TCP handshake → TLS Client Hello (SNI)

Date: 2025-12-26  
Goal: capture one clean “browser connection story” from start to encryption.

---

## Why I did this
Last time I could see DNS and the handshake, but I wanted to go one step further and catch the exact moment the browser says:

“Hi server — I want to talk securely, and this is the domain I’m trying to reach.”

That’s the TLS **Client Hello**, and it’s a big deal in SOC work because it can show the **SNI** (the hostname the client claims it wants).

---

## 1) DNS response — domain to IP(s)

### Packet captured
7 1.958305445 192.168.0.1 192.168.0.113 DNS 274 Standard query response 0x5a2c HTTPS chromium.dns.nextdns.io
CNAME steering.nextdns.io
HTTPS A 139.84.177.196 A 217.146.10.59
AAAA 2401:c080:3400:2f77:5400:5ff:fe08:2fac
AAAA 2a00:11c0:27:1::53


### What I understand from this
- DNS reply came from my router (`192.168.0.1`) back to my machine (`192.168.0.113`).
- The domain `chromium.dns.nextdns.io` is linked to a CNAME: `steering.nextdns.io`.
- I got multiple possible IPs:
  - IPv4: `139.84.177.196` and `217.146.10.59`
  - IPv6: two AAAA addresses

**My takeaway:** the service is giving multiple routes, and my device will pick one to connect to.

---

## 2) TCP handshake — setting up the connection on 443

### Packets captured
12 1.986307410 192.168.0.113 → 217.146.10.59 TCP 54262 → 443 [SYN]
13 2.062986310 217.146.10.59 → 192.168.0.113 TCP 443 → 54234 [SYN, ACK]
18 2.090105112 192.168.0.113 → 217.146.10.59 TCP 54246 → 443 [ACK]


### What I think is happening
This is the normal **3-way handshake**:
- **SYN** (I request a connection)
- **SYN/ACK** (server accepts)
- **ACK** (I confirm)

After this, the connection is ready for encrypted traffic.

### Small note for myself
The ports don’t match perfectly across the lines, which usually means I pulled packets from nearby connections, not a single perfect stream.
The overall flow is still correct, but next time I’ll lock it down by filtering one stream with:
- `tcp.stream == X` (Wireshark), or
- following the conversation from one chosen source port.

---

## 3) TLS Client Hello — the “secure chat request” + SNI

### Packet captured

28 2.194280287 192.168.0.113 → 217.146.10.59 TLSv1.3 Client Hello (SNI=chromium.dns.nextdns.io)


### Why this packet matters
- This is the point where the client (my machine) starts the encrypted negotiation.
- Even though the content will be encrypted later, the **Client Hello** often still shows the **SNI**.
- Here the SNI matches the domain I looked up in DNS:  
  `chromium.dns.nextdns.io`

**That’s exactly what I wanted to confirm:** DNS name → connection → TLS hello carrying the same hostname.

---

## SOC-style interpretation (how I’d explain it in a ticket)
- DNS returned normal records (CNAME + multiple A/AAAA).
- The client then connected to one of the A-record IPs on 443.
- A TLSv1.3 Client Hello followed with SNI set to the expected hostname.

This looks like normal browser / DNS resolver traffic.

---

## What I want to improve next (Lab 15 ideas)
- Capture the rest of the TLS handshake (Server Hello, certificates, etc.)
- Confirm which IP my system prefers when IPv6 is available (AAAA vs A).
- Repeat the same test for a different domain and compare SNI + destination IP behavior.
