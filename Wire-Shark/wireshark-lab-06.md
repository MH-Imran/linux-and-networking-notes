# Wireshark Lab 6 — Two Packets That Actually Made Sense

Today I did a short capture and forced myself to *not* get distracted by the “packet waterfall.”  
Instead of trying to understand everything (which always makes me stuck), I picked just two packets that looked meaningful and tried to explain them in plain words.

My target was simple:
- one DNS packet
- one TCP SYN packet

---

## Packet 1: DNS query (my machine asking “where is this domain?”)

What I captured:
- **From:** `192.168.0.113` (my device)
- **To:** `192.168.0.1` (my router / local DNS)
- **Protocol:** DNS (UDP port 53)
- **Query:** `HTTPS chromium.dns.nextdns.io`

How I understand it:
My device asked the router something like:
> “Hey, can you resolve this domain for me?”

The domain name is interesting because it looks Chromium-related and connected to NextDNS. My guess is it’s something about browser DNS settings (maybe DoH), but I’m not fully sure yet. I’m just recording what I saw.

Why I’m noting this (SOC mindset):
DNS is often the “first breadcrumb.” Even when everything later becomes encrypted, DNS can still show where a device *wanted* to go.

---

## Packet 2: TCP SYN (my machine trying to start a connection)

What I captured:
- **From:** `192.168.0.113` (my device)
- **To:** `192.168.0.102` (another device in my LAN)
- **TCP flags:** `[SYN]` (connection start)
- **Destination port:** `8009`
- **Source port:** `33590` (random high port)

How I understand it:
This was basically my computer saying:
> “Can I open a connection with you?”

The part that made me pause was the destination port **8009**. It’s not like 80/443 where I instantly think “web browsing.” It might still be normal (maybe a local service, device discovery, or something on my network), but I don’t know yet.

I also noticed it’s LAN-to-LAN traffic, not going out to the internet. That’s a useful detail because it changes how suspicious it feels.

---

## What I *think* this means (without overthinking)
- The DNS packet looks like normal “background browser” stuff.
- The SYN packet shows my device trying to talk to another local device on port 8009.
- I don’t have enough context yet to call anything suspicious — but it’s a good habit to note:
  - who initiated it
  - where it went
  - what port it used

---

## What I learned today
I’m starting to realize Wireshark is not about understanding every packet. It’s about asking small questions and answering them:

- What domain did my machine query?
- Who did my machine try to connect to?
- Is it local or internet?
- Is the port normal or unusual?

Focusing like this feels way more manageable than scrolling forever.

---

## Next time (what I want to check)
- Figure out what device `192.168.0.102` actually is (phone? router service? another VM?)
- See if there was a **SYN/ACK** response (did the connection complete or fail?)
- Do a capture while doing one controlled action (open one site, stop capture)
