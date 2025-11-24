# Data Patterns Discussion Category

**Category:** Data Patterns  
**Icon:** 🔍  
**Description:** Share and discuss Sri Lankan-specific data patterns for DLP implementation.

## Purpose

This category focuses on identifying, validating, and sharing data patterns specific to Sri Lanka. Collaborate on creating accurate detection patterns for sensitive data formats used in Sri Lankan organizations.

## Topics Covered

### Identity Documents
- **National Identity Card (NIC) patterns**
  - Old format (9 digits + V/X)
  - New format (12 digits)
  - Validation algorithms
  - Format variations

- **Passport numbers**
  - Sri Lankan passport formats
  - Validation rules
  - International travel documents

- **Driving License numbers**
  - Format patterns
  - Regional variations

### Financial Data
- **Bank account numbers**
  - Different bank formats
  - IBAN/SWIFT codes
  - Credit/debit card patterns

- **Tax identification**
  - Tax Identification Number (TIN)
  - VAT registration numbers
  - EPF/ETF numbers

- **Payment information**
  - Mobile payment identifiers
  - Digital wallet formats
  - Payment card data

### Healthcare Data
- **Medical record identifiers**
  - Patient ID formats
  - Medical registration numbers
  - Health insurance numbers

- **Prescription numbers**
- **Lab report identifiers**

### Organizational Data
- **Employee identification**
  - Employee numbers
  - Payroll identifiers
  - Social security numbers (if applicable)

- **Student identification**
  - Student ID formats
  - Registration numbers
  - Exam index numbers

### Contact Information
- **Phone numbers**
  - Mobile: +94 XX XXX XXXX
  - Landline: +94 XX XXX XXXX
  - Validation patterns

- **Email addresses**
  - Government domains (.gov.lk)
  - Educational domains (.edu.lk)
  - Commercial domains (.lk)

- **Postal codes and addresses**
  - Sri Lankan postal code format
  - Address patterns

### Custom Patterns
- Industry-specific identifiers
- Organization-specific formats
- Custom validation logic

## Pattern Contribution Guidelines

### Pattern Submission Format
When sharing a new pattern, include:

1. **Pattern Name**: Clear, descriptive name
2. **Description**: What data it detects
3. **Regex Pattern**: The actual regular expression
4. **Test Cases**: Examples (sanitized)
5. **Validation Logic**: Any additional validation needed
6. **Use Cases**: Where this pattern is applicable
7. **Confidence Level**: How accurate is detection

### Example Pattern Contribution

```markdown
**Pattern Name:** Sri Lankan NIC (Old Format)

**Description:** Detects old format National Identity Cards (9 digits followed by V or X)

**Regex Pattern:** 
\b[0-9]{9}[VvXx]\b


**Test Cases:**
- Valid: 123456789V, 987654321X
- Invalid: 12345678V (too short), 1234567890V (too long)

**Validation Logic:**
- First 2 digits: Year of birth (00-99)
- Next 3 digits: Day of year (001-366 for females, add 500)
- Last 4 digits: Serial number
- Last character: V or X

**Use Cases:**
- Employee records
- Customer databases
- Government systems

**Confidence Level:** High (95%+) - well-defined format
```

## Pattern Testing and Validation

### Before Sharing
1. ✅ Test pattern with multiple valid examples
2. ✅ Test with edge cases
3. ✅ Check for false positives
4. ✅ Verify with real-world (sanitized) data
5. ✅ Document any limitations

### Pattern Quality Criteria
- **Accuracy**: Minimizes false positives/negatives
- **Performance**: Efficient regex execution
- **Completeness**: Covers format variations
- **Documentation**: Clear usage instructions

## Discussion Guidelines

### What to Post
- ✅ New data pattern proposals
- ✅ Pattern improvement suggestions
- ✅ Validation logic discussions
- ✅ False positive/negative reports
- ✅ Testing results and feedback
- ✅ Integration examples

### What Not to Post
- ❌ Real personal data (use sanitized examples)
- ❌ Unvalidated or untested patterns
- ❌ Patterns without proper documentation
- ❌ Sensitive organizational data

## Example Discussion Topics

1. **"Validating new format NIC (12 digits)"**
   - Pattern proposal
   - Checksum algorithm
   - Test cases and edge cases

2. **"Bank account number patterns for major Sri Lankan banks"**
   - Format survey across banks
   - Common patterns
   - Bank-specific variations

3. **"Improving mobile number detection accuracy"**
   - International format handling
   - Area code variations
   - Reducing false positives

4. **"Custom pattern for Sri Lankan passport numbers"**
   - Format analysis
   - Validation requirements
   - Integration with DLP tools

## Pattern Library Resources

- [Sri Lanka Data Patterns Repository](https://github.com/OpenDLP-LK/sri-lanka-data-patterns)
- [Pattern Testing Tools](https://opendlp-lk.github.io/tools/pattern-tester)
- [Implementation Guide - Data Patterns](https://opendlp-lk.github.io/implementation/data-patterns)

## Tags

Use these tags to improve discoverability:
- `data-pattern`
- `nic-pattern`
- `bank-account`
- `passport`
- `mobile-number`
- `validation`
- `regex`
- `pattern-contribution`
- `false-positive`

---

**Security Note:** Always use sanitized, anonymized, or synthetic data when sharing examples. Never post real personal or sensitive data.
