# API Endpoint Patterns Reference

## Common Imports by Pattern

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

## Sharetribe Integration SDK Setup

```javascript
const integrationSdk = flexIntegrationSdk.createInstance({
  clientId: process.env.SHARETRIBE_INTEGRATION_CLIENT_ID,
  clientSecret: process.env.SHARETRIBE_INTEGRATION_CLIENT_SECRET,
  baseUrl: 'https://flex-integ-api.sharetribe.com',
});
```

## MongoDB Connection Pattern

```javascript
const MONGODB_CONNECTIONSTRING = process.env.MONGODB_CONNECTIONSTRING;

module.exports = async (req, res) => {
  const client = new MongoClient(MONGODB_CONNECTIONSTRING);

  try {
    await client.connect();
    const db = client.db('sharetribe');
    const collection = db.collection('collection_name');

    // ... operations

    res.json({ success: true });
  } catch (error) {
    console.error('Error:', error);
    res.status(500).json({ error: 'Failed', details: error.message });
  } finally {
    await client.close();
  }
};
```

---

## Pattern A: Simple Notification

Use for basic notifications without database or encryption.

**Examples**: Meeting scheduled, employer notification

```javascript
const sgMail = require('@sendgrid/mail');

module.exports = (req, res) => {
  const { recipientEmail, recipientName, senderName, contextData } = req?.body || {};

  if (!recipientEmail || !senderName) {
    return res.status(400).send({ error: 'Missing required fields' });
  }

  try {
    const marketplaceUrl = process.env.REACT_APP_MARKETPLACE_ROOT_URL;
    const emailContent = `...`; // Build HTML

    sgMail.setApiKey(process.env.SENDGRID_API_KEY);
    const msg = {
      from: process.env.SENDGRID_SENDER_EMAIL,
      to: recipientEmail,
      subject: 'Subject line',
      html: emailContent
    };

    sgMail.send(msg)
      .then(() => res.send({ success: true }))
      .catch(err => res.status(500).send({ error: 'Email failed', details: err }));

  } catch (e) {
    console.error('Unexpected error:', e);
    res.status(500).send({ error: 'Unexpected error', message: e.message });
  }
};
```

---

## Pattern B: Batch Invitations

Use when sending to multiple recipients in parallel.

**Examples**: Consultant invite, vendor invite

```javascript
const sgMail = require('@sendgrid/mail');
const CryptoJS = require('crypto-js');
const pako = require('pako');

async function sendInvitation(recipient, senderName, senderId, contextData) {
  // Encryption
  const secretKey = 'my-secret-key';
  const compress = data => Buffer.from(pako.deflate(data)).toString('base64');
  const base64url = source => source.replace(/=+$/, '').replace(/\+/g, '-').replace(/\//g, '_');

  const data = { senderId };
  const compressedData = compress(JSON.stringify(data));
  const encrypted = CryptoJS.AES.encrypt(compressedData, secretKey).toString();
  const encodedEncrypted = base64url(encrypted);

  const marketplaceUrl = `${process.env.REACT_APP_MARKETPLACE_ROOT_URL}/signuplink/?${encodedEncrypted}`;
  const emailContent = `...`; // Build HTML

  sgMail.setApiKey(process.env.SENDGRID_API_KEY);
  const msg = {
    from: process.env.SENDGRID_SENDER_EMAIL,
    subject: `Invitation from ${senderName}`,
    html: emailContent,
    personalizations: [{ to: [{ email: recipient.email }] }]
  };

  await sgMail.send(msg);
  return marketplaceUrl;
}

module.exports = async (req, res) => {
  const { recipients, senderName, senderId, contextData } = req?.body || {};

  try {
    if (!recipients || recipients.length === 0) {
      return res.status(400).send({ error: 'No recipients provided' });
    }

    const promises = recipients.map(r => sendInvitation(r, senderName, senderId, contextData));
    await Promise.all(promises);

    res.send({ success: true, message: `Sent ${recipients.length} invitation(s)` });
  } catch (e) {
    console.error('Unexpected error:', e);
    res.status(500).send({ error: 'Unexpected error', message: e.message });
  }
};
```

---

## Pattern C: With Listing Update

Use when invitation requires updating Sharetribe listing publicData.

**Examples**: Posting invitations that track invited users

```javascript
const flexIntegrationSdk = require('sharetribe-flex-integration-sdk');
const sgMail = require('@sendgrid/mail');

const integrationSdk = flexIntegrationSdk.createInstance({
  clientId: process.env.SHARETRIBE_INTEGRATION_CLIENT_ID,
  clientSecret: process.env.SHARETRIBE_INTEGRATION_CLIENT_SECRET,
  baseUrl: 'https://flex-integ-api.sharetribe.com',
});

function sendInvitation({ toEmail, listingId, listingName, res }) {
  const emailContent = `...`; // Build HTML

  sgMail.setApiKey(process.env.SENDGRID_API_KEY);
  const msg = {
    from: process.env.SENDGRID_SENDER_EMAIL,
    to: toEmail,
    subject: 'Invitation',
    html: emailContent
  };

  return sgMail.send(msg)
    .then(() => res.send({ success: true }))
    .catch(err => res.status(500).send({ error: 'Email failed', details: err }));
}

module.exports = (req, res) => {
  const { listingId, toEmail, toUserId, listingName } = req?.body || {};
  const UUID = flexIntegrationSdk.types.UUID;

  if (!listingId || !toEmail || !toUserId) {
    return res.status(400).send({ error: 'Missing required fields' });
  }

  try {
    integrationSdk.listings
      .show({ id: new UUID(listingId) })
      .then(result => {
        let invitedUsers = result?.data?.data?.attributes?.publicData?.invitedUsersToPosting || [];
        invitedUsers.push({ toEmail, toUserId, invitedAt: new Date().toISOString() });

        return integrationSdk.listings
          .update({ id: new UUID(listingId), publicData: { invitedUsersToPosting: invitedUsers } })
          .then(() => sendInvitation({ toEmail, listingId, listingName, res }));
      })
      .catch(err => {
        console.error('Error updating listing:', err);
        res.status(500).send({ error: 'Failed to update listing', details: err.message });
      });
  } catch (e) {
    console.error('Unexpected error:', e);
    res.status(500).send({ error: 'Unexpected error', message: e.message });
  }
};
```

---

## Pattern D: MongoDB Operations

Use for access control flows with database state management.

**Examples**: Request access, approve access, reject access

```javascript
const { MongoClient, ObjectId } = require('mongodb');
const sgMail = require('@sendgrid/mail');

const MONGODB_CONNECTIONSTRING = process.env.MONGODB_CONNECTIONSTRING;

module.exports = async (req, res) => {
  const { requestId, additionalData } = req.body;

  if (!requestId) {
    return res.status(400).json({ error: 'requestId is required' });
  }

  const client = new MongoClient(MONGODB_CONNECTIONSTRING);

  try {
    await client.connect();
    const db = client.db('sharetribe');
    const collection = db.collection('profile_access_requests');

    // Find record
    const record = await collection.findOne({ _id: new ObjectId(requestId) });

    if (!record) {
      return res.status(404).json({ error: 'Request not found' });
    }

    // Update record
    await collection.updateOne(
      { _id: new ObjectId(requestId) },
      { $set: { status: 'approved', updatedAt: new Date() } }
    );

    // Send notification email
    const emailContent = `...`;
    sgMail.setApiKey(process.env.SENDGRID_API_KEY);
    await sgMail.send({
      from: process.env.SENDGRID_SENDER_EMAIL,
      to: record.email,
      subject: 'Your request has been processed',
      html: emailContent
    });

    res.json({ success: true, message: 'Request processed' });
  } catch (error) {
    console.error('Error:', error);
    res.status(500).json({ error: 'Failed', details: error.message });
  } finally {
    await client.close();
  }
};
```

---

## Pattern E: Secure Share Links

Use for profile/content sharing with encrypted tokens and MongoDB tracking.

**Examples**: Profile sharing with OTP verification

```javascript
const CryptoJS = require('crypto-js');
const pako = require('pako');
const sgMail = require('@sendgrid/mail');
const { MongoClient } = require('mongodb');

const MONGODB_CONNECTIONSTRING = process.env.MONGODB_CONNECTIONSTRING;

const compress = data => Buffer.from(pako.deflate(data)).toString('base64');
const base64url = source => source.replace(/=+$/, '').replace(/\+/g, '-').replace(/\//g, '_');

const encryptData = (data, secretKey) => {
  const compressedData = compress(JSON.stringify(data));
  const encrypted = CryptoJS.AES.encrypt(compressedData, secretKey).toString();
  return base64url(encrypted);
};

module.exports = async (req, res) => {
  const { email, profileListingId, profileListingSlug, displayName } = req.body;

  if (!email || !profileListingId) {
    return res.status(400).json({ error: 'Email and profileListingId required' });
  }

  const client = new MongoClient(MONGODB_CONNECTIONSTRING);

  try {
    await client.connect();
    const db = client.db('sharetribe');
    const collection = db.collection('share_tokens');

    // Create encrypted token
    const payload = {
      email,
      profileListingId,
      expiresAt: Date.now() + 24 * 60 * 60 * 1000, // 24 hours
      createdAt: Date.now()
    };

    const secretKey = process.env.SHARE_SECRET_KEY || 'my-secret-key';
    const encryptedToken = encryptData(payload, secretKey);

    // Store in DB
    await collection.insertOne({
      token: encryptedToken,
      email,
      profileListingId,
      isUsed: false,
      isVerified: false,
      createdAt: new Date(),
      expiresAt: new Date(payload.expiresAt)
    });

    // Generate share link
    const marketplaceUrl = process.env.REACT_APP_MARKETPLACE_ROOT_URL;
    const shareLink = `${marketplaceUrl}/l/${profileListingSlug}/${profileListingId}?share=${encryptedToken}`;

    // Send email
    const emailContent = `...`;
    sgMail.setApiKey(process.env.SENDGRID_API_KEY);
    await sgMail.send({
      from: process.env.SENDGRID_SENDER_EMAIL,
      to: email,
      subject: `${displayName} shared their profile with you`,
      html: emailContent
    });

    res.json({ success: true, shareLink });
  } catch (error) {
    console.error('Error:', error);
    res.status(500).json({ error: 'Failed to share', details: error.message });
  } finally {
    await client.close();
  }
};
```

---

## Pattern F: OTP Verification

Use for sending secure one-time passwords.

**Examples**: Share OTP verification emails

```javascript
const sgMail = require('@sendgrid/mail');
const { MongoClient } = require('mongodb');
const crypto = require('crypto');

const MONGODB_CONNECTIONSTRING = process.env.MONGODB_CONNECTIONSTRING;

const generateOTP = () => crypto.randomInt(100000, 999999).toString();
const hashOTP = (otp) => crypto.createHash('sha256').update(otp).digest('hex');

module.exports = async (req, res) => {
  const { token } = req.body;

  if (!token) {
    return res.status(400).json({ error: 'Token is required' });
  }

  const client = new MongoClient(MONGODB_CONNECTIONSTRING);

  try {
    await client.connect();
    const db = client.db('sharetribe');
    const collection = db.collection('share_tokens');

    // Find token record
    const shareRecord = await collection.findOne({ token });

    if (!shareRecord) {
      return res.status(404).json({ error: 'Token not found' });
    }

    if (shareRecord.isUsed) {
      return res.status(400).json({ error: 'Link already used' });
    }

    // Generate OTP
    const otp = generateOTP();
    const otpHash = hashOTP(otp);

    // Update DB with OTP
    await collection.updateOne(
      { token },
      {
        $set: {
          otpHash,
          otpExpiresAt: new Date(Date.now() + 10 * 60 * 1000), // 10 minutes
          otpSentAt: new Date(),
          otpAttempts: 0
        }
      }
    );

    // Send OTP email
    const emailContent = `
      <div style='...'>
        <div style='font-size:36px;font-weight:bold;letter-spacing:8px;'>
          ${otp}
        </div>
      </div>
    `;

    sgMail.setApiKey(process.env.SENDGRID_API_KEY);
    await sgMail.send({
      from: process.env.SENDGRID_SENDER_EMAIL,
      to: shareRecord.email,
      subject: 'Your Verification Code',
      html: emailContent
    });

    res.json({ success: true, otpSent: true });
  } catch (error) {
    console.error('Error:', error);
    res.status(500).json({ error: 'Failed to send OTP', details: error.message });
  } finally {
    await client.close();
  }
};
```

---

## Route Registration

Add to `server/apiRouter.js`:

```javascript
// Import (top of file)
const sendEmailName = require('./api/sendEmailName');

// Register route (in route section)
router.post('/sendEmailName', sendEmailName);
```

---

## Environment Variables

| Variable | Description |
|----------|-------------|
| `SENDGRID_API_KEY` | SendGrid API key |
| `SENDGRID_SENDER_EMAIL` | Verified sender email |
| `REACT_APP_MARKETPLACE_ROOT_URL` | Base marketplace URL |
| `MONGODB_CONNECTIONSTRING` | MongoDB connection string |
| `SHARETRIBE_INTEGRATION_CLIENT_ID` | Sharetribe API client ID |
| `SHARETRIBE_INTEGRATION_CLIENT_SECRET` | Sharetribe API client secret |
| `SHARE_SECRET_KEY` | Secret key for encryption |

---

## URL Patterns

| Type | Pattern |
|------|---------|
| Signup Link | `/signuplink/?${encodedData}` |
| Listing | `/l/${listingId}` |
| Listing with Slug | `/l/${slug}/${listingId}` |
| Share Link | `/l/${slug}/${id}?share=${token}` |
| Profile Settings | `/profile-settings` |
| External | Direct URL |
