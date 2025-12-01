# What Is a Network?


This is my first room from the “Network Fundamentals” module. I kept my notes simple so I can come back to them quickly.



### What I Learned
- A network is basically a way for devices to talk to each other.
- Devices can share information, resources, or services through connections.
- Networks can be small (home network) or huge (the internet).
- Every device needs an identifier (an IP address) to communicate.


### Key Points (In My Own Words)
- The internet is just a giant network of many smaller networks.
- Networks help computers share files, send messages, and access websites.
- Without networks, security monitoring wouldn’t exist — no logs, no traffic, nothing to analyze.



### Why This Matters for SOC Work
Understanding what a network actually is makes it easier to understand:
- how attackers move from one device to another  
- why logs are generated  
- how traffic flows  
- what “normal” communication looks like  

All of this becomes important later in detection and investigation.



Very basic room, but it sets the foundation for everything that comes next.
 
  _**Completed**: Nov 28, 2025_







---








# Intro to LAN 


### What I Learned
- How devices communicate inside a local network
- Role of switches and broadcast domains
- MAC addressing basics
- Difference between unicast, broadcast, and multicast
- How local network traffic flows
    
_**Completed**: Nov 29, 2025_



---




# OSI Model 



I just finished the OSI Model room on TryHackMe, and these are basically the notes I took while trying to make sense of everything. I’m still wrapping my head around some parts, but this is what clicked for me so far:



### What I Understand So Far

- The OSI Model is basically a **7-step explanation** of how data moves from one device to another. I never really thought about how much happens behind the scenes until now.
- The lower layers (1–4) seem to deal more with “how stuff actually travels,” while the upper ones (5–7) feel more like “how apps and humans interact.” Not sure if that’s a perfect way to say it, but it helps me remember.
- Data goes **down the layers** when sending and **up the layers** when receiving. This simple idea made a lot of things suddenly make more sense.



### Layer Thoughts (My Own Words)

- **Physical Layer:** literally the wires and signals. Easy enough.
- **Data Link:** MAC addresses, frames… still need to get more comfortable with this, but I get the basic idea.
- **Network Layer:** IP addresses. This one I understand better because I’ve seen IPs a million times.
- **Transport Layer:** ports, TCP/UDP — feels very important for SOC stuff.
- **Session, Presentation, Application:** these three are still a bit fuzzy, but I know they’re more about how software and services talk to each other.



### My Real Takeaways

- Thinking of it as a “ladder” really helped.
- When I see logs in the future (like in a SIEM), I’ll probably be able to recognize which layer the problem is happening in.
- I haven’t memorized all the layers perfectly, but I honestly understand them better than before.
- This room didn’t feel difficult, but it exposed how much happens during normal network communication.



### What I Still Need to Review

- Difference between frames vs packets vs segments
- The three upper layers (5, 6, 7) — I understand them, but not confidently
- How the OSI layers map to real protocols (e.g., where HTTPS sits, where ARP sits, etc.)

_**Completed:** Nov 29, 2025_




---






# Packets & Frames 


These are my personal notes from finishing the Packets & Frames room on TryHackMe. I wrote them mainly to remind myself how data actually moves across a network.



### What I Learned

- Data doesn't just “travel”; it’s broken into **frames** (Layer 2) and **packets** (Layer 3).
- A **frame** uses MAC addresses and is used inside the local network.
- A **packet** uses IP addresses and can travel across networks.
- Frames stay within a LAN, but packets can move between networks, through routers.
- The headers contain all the important details like source/destination MAC, IP, protocol, etc.
- Encapsulation basically wraps data in different layers depending on where it’s going.
- Understanding packets and frames helps a lot when reading logs or looking at alerts.



### Takeaways

- Frames = local network communication  
- Packets = bigger network communication  
- Seeing the difference helps me understand why certain attacks or alerts show up the way they do.
- This also helps prepare for deeper packet analysis in the future.


_**Completed:** Nov 30, 2025_








---









# Extending Your Network


These are my own notes from the “Extending Your Network” room in the Network Fundamentals module. I kept them simple because I'm trying to understand how networks scale in real life.



### What I Learned
- As networks grow, you can’t keep everything inside one big LAN. It becomes messy and noisy, so you start splitting things into smaller parts.
- Switches help expand the network but still keep everything inside the same broadcast domain.
- Routers connect different networks and make decisions using IP addresses.  
- VLANs were confusing before, but now they make sense: they’re basically “virtual sections” of the same physical switch.
- Collision domains vs broadcast domains finally clicked for me.


### Firewall 
- Firewalls sit between networks to block or allow traffic.  
- It’s basically a security checkpoint between segments or between the internal network and the internet.
- They work mostly at **Layer 3 and Layer 4** (IP + ports), which connects perfectly with what I learned about routers and segmentation.
- Without firewalls, “extending the network” would also mean “extending attack surface,” so segmentation alone isn’t enough.
- Routers decide *where* traffic goes; firewalls decide *whether it should go* at all.


### Takeaways
- Big networks need structure, not just more cables.
- Routers = layer 3 decisions (IP-based).  
- Switches = layer 2 organization (MAC-based).  
- VLANs = keep networks tidy without extra hardware.  
- Firewalls = control and protect the traffic between those segments.

This room helped me understand how networks grow without turning into complete chaos — and why security has to grow with it.


_**Completed:** Dec 01, 2025_
