
# PCI DSS v4.0.1 Security Lab

A realistic enterprise-style PCI DSS v4.0.1 laboratory environment designed to simulate a secure payment card infrastructure.

This project was built to gain hands-on experience with network segmentation, Cardholder Data Environment (CDE) protection, centralized monitoring, vulnerability management, privileged access control, and secure payment processing workflows.

The objective was not only to deploy security technologies, but also to validate PCI DSS controls through attack simulation, monitoring, detection, and evidence collection.

---

## Lab Objectives

* Design a segmented PCI DSS environment
* Build and protect a Cardholder Data Environment (CDE)
* Secure cardholder data using encryption and key management
* Implement centralized monitoring and alerting
* Enforce identity and access management controls
* Validate security controls through offensive testing
* Produce evidence aligned with PCI DSS requirements

---

# Environment Overview

## Firewall & Network Security

pfSense acts as the central security gateway between all zones.

### Network Zones

| Zone          | Network          | Gateway         |
| ------------- | ---------------- | --------------- |
| Internal Zone | 192.168.40.0/24  | 192.168.40.254  |
| DMZ Zone      | 192.168.50.0/24  | 192.168.50.254  |
| Attack Zone   | 192.168.60.0/24  | 192.168.60.254  |
| CDE Zone      | 192.168.70.0/24  | 192.168.70.254  |
| Backup Zone   | 192.168.100.0/24 | 192.168.100.254 |

Security Controls:

* Default deny firewall policy
* Network segmentation
* Inter-zone access restrictions
* OpenVPN remote access
* Suricata IDS/IPS
* Dedicated security zones

---

# Internal Zone

Network: 192.168.40.0/24

Systems:

* Active Directory Domain Controller
* Windows Server 2016
* Windows 10 Client

Domain:

emin.com.local

Implemented Controls:

* Group Policies
* Password Policies
* Account Lockout Policies
* Session Timeout Controls

---

# DMZ Zone

Network: 192.168.50.0/24

Systems:

* Wazuh SIEM Server
* HashiCorp Vault
* OpenLDAP
* Jump Server
* Web Server
* Linux Server
* Administrative Windows Workstation

Security Features:

* Reverse Proxy
* ModSecurity WAF
* Centralized Monitoring
* Secure Administrative Access

---

# Cardholder Data Environment (CDE)

Network: 192.168.70.0/24

Systems:

* Application Server
* Database Server

This zone contains systems that process and store cardholder data.

Security Controls:

* Dedicated isolated network
* Restricted access paths
* Jump Server access requirement
* MFA protected administrative access
* Disk encryption
* Centralized monitoring

---

# Payment Processing Workflow

1. User accesses payment page.
2. Payment page communicates with Flask application.
3. Application authenticates to HashiCorp Vault using AppRole.
4. Vault provides encryption material.
5. Sensitive card data is encrypted.
6. Encrypted data is stored in the database.
7. CVV data is never stored.
8. PAN data is displayed in truncated format.

---

# Identity & Access Management

Active Directory

* Centralized authentication
* Password complexity requirements
* Account lockout controls

LDAP

* User lifecycle management
* Authentication services
* Directory monitoring

Administrative Access

* Jump Server
* MFA authentication
* OTP verification
* Session monitoring

---

# Security Monitoring

All servers are onboarded to Wazuh.

Monitored Events:

* File Integrity Monitoring (FIM)
* Root activity
* Privilege escalation
* LDAP modifications
* WAF events
* Malware detection
* Authentication failures
* Vault access attempts
* Administrative actions

Alerting:

* Email notifications
* Dashboard monitoring
* Correlation rules
* Custom detection rules

---

# Endpoint Protection

Linux Systems

* ClamAV
* Scheduled malware scans

Windows Systems

* Microsoft Defender

---

# Time Synchronization

Chrony is deployed across the environment.

NTP source:

pfSense NTP Pool

Benefits:

* Accurate log correlation
* Consistent forensic timelines
* Compliance alignment

---

# Vulnerability Management

Credentialed vulnerability assessments were performed using Nessus Professional.

Activities:

* Full infrastructure scanning
* Vulnerability validation
* Remediation verification
* Evidence collection

Assessment targets:

* Internal Zone
* DMZ Systems
* CDE Systems
* Infrastructure Components

---

# Backup & Recovery

Dedicated Backup Zone:

192.168.100.0/24

Backup Server:

192.168.100.10

Features:

* Database backup jobs
* Segregated backup storage
* Recovery testing
* Encrypted storage

---

# Technologies Used

* pfSense
* Suricata
* Wazuh
* OpenSearch
* HashiCorp Vault
* OpenLDAP
* Active Directory
* OpenVPN
* ModSecurity
* Nginx
* Flask
* MySQL
* Chrony
* ClamAV
* Nessus Professional

---

# Disclaimer

This project was developed for educational, research, and defensive security purposes to demonstrate PCI DSS security concepts and practical implementation techniques.

