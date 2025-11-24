# Example: Implementing PDPA Consent Management for Email Marketing

**Category:** PDPA Compliance  
**Tags:** `pdpa-compliance`, `consent-management`, `marketing`, `email`

---

## Question

We're a mid-sized e-commerce company in Sri Lanka looking to implement PDPA-compliant consent management for our email marketing campaigns. We currently have a subscriber list of ~50,000 customers, but we're not sure if we have proper consent records.

**Key Questions:**
1. What constitutes valid consent under PDPA?
2. Do we need to re-consent existing subscribers?
3. How should we document and manage consent?
4. What are the technical requirements for consent withdrawal?

---

## Our Current Situation

### Current Practices
- We collect email addresses at checkout
- We have a checkbox (pre-checked) for marketing emails
- No specific consent records in database
- Unsubscribe link in all emails (managed by MailChimp)
- No formal consent documentation

### What We've Done
- Read through PDPA Act No. 9 of 2022
- Reviewed privacy policies from similar companies
- Consulted with our legal team (they advised getting community input)

---

## Specific Concerns

1. **Pre-checked Boxes**: Our checkout form has a pre-checked box for marketing emails. Is this PDPA-compliant?

2. **Existing Subscribers**: We have 50,000 existing subscribers. Do we need to:
   - Re-consent all of them?
   - Document when they originally opted in?
   - Send a confirmation email?

3. **Consent Records**: What information should we store?
   - Just yes/no?
   - Timestamp of consent?
   - IP address?
   - Specific wording they agreed to?

4. **Withdrawal Process**: What's required for consent withdrawal?
   - One-click unsubscribe?
   - Confirmation email?
   - How quickly must we process?

---

## Business Context

- **Industry**: E-commerce (fashion and lifestyle)
- **Company Size**: 75 employees
- **Customer Base**: Primarily Sri Lankan, some international
- **Email Volume**: ~200,000 emails/month
- **Platform**: Using MailChimp for email campaigns

---

## What Success Looks Like

✅ PDPA-compliant consent collection process  
✅ Proper documentation and record-keeping  
✅ Easy withdrawal mechanism for users  
✅ Clear audit trail for compliance verification  
✅ Minimal disruption to existing operations  

---

## Timeline

- **Immediate**: Stop using pre-checked boxes
- **1 month**: Implement proper consent mechanism
- **3 months**: Full compliance with record-keeping

---

## Comments / Discussion

**@data-protection-officer** commented:

Great that you're taking PDPA compliance seriously! Here's guidance based on the PDPA requirements:

### 1. Valid Consent Requirements

Under PDPA, valid consent must be:
- **Freely given**: Not coerced or conditional
- **Specific**: For a particular purpose (marketing)
- **Informed**: User understands what they're agreeing to
- **Unambiguous**: Clear affirmative action

❌ **Pre-checked boxes DO NOT meet PDPA requirements**

✅ **You need:**
- Unchecked box by default
- Clear explanation of what they're consenting to
- Separate consent for different purposes (transactional vs. marketing)

### 2. Existing Subscribers - YES, You Need to Re-consent

**Why?**
- Your current consent mechanism (pre-checked box) is not PDPA-compliant
- You likely don't have proper consent records
- PDPA requires documented, verifiable consent

**How to Re-consent:**

```
Subject: Update Your Email Preferences - Action Required

Dear [Name],

As part of our commitment to your privacy and compliance with Sri Lanka's 
Personal Data Protection Act (PDPA), we're updating how we manage your 
email preferences.

We'd love to keep you updated on:
☐ New products and exclusive offers
☐ Sales and promotions  
☐ Style tips and fashion news

[Confirm My Preferences] button

If you don't respond within 30 days, we'll remove you from marketing emails
but will continue sending order updates and essential account information.

You can change your preferences anytime at [Preference Center Link]

Questions? Contact our Data Protection team at privacy@company.lk
```

### 3. Consent Documentation Requirements

Store the following for each consent:

```json
{
  "user_id": "12345",
  "email": "user@example.com",
  "consent_type": "email_marketing",
  "consent_given": true,
  "consent_timestamp": "2025-11-24T10:30:00Z",
  "consent_method": "web_form",
  "consent_version": "v2.1",
  "ip_address": "192.168.1.1",
  "user_agent": "Mozilla/5.0...",
  "consent_text": "I agree to receive marketing emails...",
  "double_opt_in_confirmed": true,
  "double_opt_in_timestamp": "2025-11-24T10:32:15Z"
}
```

### 4. Withdrawal Requirements

PDPA requires withdrawal to be:
- **As easy as giving consent**: If they can opt-in with one click, withdrawal should be one click
- **Immediate or near-immediate**: Process within 24-48 hours maximum
- **No barriers**: No login required, no explanation needed

**Recommended Implementation:**
- One-click unsubscribe link in every email
- Preference center for granular control
- Confirmation message (not a confirmation requirement)
- Suppression list to prevent re-adding

---

**@email-marketing-specialist** commented:

From a technical implementation perspective, here's what we did:

### Double Opt-In Flow

1. User checks (unchecked) marketing consent box at checkout
2. Complete the purchase (don't make it conditional)
3. Send confirmation email: "Please confirm your subscription"
4. User clicks confirmation link
5. Record confirmed consent with all metadata

### MailChimp Setup

In MailChimp:
1. **Enable Double Opt-In**: Settings → Audiences → Enable double opt-in
2. **Custom Fields**: Add fields for:
   - `consent_date`
   - `consent_method`
   - `consent_ip`
3. **Unsubscribe Settings**: Enable one-click unsubscribe
4. **Suppression List**: Automatically maintained by MailChimp

### Database Schema

```sql
CREATE TABLE email_consents (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id),
    email VARCHAR(255) NOT NULL,
    consent_type VARCHAR(50),
    consent_given BOOLEAN,
    consent_timestamp TIMESTAMP,
    consent_method VARCHAR(50),
    ip_address VARCHAR(45),
    user_agent TEXT,
    consent_version VARCHAR(10),
    withdrawn_timestamp TIMESTAMP,
    withdrawal_method VARCHAR(50)
);

CREATE INDEX idx_email_consents_user ON email_consents(user_id);
CREATE INDEX idx_email_consents_email ON email_consents(email);
```

---

**@legal-compliance-advisor** commented:

⚠️ **Important Legal Considerations:**

### Grace Period for Existing Subscribers

While you should re-consent as soon as possible, you typically have a grace period:
- Continue marketing to existing subscribers during re-consent campaign
- Actively attempt to obtain new consent
- Remove non-responding subscribers after 60-90 days
- Document your re-consent efforts

### Legitimate Interest vs. Consent

**You DON'T need consent for:**
- Transactional emails (order confirmations, shipping updates)
- Account security notifications
- Service updates that affect the customer

**You DO need consent for:**
- Marketing and promotional content
- Newsletter subscriptions
- Product recommendations (unless directly related to their purchase)

### Audit Trail Requirements

Keep records for at least **5 years** (some advise 7 years):
- Original consent records
- Any consent modifications
- Withdrawal requests
- Communication preferences

### PDPA Article References

- **Article 12**: Consent requirements
- **Article 13**: Right to withdraw consent
- **Article 27**: Data controller obligations
- **Article 33**: Record-keeping requirements

---

**Original Poster** commented:

This is incredibly helpful! Here's our action plan:

**Immediate Actions (This Week):**
- [ ] Change all signup forms to unchecked consent boxes
- [ ] Draft re-consent email campaign
- [ ] Set up database schema for consent records

**Short-term (1 Month):**
- [ ] Implement double opt-in flow
- [ ] Configure MailChimp with custom consent fields
- [ ] Launch re-consent campaign for existing subscribers
- [ ] Create preference center

**Long-term (3 Months):**
- [ ] Complete re-consent campaign
- [ ] Remove non-responding subscribers
- [ ] Implement automated consent documentation
- [ ] Train customer service team on consent procedures
- [ ] Schedule quarterly consent audit

**Documentation:**
- [ ] Update privacy policy
- [ ] Create consent FAQ for customers
- [ ] Document consent management procedures
- [ ] Create DPO compliance checklist

Thank you all for the detailed guidance! Marking @data-protection-officer's answer as the solution.

---

**Resources Shared:**
- [PDPA Consent Management Toolkit](https://github.com/OpenDLP-LK/pdpa-compliance-toolkit)
- [Email Consent Template](https://opendlp-lk.github.io/resources/templates/consent)
- [MailChimp GDPR/PDPA Guide](https://mailchimp.com/help/about-the-general-data-protection-regulation/)
