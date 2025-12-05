# Linux Command Practice Notes

These are my personal notes from practicing Linux commands between **01 Dec – 04 Dec 2025**.  
I wrote them in a simple way so I can actually remember why each command matters, not just what it does.  
The goal wasn’t to memorize everything — just to build hands-on comfort and understand how Linux behaves.

---

# 1. Networking Commands

## `ip a`
Shows all network interfaces and their IP addresses.  
I use this to quickly see my machine’s current IP or check if an interface is up.

## `ip r`
Displays the routing table.  
This helped me understand how traffic leaves my machine and which gateway it uses.

## `ss -tunap`
Shows active network connections and listening ports.  
Useful for spotting which services are open or if something suspicious is listening.

---

# 2. Log Investigation Commands

*(Since I’m using Kali, most logs live in `journalctl` instead of `/var/log/auth.log`.)*

## `journalctl -xe`
Shows recent system logs with detailed error messages.  
This helps me understand what went wrong or what the system is reacting to.

## `journalctl | grep -i failed`
Searches logs for anything related to “failed.”  
Especially helpful for failed login attempts or service errors.

## `journalctl | grep -i ssh`
Shows SSH-related events.  
Good for spotting when someone logs in, fails to log in, or restarts the SSH service.

---

# 3. Process & System Monitoring

## `ps aux --sort=-%cpu`
Lists all running processes and sorts them by CPU usage.  
Helps me notice if something is eating resources unexpectedly.

## `top`
Real-time system monitor for CPU, RAM, and processes.  
Good for watching behavior as it happens.

## `htop` (when installed)
A prettier, more readable version of `top`.

## `du -sh /var/*`
Shows which folders consume the most space.  
Useful for understanding storage usage.

---

# 4. User & Access Commands (04 Dec Focus)

## `cat /etc/passwd`
Shows all user accounts on the system.  
Helped me distinguish system accounts from normal ones.

## `cat /etc/group`
Lists all groups and their members.  
Good for understanding permissions.

## `id`
Shows the current user’s UID, GID, and group memberships.

## `who`
Shows who is currently logged in.

## `last`
Displays recent login history, reboots, and sessions.

## `lastlog`
Shows last login time for each user.  
Very useful for spotting accounts that shouldn't be used.

## `sudo -l`
Lists what commands the current user is allowed to run with sudo.  
Important for privilege escalation awareness.

---

# 5. My Takeaways So Far

- Linux isn’t about memorizing commands — it’s about knowing **why** and **when** to use them.
- `journalctl` is extremely powerful once you get used to filtering logs.
- Understanding users, groups, and login patterns makes it much easier to spot abnormal behavior.
- Networking commands (`ip`, `ss`) are essential — almost every investigation touches networking in some way.
- Doing this every day for a few minutes is already helping me build muscle memory.

---

These notes will grow as I continue practicing Linux for SOC work.  
I’m starting to appreciate how much information Linux gives you — if you know where to look.
