# PCI DSS v4.0.1 Control Mapping

## Overview

This document maps the security controls implemented within the PCI DSS Lab environment to the corresponding PCI DSS v4.0.1 requirements.

The purpose of this laboratory was to simulate a realistic Cardholder Data Environment (CDE) and validate security controls through monitoring, vulnerability management, access control, encryption, segmentation, and attack simulation activities.

---

# Requirement 1

## Install and Maintain Network Security Controls

### Implemented Controls

* pfSense deployed as the central firewall platform.
* Network segmentation implemented between:

  * Internal Zone
  * DMZ Zone
  * Attack Zone
  * CDE Zone
  * Backup Zone
* Default deny firewall policy applied.
* Inter-zone communication restricted through firewall rules.
* OpenVPN configured for secure remote administration.
* Suricata deployed in IDS/IPS mode.

### Evidence

* Network topology
* Firewall configuration
* Suricata configuration

---

# Requirement 2

## Apply Secure Configurations to All System Components

### Implemented Controls

* Linux system hardening performed.
* SSH hardening implemented.
* RDP access restrictions configured.
* Active Directory Group Policies configured.
* Password complexity policies enforced.
* Account lockout policies configured.
* Service minimization applied where applicable.
* Zone-based network separation implemented.

### Evidence

* Active Directory policies
* Server hardening configurations
* Firewall segmentation rules

---

# Requirement 3

## Protect Stored Account Data

### Implemented Controls

* Payment application developed to simulate cardholder data processing.
* CVV data is never stored.
* PAN values displayed in truncated format.
* Sensitive cardholder data encrypted before database storage.
* HashiCorp Vault used for encryption key management.
* Vault AppRole authentication implemented.
* Database server protected using disk-level encryption.

### Evidence

* Payment application workflow
* HashiCorp Vault configuration
* Encrypted database records

---

# Requirement 4

## Protect Cardholder Data with Strong Cryptography During Transmission

### Implemented Controls

* Application-to-Vault communication secured.
* Administrative access protected through encrypted channels.
* VPN access implemented through OpenVPN.
* Internal communication paths restricted and controlled.

### Evidence

* OpenVPN configuration
* Secure application workflow
* Vault integration architecture

---

# Requirement 5

## Protect All Systems and Networks from Malicious Software

### Implemented Controls

* ClamAV deployed on Linux systems.
* Microsoft Defender used on Windows systems.
* Scheduled malware scans configured.
* Malware events monitored through Wazuh SIEM.
* Centralized visibility provided for malware-related alerts.

### Evidence

* ClamAV configuration
* Malware detection alerts
* Wazuh monitoring dashboards

---

# Requirement 6

## Develop and Maintain Secure Systems and Software

### Implemented Controls

* Vulnerability assessments performed regularly.
* Nessus Professional used for credentialed vulnerability scanning.
* High and Critical findings analyzed.
* Remediation activities performed and validated.
* Flask application isolated through Python virtual environments.

### Evidence

* Nessus scan reports
* Vulnerability remediation records
* Application deployment configuration

---

# Requirement 7

## Restrict Access to System Components and Cardholder Data by Business Need to Know

### Implemented Controls

* Active Directory deployed for centralized access management.
* OpenLDAP implemented for authentication services.
* Access to CDE restricted through network segmentation.
* Administrative access controlled through Jump Server.
* Role-based access principles applied where possible.

### Evidence

* LDAP configuration
* Active Directory screenshots
* Jump Server access controls

---

# Requirement 8

## Identify Users and Authenticate Access to System Components

### Implemented Controls

* Active Directory authentication implemented.
* Password policies enforced through Group Policy.
* Account lockout controls configured.
* Multi-Factor Authentication enabled for privileged access.
* OTP verification implemented for administrative sessions.
* Jump Server used as the primary administrative access point.

### Evidence

* MFA configuration
* Active Directory policies
* Jump Server authentication workflow

---

# Requirement 9

## Restrict Physical Access to Cardholder Data

### Status

Out of Scope

### Reason

This project was developed in a virtualized laboratory environment and does not include:

* Physical data center facilities
* Physical server rooms
* Badge access systems
* CCTV systems
* Visitor management controls
* Physical media destruction procedures

These controls would normally be implemented in enterprise production environments.

---

# Requirement 10

## Log and Monitor All Access to System Components and Cardholder Data

### Implemented Controls

* Wazuh SIEM deployed for centralized log management.
* All servers integrated with Wazuh agents.
* Security logs collected and correlated centrally.
* Alert generation configured for critical security events.

### Monitored Events

* Authentication failures
* LDAP modifications
* Privileged activity
* Vault access attempts
* Malware detections
* Root activity
* WAF events
* Administrative actions

### Evidence

* Wazuh dashboards
* Alert screenshots
* Event correlation examples

---

# Requirement 11

## Test Security of Systems and Networks Regularly

### Implemented Controls

* Nessus Professional deployed for vulnerability assessments.
* Credentialed vulnerability scans performed against lab systems.
* Dedicated attack simulation environment established.

### Attack Simulation Environment

* Kali Linux
* Metasploitable 2

### Validation Activities

* Vulnerability scanning
* Security control validation
* Detection testing
* Remediation verification
* Attack simulation exercises

### Evidence

* Nessus reports
* Validation screenshots
* Security testing documentation

---

# Requirement 11.5

## File Integrity Monitoring (FIM)

### Implemented Controls

* Wazuh File Integrity Monitoring enabled.
* Critical operating system files continuously monitored.
* Alert generation configured for unauthorized modifications.

### Monitored Examples

* /etc/passwd
* /etc/shadow
* Critical configuration files

### Evidence

* FIM alerts
* Wazuh dashboards
* File modification detection screenshots

---

# Requirement 12

## Support Information Security with Organizational Policies and Programs

### Status

Partially Implemented

### Implemented Components

* Secure architecture design
* Security hardening procedures
* Security monitoring processes
* Evidence collection methodology
* Vulnerability management workflow
* Compliance validation activities

### Not Implemented

The following controls require a real organizational environment and were outside the scope of this laboratory:

* Human Resources security processes
* Security awareness training programs
* Employee onboarding/offboarding procedures
* Corporate governance activities
* Enterprise risk management processes
* Third-party service provider management

### Reason

The laboratory environment focuses on technical PCI DSS controls and does not represent a complete organizational structure.

---

# Requirement Coverage Summary

| Requirement    | Status                |
| -------------- | --------------------- |
| Requirement 1  | Implemented           |
| Requirement 2  | Implemented           |
| Requirement 3  | Implemented           |
| Requirement 4  | Implemented           |
| Requirement 5  | Implemented           |
| Requirement 6  | Implemented           |
| Requirement 7  | Implemented           |
| Requirement 8  | Implemented           |
| Requirement 9  | Out of Scope          |
| Requirement 10 | Implemented           |
| Requirement 11 | Implemented           |
| Requirement 12 | Partially Implemented |

---

## Conclusion

This PCI DSS laboratory was designed to simulate a realistic enterprise payment-card environment and demonstrate the implementation of key PCI DSS v4.0.1 technical security controls, including network segmentation, secure cardholder data processing, centralized monitoring, vulnerability management, privileged access control, and attack validation activities.
