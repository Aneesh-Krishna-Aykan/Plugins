# Field Schema Types Reference

## Available Schema Types

| Type | Description | Use Case |
|------|-------------|----------|
| `enum` | Single selection from options | Dropdowns, radio buttons |
| `multi-enum` | Multiple selections from options | Checkboxes, multi-select |
| `text` | Short text input | Names, titles, short descriptions |
| `long` | Numeric value | Counts, quantities, ratings |
| `boolean` | True/false value | Yes/No questions, toggles |

## Listing Field Structure

```javascript
{
  key: 'fieldKey',                    // Unique identifier (camelCase)
  scope: 'public',                    // 'public' or 'private'
  schemaType: 'enum',                 // Schema type from above

  // For enum/multi-enum types only
  enumOptions: [
    { option: 'option-key', label: 'Display Label' },
  ],

  // Optional: Limit to specific listing types
  listingTypeConfig: {
    limitToListingTypeIds: true,
    listingTypeIds: ['consultant', 'vendor'],
  },

  // Optional: Limit to specific categories
  categoryConfig: {
    limitToCategoryIds: true,
    categoryIds: ['category-id'],
  },

  // Search/filter configuration
  filterConfig: {
    indexForSearch: true,             // Enable search indexing
    filterType: 'SelectMultipleFilter', // or 'SelectSingleFilter'
    label: 'Filter Label',
    searchMode: 'has_all',            // For multi-enum: 'has_all' or 'has_any'
    group: 'primary',                 // 'primary' or 'secondary'
  },

  // Display configuration
  showConfig: {
    label: 'Display Label',
    isDetail: true,                   // Show in listing details
  },

  // Form/save configuration
  saveConfig: {
    label: 'Form Label',
    placeholderMessage: 'Select an option...',
    isRequired: true,
    requiredMessage: 'This field is required.',
  },
}
```

## User Field Structure

```javascript
{
  key: 'fieldKey',                    // Unique identifier (camelCase)
  scope: 'public',                    // 'public', 'protected', or 'private'
  schemaType: 'enum',                 // Schema type

  // For enum/multi-enum types only
  enumOptions: [
    { option: 'option-key', label: 'Display Label' },
  ],

  // Display configuration
  showConfig: {
    label: 'Display Label',
    displayInProfile: true,           // Show on profile page
  },

  // Form/save configuration
  saveConfig: {
    label: 'Form Label',
    placeholderMessage: 'Select...',
    isRequired: true,
    requiredMessage: 'This field is required.',
    displayInSignUp: true,            // Show on signup page
  },

  // Optional: Limit to specific user types
  userTypeConfig: {
    limitToUserTypeIds: true,
    userTypeIds: ['consultant', 'vendor', 'customer'],
  },
}
```

## Field Options Structure (fieldOptions.js)

```javascript
// For standard options
fieldNameOptions: [
  { key: 'option-key', label: 'Display Label' },
  { key: 'another-key', label: 'Another Label' },
],

// For options with "Add New" capability
fieldNameOptions: [
  { key: 'option1', label: 'Option 1' },
  { key: 'option2', label: 'Option 2' },
  { key: 'add_new_option', label: '-- Add New --' },
],
```

## Scope Definitions

| Scope | Listing | User | Description |
|-------|---------|------|-------------|
| `public` | Yes | Yes | Visible to everyone, searchable |
| `private` | Yes | Yes | Only visible to owner |
| `protected` | No | Yes | Visible after transaction/relationship |

## Filter Types

| Filter Type | Schema | Description |
|-------------|--------|-------------|
| `SelectSingleFilter` | enum | Single selection dropdown |
| `SelectMultipleFilter` | enum, multi-enum | Multiple selection checkboxes |

## Search Modes (multi-enum only)

| Mode | Description |
|------|-------------|
| `has_all` | Listing must have ALL selected values |
| `has_any` | Listing must have ANY selected value |

## Number Config (for 'long' type)

```javascript
{
  key: 'gears',
  schemaType: 'long',
  numberConfig: {
    minimum: 1,
    maximum: 24,
  },
  // ...
}
```
