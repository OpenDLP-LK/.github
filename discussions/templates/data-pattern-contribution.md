# Data Pattern Contribution Template

Use this template when contributing a new data pattern or suggesting improvements to existing patterns.

---

## Pattern Name
[Clear, descriptive name for the data pattern]

**Example:** "Sri Lankan National Identity Card - New Format (12 digits)"

---

## Pattern Information

### Pattern Type
- [ ] Identity Document
- [ ] Financial Data
- [ ] Healthcare Data
- [ ] Contact Information
- [ ] Organizational Data
- [ ] Custom/Other: _______________

### Data Category
[What type of sensitive data does this pattern detect?]

---

## Pattern Definition

### Description
[Detailed description of what this pattern detects and its purpose]

### Regular Expression
```regex
[Your regex pattern here]
```

### Alternative Patterns (if any)
```regex
[Alternative regex patterns for variations]
```

---

## Validation Rules

### Format Requirements
[Describe the format rules and structure]

Example:
- Position 1-4: Year of birth (YYYY)
- Position 5-7: Day of year (001-366)
- Position 8-12: Serial number
- Checksum: [If applicable]

### Validation Algorithm
```
[Pseudocode or description of validation logic]
```

---

## Test Cases

### Valid Examples (Sanitized)
```
1. [Example 1 - with explanation]
2. [Example 2 - with explanation]
3. [Example 3 - with explanation]
```

### Invalid Examples (Should NOT Match)
```
1. [Example 1 - with reason why invalid]
2. [Example 2 - with reason why invalid]
3. [Example 3 - with reason why invalid]
```

### Edge Cases
```
1. [Edge case 1]
2. [Edge case 2]
3. [Edge case 3]
```

---

## Pattern Performance

### Confidence Level
- [ ] Very High (>95%) - Well-defined format with validation
- [ ] High (85-95%) - Standard format with minor variations
- [ ] Medium (70-85%) - Common format with some ambiguity
- [ ] Low (<70%) - Requires additional context

### False Positive Risk
- [ ] Very Low - Highly specific pattern
- [ ] Low - Some risk but manageable
- [ ] Medium - Needs context-based filtering
- [ ] High - Requires additional validation

### Performance Impact
- [ ] Minimal - Simple, efficient regex
- [ ] Low - Standard complexity
- [ ] Medium - Complex pattern, moderate overhead
- [ ] High - Requires significant processing

---

## Implementation Details

### Use Cases
[Where and how should this pattern be used?]

Examples:
- Employee record systems
- Customer databases
- Document scanning
- Email content filtering

### Recommended Actions
When this pattern is detected:
- [ ] Block/Prevent
- [ ] Alert/Notify
- [ ] Encrypt
- [ ] Quarantine
- [ ] Log only
- [ ] Other: _______________

### Context Requirements
[Any additional context needed for accurate detection?]

---

## Integration

### Compatible Systems
- [ ] DLP Tools: [List specific tools if known]
- [ ] SIEM Systems
- [ ] Document Management Systems
- [ ] Email Security Gateways
- [ ] Other: _______________

### Configuration Example
```yaml
# Example configuration (adjust for your DLP tool)
pattern:
  name: "Sri Lankan NIC"
  regex: "[your-regex-here]"
  confidence: "high"
  action: "alert"
  validation: true
```

---

## Sources and References

### Official Documentation
- [Link to official format specifications]
- [Link to regulatory guidelines]

### Research and Testing
- Testing methodology: [Describe how you validated this pattern]
- Sample size: [Number of test cases]
- Testing environment: [Where you tested]

---

## Pattern Metadata

### Author Information
- GitHub Username: @[your-username]
- Date Submitted: [YYYY-MM-DD]
- Version: [1.0]

### License
- [ ] MIT License
- [ ] Apache 2.0
- [ ] Public Domain
- [ ] Other: _______________

---

## Checklist

Before submitting, ensure you have:

- [ ] Tested the pattern with multiple valid examples
- [ ] Tested with invalid examples to check false positives
- [ ] Sanitized all example data (no real personal information)
- [ ] Documented validation logic clearly
- [ ] Provided performance characteristics
- [ ] Included usage recommendations
- [ ] Tagged the discussion appropriately
- [ ] Reviewed existing patterns for duplicates

---

## Tags

Select appropriate tags:
- [ ] `data-pattern`
- [ ] `pattern-contribution`
- [ ] `nic-pattern`
- [ ] `bank-account`
- [ ] `passport`
- [ ] `validation`
- [ ] `regex`
- [ ] Other: _______________

---

**Security Reminder:** Never include real personal data in your examples. Use sanitized, anonymized, or synthetic data only.
