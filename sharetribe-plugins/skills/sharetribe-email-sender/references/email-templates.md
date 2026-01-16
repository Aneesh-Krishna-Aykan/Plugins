# Email Templates Reference

## Base HTML Email Structure

All email templates follow this consistent HTML structure:

```html
<html lang='en'>
<head>
  <meta http-equiv='Content-Type' content='text/html; charset=UTF-8' />
</head>
<body>
  <table style='background-color:#ffffff;margin:0 auto;padding:24px 12px 0;font-family:-apple-system,BlinkMacSystemFont,Segoe UI,Roboto,Oxygen,Ubuntu,Cantarell,Fira Sans,Droid Sans,Helvetica Neue,sans-serif' align='center' role='presentation' cellSpacing='0' cellPadding='0' border='0' width='100%'>
    <tbody>
      <tr>
        <td>
          <table align='center' role='presentation' cellSpacing='0' cellPadding='0' border='0' width='100%' style='max-width:600px;margin:0 auto'>
            <tr style='width:100%'>
              <td>
                <!-- GREETING -->
                <h3 style='font-size:26px;line-height:1.3;font-weight:700;color:#484848'>Hi ${recipientName},</h3>

                <!-- MAIN MESSAGE -->
                <p style='font-size:16px;line-height:1.4;margin:16px 0;color:#484848'>
                  ${mainMessage}
                </p>

                <!-- SECONDARY MESSAGE (optional) -->
                <p style='font-size:16px;line-height:1.4;margin:16px 0;color:#484848'>
                  ${secondaryMessage}
                </p>

                <!-- CTA BUTTON -->
                <table style='padding:16px 0 0' align='center' role='presentation' cellSpacing='0' cellPadding='0' border='0' width='100%'>
                  <tbody>
                    <tr>
                      <td>
                        <a href='${ctaUrl}' target='_blank' style='background-color:#0e9941;border-radius:4px;color:#fff;font-size:15px;text-decoration:none;text-align:center;display:inline-block;min-width:210px;padding:16px 32px;line-height:120%;max-width:100%'>
                          ${ctaButtonText}
                        </a>
                        <div>
                          <p style='font-size:14px;line-height:1.5;margin:16px 0;color:#484848'>
                            Can't click the button? Here's the link:
                            <a target='_blank' style='color:#0e9941;text-decoration:none' href='${ctaUrl}'>${ctaUrl}</a>
                          </p>
                        </div>
                      </td>
                    </tr>
                  </tbody>
                </table>

                <!-- FOOTER -->
                <div>
                  <p style='font-size:16px;line-height:1.4;margin:16px 0;color:#484848'>
                    Have a great day!<br/>The iParley Dev team
                  </p>
                  <hr style='width:100%;border:none;border-top:1px solid #E1E1E1;margin:20px 0'/>
                  <p style='font-size:12px;line-height:15px;margin:0 auto;color:#b7b7b7;text-align:left;margin-bottom:50px'>
                    ${footerText}
                  </p>
                </div>
              </td>
            </tr>
          </table>
        </td>
      </tr>
    </tbody>
  </table>
</body>
</html>
```

---

## Email Type Templates

### 1. Invitation Emails

#### Consultant Invitation
- **Subject**: `Invitation from ${senderName} to join ${organizationName}`
- **Main Message**: `${senderName} has added you as consultant in ${organizationName}.`
- **Secondary Message**: `By joining, you'll be able to collaborate with your team and access all the resources available in the organization.`
- **CTA Button**: `Accept & Join`
- **URL Pattern**: `/signuplink/?${encodedEncryptedData}`

#### Vendor Invitation
- **Subject**: `Vendor Invitation from ${customerName}`
- **Main Message**: `${customerName} has added you as a ${vendorType} vendor in their profile.`
- **CTA Button (Registered)**: `Login to Accept/Reject`
- **CTA Button (Unregistered)**: `Learn More About iParley`
- **URL Pattern**: `/l/${vendorListingId}` or marketplace root

#### Job/RFP Posting Invitation
- **Subject**: `Invitation from ${adminName} to join ${orgName}`
- **Main Message**: `${adminName} has invited you as ${roleDescription} to join ${orgName} for the ${postingType} titled ${listingName}.`
- **CTA Button**: `Accept & Join`
- **URL Pattern**: `/l/${listingSlug}/${listingId}?enc=${encodedData}`

---

### 2. Notification Emails

#### Meeting Scheduled
- **Subject**: `Meeting Scheduled regarding: ${listingName}`
- **Main Message**: `This is a confirmation that your meeting, arranged by ${senderName} regarding the listing ${listingName}, has been successfully scheduled.`
- **CTA Button**: `Join Meeting`
- **URL Pattern**: External meeting link

#### Consultant/Employer Added
- **Subject**: `Consultant` or `Current employer`
- **Main Message**: `${senderName} has added you to their profile as a consultant/current employer on our marketplace.`
- **CTA Button**: `Visit Marketplace`
- **URL Pattern**: Marketplace root

---

### 3. Access Control Emails

#### Profile Access Request (to profile owner)
- **Subject**: `New Profile Access Request from ${firstName} ${lastName}`
- **Heading**: `New Profile Access Request`
- **Main Message**: `Someone has requested access to view your full profile on iParley.`
- **Info Box**: Request details (name, email, company, message)
- **CTA Button**: `Review Access Request`
- **URL Pattern**: `/profile-settings`

#### Access Request Approved
- **Subject**: `Your profile access request has been approved`
- **Heading**: `Profile Access Granted!`
- **Main Message**: `Great news! ${profileOwnerName} has approved your request to view their full profile on iParley.`
- **Note**: `This link can only be used once and will expire in 24 hours.`
- **CTA Button**: `View Full Profile`
- **URL Pattern**: `/l/${slug}/${id}?share=${token}`

#### Access Request Rejected
- **Subject**: `Update on your profile access request`
- **Heading**: `Profile Access Request Update`
- **Main Message**: `Unfortunately, ${profileOwnerName} has declined your request to view their full profile at this time.`
- **CTA Button**: None (informational only)

---

### 4. Sharing Emails

#### Profile Shared
- **Subject**: `${displayName} shared their iParley profile with you`
- **Main Message**: `${displayName} has shared their iParley profile with you.`
- **Note**: `When you click the link, a Secure Verification Code will be sent to this email address for verification. This link can only be used once and will expire in 24 hours.`
- **CTA Button**: `View Shared Profile`
- **URL Pattern**: `/l/${slug}/${id}?share=${token}`

#### Credential Shared (with QR Code)
- **Subject**: `${issuerName} shared a credential with you`
- **Main Message**: `${issuerName} has shared their ${documentType} credential with you.`
- **Special Element**: QR code image
- **Warning Box**: `For security purposes, this credential can only be viewed once.`
- **CTA Button**: `View Credential`
- **URL Pattern**: External presentation URL

---

### 5. Verification Emails

#### OTP Verification Code
- **Subject**: `Your Secure Verification Code to View Shared Profile - iParley`
- **Heading**: `Your Secure Verification Code to View Shared Profile`
- **Main Message**: `Here is your one-time password to access the shared profile:`
- **OTP Display**: Large styled box with 6-digit code
- **Important Notes**:
  - Valid for 10 minutes
  - Link can only be used once
  - Do not share this code
- **CTA Button**: None

---

## Special Components

### Info Box (for request details)
```html
<div style='background-color:#f5f5f5;padding:16px;border-radius:8px;margin:20px 0'>
  <h4 style='margin:0 0 12px 0;color:#333;font-size:18px'>Request Details:</h4>
  <table style='width:100%;border-collapse:collapse'>
    <tr>
      <td style='padding:6px 0;color:#666;font-size:14px'><strong>Name:</strong></td>
      <td style='padding:6px 0;color:#333;font-size:14px'>${value}</td>
    </tr>
  </table>
</div>
```

### OTP Code Display
```html
<div style='background-color: #f8f9fa; border: 2px solid #0e9941; border-radius: 8px; padding: 30px; text-align: center; margin: 30px 0;'>
  <div style='font-size: 36px; font-weight: bold; color: #0e9941; letter-spacing: 8px; font-family: monospace;'>
    ${otpCode}
  </div>
</div>
```

### Warning Box
```html
<div style='margin-top:24px;padding:16px;background-color:#fff3cd;border-left:4px solid #ffc107;border-radius:4px'>
  <p style='font-size:14px;line-height:1.5;margin:0;color:#856404'>
    <strong>Important:</strong> ${warningText}
  </p>
</div>
```

### Info Note Box
```html
<div style='margin-top:16px;padding:16px;background-color:#d1ecf1;border-left:4px solid #0e9941;border-radius:4px'>
  <p style='font-size:14px;line-height:1.5;margin:0;color:#0c5460'>
    <strong>Note:</strong> ${noteText}
  </p>
</div>
```

### Quoted Message
```html
<p style='font-size:16px;line-height:1.4;margin:16px 0;padding:12px;background-color:#f9f9f9;border-left:3px solid #0e9941;border-radius:4px;color:#484848;font-style:italic'>
  "${message}"
</p>
```

---

## Style Constants

| Element | Style |
|---------|-------|
| Primary Color | `#0e9941` (green) |
| Text Color | `#484848` |
| Footer Text | `#b7b7b7` |
| Border Color | `#E1E1E1` |
| Info Box Background | `#f5f5f5` |
| Warning Box Background | `#fff3cd` |
| Note Box Background | `#d1ecf1` |
| Font Family | `-apple-system, BlinkMacSystemFont, Segoe UI, Roboto, ...` |
| Heading Size | `26px` |
| Body Text Size | `16px` |
| Small Text Size | `12px-14px` |
| OTP Font Size | `36px` |
| Button Padding | `16px 32px` |
| Button Border Radius | `4px` |
