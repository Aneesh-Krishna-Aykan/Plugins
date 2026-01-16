---
name: filter
description: Add or configure search filters in Sharetribe marketplace
agent: filter-management
---

# Sharetribe Filter Command

Invoke the filter management agent to add or configure search filters.

## Usage

```
/sharetribe-plugin:filter
/sharetribe-plugin:filter [filter description]
```

## What This Does

This command invokes the `filter-management` agent which:

1. Configures built-in or custom filters
2. Sets up proper schema types and UI components
3. Configures primary/secondary filter placement
4. Provides backend search schema CLI commands
5. Documents query parameter formats

## Examples

```
/sharetribe-plugin:filter "Add an amenities filter"
/sharetribe-plugin:filter "Configure price range filter"
/sharetribe-plugin:filter "Create a category filter"
```
