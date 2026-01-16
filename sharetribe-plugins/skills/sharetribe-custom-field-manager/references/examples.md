# Field Configuration Examples

## Example 1: Enum Field (Single Select)

### Use Case: Work Site Selection

**configListing.js:**
```javascript
{
  key: 'workSite',
  scope: 'public',
  schemaType: 'enum',
  enumOptions: [
    { option: 'onsite', label: 'On-site' },
    { option: 'remote', label: 'Remote' },
    { option: 'hybrid', label: 'Hybrid' },
  ],
  filterConfig: {
    indexForSearch: true,
    filterType: 'SelectSingleFilter',
    label: 'Work Site',
    group: 'primary',
  },
  showConfig: {
    label: 'Work Site',
    isDetail: true,
  },
  saveConfig: {
    label: 'Work Site',
    placeholderMessage: 'Select work arrangement...',
    isRequired: true,
    requiredMessage: 'Please select a work site option.',
  },
}
```

---

## Example 2: Multi-Enum Field (Multiple Select)

### Use Case: Skills Selection

**configListing.js:**
```javascript
{
  key: 'requiredSkills',
  scope: 'public',
  schemaType: 'multi-enum',
  enumOptions: [
    { option: 'react', label: 'React' },
    { option: 'angular', label: 'Angular' },
    { option: 'aws', label: 'AWS' },
    { option: 'salesforce', label: 'Salesforce' },
  ],
  filterConfig: {
    indexForSearch: true,
    filterType: 'SelectMultipleFilter',
    label: 'Required Skills',
    searchMode: 'has_any',
    group: 'secondary',
  },
  showConfig: {
    label: 'Required Skills',
  },
  saveConfig: {
    label: 'Required Skills',
    placeholderMessage: 'Select required skills...',
    isRequired: false,
  },
}
```

---

## Example 3: Boolean Field

### Use Case: Remote Work Available

**configListing.js:**
```javascript
{
  key: 'remoteAvailable',
  scope: 'public',
  schemaType: 'boolean',
  filterConfig: {
    indexForSearch: true,
    label: 'Remote Available',
    group: 'secondary',
  },
  showConfig: {
    label: 'Remote Work Available',
    isDetail: true,
  },
  saveConfig: {
    label: 'Is remote work available?',
    isRequired: false,
  },
}
```

---

## Example 4: Long (Number) Field

### Use Case: Years of Experience

**configListing.js:**
```javascript
{
  key: 'yearsExperience',
  scope: 'public',
  schemaType: 'long',
  numberConfig: {
    minimum: 0,
    maximum: 50,
  },
  filterConfig: {
    indexForSearch: true,
    label: 'Years of Experience',
    group: 'secondary',
  },
  showConfig: {
    label: 'Years of Experience',
    isDetail: true,
  },
  saveConfig: {
    label: 'Years of Experience',
    placeholderMessage: 'Enter years...',
    isRequired: true,
    requiredMessage: 'Please enter years of experience.',
  },
}
```

---

## Example 5: Text Field

### Use Case: Special Instructions

**configListing.js:**
```javascript
{
  key: 'specialInstructions',
  scope: 'public',
  schemaType: 'text',
  showConfig: {
    label: 'Special Instructions',
  },
  saveConfig: {
    label: 'Special Instructions',
    placeholderMessage: 'Enter any special instructions...',
    isRequired: false,
  },
}
```

---

## Example 6: User Field with Type Restriction

### Use Case: Consultant-only Field

**configUser.js:**
```javascript
{
  key: 'hourlyRate',
  scope: 'public',
  schemaType: 'long',
  numberConfig: {
    minimum: 0,
    maximum: 1000,
  },
  showConfig: {
    label: 'Hourly Rate ($)',
    displayInProfile: true,
  },
  saveConfig: {
    label: 'Your Hourly Rate ($)',
    placeholderMessage: 'Enter your hourly rate...',
    isRequired: true,
    requiredMessage: 'Please enter your hourly rate.',
    displayInSignUp: false,
  },
  userTypeConfig: {
    limitToUserTypeIds: true,
    userTypeIds: ['consultant'],
  },
}
```

---

## Example 7: Protected User Field

### Use Case: Contact Information (visible after booking)

**configUser.js:**
```javascript
{
  key: 'directPhone',
  scope: 'protected',
  schemaType: 'text',
  showConfig: {
    label: 'Direct Phone',
    displayInProfile: false,
  },
  saveConfig: {
    label: 'Direct Phone Number',
    placeholderMessage: 'Enter your direct phone...',
    isRequired: false,
    displayInSignUp: false,
  },
}
```

---

## Example 8: Field Options with Add New

**fieldOptions.js:**
```javascript
certificationsOptions: [
  { key: 'MinorityOwned', label: 'Minority-Owned' },
  { key: 'WomanOwned', label: 'Woman-Owned' },
  { key: 'VeteranOwned', label: 'Veteran-Owned' },
  { key: 'ISO9001', label: 'ISO 9001' },
  { key: 'add_new_option', label: '-- Add New --' },
],
```

---

## Example 9: Listing Type Restricted Field

### Use Case: Field only for Job Postings

**configListing.js:**
```javascript
{
  key: 'positionLevel',
  scope: 'public',
  schemaType: 'enum',
  enumOptions: [
    { option: 'entry', label: 'Entry Level' },
    { option: 'mid', label: 'Mid Level' },
    { option: 'senior', label: 'Senior Level' },
    { option: 'lead', label: 'Lead/Principal' },
  ],
  listingTypeConfig: {
    limitToListingTypeIds: true,
    listingTypeIds: ['job-posting'],
  },
  filterConfig: {
    indexForSearch: true,
    filterType: 'SelectMultipleFilter',
    label: 'Position Level',
    group: 'primary',
  },
  showConfig: {
    label: 'Position Level',
    isDetail: true,
  },
  saveConfig: {
    label: 'Position Level',
    placeholderMessage: 'Select position level...',
    isRequired: true,
    requiredMessage: 'Please select a position level.',
  },
}
```
