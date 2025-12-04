# DNS in Detail - TryHackMe Notes


These are my personal notes from the “DNS in Detail” room in TryHackMe’s How The Web Works module. I tried to simplify everything so I actually understand how DNS works behind the scenes, not just the definitions.



### What I Learned

- DNS is basically the internet’s phonebook. It takes human-friendly names like `google.com` and translates them into IP addresses.
- Every DNS lookup doesn’t happen instantly — it goes through a specific chain:  
  **browser cache → OS cache → router → recursive resolver → authoritative servers**
- Recursive DNS servers do most of the work. They go out and ask other DNS servers if they don’t have the answer.
- Authoritative DNS servers hold the final, real answers for domains.
- I didn’t know there were so many record types before:  
  - **A** = IPv4 address  
  - **AAAA** = IPv6  
  - **CNAME** = alias  
  - **MX** = mail servers  
  - **TXT** = random text (used heavily in security)  
- DNS caching is super important for performance. Without caching, every website load would take forever.



### Why This Matters for SOC Work

- Many phishing domains show up in logs as strange DNS requests.  
- Malware often uses DNS to contact command-and-control servers.  
- DNS tunneling is a real attack technique (data exfiltration via DNS).  
- As a SOC analyst, DNS logs can reveal malicious domains early.



### Takeaways

- DNS seems simple from the outside but has a lot of moving parts.
- The caching layers make everything faster but can also hide recent changes.
- Understanding DNS helps me read logs and alerts more confidently.

_**Completed:** 02 Dec 2025_




---







# HTTP in Detail - TryHackMe Notes


These are my personal notes from the “HTTP in Detail” room in the How the Web Works module. I tried to write this in simple language so I can remember how HTTP actually works when looking at logs or alerts later.



### What I Learned

- HTTP is a request–response protocol. The browser sends a request and the server sends a response.
- An HTTP message has two main parts:
  - **Headers** (information about the request/response)
  - **Body** (the actual content, if any)
- HTTP methods make more sense now:
  - **GET** – retrieve data  
  - **POST** – send data  
  - **PUT** – update something  
  - **DELETE** – remove something  
- I finally understand why status codes are important:
  - `200` → OK  
  - `301/302` → redirects  
  - `400` → bad request  
  - `403` → forbidden  
  - `404` → not found  
  - `500` → server error  
- What surprised me is how much information is in the headers. Things like:
  - User-Agent  
  - Content-Type  
  - Authorization  
  - Cookies  
  - Host  
  All of this can show attacker behavior in SOC logs.



### Why This Matters for SOC Work

- Many attacks come through HTTP: SQL injection, XSS, brute force, directory traversal, etc.
- Malicious requests often have weird headers or unusual methods.
- Understanding how HTTP works makes it easier to analyze alerts from:
  - WAF (Web Application Firewall)
  - Proxy logs
  - SIEM (Elastic, Splunk, Security Onion)
- Things like 404 bursts, 500 spikes, or strange user-agents can indicate probes or attacks.


### Takeaways

- HTTP is simple but extremely powerful.
- The structure of requests and responses is important for detecting abnormal activity.
- Knowing the normal behavior helps spot the abnormal behavior later.

_**Completed:** 04 Dec 2025_
