# Example: DLP Workflow for Detecting Sensitive Data in Shared Drives

**Category:** DLP Workflow  
**Tags:** `dlp-workflow`, `detection`, `file-scanning`, `shared-drives`, `implementation`

---

## Question

We're implementing DLP for a government organization in Sri Lanka with ~200 users. Our primary concern is detecting sensitive data (NICs, passport numbers, confidential documents) stored in shared network drives. 

**Specific Questions:**
1. What's the best workflow for scanning existing files?
2. How do we handle false positives without overwhelming our security team?
3. What detection strategy works for various file types (PDFs, Word docs, scanned images)?
4. How do we prevent future sensitive data from being stored insecurely?

---

## Environment Information

### Infrastructure
- **File Storage**: Windows Server 2019 file shares (5TB of data)
- **Users**: 200 employees across 5 departments
- **File Types**: Office documents (60%), PDFs (25%), Images (10%), Others (5%)
- **DLP Solution**: Evaluating options (Microsoft Purview, Symantec DLP, Forcepoint)

### Current State
- No existing DLP controls
- Files organized by department, but permissions are loose
- Many legacy files (some from 2010+)
- No consistent naming conventions
- Mix of English and Sinhala content

---

## Challenges

1. **Scale**: 5TB of existing data to scan
2. **False Positives**: Initial tests show 40-50% false positive rate
3. **Performance**: Can't impact user productivity during scans
4. **Resources**: 2-person IT security team
5. **Legacy Data**: Many old files that may contain sensitive data
6. **Multiple Languages**: Sinhala Unicode text detection

---

## Our Initial Approach

### Phase 1: Discovery (Weeks 1-2)
```
1. Run discovery scan (read-only mode)
2. Catalog sensitive data locations
3. Analyze file types and patterns
4. Identify high-risk areas
```

### Phase 2: Pilot (Weeks 3-4)
```
1. Select one department for pilot
2. Configure policies with alerts only
3. Tune patterns based on results
4. Train department staff
```

### Phase 3: Rollout (Weeks 5-8)
```
1. Deploy to all departments
2. Enable prevention controls gradually
3. Establish incident response workflow
4. Regular tuning and optimization
```

---

## What We Need Help With

### 1. Scanning Strategy
- Full scan vs. incremental scan?
- Scanning schedule (nights/weekends)?
- How to prioritize high-risk areas?
- Performance optimization tips?

### 2. Detection Patterns
We plan to detect:
- Sri Lankan NICs (old and new format)
- Passport numbers
- Bank account numbers
- Documents marked "Confidential"
- Government classification markings

How do we balance accuracy and completeness?

### 3. False Positive Management
With 2-person team, we can review ~50 alerts per day. How do we:
- Reduce false positives through tuning?
- Automate some review processes?
- Prioritize high-confidence alerts?

### 4. Prevention Workflow
Once detected, what should we do?
- Block file sharing?
- Encrypt automatically?
- Alert users and require action?
- Quarantine sensitive files?

---

## Comments / Discussion

**@dlp-architect** commented:

I've implemented similar DLP deployments for government organizations. Here's my recommended workflow:

### Scanning Strategy

**Phase 1: Quick Win Areas (Week 1)**
```
Priority 1: User home directories (highest risk, smallest volume)
Priority 2: Recently modified files (last 6 months)
Priority 3: Public/shared folders
Priority 4: Archive areas (low risk, can do later)
```

**Scanning Configuration:**
```yaml
initial_scan:
  mode: "discovery_only"  # No enforcement
  schedule:
    - time: "22:00-06:00"  # Off-hours
    - days: "weekdays"
  threads: 4  # Adjust based on server capacity
  throttle: "50%"  # Limit I/O impact
  
incremental_scan:
  mode: "continuous"
  monitor: "file_changes"  # Real-time for new/modified files
  batch_size: 1000
```

**Performance Tips:**
- Start with metadata-only scans (filename, path, owner)
- Then content scanning for high-risk files only
- Use file type filtering (skip .exe, .dll, system files)
- Implement file size limits (e.g., skip files >100MB initially)

### Detection Strategy with Accuracy Focus

**Tier 1: High Confidence (Auto-action)**
```yaml
patterns:
  - type: "nic_old_format"
    regex: '\b[0-9]{9}[VvXx]\b'
    context: ["NIC", "Identity", "Citizen"]
    confidence: 95
    action: "auto_encrypt"
    
  - type: "document_classification"
    keywords: ["TOP SECRET", "CONFIDENTIAL", "CLASSIFIED"]
    location: "header"
    confidence: 99
    action: "quarantine"
```

**Tier 2: Medium Confidence (Alert for review)**
```yaml
patterns:
  - type: "nic_new_format"
    regex: '\b[12][09][0-9]{10}\b'
    validation: "custom_nic_validator"
    confidence: 75
    action: "alert"
    
  - type: "passport"
    regex: '\b[N][0-9]{7}\b'
    context: ["passport", "travel"]
    confidence: 70
    action: "alert"
```

**Tier 3: Low Confidence (Log only)**
```yaml
patterns:
  - type: "potential_sensitive"
    keywords: ["confidential", "private", "sensitive"]
    confidence: 40
    action: "log"
```

### False Positive Reduction Techniques

**1. Context-Based Filtering**
```python
def validate_detection(match, context):
    # Check proximity to keywords
    if has_nearby_keywords(context, ["NIC", "Identity Card"], distance=50):
        confidence += 30
    
    # Check document type
    if document_type in ["HR_form", "KYC_document"]:
        confidence += 20
    
    # Check location sensitivity
    if path.startswith("/HR/Personnel/"):
        confidence += 20
    
    # Check file metadata
    if file_classification == "CONFIDENTIAL":
        confidence += 25
    
    return confidence > threshold
```

**2. Whitelist Approach**
```yaml
exceptions:
  - pattern: "sample_nic"  # Test data
    files: ["**/test/**", "**/samples/**"]
    
  - pattern: "nic"
    content_match: "Example: 123456789V"  # Documentation
    
  - pattern: "any"
    owner: "security_team"  # Security team files
    path: "/Security/DLP_Testing/**"
```

**3. Machine Learning Enhancement**
Consider implementing an ML classifier:
- Train on validated true/false positives
- Improve pattern matching over time
- Reduce manual review burden

### Recommended Workflow

```
┌─────────────────────────────────────────────────┐
│ 1. FILE SCAN                                    │
│    - Discovery scan identifies potential files  │
└──────────────────┬──────────────────────────────┘
                   │
                   ▼
┌─────────────────────────────────────────────────┐
│ 2. PATTERN MATCHING                             │
│    - Apply detection patterns                   │
│    - Calculate confidence scores                │
└──────────────────┬──────────────────────────────┘
                   │
                   ▼
┌─────────────────────────────────────────────────┐
│ 3. CONFIDENCE EVALUATION                        │
│    ├─ High (>85): Auto-action                   │
│    ├─ Medium (60-85): Alert for review          │
│    └─ Low (<60): Log only                       │
└──────────────────┬──────────────────────────────┘
                   │
                   ▼
┌─────────────────────────────────────────────────┐
│ 4. AUTOMATED ACTIONS (High Confidence)          │
│    - Encrypt file                               │
│    - Update permissions                         │
│    - Move to secure location                    │
│    - Notify file owner                          │
└──────────────────┬──────────────────────────────┘
                   │
                   ▼
┌─────────────────────────────────────────────────┐
│ 5. MANUAL REVIEW (Medium Confidence)            │
│    - Security team reviews alert                │
│    - Validates true/false positive              │
│    - Takes appropriate action                   │
│    - Feedback loop for tuning                   │
└──────────────────┬──────────────────────────────┘
                   │
                   ▼
┌─────────────────────────────────────────────────┐
│ 6. INCIDENT RESPONSE (If needed)                │
│    - Create incident ticket                     │
│    - Investigate data exposure                  │
│    - Document findings                          │
│    - Implement remediation                      │
└─────────────────────────────────────────────────┘
```

---

**@government-it-security** commented:

For government organizations, add these considerations:

### Classification-Based Workflow

```yaml
classification_levels:
  top_secret:
    detection: ["TOP SECRET", "TS//"]
    action: "immediate_quarantine"
    notification: "security_officer"
    storage: "classified_vault"
    
  secret:
    detection: ["SECRET", "S//"]
    action: "encrypt_and_alert"
    notification: "department_head"
    storage: "restricted_share"
    
  confidential:
    detection: ["CONFIDENTIAL", "C//"]
    action: "encrypt"
    notification: "file_owner"
    storage: "secure_share"
    
  unclassified:
    detection: ["UNCLASSIFIED", "U//"]
    action: "log_only"
    storage: "general_share"
```

### Compliance Requirements

For government PDPA compliance:
1. **Audit Trail**: Log all detections and actions
2. **Retention**: Keep logs for 7 years
3. **Reporting**: Monthly reports to DPO
4. **Access Control**: Role-based access to DLP console
5. **Breach Notification**: Automated alerts for high-risk findings

---

**@dlp-operations-specialist** commented:

### Alert Management Strategy for Small Teams

With only 2 people, you need efficient alert handling:

**Daily Alert Review Workflow (30-40 mins)**
```
08:00-08:15 (15 min): Review high-confidence alerts
  - Automated actions taken overnight
  - Verify no false positives in auto-actions
  - Escalate any issues

08:15-08:30 (15 min): Triage medium-confidence alerts
  - Sort by risk score
  - Quick review of top 20 alerts
  - Classify true/false positives

08:30-08:40 (10 min): Pattern tuning
  - Update patterns based on false positives
  - Add exception rules
  - Adjust confidence thresholds
```

**Weekly Review (2 hours)**
```
- Analyze detection trends
- Review false positive rate
- Update detection patterns
- User training for repeat offenders
- Report generation for management
```

### Automation Recommendations

**1. Auto-Remediation for Known Patterns**
```python
auto_actions = {
    "nic_in_public_share": [
        "move_to_secure_location",
        "encrypt_file",
        "notify_owner",
        "remove_public_access"
    ],
    
    "classified_doc_wrong_location": [
        "immediate_quarantine",
        "notify_security_officer",
        "create_incident_ticket",
        "block_user_access"
    ]
}
```

**2. Intelligent Alert Grouping**
```python
# Group similar alerts to reduce review burden
group_alerts_by = [
    "file_owner",      # All alerts for same user
    "file_location",   # All alerts in same folder
    "pattern_type",    # All NIC detections together
    "time_window"      # Within same 1-hour period
]
```

---

**Original Poster** commented:

Excellent guidance! Here's our revised implementation plan:

### Updated Timeline

**Week 1: Quick Wins**
- [ ] Scan user home directories (highest risk)
- [ ] Deploy high-confidence patterns only
- [ ] Set up automated encryption for NICs in public shares

**Week 2-3: Expand Coverage**
- [ ] Scan recently modified files (last 6 months)
- [ ] Add medium-confidence patterns with manual review
- [ ] Implement alert grouping for efficiency

**Week 4: Tuning**
- [ ] Analyze false positive rate
- [ ] Refine patterns based on results
- [ ] Add context-based filtering
- [ ] Train ML classifier on validated data

**Week 5-6: Department Rollout**
- [ ] Enable real-time monitoring for new files
- [ ] Deploy prevention controls for high-confidence detections
- [ ] User training per department
- [ ] Establish incident response process

**Week 7-8: Full Deployment**
- [ ] Complete scanning of remaining file areas
- [ ] Optimize performance
- [ ] Finalize automation rules
- [ ] Documentation and runbooks

### Key Decisions
1. **Confidence Tiers**: 85+ auto-action, 60-85 alert, <60 log
2. **Priority**: User dirs → Recent files → Public shares → Archives
3. **Team Focus**: 30-40 min daily alert review + weekly tuning
4. **Performance**: Night scanning + 50% I/O throttle

Thank you all! This is exactly what we needed. 🙏

✅ Marking @dlp-architect's answer as the solution
