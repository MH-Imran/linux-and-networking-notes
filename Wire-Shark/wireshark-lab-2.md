# Wireshark Lab 2 — Getting Comfortable with Filters

## Why I did this lab
After my first Wireshark session, I realized that filters are everything.  
Without filters, Wireshark feels chaotic. With filters, it becomes manageable.

This lab was focused on *using filters with intention*, not exploring randomly.

---

## Traffic generation
I captured traffic while:
- Browsing a small number of websites
- Keeping activity simple and controlled

This helped me focus on understanding patterns instead of noise.

---

## DNS focus
Filter used:


What I noticed:
- Multiple DNS queries for a single website
- Some responses returned multiple IPs
- DNS traffic happens very frequently, even in normal browsing

This made it clear why DNS logs are important in SOC investigations.

---

## HTTP focus
Filter used:


Observations:
- Clear GET requests when loading pages
- Requests included paths and hostnames
- HTTP traffic is readable compared to encrypted HTTPS

It helped me understand why analysts care about unencrypted traffic when it exists.

---

## TCP connections
Filter used:


What stood out:
- Many connections start even for simple actions
- Each website triggers multiple TCP sessions

This showed me how “normal” traffic can already look busy.

---

## What confused me (and that’s okay)
- I didn’t understand every field in the packet details
- Some packets appeared repeatedly
- There was more background traffic than I expected

Instead of getting stuck, I focused only on protocol, source, and destination.

---

## Key takeaway
Wireshark becomes useful when I decide *what I’m looking for first*.  
This lab helped me slow down and focus instead of trying to understand everything at once.
