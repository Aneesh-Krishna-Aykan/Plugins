---
name: sharetribe-email-sender
description: Generate email sender API endpoints for Sharetribe marketplace. Use when creating any transactional email endpoint including invitations, notifications, access requests, OTP verification, credential sharing, approval/rejection emails, profile sharing, or meeting notifications. This skill handles complete endpoint creation with HTML email templates, optional encryption for secure links, SendGrid integration, MongoDB operations, and route registration.
---

# Sharetribe Email Sender

Generate complete email sender API endpoints following established project patterns.

## Email Categories

| Category | Examples |
|----------|----------|
| **Invitations** | Consultant invite, vendor invite, job/RFP posting invite |
| **Notifications** | Meeting scheduled, profile added, employer notification |
| **Access Control** | Request access, approve access, reject access |
| **Verification** | OTP emails, secure verification codes |
| **Sharing** | Profile sharing, credential sharing |

## Workflow

### Step 1: Gather Requirements

Ask the user:
1. **Email category** - Invitation, notification, access control, verification, or sharing?
2. **Email name** (e.g., "TeamMemberInvite", "AccessApproved")
3. **Email purpose** - What triggers this email?
4. **Required fields** - Recipient info, sender info, context data
5. **CTA action** - Button action (signup, login, view, external link, none)
6. **Database operations** - MongoDB read/write needed?
7. **Secure link** - Encryption required for URL parameters?
8. **Batch or single** - Multiple recipients or one at a time?

### Step 2: Select Pattern

Choose the appropriate pattern based on requirements:

**Pattern A: Simple Notification** (no DB, no encryption)
- Meeting scheduled, employer notification
- See [references/api-patterns.md](references/api-patterns.md#pattern-a-simple-notification)

**Pattern B: Batch Invitations** (multiple recipients)
- Consultant invite, vendor invite
- See [references/api-patterns.md](references/api-patterns.md#pattern-b-batch-invitations)

**Pattern C: With Listing Update** (Sharetribe SDK)
- Posting invitations that track invited users
- See [references/api-patterns.md](references/api-patterns.md#pattern-c-with-listing-update)

**Pattern D: MongoDB Operations** (access control)
- Request access, approve/reject access
- See [references/api-patterns.md](references/api-patterns.md#pattern-d-mongodb-operations)

**Pattern E: Secure Share Links** (encryption + MongoDB)
- Profile sharing with OTP verification
- See [references/api-patterns.md](references/api-patterns.md#pattern-e-secure-share-links)

**Pattern F: OTP Verification**
- Secure verification code emails
- See [references/api-patterns.md](references/api-patterns.md#pattern-f-otp-verification)

### Step 3: Generate Endpoint

Create file at `server/api/send{EmailName}.js` or `server/api/{action}{Entity}.js`

Basic structure:
```javascript
const sgMail = require('@sendgrid/mail');
// Add other imports based on pattern

module.exports = async (req, res) => {
  const { /* destructure body */ } = req?.body || {};

  // Validate required fields
  if (!requiredField) {
    return res.status(400).send({ error: 'Missing required fields' });
  }

  try {
    // Build email content (see references/email-templates.md)
    const emailContent = `...`;

    // Send via SendGrid
    sgMail.setApiKey(process.env.SENDGRID_API_KEY);
    const msg = {
      from: process.env.SENDGRID_SENDER_EMAIL,
      to: recipientEmail,
      subject: 'Subject line',
      html: emailContent
    };
    await sgMail.send(msg);

    res.json({ success: true, message: 'Email sent' });
  } catch (error) {
    console.error('Error:', error);
    res.status(500).json({ error: 'Failed to send email', details: error.message });
  }
};
```

### Step 4: Register Route

Add to `server/apiRouter.js`:
```javascript
// Import
const sendEmailName = require('./api/sendEmailName');
// Route
router.post('/sendEmailName', sendEmailName);
```

### Step 5: Verification Checklist

- [ ] Required imports present
- [ ] Input validation complete
- [ ] HTML template follows project styling
- [ ] SendGrid configuration correct
- [ ] Error handling with try-catch
- [ ] Route registered in apiRouter.js
- [ ] MongoDB client closed in finally block (if used)

## Reference Files

- **Email templates**: See [references/email-templates.md](references/email-templates.md) for HTML structure and all template types
- **API patterns**: See [references/api-patterns.md](references/api-patterns.md) for complete code patterns

## Existing Endpoints Reference

| File | Category | Pattern |
|------|----------|---------|
| `sendConsultantInvite.js` | Invitation | Batch |
| `sendVendorInvite.js` | Invitation | Batch + conditional templates |
| `sendPostingInvitation.js` | Invitation | Listing update |
| `sendMeetingScheduledMail.js` | Notification | Simple |
| `sendConsCurrEmpMail.js` | Notification | Simple + conditional |
| `requestProfileAccess.js` | Access Control | MongoDB + notification |
| `approveAccessRequest.js` | Access Control | MongoDB + secure link |
| `rejectAccessRequest.js` | Access Control | MongoDB + notification |
| `shareProfile.js` | Sharing | Secure link + MongoDB |
| `requestShareOtp.js` | Verification | OTP + MongoDB |
| `shareCredential.js` | Sharing | QR code + external API |

## Quick Reference

### Required Environment Variables
```
SENDGRID_API_KEY
SENDGRID_SENDER_EMAIL
REACT_APP_MARKETPLACE_ROOT_URL
MONGODB_CONNECTIONSTRING (if using MongoDB)
SHARETRIBE_INTEGRATION_CLIENT_ID (if using SDK)
SHARETRIBE_INTEGRATION_CLIENT_SECRET (if using SDK)
```

### Common Imports by Pattern
```javascript
// All emails
const sgMail = require('@sendgrid/mail');

// Encryption (secure links)
const CryptoJS = require('crypto-js');
const pako = require('pako');

// MongoDB operations
const { MongoClient, ObjectId } = require('mongodb');

// Sharetribe SDK (listing updates)
const flexIntegrationSdk = require('sharetribe-flex-integration-sdk');

// OTP generation
const crypto = require('crypto');

// QR codes (credential sharing)
const QRCode = require('qrcode');
```
