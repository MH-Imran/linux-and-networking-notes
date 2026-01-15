# Wireshark Lab 16 — TCP Handshake + TLS SNI (ClientHello)

## Goal
Practice two things from the same capture:
1) Verify the TCP handshake is normal [no obvious RST in the handshake]
2) Extract a useful TLS artifact: **SNI [Server Name Indication]** from the TLS clienthello

---

## Evidence I collected (sanitized)
> I am using sanitized IP addresses here because the my original capture contains my original local IP addresses.

- Client (sanitized): `10.12.34.56`
- Server A (handshake server, sanitized): `198.51.100.77` (TCP/443)
- Server B (TLS server, sanitized): `203.0.113.90` (TLS)
- Observed SNI: `www.wikipedia.org`

---

## 1) TCP 3-way handshake (HTTPS / 443)

### Packets (handshake)
- **Frame 37** — Client → Server: `48752 → 443` **[SYN]**
- **Frame 39** — Server → Client: `443 → 48752` **[SYN, ACK]**
- **Frame 40** — Client → Server: `48752 → 443` **[ACK]**

### What it means
This is a normal TCP handshake:
- client requests a connection (SYN)
- server accepts (SYN/ACK)
- client confirms (ACK)

 Conclusion: **TCP handshake completed successfully.**  
From these handshake packets, I don’t see retransmission or reset behavior.

---

## 2) TLS evidence (ClientHello with SNI)

### Packet (TLS)
- **Frame 4829** — TLSv1.3 **ClientHello**
  - **SNI = `www.wikipedia.org`**

### Why SNI matters (SOC mindset)
Even when the traffic is encrypted (HTTPS), SNI can tell you **which hostname** the client is trying to reach.
That’s useful for:
- threat hunting (odd/rare domains)
- confirming user browsing activity during an investigation
- detecting suspicious beaconing destinations (when combined with timing + endpoint/process info)

✅ Conclusion: TLS handshake setup includes **SNI for `www.wikipedia.org`**.

---

## Wireshark filters I used / would use
**Find TLS SNI quickly:**
- `tls.handshake.extensions_server_name`

**Show only SYN / SYN-ACK / ACK:**
- `tcp.flags.syn==1 || tcp.flags.ack==1`

**Check for resets:**
- `tcp.flags.reset==1`

**Check retransmissions:**
- `tcp.analysis.retransmission`
- `tcp.analysis.fast_retransmission`

---

## 3-line summary (SOC style)
 TCP 3-way handshake to port 443 completed normally [SYN → SYN/ACK → ACK].  
 TLS ClientHello observed with SNI = `www.wikipedia.org` [TLSv1.3].  
 Next step: check if any retransmissions/RST happen later in the stream [not just at handshake].
