# Lab Documentation - Firewall Rules and Network Segmentation

## Overview
This document provides a detailed breakdown of the firewall rule design and implementation performed in OPNsense.  
The lab is based on a segmented enterprise network scenario (SecureOffice AB) and focuses on applying security best practices such as least privilege, default deny, and network segmentation.

---

## Network Analysis

### VLAN Overview
The network is divided into multiple VLANs to separate traffic and improve security:

- USERS (10.10.10.0/24): End-user devices  
- SERVERS (10.10.20.0/24): Internal servers  
- MANAGEMENT (10.10.30.0/24): Administrative systems  
- DMZ (10.10.40.0/24): Public-facing services  
- GUEST (10.10.50.0/24): Guest network  
- VPN (10.10.60.0/24): Remote users  

---

### Sensitive Systems
The most critical systems are located in:

- MANAGEMENT network (administrative access)  
- SERVERS network (domain controller, file server, internal services)  

These systems require strict access control.

---

### Allowed Communication
- USERS → SERVERS (limited access: SMB, HTTPS)  
- USERS → Internet (HTTP/HTTPS)  
- USERS → DNS (DC01)  
- MANAGEMENT → all networks (administration)  
- SERVERS → Internet (updates, DNS)  

---

### Restricted Communication
- GUEST → Internal networks (blocked)  
- USERS → MANAGEMENT (blocked)  
- USERS → full SERVERS access (blocked)  
- DMZ → Internal networks (restricted)  

---

### Required Services
- DNS (53)  
- HTTP/HTTPS (80, 443)  
- SMB (445)  
- Mail services (25, 587, 993)  

---

### Administrative Services
- SSH (22)  
- RDP (3389)  
- HTTPS (administrative interfaces)  

These services are restricted to the MANAGEMENT network.

---

### Logging Strategy
Logging is enabled for:
- Block rules  
- Administrative access  
- Sensitive services such as SMB  

---

## Alias Configuration

### Host Aliases
- DC01 → 10.10.20.10  
- FILE01 → 10.10.20.20  
- INTRANET01 → 10.10.20.30  
- BACKUP01 → 10.10.20.40  
- WEB01 → 10.10.40.10  
- MAILGW01 → 10.10.40.20  
- ADMIN_PC01 → 10.10.30.10  
- ADMIN_PC02 → 10.10.30.11  

---

### Network Aliases
- USERS_NET → 10.10.10.0/24  
- SERVERS_NET → 10.10.20.0/24  
- MGMT_NET → 10.10.30.0/24  
- DMZ_NET → 10.10.40.0/24  
- GUEST_NET → 10.10.50.0/24  
- VPN_NET → 10.10.60.0/24  
- INTERNAL_NETS → Combined internal networks  

---

### Port Aliases
- WEB_PORTS → 80, 443  
- DNS_PORTS → 53  
- ADMIN_PORTS → 22, 3389, 443  
- FILE_PORTS → 445  
- MAIL_PORTS → 25, 587, 993  

---

## Firewall Rule Design

Due to limitations in the Hyper-V lab environment, not all VLANs were implemented.  
Firewall rules were created on the LAN interface in OPNsense to simulate real-world behavior.

The focus was on:
- Correct rule structure  
- Proper use of aliases  
- Secure rule logic  

---

## Implemented Firewall Rules

### Allow USERS to DC01 DNS
- Source: USERS_NET  
- Destination: DC01  
- Port: DNS_PORTS  
- Action: Pass  

---

### Allow USERS to Internet
- Source: USERS_NET  
- Destination: any  
- Port: WEB_PORTS  
- Action: Pass  

---

### Allow USERS to FILE01 SMB
- Source: USERS_NET  
- Destination: FILE01  
- Port: FILE_PORTS  
- Action: Pass  

---

### Block USERS to MANAGEMENT
- Source: USERS_NET  
- Destination: MGMT_NET  
- Action: Block  
- Logging: Enabled  

---

### Block USERS to SERVERS
- Source: USERS_NET  
- Destination: SERVERS_NET  
- Action: Block  
- Logging: Enabled  

---

### Allow USERS to INTRANET01 HTTPS
- Source: USERS_NET  
- Destination: INTRANET01  
- Port: WEB_PORTS  
- Action: Pass  

---

## Rule Justification

### Allow USERS to FILE01 SMB
Users require access to the file server for daily operations.  
The rule restricts SMB access to a specific server instead of the entire server network.  
If the rule were too broad, attackers could move laterally between systems.  
Logging is enabled because SMB is a sensitive service.

---

### Block USERS to MANAGEMENT
This rule protects administrative systems from unauthorized access.  
Without this restriction, attackers could target admin interfaces.  
Logging is enabled to detect unauthorized attempts.

---

## Reflection
This lab demonstrates that network segmentation alone is not sufficient without proper firewall rules.  
By applying least privilege and default deny, the network becomes significantly more secure.

The use of aliases improves readability, scalability, and maintainability of firewall rules.

Working in a simulated Hyper-V environment also shows that strong security design can be implemented even without full infrastructure deployment.

---

## Conclusion
The implementation of structured firewall rules combined with segmentation ensures a secure and controlled network environment.  
This lab highlights the importance of precise rule design, proper logging, and security-focused thinking in modern network environments.
