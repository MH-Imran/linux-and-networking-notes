# Wireshark Lab 9 — A Clean TCP Handshake (SYN → SYN/ACK → ACK)

Today I wanted to stop guessing and finally capture a **complete TCP three-way handshake** on purpose.  
I’ve seen “SYN, SYN/ACK, ACK” in books a lot, but seeing it in my own capture made it click.

This was a connection from my device to a server over **HTTPS (port 443)**.

---

## The three packets (in order)

### 1) SYN — “Can we talk?”
- **Time:** 61.309s  
- **From:** `192.168.0.113`  
- **To:** `139.84.177.196`  
- **Ports:** `49480 → 443`  
- **Flags:** SYN  

This is my machine starting the connection.  
The destination is port **443**, so it’s likely normal web/HTTPS traffic.

---

### 2) SYN/ACK — “Yes, I’m here”
- **Time:** 61.421s  
- **From:** `139.84.177.196`  
- **To:** `192.168.0.113`  
- **Ports:** `443 → 49480`  
- **Flags:** SYN, ACK  

This is the server responding and acknowledging my request.  
For me, this is the “green light” packet — it means:
- the server is reachable
- the port is open
- the server is willing to establish a session

---

### 3) ACK — “Great, connection is established”
- **Time:** 61.421s  
- **From:** `192.168.0.113`  
- **To:** `139.84.177.196`  
- **Ports:** `49480 → 443`  
- **Flags:** ACK  

This final ACK completes the handshake.  
At this point the TCP connection is officially set up, and the browser/app can start the encrypted HTTPS part.

---

## Why I’m saving this (SOC mindset)
This capture is a good example of **normal network behavior**:
- no retransmissions
- no RST packets
- no ICMP errors

It’s useful as a baseline. When something looks “weird” later (SYN retries, RST resets, timeouts), I can compare it to a clean handshake like this.

---

## What I learned today
- A successful TCP connection really does look like **SYN → SYN/ACK → ACK**
- The **IP + ports** are what tie the packets together (easy to get wrong if I don’t check)
- A SYN packet by itself doesn’t mean anything bad — the full pattern matters

---

## Next thing I want to try
- Follow the traffic after the handshake and see what comes next (TLS/HTTPS)
- Compare this with a failed connection:
  - SYN retransmissions (no reply)
  - SYN then RST (connection refused)
