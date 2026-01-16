---
name: sharetribe-custom-field-manager
description: Add, modify, or remove custom extended data fields for listings and users in Sharetribe marketplace. Use when adding new fields to listing forms, user profiles, or signup forms. Handles field definitions in configListing.js/configUser.js, enum options in fieldOptions.js, filter configurations, and search indexing setup. Supports all schema types (enum, multi-enum, text, long, boolean).
---

# Sharetribe Custom Field Manager

Manage custom extended data fields for listings and users following Sharetribe patterns.

## Workflow

### Step 1: Gather Requirements

Ask the user:
1. **Field target**: Listing field or User field?
2. **Field name/key**: What should the field be called? (camelCase)
3. **Schema type**: enum, multi-enum, text, long, or boolean?
4. **Options** (if enum/multi-enum): What are the selectable options?
5. **Scope**: public, private, or protected (user only)?
6. **Searchable**: Should this field be filterable on search page?
7. **Required**: Is this field mandatory?
8. **Restrictions**: Limited to specific listing types or user types?

### Step 2: Generate Field Configuration

#### For Listing Fields

Add to `src/config/configListing.js` in `listingFields` array:

```javascript
{
  key: 'fieldKey',
  scope: 'public',
  schemaType: 'enum', // or multi-enum, text, long, boolean
  enumOptions: [
    { option: 'key', label: 'Label' },
  ],
  filterConfig: {
    indexForSearch: true,
    filterType: 'SelectMultipleFilter',
    label: 'Filter Label',
    group: 'primary',
  },
  showConfig: {
    label: 'Display Label',
    isDetail: true,
  },
  saveConfig: {
    label: 'Form Label',
    placeholderMessage: 'Select...',
    isRequired: true,
    requiredMessage: 'This field is required.',
  },
}
```

#### For User Fields

Add to `src/config/configUser.js` in `userFields` array:

```javascript
{
  key: 'fieldKey',
  scope: 'public',
  schemaType: 'enum',
  enumOptions: [
    { option: 'key', label: 'Label' },
  ],
  showConfig: {
    label: 'Display Label',
    displayInProfile: true,
  },
  saveConfig: {
    label: 'Form Label',
    placeholderMessage: 'Select...',
    isRequired: true,
    displayInSignUp: true,
  },
  userTypeConfig: {
    limitToUserTypeIds: false,
  },
}
```

### Step 3: Add Enum Options (if applicable)

For reusable options, add to `src/config/fieldOptions.js`:

```javascript
fieldNameOptions: [
  { key: 'option1', label: 'Option 1' },
  { key: 'option2', label: 'Option 2' },
  { key: 'add_new_option', label: '-- Add New --' }, // Optional
],
```

Then reference in field config:
```javascript
enumOptions: fieldOptions.fieldNameOptions.map(o => ({ option: o.key, label: o.label })),
```

### Step 4: Add Translation Keys

Add to `src/translations/en.json`:

```json
"EditListingDetailsForm.fieldKeyLabel": "Field Label",
"EditListingDetailsForm.fieldKeyPlaceholder": "Select...",
"EditListingDetailsForm.fieldKeyRequired": "This field is required",
"ListingPage.fieldKey": "Field Label",
"SearchPage.fieldKeyFilterLabel": "Filter Label"
```

### Step 5: Set Search Index (if searchable)

If `indexForSearch: true`, inform user to run CLI:

```bash
sharetribe listing set-search-schema --field fieldKey --type enum
```

## Reference Files

- **Field schemas**: See [references/field-schemas.md](references/field-schemas.md) for complete structure
- **File locations**: See [references/file-locations.md](references/file-locations.md) for all affected files
- **Examples**: See [references/examples.md](references/examples.md) for common patterns

## Quick Reference

### Schema Types
| Type | Use For |
|------|---------|
| `enum` | Single select dropdown |
| `multi-enum` | Multiple select checkboxes |
| `text` | Short text input |
| `long` | Numbers |
| `boolean` | Yes/No toggle |

### Scopes
| Scope | Visibility |
|-------|------------|
| `public` | Everyone can see |
| `private` | Owner only |
| `protected` | After transaction (user fields only) |

### Files to Modify
| Task | Files |
|------|-------|
| Listing field | `configListing.js`, `fieldOptions.js`, `en.json` |
| User field | `configUser.js`, `fieldOptions.js`, `en.json` |
| Options only | `fieldOptions.js` |

## Checklist

- [ ] Field added to correct config file
- [ ] Enum options defined (if applicable)
- [ ] filterConfig set correctly (if searchable)
- [ ] showConfig has proper label
- [ ] saveConfig has label, placeholder, required settings
- [ ] Translation keys added
- [ ] CLI command provided (if search indexed)
