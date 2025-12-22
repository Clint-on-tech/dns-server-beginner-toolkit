DNS Zones Explained (Forward & Reverse)

Objective

This document explains DNS zones in simple, practical terms, based on the
actual BIND9 configuration implemented in this project.

By the end of this section, a beginner should understand:
- What a DNS zone is
- The difference between forward and reverse zones
- Why both are required in real networks
- How the zones relate to our configured IP addresses

What Is a DNS Zone?

A DNS zone is a portion of the DNS namespace that a DNS server is responsible for managing.

In simple terms:
- A zone tells the DNS server which names it can answer for
- Each zone has its own configuration and data files
- Zones are loaded and served by BIND9

There are two main types we configured:
1. Forward Lookup Zone
2. Reverse Lookup Zone

Forward Lookup Zone (Name → IP)

A forward lookup zone resolves:
hostname → IP address
Example;server1.mycompany.local → 192.168.56.20


This is the most common DNS operation and is what happens when:
- A user types a hostname
- An application connects to a service
- A browser loads a website

Forward Zone Used in This Project

Zone name:
mycompany.local

Zone file:

/etc/bind/zones/mycompany.local.db

Example: server1.mycompany.local → 192.168.56.20

Reverse Lookup Zone (IP → Name)

A reverse lookup zone resolves:
IP address → hostname

Example: 192.168.56.20 → server1.mycompany.local



Reverse DNS is commonly used by:
- Email servers - anti-spam checks
- Security logging
- Authentication systems
- Network monitoring tools

NOTE: 
> Forward DNS tells us where to go.  
> Reverse DNS tells us who connected.

Reverse Zone Used in This Project

Our network:
192.168.56.0/24

Reverse DNS format:
56.168.192.in-addr.arpa

Reverse zone file:
/etc/bind/zones/db.192.168.56

Why the IP Is Reversed

DNS reverse zones follow a hierarchical structure.

Given:
192.168.56.20

It becomes:
20.56.168.192.in-addr.arpa

How This Applies to Our Lab Setup
Configured IP Address
192.168.56.20

Forward Mapping
server1.mycompany.local → 192.168.56.20

Reverse Mapping
192.168.56.20 → server1.mycompany.local


This confirms:
- Correct DNS design
- Enterprise-ready configuration
- Proper internal name resolution

Verification Commands Used

Forward Lookup Testing
dig @127.0.0.1 server1.mycompany.local
nslookup server1.mycompany.local 127.0.0.1

Reverse Lookup Testing
dig @127.0.0.1 -x 192.168.56.20
nslookup 192.168.56.20 127.0.0.1

Expected result:
Forward lookup returns the correct IP
Reverse lookup returns the correct hostname







