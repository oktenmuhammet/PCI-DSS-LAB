# Network Segmentation

## Overview

One of the primary objectives of this lab was to implement a segmented network architecture aligned with PCI DSS principles.

Rather than placing all systems on a single flat network, dedicated security zones were created to isolate systems based on their business function and risk level.

This approach reduces the attack surface, limits lateral movement opportunities, and protects the Cardholder Data Environment (CDE) from unauthorized access.

All network traffic is controlled through a central pfSense firewall operating as the security boundary between zones.

---

# Segmentation Architecture

The environment is divided into five primary security zones.

## Internal Zone

Network:

192.168.40.0/24

Purpose:

Provides corporate identity and workstation services.

Systems:

* Active Directory Domain Controller
* Windows Server 2016
* Windows 10 Client

Security Controls:

* Domain-based authentication
* Group Policies
* Password policies
* Account lockout controls

---

## DMZ Zone

Network:

192.168.50.0/24

Purpose:

Hosts externally facing and management-related services.

Systems:

* Wazuh SIEM
* HashiCorp Vault
* OpenLDAP
* Jump Server
* Web Server
* Linux Server
* Administrative Workstation

Security Controls:

* Reverse Proxy
* Web Application Firewall (ModSecurity)
* Centralized Monitoring
* Secure Administrative Access

The DMZ acts as a controlled buffer zone between internal systems and sensitive cardholder data systems.

---

## Cardholder Data Environment (CDE)

Network:

192.168.70.0/24

Purpose:

Contains systems responsible for processing and storing payment card information.

Systems:

* Application Server
* Database Server

Security Controls:

* Isolated subnet
* Restricted firewall access
* Administrative access only through Jump Server
* Multi-Factor Authentication
* Disk encryption
* Continuous monitoring through Wazuh

This zone represents the most sensitive area of the environment.

---

## Attack Zone

Network:

192.168.60.0/24

Purpose:

Provides a dedicated environment for offensive security testing and validation activities.

Systems:

* Kali Linux
* Metasploitable 2

Activities:

* Vulnerability validation
* Attack simulation
* Detection testing
* Security control verification

This zone allows security controls to be tested without impacting production-like systems.

---

## Backup Zone

Network:

192.168.100.0/24

Purpose:

Dedicated storage area for backup operations.

Systems:

* Backup Server

Security Controls:

* Segregated backup storage
* Restricted access paths
* Automated backup jobs

The backup infrastructure is intentionally isolated from the CDE to improve resilience and recovery capabilities.

---

# Central Security Gateway

All zones communicate through a centralized pfSense firewall.

pfSense Responsibilities:

* Network segmentation enforcement
* Firewall rule management
* OpenVPN access
* Traffic inspection
* IDS/IPS integration through Suricata

Default-deny principles are applied wherever possible.

Only explicitly authorized communication paths are allowed.

---

# Security Benefits

The segmentation model provides several security advantages:

### Reduced Attack Surface

Systems are separated according to their business function, reducing unnecessary exposure.

### Limited Lateral Movement

Compromise of one zone does not automatically provide access to other zones.

### CDE Protection

Cardholder data systems remain isolated from less trusted environments.

### Improved Monitoring

Security events can be analyzed based on zone-specific activity.

### PCI DSS Alignment

The architecture supports PCI DSS requirements related to network security controls, access restriction, monitoring, and protection of cardholder data.

---

# Traffic Flow Example

Typical payment transaction flow:

1. User accesses the payment application hosted in the DMZ.
2. Requests are forwarded to the Application Server located inside the CDE.
3. The Application Server authenticates to HashiCorp Vault using AppRole authentication.
4. Vault provides encryption material.
5. Sensitive payment data is encrypted.
6. Encrypted data is stored in the Database Server.
7. Database backups are transferred to the Backup Zone.

At every stage, traffic is controlled through segmentation boundaries and firewall policies.

---

# Conclusion

Network segmentation is one of the most important security controls implemented in this PCI DSS laboratory.

By separating systems into dedicated security zones and enforcing communication through a centralized firewall, the environment provides stronger protection for cardholder data while supporting monitoring, vulnerability management, and attack validation activities.
