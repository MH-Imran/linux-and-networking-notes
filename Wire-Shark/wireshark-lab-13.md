# Wireshark Lab 13 — “Name → IP → Connection” (DNS + TCP to 443)

---

## Why I did this lab
I’m trying to get comfortable with the “boring but important” traffic that happens every time I open a browser.

In my head, the flow is simple:

1) My PC asks DNS: “What IP is this domain?”  
2) DNS replies: “Here are the IP(s).”  
3) My PC starts a TCP connection to one of those IPs on **443** (HTTPS)

So I captured a small set of packets to prove that chain to myself.

---

## 1) DNS response (domain → records)

### What I captured
7 1.958305445 192.168.0.1 192.168.0.113 DNS 274 Standard query response 0x5a2c HTTPS chromium.dns.nextdns.io
CNAME steering.nextdns.io
HTTPS A 139.84.177.196 A 217.146.10.59
AAAA 2401:c080:3400:2f77:5400:5ff:fe08:2fac
AAAA 2a00:11c0:27:1::53


### What it means (in plain words)
- My router/DNS server (`192.168.0.1`) replied to my machine (`192.168.0.113`).
- The name I cared about was: `chromium.dns.nextdns.io`.
- DNS basically said:  
  “That name points to another name first (CNAME), and here are the IPs.”

**CNAME:**
- `chromium.dns.nextdns.io` → `steering.nextdns.io`  
  I read this as: “go through the steering name — it decides where to send you.”

**IP answers:**
- IPv4 (**A** records): `139.84.177.196`, `217.146.10.59`
- IPv6 (**AAAA** records): `2401:...`, `2a00:...`

### My takeaway
This is totally normal in real life:
- services return multiple IPs for reliability/load balancing,
- and you’ll often see you get both IPv4 and IPv6 options.

---

## 2) TCP handshake (starting the HTTPS connection)

### What I captured



### What it means (in plain words)
- My router/DNS server (`192.168.0.1`) replied to my machine (`192.168.0.113`).
- The name I cared about was: `chromium.dns.nextdns.io`.
- DNS basically said:  
  “That name points to another name first (CNAME), and here are the IPs.”

**CNAME:**
- `chromium.dns.nextdns.io` → `steering.nextdns.io`  
  I read this as: “go through the steering name — it decides where to send you.”

**IP answers:**
- IPv4 (**A** records): `139.84.177.196`, `217.146.10.59`
- IPv6 (**AAAA** records): `2401:...`, `2a00:...`

### My takeaway
This is totally normal in real life:
- services return multiple IPs for reliability/load balancing,
- and you’ll often see you get both IPv4 and IPv6 options.

---

## 2) TCP handshake (starting the HTTPS connection)

### What I captured


### What I understand from it
This is the classic **TCP 3-way handshake**:
- **SYN**: I’m asking the server, “Can we talk?”
- **SYN/ACK**: Server replies, “Yes, I’m listening.”
- **ACK**: I confirm, “Cool — connection is established.”

Because it’s **port 443**, the next step *should* be TLS (encryption) right after this.

### Something I noticed (and I’m not hiding it)
The source ports don’t match perfectly across these lines (`54262`, `54234`, `54246`).  
That usually means I accidentally grabbed handshake packets from more than one connection attempt.

The idea is still correct — but if I want a “perfect screenshot” example next time, I should:
- right-click one of these and **Follow → TCP Stream**, or
- filter using `tcp.stream == X` so I’m looking at one single connection end-to-end.

---

## Quick SOC-style notes (how I’d describe this to myself)
- DNS response returned normal records (CNAME + multiple IPs).
- Then I saw a TCP handshake to one of the returned IPv4 addresses on 443.
- This looks like normal browser behavior.

If it was suspicious, I’d expect things like:
- lots of SYN retransmissions,
- repeated connections to many IPs fast,
- weird domains,
- or unusual ports.

---

## Next improvement (Lab 14 plan)
- Capture the **TLS Client Hello** right after the handshake.
- Check if **SNI** matches the DNS name I looked up.
- Keep the whole story inside one `tcp.stream` so the ports match cleanly.
