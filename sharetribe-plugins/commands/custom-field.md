---
name: custom-field
description: Add or manage custom extended data fields for listings and users
agent: custom-field-manager
---

# Sharetribe Custom Field Command

Invoke the custom field manager agent to add, modify, or remove extended data fields.

## Usage

```
/sharetribe-plugin:custom-field
/sharetribe-plugin:custom-field [field description]
```

## What This Does

This command invokes the `custom-field-manager` agent which:

1. Gathers requirements for the field (target, type, scope, options)
2. Generates configuration for configListing.js or configUser.js
3. Creates enum options for fieldOptions.js
4. Provides translation keys
5. Gives CLI commands for search indexing

## Examples

```
/sharetribe-plugin:custom-field "Add a listing field for equipment type"
/sharetribe-plugin:custom-field "Create a user field for company name"
```
