# OpenDLP-LK Terminology Guide

A comprehensive guide to terminology used in OpenDLP-LK discussions, documentation, and implementations.

## Table of Contents
- [PDPA (Personal Data Protection Act)](#pdpa-personal-data-protection-act)
- [Data Patterns](#data-patterns)
- [DLP Workflow](#dlp-workflow)
- [Implementation Terms](#implementation-terms)
- [Sri Lankan Context](#sri-lankan-context)

---

## PDPA (Personal Data Protection Act)

### Core PDPA Terms

**PDPA (Personal Data Protection Act)**
- Sri Lanka's data protection law (Act No. 9 of 2022)
- Governs collection, use, and protection of personal data
- Similar to EU's GDPR but tailored for Sri Lankan context

**Personal Data**
- Any information relating to an identified or identifiable natural person
- Examples: Name, NIC, email, phone number, biometric data, IP address

**Sensitive Personal Data**
- Special category of personal data requiring additional protection
- Examples: Health data, financial data, biometric data, criminal records

**Data Controller**
- Entity that determines purposes and means of processing personal data
- Typically your organization if you collect customer/employee data

**Data Processor**
- Entity that processes personal data on behalf of data controller
- Examples: Cloud service providers, payroll processors, marketing agencies

**Data Protection Officer (DPO)**
- Designated person responsible for PDPA compliance
- Required for certain organizations under PDPA
- Acts as liaison with data protection authority

**Consent**
- Freely given, specific, informed agreement to process personal data
- Must be documented and verifiable
- Can be withdrawn at any time

**Data Subject**
- The individual whose personal data is being processed
- Has rights under PDPA (access, rectification, erasure, etc.)

**Data Protection Impact Assessment (DPIA)**
- Systematic assessment of privacy risks
- Required for high-risk processing activities
- Helps identify and mitigate privacy issues

**Breach Notification**
- Requirement to report data breaches to authority and affected individuals
- Timeline: As soon as practicable (typically 72 hours to authority)
- Must document nature, impact, and remediation of breach

---

## Data Patterns

### Sri Lankan Data Patterns

**NIC (National Identity Card)**
- **Old Format**: 9 digits followed by V or X
  - Format: `YYDDDNNNNV` (Y=year, D=day of year, N=serial, V/X=suffix)
  - Example: `923561234V` (born in 1992, day 356, male)
  - Female day numbers: 001-366 + 500 = 501-866

- **New Format**: 12 digits
  - Format: `YYYYDDDNNNNN` (YYYY=year, DDD=day, N=serial)
  - Example: `199912534001` (born in 1999, day 125)
  - Female day numbers: 501-866

**Passport Number**
- Format: N followed by 7 digits
- Example: `N1234567`
- Sri Lankan passport issued by Department of Immigration and Emigration

**TIN (Tax Identification Number)**
- Unique identifier for tax purposes
- Format varies by type (individual vs. corporate)

**Bank Account Number**
- Format varies by bank
- Common patterns include 10-16 digit account numbers
- Different formats for savings, current, and other account types

**Mobile Phone Number**
- Format: +94 XX XXX XXXX
- Country code: +94 (or 94)
- Area codes vary by operator (70, 71, 72, 75, 76, 77, 78)
- Example: +94 77 123 4567

**Landline Phone Number**
- Format: +94 XX XXX XXXX
- Area codes indicate geographic region
- Example: +94 11 234 5678 (Colombo)

---

## DLP Workflow

### Detection Phase

**Data Discovery**
- Process of identifying where sensitive data resides
- Scanning file systems, databases, emails, cloud storage
- Creating inventory of sensitive data locations

**Pattern Matching**
- Using regular expressions (regex) to identify sensitive data
- Matching against known data patterns (NIC, passport, etc.)
- Primary detection mechanism in DLP

**Content Analysis**
- Examining file content for sensitive information
- Can include: Text extraction, OCR, metadata analysis
- Goes beyond simple pattern matching

**Fingerprinting**
- Creating unique signatures for sensitive documents
- Allows detection of protected documents even if modified
- Also called "document matching" or "exact data matching"

**Context Analysis**
- Evaluating surrounding context of detected data
- Helps reduce false positives
- Examples: Proximity to keywords, document classification, user context

**Confidence Score**
- Numerical rating of detection accuracy
- Higher score = more confident the detection is accurate
- Used to determine appropriate action (auto-block vs. manual review)

### Prevention Phase

**Policy**
- Set of rules defining what to detect and how to respond
- Includes: What data to protect, where to monitor, what actions to take

**Rule**
- Individual component of a policy
- Defines specific conditions and actions
- Multiple rules can make up a policy

**Action**
- What happens when sensitive data is detected
- Common actions: Block, Alert, Encrypt, Quarantine, Log

**Enforcement Point**
- Where DLP controls are applied
- Examples: Email gateway, web proxy, endpoint, network, cloud

**Exception**
- Condition that allows bypass of DLP policy
- Should be documented and reviewed regularly
- Examples: Security team exclusions, executive exemptions

**Quarantine**
- Isolated area where suspicious content is held
- Allows for manual review before delivery/release
- Prevents potential data loss while under investigation

### Response Phase

**Alert**
- Notification generated when policy violation occurs
- Can be sent to security team, user, or manager
- Should include: What was detected, where, when, by whom

**Incident**
- Confirmed policy violation requiring investigation
- May require formal incident response process
- Should be documented for audit and compliance

**False Positive**
- Alert triggered incorrectly (no actual policy violation)
- Common challenge in DLP implementations
- Reduced through tuning and context analysis

**False Negative**
- Sensitive data that was NOT detected by DLP
- More serious than false positives
- Identified through testing and validation

**Remediation**
- Actions taken to address policy violation
- Examples: Remove sensitive data, encrypt file, revoke access, user training

**Audit Trail**
- Record of all DLP detections, actions, and decisions
- Required for compliance and forensics
- Should be tamper-proof and retained per policy

---

## Implementation Terms

**Deployment Model**
- How DLP solution is deployed in your environment
- Options: On-premises, Cloud, Hybrid, SaaS

**Endpoint DLP**
- DLP agent installed on user devices (laptops, desktops, mobile)
- Monitors data in use, in motion, and at rest on endpoints
- Controls: USB, print, screen capture, clipboard, local apps

**Network DLP**
- DLP appliance monitoring network traffic
- Detects data in motion across network
- Typically deployed at network perimeter or key segments

**Cloud DLP**
- DLP capabilities for cloud services and SaaS applications
- Examples: Microsoft 365, Google Workspace, Salesforce
- May use API-based or proxy-based approaches

**Data in Motion**
- Data actively moving across network
- Examples: Email, web uploads, file transfers, messaging

**Data at Rest**
- Data stored on systems
- Examples: File shares, databases, cloud storage, backups

**Data in Use**
- Data being actively accessed or processed
- Examples: Open documents, data in memory, displayed on screen

**Policy Tuning**
- Process of refining DLP policies to improve accuracy
- Goals: Reduce false positives, improve detection, optimize performance

**Scan Schedule**
- When DLP performs data discovery scans
- Considerations: Business hours impact, resource usage, scan duration

**Exception Management**
- Process for handling policy exceptions
- Should include: Request process, approval workflow, documentation, review

---

## Sri Lankan Context

### Government and Regulatory

**Data Protection Authority**
- Government body overseeing PDPA compliance
- Responsible for enforcement and guidance
- Receives breach notifications

**Personal Data Protection Commission**
- Commission established under PDPA
- Develops guidelines and regulations
- Handles complaints and investigations

### Business Terms

**Data Champion**
- Individual responsible for data protection in their department
- Part of OpenDLP-LK's Data Champions Program
- Acts as liaison between department and DPO/security team

**Data Champions Program**
- OpenDLP-LK initiative to build data protection capability
- Provides training, certification, and community support
- Creates network of data protection advocates

### Technical Terms

**OpenDLP-LK**
- Open-source initiative for DLP in Sri Lanka
- Provides: Implementation guides, data patterns, compliance toolkit, community

**PDPA Compliance Toolkit**
- Collection of templates, checklists, and tools
- Helps organizations achieve PDPA compliance
- Available on OpenDLP-LK GitHub

**Sri Lanka Data Patterns Library**
- Repository of Sri Lankan-specific detection patterns
- Includes: NIC, passport, bank accounts, phone numbers
- Community-contributed and validated

---

## Usage in Discussions

When participating in OpenDLP-LK discussions:

1. **Use consistent terminology** from this guide
2. **Explain acronyms** on first use (except widely known ones)
3. **Provide context** for Sri Lankan-specific terms when discussing with international audience
4. **Link to this guide** when introducing complex terms
5. **Suggest additions** to this guide as terminology evolves

---

## Abbreviations Quick Reference

| Abbreviation | Full Term | Category |
|--------------|-----------|----------|
| PDPA | Personal Data Protection Act | Legal |
| DPO | Data Protection Officer | Role |
| DPIA | Data Protection Impact Assessment | Process |
| NIC | National Identity Card | Data Pattern |
| TIN | Tax Identification Number | Data Pattern |
| DLP | Data Loss Prevention | Technology |
| SIEM | Security Information and Event Management | Technology |
| OCR | Optical Character Recognition | Technology |
| API | Application Programming Interface | Technology |
| SaaS | Software as a Service | Technology |
| GDPR | General Data Protection Regulation | Legal (EU) |
| KYC | Know Your Customer | Business Process |
| HR | Human Resources | Department |

---

## Contributing to This Guide

This terminology guide is a living document. To suggest additions or corrections:

1. Open a discussion in the General category
2. Tag with `terminology` and `documentation`
3. Provide the term, definition, and context
4. Explain why it should be added/changed

---

**Last Updated:** 2025-11-24  
**Version:** 1.0
