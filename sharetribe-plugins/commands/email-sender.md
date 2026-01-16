---
name: email-sender
description: Generate email sender API endpoints with HTML templates
agent: email-sender
---

# Sharetribe Email Sender Command

Invoke the email sender agent to create transactional email API endpoints.

## Usage

```
/sharetribe-plugin:email-sender
/sharetribe-plugin:email-sender [email description]
```

## What This Does

This command invokes the `email-sender` agent which:

1. Determines the email category (invitation, notification, etc.)
2. Selects the appropriate pattern
3. Generates complete endpoint code
4. Creates HTML email templates
5. Provides route registration

## Examples

```
/sharetribe-plugin:email-sender "Create an invitation email for vendors"
/sharetribe-plugin:email-sender "Generate OTP verification email endpoint"
```
