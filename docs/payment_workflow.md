# Payment Workflow

## Overview

One of the primary objectives of this lab was to simulate how cardholder data can be securely processed and stored in a PCI DSS-oriented environment.

A custom payment application was developed to demonstrate secure handling of payment card information while implementing multiple security controls such as encryption, key management, network segmentation, access control, and monitoring.

The design focuses on minimizing cardholder data exposure and protecting sensitive information throughout the entire payment lifecycle.

---

# Architecture Components

The payment workflow consists of the following systems:

| Component          | Purpose                                               |
| ------------------ | ----------------------------------------------------- |
| Web Server         | Hosts the payment page and receives customer requests |
| Application Server | Processes payment requests                            |
| HashiCorp Vault    | Stores and manages encryption keys                    |
| Database Server    | Stores encrypted cardholder data                      |
| Wazuh SIEM         | Monitors security events                              |
| pfSense Firewall   | Controls network communication                        |

---

# Payment Processing Flow

## Step 1 – Customer Access

A user accesses the payment page hosted on the Web Server located within the DMZ network.

The web application is protected by:

* Reverse Proxy
* ModSecurity Web Application Firewall (WAF)
* Firewall segmentation rules

These controls help protect the application against common web attacks such as:

* SQL Injection
* Cross-Site Scripting (XSS)
* Malicious requests

---

## Step 2 – Payment Submission

The customer enters payment information through the payment form.

Example data:

* Cardholder Name
* PAN (Primary Account Number)
* Expiration Date

Important Security Note:

CVV data is never stored within the application or database.

This behavior aligns with PCI DSS requirements regarding sensitive authentication data.

---

## Step 3 – Application Processing

The payment request is forwarded from the Web Server to the Application Server located inside the Cardholder Data Environment (CDE).

The application is deployed using:

* Python Flask
* Dedicated Python Virtual Environment (venv)

The Application Server is responsible for processing cardholder information before storage.

---

## Step 4 – Vault Authentication

The Application Server does not contain hardcoded encryption keys.

Instead, it authenticates to HashiCorp Vault using AppRole authentication.

Benefits:

* No hardcoded secrets
* Centralized key management
* Controlled access to encryption material
* Improved security posture

This approach reduces the risk of credential exposure within application source code.

---

## Step 5 – Encryption

After successful authentication, the Application Server retrieves the required encryption capability from Vault.

Sensitive cardholder data is encrypted before being written to the database.

Security Controls:

* Encryption performed before storage
* Key management separated from application logic
* Encryption material protected by Vault policies

---

## Step 6 – Database Storage

The encrypted data is stored inside the Database Server located in the dedicated CDE network.

Stored data characteristics:

* Encrypted format
* No plaintext cardholder data
* PAN displayed in truncated format where applicable
* CVV never stored

Additional Protection:

* Disk-level encryption enabled on the database server
* Restricted network access
* Administrative access controlled through Jump Server

---

## Step 7 – Monitoring and Logging

All payment-related infrastructure components are monitored through Wazuh SIEM.

Monitored Events:

* Vault authentication attempts
* Unauthorized Vault access attempts
* Web application events
* System activity
* Administrative access
* Security alerts

This provides centralized visibility across the payment environment.

---

# Access Control Model

Administrative access to payment infrastructure follows a controlled access model.

Requirements:

* Jump Server access
* Multi-Factor Authentication (MFA)
* OTP verification
* Centralized authentication controls

Direct administrative access to critical systems is restricted.

---

# Security Layers

Multiple security controls work together to protect cardholder data.

### Network Security

* pfSense Firewall
* Network Segmentation
* Default Deny Policies

### Application Security

* Flask Application
* Reverse Proxy
* ModSecurity WAF

### Identity Security

* Active Directory
* OpenLDAP
* MFA

### Data Security

* HashiCorp Vault
* Encryption Before Storage
* Disk Encryption

### Monitoring

* Wazuh SIEM
* File Integrity Monitoring
* Security Alerting

### Vulnerability Management

* Nessus Professional
* Credentialed Scanning
* Remediation Validation

---

# Security Objectives Achieved

This workflow demonstrates several key PCI DSS security concepts:

* Protection of cardholder data
* Segmentation of sensitive systems
* Secure key management
* Strong access controls
* Centralized monitoring
* Vulnerability management
* Defense-in-depth architecture

---

# Conclusion

The payment workflow was designed to simulate how cardholder data can be securely processed within a PCI DSS-aligned environment.

Rather than storing sensitive information directly, the solution leverages HashiCorp Vault for centralized key management, encrypts data before storage, restricts access through network segmentation and MFA controls, and continuously monitors security events through Wazuh SIEM.

This architecture demonstrates practical implementation of several PCI DSS security principles within a controlled laboratory environment.
