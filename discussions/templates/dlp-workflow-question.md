# DLP Workflow Question Template

Use this template when asking technical questions about DLP workflow implementation, configuration, or operations.

---

## Question Title
[Clear, specific title describing your DLP workflow question]

**Example:** "How to configure email DLP to detect NIC numbers in attachments?"

---

## Environment Information

### DLP Solution
- **Product/Tool:** [Name and version of your DLP solution]
- **Deployment Type:** [Cloud / On-premises / Hybrid]
- **Components:** [Endpoint agents, Network appliances, Cloud connectors, etc.]

### Infrastructure
- **Operating System:** [Windows, Linux, macOS, etc.]
- **Scale:** [Number of endpoints, users, data volume]
- **Network Architecture:** [Relevant network topology details]

---

## Workflow Phase

Select the primary phase your question relates to:

- [ ] **Detection** - Data discovery, scanning, pattern matching
- [ ] **Prevention** - Policy enforcement, blocking, encryption
- [ ] **Response** - Incident management, remediation, reporting
- [ ] **Integration** - System connectivity, APIs, automation
- [ ] **Operations** - Maintenance, tuning, optimization

---

## Problem Description

### Current Situation
[Describe what you're trying to accomplish or the problem you're facing]

### Expected Behavior
[What should happen or what are you trying to achieve?]

### Actual Behavior
[What is actually happening? Include specific symptoms or issues]

---

## Configuration Details

### Current Policy/Rule
```yaml
# Paste your current policy configuration (sanitize sensitive info)
# Example:
policy_name: "Detect NIC Numbers"
pattern: "\\b[0-9]{9}[VvXx]\\b"
action: "alert"
channels: ["email", "web"]
```

### Relevant Settings
[List any relevant configuration settings or parameters]

---

## What You've Tried

1. [First troubleshooting step you attempted]
   - Result: [What happened]

2. [Second troubleshooting step]
   - Result: [What happened]

3. [Additional attempts]
   - Result: [What happened]

---

## Questions

### Specific Questions
1. [Your first specific question]
2. [Your second specific question]
3. [Additional questions]

### Clarifications Needed
- [Any uncertainties or areas where you need guidance]

---

## Additional Context

### Logs and Error Messages
```
[Paste relevant log entries or error messages - sanitize sensitive data]
```

### Screenshots
[Attach screenshots if helpful - ensure no sensitive data is visible]

### Performance Metrics
- Scan duration: [if relevant]
- Detection rate: [if relevant]
- False positive rate: [if relevant]

### Timeline
- When did this issue start? [Date/time or "always been this way"]
- Recent changes: [Any recent configuration or system changes]

---

## Business Requirements

### Compliance Needs
[PDPA or other compliance requirements driving this implementation]

### Use Case Priority
- [ ] Critical - Production issue affecting operations
- [ ] High - Planned implementation, time-sensitive
- [ ] Medium - Optimization or improvement
- [ ] Low - Research or future planning

### Constraints
- Performance requirements: [e.g., minimal impact on user experience]
- Budget limitations: [if relevant]
- Timeline: [when you need this working]

---

## Expected Outcome

### Success Criteria
[What would a successful solution look like?]

### Acceptance Criteria
- [ ] [Criterion 1]
- [ ] [Criterion 2]
- [ ] [Criterion 3]

---

## Research Completed

### Documentation Reviewed
- [ ] Official product documentation
- [ ] Implementation guide
- [ ] Knowledge base articles
- [ ] Community discussions

### Similar Issues
[Links to similar discussions or issues you've found]

---

## Checklist

Before posting, ensure you have:

- [ ] Searched existing discussions for similar questions
- [ ] Reviewed official documentation
- [ ] Sanitized all sensitive information (credentials, data, IPs)
- [ ] Included version information
- [ ] Described what you've already tried
- [ ] Provided sufficient technical details
- [ ] Tagged the discussion appropriately
- [ ] Chosen the correct category (DLP Workflow)

---

## Tags

Select appropriate tags:
- [ ] `dlp-workflow`
- [ ] `detection`
- [ ] `prevention`
- [ ] `incident-response`
- [ ] `policy-configuration`
- [ ] `integration`
- [ ] `performance`
- [ ] `troubleshooting`
- [ ] `email-dlp`
- [ ] `endpoint-dlp`
- [ ] Other: _______________

---

## Follow-up Commitment

If you find a solution:
- [ ] I will post the solution back to this discussion
- [ ] I will mark the helpful answer as the solution
- [ ] I will update the discussion if the situation changes

---

**Troubleshooting Tip:** Enable debug logging (if available) before posting technical issues. Debug logs often reveal the root cause more clearly.
