# Wireshark Lab 8 — SYN to 443, a Surprise RST, and ICMP “Port Unreachable”

Today I kept the capture short and tried to focus on three things only:
- TCP connection starts (**SYN**)
- what happens when a connection gets interrupted (**RST**)
- and those ICMP messages that show up and confuse me (**Port unreachable**)

I’m writing this in my own words so I don’t forget what I saw.

---

## What I captured (the exact packets I picked)

### 1) ICMP: Destination unreachable (Port unreachable)
I saw this twice:

- **Packet 7 (0.057s)**  
  `192.168.0.113 → 192.168.0.1`  
  ICMP: **Destination unreachable (Port unreachable)**

- **Packet 27411 (60.810s)**  
  `192.168.0.113 → 192.168.0.1`  
  ICMP: **Destination unreachable (Port unreachable)**

**What I think this means (simple):**  
Some traffic hit a port where nothing was listening, and the system responded with “port unreachable.”

The weird part (for me) is that it’s going to my router (`192.168.0.1`). My guess is:
- something on my machine tried to talk to the router on a closed port (maybe UDP),
- and ICMP is basically the router/my system saying “no service there.”

I’m not calling it malicious — I’m just noting it because it’s a useful clue when troubleshooting.

---

### 2) TCP SYN: starting connections to HTTPS (port 443)
I captured these SYN packets:

- **Packet 4 (0.035s)**  
  `192.168.0.113 → 139.84.177.196`  
  `42730 → 443 [SYN]`

- **Packet 83 (0.565s)**  
  `192.168.0.113 → 108.157.188.72`  
  `35350 → 443 [SYN]`

**How I understand it:**  
My computer was trying to start HTTPS connections (443 is the normal HTTPS port).  
The source ports (`42730`, `35350`) are random high ports, which is normal for a client.

So far, this part looks like normal browsing/background traffic.

---

### 3) TCP RST: connection got reset
This one was interesting:

- **Packet 11 (0.101s)**  
  `192.168.0.113 → 139.84.177.196`  
  `42740 → 443 [RST]`

**What I think it means:**  
RST usually means “reset / stop this connection immediately.”

So either:
- my machine decided to drop the connection,
- or something about the session setup was wrong,
- or the app opened a connection and then immediately killed it.

One detail I noticed: the SYN to `139.84.177.196` used source port **42730**, but the RST line shows **42740** (different port).  
So this RST might not be the exact same connection as Packet 4 — it could be another attempt happening right around the same time.

This is exactly why Wireshark can be confusing if I assume everything is linked without checking the ports.

---

## My “SOC thinking” takeaway
What I’m training myself to do is this:

- **SYN** tells me: “a connection attempt started”
- **RST** tells me: “a connection was killed/reset”
- **ICMP port unreachable** tells me: “something tried a port that wasn’t open”

None of these are automatically “bad,” but they’re useful signals.

---

## What I want to check next time
- Follow the exact TCP stream (right click → Follow → TCP stream) for one connection attempt
- Confirm if I ever see a **SYN/ACK** back from the destination
- When I see RST again, confirm if it’s the same flow (same src/dst IP + same ports)
- For ICMP, check what packet happened right before it to understand what triggered it

Learning in progress, but I’m getting better at not panicking when I see “weird” packets.
