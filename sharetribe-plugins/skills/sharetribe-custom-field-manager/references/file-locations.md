# File Locations Reference

## Configuration Files

| File | Purpose |
|------|---------|
| `src/config/configListing.js` | Listing field definitions, listing types |
| `src/config/configUser.js` | User field definitions, user types |
| `src/config/fieldOptions.js` | Enum option arrays, helper functions |
| `src/config/configSearch.js` | Search filter configurations |

## Translation Files

| File | Language |
|------|----------|
| `src/translations/en.json` | English (primary) |
| `src/translations/es.json` | Spanish |
| `src/translations/de.json` | German |
| `src/translations/fr.json` | French |

## Translation Key Patterns

### Listing Fields
```
EditListingDetailsForm.{fieldKey}Label
EditListingDetailsForm.{fieldKey}Placeholder
EditListingDetailsForm.{fieldKey}Required
ListingPage.{fieldKey}
SearchPage.{fieldKey}FilterLabel
```

### User Fields
```
ProfileSettingsForm.{fieldKey}Label
ProfileSettingsForm.{fieldKey}Placeholder
ProfileSettingsForm.{fieldKey}Required
SignupForm.{fieldKey}Label
ProfilePage.{fieldKey}
```

### Enum Options
```
FieldOptions.{optionKey}
```

## Search Index Setup

After adding a field with `indexForSearch: true`, run Sharetribe CLI:

```bash
# Set search index for the field
sharetribe listing set-search-schema --field {fieldKey} --type {schemaType}
```

Schema types for CLI:
- `enum` -> `enum`
- `multi-enum` -> `multi-enum`
- `long` -> `long`
- `boolean` -> `boolean`

## Files to Modify by Task

### Adding a New Listing Field

1. `src/config/configListing.js` - Add field to `listingFields` array
2. `src/config/fieldOptions.js` - Add enum options (if enum/multi-enum)
3. `src/translations/en.json` - Add translation keys
4. Run CLI for search index (if `indexForSearch: true`)

### Adding a New User Field

1. `src/config/configUser.js` - Add field to `userFields` array
2. `src/config/fieldOptions.js` - Add enum options (if enum/multi-enum)
3. `src/translations/en.json` - Add translation keys

### Adding Options to Existing Field

1. `src/config/fieldOptions.js` - Add to existing options array
2. `src/translations/en.json` - Add translation for new option

### Modifying Search Filters

1. `src/config/configSearch.js` - Modify filter configuration
2. Update field's `filterConfig` in configListing.js

## Import Patterns

### In configListing.js / configUser.js

```javascript
// If referencing fieldOptions
import fieldOptions from './fieldOptions';

// Usage
enumOptions: fieldOptions.workTypeOptions,
```

### In Components

```javascript
import { listingFields } from '../../config/configListing';
import { userFields } from '../../config/configUser';
import fieldOptions from '../../config/fieldOptions';
```
