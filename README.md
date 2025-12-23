Title: Prompt-Powered Kickstart: Building a Beginner’s Toolkit for DNS Server Installation and Configuration (BIND9)
1.	Objective
- Understand DNS fundamentals and zone concepts
- Install and manage BIND9 on Kali Linux
- Configure forward and reverse lookup zones
- Validate DNS configurations using BIND utilities
- Test name resolution using dig and nslookup
- Document errors, fixes, and learning outcomes
Technology Chosen: DNS Server (BIND9)  
Why This Technology
o	DNS is a core component of all networks. Understanding how DNS works, how to install it, and how to integrate it into a small enterprise LAN is a foundational skill for network and systems administrators.
End Goal: 
o	To install, configure, and test a working DNS server using BIND9 on Kali Linux, and demonstrate how it integrates into a small enterprise network.
2.	Quick Summary of the Technology
o	The Domain Name System (DNS) is a hierarchical naming system used to translate human-readable domain names into IP addresses.
Where it is used:
•	Enterprise networks
•	Internet infrastructure
•	Local and private networks
Real-world example:
o When a user types `www.google.com`, DNS resolves the name to an IP address so the browser can connect to the correct server.

3.	System Requirements

•	Host OS: Windows 10
•	Virtualization: Oracle VirtualBox
•	Guest OS: Kali Linux (Rolling)
•	DNS Software: BIND9
•	Tools:
-	Terminal
-	systemctl
-	dig, 
-	nslookup

4.	Project Scope & Assumptions

o	This project focuses on deploying a local authoritative DNS server for a small enterprise or lab environment.
Scope:
•	Internal DNS resolution for private domains (e.g. mycompany.local)
•	Forward and reverse DNS zones
•	Manual static records - no dynamic updates
•	Single DNS server deployment
Assumptions:
•	This DNS server is deployed inside a trusted LAN - Kali
•	Clients receive DNS server IP via DHCP
•	Internet access is available for external resolution via forwarders
•	Security hardening (DNSSEC, views, ACLs) is outside the scope of this beginner toolkit

5.	Installation & Setup Instructions
How to Run / Reproduce This Project
1. Set up a Kali Linux virtual machine using VirtualBox
2. Update the system:
   sudo apt update
3. Install BIND9:
   sudo apt install bind9 bind9-utils bind9-dnsutils -y
4. Copy configuration files from the configs/ folder to /etc/bind/
5. Validate configurations:
   sudo named-checkconf
   sudo systemctl start named
   sudo systemctl enable named
      (i) forward lookup
   sudo named-checkzone mycompany.local /etc/bind/zones/mycompany.local.db
      (ii) reverse lookup
   sudo named-checkzone 56.168.192.in-addr.arpa /etc/bind/zones/db.192.168.56
7. Restart the DNS service:
   sudo systemctl restart named
8. Test DNS resolution:
       (i) forward lookup testing
   dig @127.0.0.1 server1.mycompany.local
   nslookup server1.mycompany.local 127.0.0.1
       (ii) reverse lookup testing
   dig @127.0.0.1 -x 192.168.56.20
   nslookup 192.168.56.20 127.0.0.1

 Installation & Setup Instructions SCREENSHOTS
Step 1: Install BIND9
[BIND9 Installation](screenshots/bind9_install.png)
Step 2: Forward Lookup Zone Configuration
o	Edit named.conf.options:
[named.conf.options](screenshots/named_conf_options.png)
o	Define Local Zone in named.conf.local:
[named.conf.local](screenshots/named_conf_local.png)
o	Create Forward Zone File:
[Forward Zone File](screenshots/zone_file_forward.png)
o	Validate Forward Zone:
[Validate Forward Zone](screenshots/validate_forward.png)
o	Forward Lookup Zone Testing:
[Forward Lookup Test](screenshots/forward_test.png)
Step 3: Reverse Lookup Zone Configuration
o	Create Reverse Zone File:
[Reverse Zone File](screenshots/zone_file_reverse.png)
o	Validate Reverse Zone:
[Validate Reverse Zone](screenshots/validate_reverse.png)
o	Reverse Lookup Zone Testing:
[Reverse Lookup Test](screenshots/reverse_test.png)

DNS Configuration Files
All configuration files used in this project are stored in the configs/ directory:

- named.conf.options – global DNS options and forwarders
- named.conf.local – local zone definitions
- mycompany.local.db – forward lookup zone file
- db.192.168.56 – reverse lookup zone file

NOTE:
In real enterprise environments, DNS administrators are often provided with existing configuration files or templates created by senior engineers.
These files may be incomplete or require modification to support new zones or hosts.
Understanding DNS concepts is essential for safely updating and troubleshooting such configurations.

IGNORE:
resolver priming query complete: failure
FORMERR resolving './NS/IN'
What they mean:
•	BIND has started successfully
•	It is attempting to contact root DNS servers
•	In a VM, this can temporarily fail due to:
-	NAT networking
-	No forwarders configured yet
Example:
 Local DNS zone created mycompany.local
o	The DNS server resolves internal hostnames such as:
server1.mycompany.local
Testing was done using:
i.	Forward Lookup Zone:
•	dig @127.0.0.1 server1.mycompany.local
•	nslookup server1.mycompany.local 127.0.0.1
Expected output:
o	The hostname resolves to the configured IP address.
ii.	Reverse Lookup Zone:
•	dig @127.0.0.1 –x 192.168.56.20
•	nslookup 192.168.56.20 127.0.0.1
Expected Result:
o	The IP address resolves back to the hostname.
NOTE:
•	@127.0.0.1 – querying dns
•	Restart named always before testing dns


	AI Prompt Journal
Prompt Used:
I need help understanding how to install and configure a DNS server using BIND9 on Kali Linux.
i.	Common Issues & Fixes
Service not starting:
o	Fixed by validating configuration files using named-checkconf.
ii.	Zone file errors
o	Fixed using named-checkzone.
sudo named-checkzone mycompany.local /etc/bind/zones/mycompany.local.db
sudo named-checkzone 56.168.192.in-addr.arpa /etc/bind/zones/db.192.168.56
iii.	Dependency Issues
Fix:
o	Resolved by using a clean Kali Linux installation to avoid broken package dependencies.
https://www.kali.org/get-kali/#kali-installer-images

Small Enterprise LAN Integration
•	DHCP assigns IP addresses to client devices
•	DHCP provides DNS server IP (BIND9) to clients
•	Clients send DNS queries to the internal DNS server
•	BIND9 resolves: 
-	Internal hostnames locally 
-	External domains via forwarders
•	Firewall and router protect and route traffic

This setup improves:
•	Centralized name resolution
•	Faster internal network access
•	Easier system administration

	References
https://www.isc.org/bind/
https://www.kali.org/docs/
https://www.zytrax.com/books/dns/

	Network Diagram
o	The following diagram illustrates how the DNS server integrates into a small enterprise LAN:
[LAN Diagram](screenshots/lan_diagram.png)
o	Detailed AI learning prompts and reflections are documented in:
docs/04-ai-prompt-journal.md




