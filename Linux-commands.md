# Linux Command Practice Notes  

These are my personal notes from practicing Linux commands across my first 6 days of SOC training.  
I’m not trying to memorize everything — just to build comfort, understand patterns, and make Linux feel “normal” during investigations.

---

# 1. Networking & Connection Commands

## `ip a`
Shows all interfaces and their IP addresses.  
I use it to check if the machine has connectivity or if an interface is down.

## `ip r`
Prints the routing table.  
Helpful for seeing *how* my machine sends traffic (which gateway, which interface).

## `ss -tunap`
Lists active TCP/UDP connections and listening ports.  
Very useful for spotting suspicious services or unknown open ports.

## `ping <ip/domain>`
Tests basic connectivity.  
Simple, but still one of the best first steps when something “doesn’t work.”

## `traceroute <ip>`
Shows the hops packets take across the network.  
Good for understanding network paths or detecting routing issues.

---

# 2. Log & Event Investigation Commands  
*(Since Kali relies heavily on systemd, most logs come through `journalctl`.)*

## `journalctl -xe`
Shows recent logs with extended info.  
My go-to command when something breaks or behaves strangely.

## `journalctl | grep -i failed`
Finds failure-related entries.  
Great for spotting login failures, service startup problems, or permission issues.

## `journalctl | grep -i ssh`
Shows SSH activity — successful & failed logins, restarts, etc.

## `journalctl | grep -i login`
Helps track session starts and authentication events.

## `journalctl | grep -i error`
Useful when troubleshooting any system-level issue.

## `journalctl | grep -i session`
Shows user session opens/closes — important for T1078 + T1110 investigations.

## `journalctl | grep -i http`
Filters logs for HTTP-related entries (Apache/Nginx events).

## `journalctl | grep -i apache`
Anything related to Apache web server.

## `journalctl | grep -i nginx`
Anything related to NGINX.

## `ls /var/log`
List all traditional log files.  
Good for learning where different logs live in Ubuntu/Kali.

---

# 3. File, Directory & Permission Commands  
(Used heavily on 04–06 Dec)

## `ls -l`
Detailed listing showing permissions, owners, groups.

## `chmod <mode> <file>`
Changes file permissions.  
Examples I practiced:
```bash
chmod 600 file.txt
chmod 644 file.txt
chmod 700 directory/
```
_**Practice Range:** 01 Dec – 06 Dec 2025_
