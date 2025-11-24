# Example: How to Validate New Format Sri Lankan NIC Numbers?

**Category:** Data Patterns  
**Tags:** `data-pattern`, `nic-pattern`, `validation`, `sri-lanka`

---

## Background

I'm implementing DLP for a financial services company in Sri Lanka, and we need to detect the new 12-digit National Identity Card (NIC) format that was introduced recently. The old 9-digit+V/X format is well-documented, but I'm having trouble with the new format validation.

---

## Question

How can I create an accurate regex pattern and validation algorithm for the new 12-digit Sri Lankan NIC format?

Specifically:
1. What is the exact structure of the 12-digit NIC?
2. Is there a checksum or validation algorithm?
3. How can I differentiate between the new NIC and other 12-digit numbers?

---

## What I Know So Far

### Format
From my research, the new NIC format is:
- 12 digits total
- First 4 digits: Year of birth (YYYY)
- Next 3 digits: Day of year (001-366, add 500 for females)
- Last 5 digits: Serial number

Example structure: `199912534001`
- `1999` - Year of birth
- `125` - Day of year (125th day = May 5th for male)
- `34001` - Serial number

### Current Pattern Attempt
```regex
\b[12][09][0-9]{2}[0-9]{3}[0-9]{5}\b
```

This matches 12-digit numbers starting with 19 or 20 (for years), but it's not specific enough.

---

## Issues

1. **False Positives**: This pattern matches many 12-digit numbers that aren't NICs
2. **Day of Year Validation**: How do I validate that digits 5-7 are between 001-866 (001-366 or 501-866)?
3. **No Checksum**: Is there any checksum or validation digit in the new format?

---

## Test Cases

### Should Match (Valid NICs)
```
199912534001  # Male born May 5, 1999
200063199876  # Female born June 11, 2000 (631 = 131 + 500)
198700112345  # Male born Jan 1, 1987
```

### Should NOT Match
```
199999934001  # Invalid day (999)
180012534001  # Year too old (1800)
202512534001  # Future year
123456789012  # Random 12 digits
```

---

## Usage Context

We need this pattern for:
- Employee HR databases
- Customer KYC records  
- Document scanning (PDFs, images with OCR)
- Email content filtering

---

## Ideal Solution

An ideal solution would include:
1. Regex pattern for initial matching
2. Additional validation logic for day-of-year
3. Examples of implementation in common DLP tools
4. Guidance on reducing false positives

---

## Additional Information

- **Environment**: Using Microsoft Purview DLP
- **Scale**: ~500 employees, ~10,000 customer records
- **Compliance**: PDPA requirement to protect personal identifiers

---

Has anyone successfully implemented new format NIC detection? Any insights would be greatly appreciated!

---

## Comments / Discussion

**@data-champion-sri-lanka** commented:

Great question! Here's what I've learned from implementing this:

### Enhanced Regex Pattern

```regex
\b(19[0-9]{2}|20[0-2][0-9])(([0-3][0-9]{2})|([5-8][0-9]{2}))[0-9]{5}\b
```

This pattern:
- Matches years 1900-2029: `(19[0-9]{2}|20[0-2][0-9])`
- Matches days 000-399 OR 500-899: `(([0-3][0-9]{2})|([5-8][0-9]{2}))`
- Matches 5-digit serial: `[0-9]{5}`

### Validation Logic

For additional validation, I use this pseudocode:

```python
def validate_new_nic(nic_string):
    if len(nic_string) != 12:
        return False
    
    year = int(nic_string[0:4])
    day_code = int(nic_string[4:7])
    
    # Check year range
    if year < 1900 or year > 2029:
        return False
    
    # Check day of year
    is_female = day_code > 500
    actual_day = day_code - 500 if is_female else day_code
    
    if actual_day < 1 or actual_day > 366:
        return False
    
    # Additional check: day 366 only valid for leap years
    if actual_day == 366:
        if not is_leap_year(year):
            return False
    
    return True
```

### DLP Implementation

In Microsoft Purview, I created a custom sensitive info type:

1. **Primary Pattern**: The regex above
2. **Supporting Element**: Additional keyword proximity ("NIC", "National ID", "Identity Card" within 300 characters)
3. **Confidence Level**: High (85+)

This approach reduced false positives by 90% compared to regex-only matching.

---

**@dlp-implementer** commented:

I'd add one more consideration - **context-based validation**:

When scanning documents, look for:
- Proximity to labels: "NIC:", "NIC No:", "Identity Card Number:"
- Document type: Official forms, HR documents, KYC forms
- Field names in structured data: `nic_number`, `national_id`, etc.

This helps differentiate NICs from other 12-digit numbers like:
- Phone numbers with country code
- Transaction IDs
- Serial numbers

---

**@security-analyst-lk** commented:

Don't forget about data format variations in real-world data:

```
# Common variations you might encounter:
199912534001          # Standard
1999 125 34001        # With spaces
1999-125-34001        # With dashes
NIC: 199912534001     # With label
199912534001 (NIC)    # With annotation
```

Your regex should handle these or use preprocessing to normalize the format.

---

**Original Poster** commented:

Thank you all! This is incredibly helpful. I've implemented the enhanced pattern with validation logic and it's working great. 

**Final Solution Summary:**
1. Enhanced regex for initial detection
2. Python validation function for day-of-year logic
3. Context-based scoring in DLP policy
4. Preprocessing to handle format variations

Detection accuracy improved from 65% to 95%, false positives down by 85%.

✅ **Marking @data-champion-sri-lanka's answer as the solution**
