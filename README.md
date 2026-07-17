# FinSecure Corp. Data Security and Systems Defense Review

## Project Overview

This project presents a data security and systems defense assessment for **FinSecure Corp.**, a fictional financial services organization. The review evaluates how the company protects sensitive information in transit and at rest, identifies weaknesses in its system architecture and operational security practices, and recommends technical controls to improve confidentiality, integrity, detection, response, and recovery.

The assessment focuses on legacy file-transfer protocols, unencrypted customer data, weak encryption-key storage, limited network segmentation, privileged administrative access, decentralized logging, and endpoint monitoring.

---

## Project Objectives

- Identify data-protection risks affecting sensitive financial and employee information.
- Recommend cryptographic controls for data in transit and at rest.
- Explain how the recommended controls protect confidentiality and integrity.
- Identify weaknesses in the current system architecture.
- Recommend network and privileged-access countermeasures.
- Evaluate FinSecure’s operational security posture.
- Recommend centralized monitoring and endpoint-security tools.
- Explain how the proposed improvements strengthen detection, response, and recovery.

---

## Skills Demonstrated

- Data-at-Rest Protection
- Data-in-Transit Protection
- Encryption and Key Management
- Secure File Transfer
- AES-256 Database Encryption
- Network Segmentation
- DMZ Design
- Privileged Access Management
- Zero Trust Concepts
- SIEM Deployment
- Endpoint Detection and Response
- Log Management
- Incident Detection and Containment
- Backup and Recovery Security
- NIST Security Control Alignment

---

## Current Security Environment

FinSecure Corp. manages customer financial transactions, employee records, payroll data, compliance documentation, and other sensitive business information.

The organization currently uses:

- An internal FTP server for HR and Finance file transfers
- HTTPS connections that support multiple TLS versions
- A VPN with split tunneling
- RDP for virtual-machine administration
- Offsite backups
- Local antivirus software
- A staging environment for software updates
- An IPSec connection between the corporate network and the data center

Although these controls provide some protection, several weaknesses reduce their effectiveness.

---

## Data Protection Risks

### Risk 1: Payroll and Employee Data Transmitted Through Legacy FTP

HR and Finance exchange payroll and employee information through an internal FTP server.

FTP transmits files and login credentials in clear text. An attacker, malicious insider, compromised workstation, or unauthorized device on the internal network could intercept or alter:

- Payroll records
- Employee information
- Banking details
- Login credentials
- Financial documents

**Likelihood:** Moderate  
**Impact:** High

The process is used regularly, creating repeated opportunities for sensitive information to be intercepted or modified.

---

### Risk 2: Customer Data Stored in Clear Text

The Finance database stores customer personally identifiable information and financial records in clear text.

Anyone who gains access to the following could read the information without defeating encryption:

- Server disks
- Database files
- Disk snapshots
- Attached backups
- Compromised administrator accounts

The risk is increased because encryption keys are stored in application configuration files on the same servers as the protected applications.

**Likelihood:** Moderate  
**Impact:** High

A single server compromise could expose both the sensitive data and the keys intended to protect it.

---

## Recommended Cryptographic Controls

### Recommendation 1: Replace FTP With SFTP

FinSecure should replace the legacy FTP service with **SSH File Transfer Protocol over SSH**.

SFTP would:

- Encrypt transferred files
- Encrypt authentication credentials
- Protect data from interception
- Help detect changes during transmission
- Support individual user accounts
- Provide detailed transfer logs
- Reduce reliance on shared credentials

Access should be restricted to approved HR and Finance systems. File-transfer logs should also be forwarded to the centralized logging platform for monitoring.

---

### Recommendation 2: Encrypt the Finance Database With AES-256

FinSecure should use **AES-256** through transparent database encryption or another approved database-encryption feature.

AES-256 would protect:

- Customer PII
- Financial records
- Database files
- Disk snapshots
- Storage media
- Backups containing database information

Encryption would convert readable information into ciphertext, reducing the amount of exposed data if storage is compromised.

---

### Recommendation 3: Centralize Encryption-Key Management

Encryption keys should not remain in configuration files on the same systems as the applications and databases they protect.

FinSecure should use:

- A centralized key management system
- A hardware security module
- Restricted application access
- Documented key rotation
- Key revocation procedures
- Secure key backups
- Key-recovery procedures
- Logging of key access and administrative activity

Separating the keys from the encrypted data prevents one system compromise from automatically exposing both.

---

## Confidentiality and Integrity Benefits

### SFTP

SFTP improves confidentiality by keeping payroll files, employee data, and credentials unreadable during transmission.

It improves integrity by helping protect transferred information from unauthorized modification while it moves between HR and Finance.

### AES-256

AES-256 protects customer information while it is stored. Even when an attacker copies a database file, disk image, or backup, the information remains unreadable without the correct encryption key.

### Centralized Key Management

Separating encryption keys from protected systems reduces the risk that an attacker can obtain both the ciphertext and the key required to decrypt it.

---

## System Architecture Weaknesses

### Vulnerability 1: No Dedicated DMZ for the Public Web Interface

The public web interface exists in the same general data-center environment as:

- Application servers
- File servers
- Customer-record databases
- Finance systems
- Accounting systems

A compromised internet-facing service could provide an attacker with a path toward critical internal systems.

---

### Vulnerability 2: Insufficient Data-Center Segmentation

Critical systems appear to communicate through a single network gateway without clearly defined security zones.

This creates opportunities for lateral movement between:

- Web systems
- Application servers
- File servers
- Databases
- Finance systems
- HR systems
- Accounting systems
- Administrative systems

A compromise of one server could allow attempts to access other high-value systems.

---

### Vulnerability 3: Domain Administrator Access From Regular Workstations

IT users can perform Domain Administrator functions from normal workstations.

If one of those workstations is compromised through phishing, malware, credential theft, or malicious software, an attacker could obtain highly privileged access to the Windows environment.

---

## Recommended Architecture Countermeasures

### Countermeasure 1: Establish a DMZ

The public web interface should be moved into a separate demilitarized zone between the internet and the internal data center.

Controls should include:

- Default-deny firewall rules
- A web application firewall
- A secure reverse proxy
- Limited inbound ports and protocols
- Restricted connections to internal systems
- Logging of all DMZ traffic
- No direct access to the Finance database

---

### Countermeasure 2: Segment Critical Systems

FinSecure should create separate security zones for:

- Public web services
- Application servers
- Database servers
- File servers
- Finance systems
- HR systems
- User workstations
- Administrative systems

Internal firewalls and access control lists should permit communication only when a documented business need exists.

This would reduce the ability of an attacker to move from one compromised system to another.

---

### Countermeasure 3: Use Privileged Access Workstations and a Jump Server

FinSecure should stop using Domain Administrator accounts on standard IT workstations.

Administrators should use:

- Hardened privileged access workstations
- A secured jump server
- Multifactor authentication
- Separate standard and administrative accounts
- Logged privileged sessions
- Approved administrative workflows
- Time-limited privileged access

Administrative accounts should not be used for email, internet browsing, or ordinary daily activities.

---

## Security Improvements

### Reduced Exposure of Critical Systems

A DMZ would separate the public web interface from internal financial and customer systems. A compromised public service would have fewer permitted paths into the internal environment.

### Limited Lateral Movement

Internal segmentation would prevent one compromised server from automatically reaching databases, financial systems, or administrative networks.

### Better Privileged Account Protection

Privileged access workstations, a secure jump server, MFA, and separate accounts would make stolen passwords less useful and reduce exposure of Domain Administrator credentials.

---

## Operational Security Findings

FinSecure’s current operational environment includes several weaknesses:

- Logs remain on the devices where they are generated.
- Log retention is limited to 30 days.
- Firewall and VPN activity is reviewed weekly.
- Endpoint antivirus alerts are stored locally.
- Security events are handled by IT administrators as part of their normal workload.
- Remote and administrative users rely on usernames and passwords.
- Password changes occur annually.
- Access reviews are performed only when requested.
- Updates are deployed quarterly and installed manually.
- Backups use standard storage permissions.
- Full restoration tests occur only once per year.
- System changes are recorded in a shared spreadsheet.
- Certificates are renewed manually.

---

## Proposed Security Tools

### Proposal 1: Centralized SIEM Platform

FinSecure should deploy a **Security Information and Event Management** platform to centralize logs from:

- Endpoints
- Servers
- Databases
- Firewalls
- VPN systems
- Network devices
- Applications
- Cloud and offsite resources
- Authentication systems
- Privileged access systems

The SIEM should generate alerts for:

- Repeated failed logins
- Unusual VPN locations
- Privileged account activity
- Disabled security controls
- Unexpected shares
- New network listeners
- Critical system changes
- Suspicious database access
- Log deletion or interruption
- Unauthorized configuration changes

Log retention should align with FinSecure’s legal, regulatory, business, and incident-response requirements rather than remaining limited to 30 days.

---

### Proposal 2: Centrally Managed Endpoint Detection and Response

FinSecure should replace or supplement local antivirus software with a centrally managed **Endpoint Detection and Response** platform.

EDR agents should be installed on:

- User laptops
- Administrator workstations
- Supported servers
- Remote endpoints
- High-value systems

The EDR platform should monitor:

- Suspicious processes
- Malicious PowerShell activity
- Credential-access attempts
- Persistence mechanisms
- Unauthorized file changes
- Unusual network connections
- Communication with malicious domains
- Data-collection activity
- Possible data theft

Authorized responders should be able to:

- Isolate endpoints remotely
- Stop malicious processes
- Quarantine files
- Collect forensic evidence
- Search for related activity on other systems
- Confirm that threats are removed before reconnecting devices

---

## Detection, Response, and Recovery Improvements

### Improved Detection

A SIEM would correlate events from multiple systems instead of leaving them separated across local devices.

For example, it could connect:

- Multiple failed VPN logins
- A successful remote login
- Privileged account activity
- Firewall alerts
- Database access
- Endpoint malware activity

Together, these events may show an account takeover or active intrusion that would be harder to recognize when reviewed separately.

---

### Improved Response

EDR would allow FinSecure to isolate an infected device immediately instead of waiting for a user or local administrator to report the issue.

Responders could stop processes, quarantine files, collect evidence, and search other systems for the same behavior.

---

### Improved Recovery

SIEM logs would help investigators determine:

- When an incident began
- Which accounts were involved
- Which systems were affected
- What actions occurred
- Which systems need rebuilding
- Whether suspicious behavior returned

EDR data would help determine whether a device should be cleaned, wiped, or rebuilt from a trusted image. It would also help confirm that malicious files and persistence mechanisms were removed before the system returned to service.

---

## SIEM and EDR Combined

SIEM and EDR provide different but complementary capabilities.

| Security Tool | Primary Purpose |
|---|---|
| SIEM | Organization-wide collection, correlation, alerting, and investigation of logs |
| EDR | Detailed endpoint monitoring, containment, investigation, and remediation |

Used together, the tools could connect endpoint activity with:

- VPN events
- Firewall logs
- Administrator logins
- Application activity
- Server events
- Network connections
- Authentication records

This would give FinSecure faster and more complete visibility into suspicious activity.

---

## Security Recommendations Summary

| Security Area | Recommended Improvement |
|---|---|
| File Transfers | Replace legacy FTP with SFTP |
| Data at Rest | Encrypt the Finance database with AES-256 |
| Key Management | Use a centralized KMS or hardware security module |
| Public Services | Place the public web interface in a DMZ |
| Internal Network | Segment critical systems with internal firewalls |
| Privileged Access | Use PAWs, a secure jump server, and MFA |
| Logging | Centralize logs in a SIEM |
| Endpoint Security | Deploy centrally managed EDR |
| Authentication | Strengthen remote and administrative authentication |
| Access Reviews | Establish a required review schedule |
| Patching | Improve deployment frequency and automation |
| Backups | Encrypt backups and increase restoration testing |
| Change Management | Replace shared spreadsheets with formal change tracking |
| Certificates | Centralize certificate management and renewal |

---

## Project Files

Because this project is a written security assessment, the repository does not require screenshots or an images folder.

Suggested repository structure:

```text
FinSecure-Data-Security-and-Systems-Defense/
│
├── README.md
└── docs/
    ├── FinSecure-Data-Security-and-Systems-Defense-Review.pdf
    └── Data-Security-and-Systems-Defense-Artifacts.pdf
```

### Documentation

- **[Full Data Security and Systems Defense Review](https://github.com/user-attachments/files/30113215/FinSecure.Corp.Data.Security.and.Systems.Defense.Review.Task.3.docx)**
- **[Data Security and Systems Defense Artifacts](https://github.com/user-attachments/files/30113214/Data.Security.and.Systems.Defense.Artifacts.docx)**

---

## Key Takeaways

This assessment shows that FinSecure’s current security controls are weakened by legacy protocols, unencrypted data, poor key storage, limited network segmentation, decentralized monitoring, and insecure privileged-access practices.

Replacing FTP with SFTP, encrypting the Finance database with AES-256, separating encryption keys from protected systems, creating a DMZ, segmenting internal systems, protecting privileged access, and deploying SIEM and EDR would significantly improve FinSecure’s ability to protect sensitive information and respond to security incidents.

---

## References

- Joint Task Force. (2020). *Security and Privacy Controls for Information Systems and Organizations*. NIST Special Publication 800-53 Revision 5.
- Kent, K., & Souppaya, M. (2006). *Guide to Computer Security Log Management*. NIST Special Publication 800-92.
- National Institute of Standards and Technology. (2023). *Advanced Encryption Standard*. FIPS 197.
- Rose, S., Borchert, O., Mitchell, S., & Connelly, S. (2020). *Zero Trust Architecture*. NIST Special Publication 800-207.

---

## Author

**Dontrell Wilson**  
Cybersecurity and Information Assurance Student  
CompTIA A+ | Network+ | Security+ | ITIL 4 Foundation
