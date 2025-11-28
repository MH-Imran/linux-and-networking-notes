# Networking Basics – My Personal Notes

These are some networking concepts and commands I’m practicing as part of my SOC learning journey.  
I’m keeping these notes simple so I can refer back to them quickly.

---

## 🌐 Concepts I'm Learning

### **IP Address**
A unique number that identifies a device on a network.

### **MAC Address**
The physical address assigned to a network interface (like WiFi or Ethernet).

### **DNS**
Turns domain names like “google.com” into actual IP addresses.

### **DHCP**
A service that automatically gives devices their IP address.

### **Subnet**
A smaller section of a larger network. Helps organize and segment traffic.

### **Gateway**
Usually the router. It's the device your traffic passes through to reach the internet.

### **Ports**
Numbers that represent different services (80 = web traffic, 22 = SSH, etc.)

### **Protocols**
Rules used for communication (TCP, UDP, ICMP, etc.)

---

## 🛠 Commands I’m Practicing

### **Check network settings**
- `ip a`  
- `ifconfig` (older systems)

### **Test connectivity**
- `ping example.com`
- `traceroute google.com`
- `tracepath google.com`

### **Check open/listening ports**
- `ss -tulnp`
- `netstat -tulnp` (older systems)

### **DNS lookups**
- `nslookup google.com`
- `dig google.com`

### **Routing**
- `ip route`
- `route -n`

---

## 🚨 Why this matters for SOC
I’m learning these because networking shows up everywhere in security alerts.  
Understanding IPs, ports, and basic traffic flow helps me:

- spot unusual connections  
- follow attacker movement  
- understand logs better  
- troubleshoot quickly  

I’ll keep updating this file as I learn more.
