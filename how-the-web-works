# DNS in Detail 


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
