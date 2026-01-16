---
name: sso
description: Implement OAuth2/OpenID Connect SSO providers for Sharetribe
agent: sso-implementation
---

# Sharetribe SSO Command

Invoke the SSO implementation agent to add social login providers.

## Usage

```
/sharetribe-plugin:sso
/sharetribe-plugin:sso [provider name]
```

## What This Does

This command invokes the `sso-implementation` agent which:

1. Sets up Passport.js strategy for the provider
2. Configures OAuth callback handling
3. Integrates with Sharetribe loginWithIdp
4. Handles token management
5. Provides Console configuration steps

## Examples

```
/sharetribe-plugin:sso Apple
/sharetribe-plugin:sso "Add GitHub login"
/sharetribe-plugin:sso Twitter
```
