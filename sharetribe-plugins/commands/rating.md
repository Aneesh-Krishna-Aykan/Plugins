---
name: rating
description: Implement average rating calculation and display for listings
agent: average-rating
---

# Sharetribe Rating Command

Invoke the average rating agent to implement rating aggregation.

## Usage

```
/sharetribe-plugin:rating
/sharetribe-plugin:rating [implementation request]
```

## What This Does

This command invokes the `average-rating` agent which:

1. Creates server API endpoint for rating calculation
2. Integrates with review submission flow
3. Ensures customer-only rating logic
4. Updates listing publicData with averages
5. Provides display component guidance

## Examples

```
/sharetribe-plugin:rating "Implement average rating for listings"
/sharetribe-plugin:rating "Add rating display to listing cards"
```
