# Wireshark Lab — TCP Handshake Check (SYN / SYN-ACK / ACK)

## What I was trying to answer
For my **Network Focus** task, I wanted to confirm one simple thing:

**Is the TCP handshake healthy? Any retransmissions or RST?**

I kept it narrow on purpose so I don’t get lost in the full capture.

---

## Evidence I captured (3 packets)
> **Sanitized IPs for public notes**
- Client: `10.12.34.56:45262`
- Server: `203.0.113.45:443`

These three frames show the classic 3-way handshake:

- **Frame 124** — Client → Server: **SYN**
- **Frame 130** — Server → Client: **SYN, ACK**
- **Frame 131** — Client → Server: **ACK**

---

## What it means (my understanding)
This is a normal handshake:

1) Client says: “Can we talk?” (**SYN**)  
2) Server replies: “Yes, and I heard you.” (**SYN-ACK**)  
3) Client confirms: “Great, connection is established.” (**ACK**)

✅ **Handshake completed successfully.**

---

## Retransmission / RST check (based on what I captured)
From these three packets only:
- ✅ No retransmissions are visible here  
- ✅ No RST packets are visible here

So the connection looks fine at the handshake stage.

Small note from my own learning: I used to assume “handshake OK = everything OK,” but that’s not always true. Problems often show up **after** this (during TLS or data transfer), so I still need to check the rest of the stream.

---

## Filters I used (or would use next)
**Retransmissions:**
- `tcp.analysis.retransmission`
- `tcp.analysis.fast_retransmission`

**Resets (RST):**
- `tcp.flags.reset == 1`

**This exact flow (sanitized):**
- `ip.addr == 10.12.34.56 && ip.addr == 203.0.113.45 && tcp.port == 443`

---

## 3-line conclusion (SOC style)
 TCP handshake completed normally (SYN → SYN/ACK → ACK).  
 No retransmission or RST observed in the handshake packets shown.  
 Next step: verify if the session stays stable after handshake (TLS/data phase).

---

## Next checks (what I’ll do if I had more time)
- Follow the TCP stream to see if data exchange continues normally
- Look for retransmissions/RST later in the conversation (not just handshake)
- Check timing spikes (big gaps) that might hint at network issues
