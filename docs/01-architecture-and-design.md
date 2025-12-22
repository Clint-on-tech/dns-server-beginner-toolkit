DNS Server Architecture & Design (BIND9)

1. Overview

This document explains the architectural design of the DNS server implemented in this project.
The goal is to show how BIND9 fits into a small enterprise LAN, how it interacts with other services,
and why each component is necessary.

2. Network Context

The DNS server was deployed in a virtualized lab environment using:

- Host OS: Windows 10
- Hypervisor: Oracle VirtualBox
- Guest OS: Kali Linux
- DNS Software: BIND9

The DNS server represents an internal enterprise DNS server, not a public Internet-facing resolver.

3. Role of the DNS Server (BIND9)

The BIND9 server performs the following roles:

- Resolves internal hostnames (e.g. `server1.mycompany.local`)
- Handles reverse DNS lookups for internal IP addresses
- Acts as the authoritative DNS server for the local domain
- Forwards external DNS queries to upstream DNS servers -optional 

This design reflects how DNS is commonly deployed in real organizations.

4. IP Addressing Design

The project uses a private IP address range:

- Network: `192.168.56.0/24`
- DNS Server IP: `192.168.56.10`
- Example Host IP: `192.168.56.20`

Why private IPs:
- Suitable for internal networks
- Not routable on the public Internet
- Common in enterprise and lab environments

5. Relationship Between DNS and DHCP

Although DHCP was not fully implemented in this lab, its role is critical in real deployments.

How they work together:

1. DHCP assigns IP addresses to client machines
2. DHCP tells clients which DNS server to use
3. Clients send DNS queries to BIND9
4. BIND9 resolves names using configured zones

Important clarification:
- DHCP has its own IP address because it is also a server
- DHCP does not “borrow” IPs — it assigns them from a pool

6. Role of Network Devices

Router / Gateway
- Has an IP address
- Routes traffic between networks
- Often provides NAT and firewalling

Switch
- May have a management IP
- Primarily forwards traffic at Layer 2
- Does not assign IP addresses

These devices enable DNS traffic to flow correctly inside the LAN.

7. Why This Architecture Was Chosen

This design was selected because it:

- Matches real-world enterprise setups
- Is simple enough for learning
- Demonstrates forward and reverse DNS clearly
- Scales easily for future expansion

8. Learning Outcome

From this architecture, the following concepts were learned:

- DNS is a core infrastructure service
- DNS does not work in isolation
- IP addressing, routing, and services are interconnected
- Proper planning prevents configuration errors

This foundation supports future projects such as:
- Active Directory integration
- Mail servers
- Web hosting
- Enterprise security monitoring
