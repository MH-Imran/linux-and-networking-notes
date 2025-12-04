# Linux Basics for SOC Analysts

Here are some essential Linux commands I regularly use while learning cybersecurity:

##  File & Directory Navigation
- `ls` – list files
- `cd` – change directory
- `pwd` – show current directory
- `mkdir` – create a directory
- `rm` – remove a file
- `rm -r` – remove a folder

##  File Viewing
- `cat file` – show file content
- `less file` – view content page by page
- `head file` – first 10 lines
- `tail file` – last 10 lines
- `tail -f file` – follow log file in real time

##  Permissions
- `chmod` – change permissions
- `chown` – change owner
- `whoami` – current user
- `sudo su` – switch to root

##  Process & System Monitoring
- `ps aux` – list running processes
- `top` – live process view
- `htop` – improved process view (if installed)
- `df -h` – disk usage
- `free -h` – memory usage

##  Networking Commands
- `ifconfig` or `ip a` – show interfaces
- `ping <ip>` – test connectivity
- `netstat -tulnp` – show listening ports
- `ss -tulnp` – modern netstat
- `curl` – send HTTP requests
- `tcpdump` – packet capture

I will keep updating this file as I learn more.
