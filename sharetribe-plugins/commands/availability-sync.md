---
name: availability-sync
description: Implement cross-listing availability blocking to prevent double-booking
agent: cross-listing-availability
---

# Sharetribe Availability Sync Command

Invoke the cross-listing availability agent to prevent double-booking.

## Usage

```
/sharetribe-plugin:availability-sync
/sharetribe-plugin:availability-sync [feature request]
```

## What This Does

This command invokes the `cross-listing-availability` agent which:

1. Blocks time slots across all provider listings
2. Handles booking acceptance triggers
3. Handles booking cancellation cleanup
4. Implements rate limiting for API calls
5. Provides error handling patterns

## Examples

```
/sharetribe-plugin:availability-sync "Implement cross-listing blocking"
/sharetribe-plugin:availability-sync "Add cancellation handler"
/sharetribe-plugin:availability-sync "Fix double-booking issue"
```
