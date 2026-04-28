# OPNsense Firewall Rules and Network Segmentation Lab

## Overview
This lab focuses on designing and implementing secure firewall rules in a segmented enterprise network using OPNsense.  
The goal is to apply real-world security principles such as least privilege, default deny, and network segmentation while working with aliases and structured rule design.

The lab is based on a fictional company scenario: SecureOffice AB.

---

## Scenario: SecureOffice AB
SecureOffice AB is a small company with approximately 80 employees.  
The network has recently been segmented using VLANs, and the task is to design firewall rules that ensure secure and controlled communication between network segments.

---

## What This Lab Demonstrates
- Firewall rule design using OPNsense  
- Use of aliases (host, network, port)  
- Implementation of least privilege access  
- Preventing lateral movement  
- Applying Zero Trust principles  
- Understanding traffic flow and rule direction  
- Working with firewall rules in a simulated environment  

---

## Network Segmentation

| VLAN | Name       | Subnet          | Purpose                        |
|------|-----------|-----------------|--------------------------------|
| 10   | USERS      | 10.10.10.0/24   | End-user devices              |
| 20   | SERVERS    | 10.10.20.0/24   | Internal servers              |
| 30   | MANAGEMENT | 10.10.30.0/24   | Administrative systems        |
| 40   | DMZ        | 10.10.40.0/24   | Public-facing services        |
| 50   | GUEST      | 10.10.50.0/24   | Guest network                 |
| 60   | VPN        | 10.10.60.0/24   | Remote users                  |

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

### Network Aliases
- USERS_NET → 10.10.10.0/24  
- SERVERS_NET → 10.10.20.0/24  
- MGMT_NET → 10.10.30.0/24  
- DMZ_NET → 10.10.40.0/24  
- GUEST_NET → 10.10.50.0/24  
- VPN_NET → 10.10.60.0/24  
- INTERNAL_NETS → Combined internal networks  

### Port Aliases
- WEB_PORTS → 80, 443  
- DNS_PORTS → 53  
- ADMIN_PORTS → 22, 3389, 443  
- FILE_PORTS → 445  
- MAIL_PORTS → 25, 587, 993  

---

## Firewall Rules (LAN Interface Simulation)

Since not all VLANs were implemented in Hyper-V, firewall rules were created on the LAN interface to simulate real-world rule design.

### Allow Rules
- Allow USERS to DC01 (DNS)  
- Allow USERS to Internet (HTTP/HTTPS)  
- Allow USERS to FILE01 (SMB)  
- Allow USERS to INTRANET01 (HTTPS)  

### Block Rules
- Block USERS to MANAGEMENT  
- Block USERS to SERVERS (default deny)  

---

## Security Principles Applied

- Default Deny: All traffic is blocked unless explicitly allowed  
- Least Privilege: Only required access is granted  
- Segmentation: Networks are logically isolated  
- Controlled Access: Administrative services restricted to management network  
- Lateral Movement Prevention: Restricted communication between segments  

---

## Lab Environment

- Platform: Hyper-V  
- Firewall: OPNsense  
- Approach:  
  - Full VLAN setup was not required  
  - Rules were implemented on LAN interface for simulation  
  - Focus was on correct rule design rather than full deployment  

---

## Screenshots

### 1. Alias Overview
![Alias Overview](images/screenshots/01-alias-overview.png)

Shows configured host, network, and port aliases in OPNsense.

### 2. Firewall Rules Overview
![Firewall Rules Overview](images/screenshots/02-firewall-rules-overview.png)

Shows the configured LAN firewall rules, including allow and block rules.

### 3. Allow Rule Example - Part 1
![Allow Rule Example Part 1](images/screenshots/03-rule-example-file01-part1.png)

Shows the general settings for the SMB allow rule.

### 4. Allow Rule Example - Part 2
![Allow Rule Example Part 2](images/screenshots/03-rule-example-file01-part2.png)

Shows source, destination, and port aliases for USERS_NET to FILE01.

### 5. Block Management Rule - Part 1
![Block Management Rule Part 1](images/screenshots/04-block-management-part1.png)

Shows the general settings for the management block rule.

### 6. Block Management Rule - Part 2
![Block Management Rule Part 2](docs/images/screenshots/04-block-management-part2.png)

Shows source, destination, and logging enabled for the management block rule.

---

## Documentation

Detailed lab documentation, including:
- Traffic analysis  
- Rule motivation  
- Step-by-step implementation  

See:  
docs/lab-documentation.md

---

## What I Learned

- How to design secure firewall rules using OPNsense  
- Importance of structured rule order and specificity  
- How aliases simplify firewall management  
- Why segmentation alone is not enough without proper traffic control  
- Practical application of Zero Trust principles  

---

## Author

**Muhammad Mehdi**  
IT Security Developer Student  
