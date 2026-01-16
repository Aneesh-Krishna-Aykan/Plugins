---
name: calendar-sync
description: Implement bi-directional Google Calendar synchronization
agent: google-calendar-sync
---

# Sharetribe Calendar Sync Command

Invoke the Google Calendar sync agent to implement calendar integration.

## Usage

```
/sharetribe-plugin:calendar-sync
/sharetribe-plugin:calendar-sync [feature request]
```

## What This Does

This command invokes the `google-calendar-sync` agent which:

1. Implements OAuth 2.0 authentication flow
2. Sets up webhook-based real-time sync
3. Blocks availability from calendar events
4. Creates calendar events from bookings
5. Handles timezone conversions

## Examples

```
/sharetribe-plugin:calendar-sync "Set up Google Calendar integration"
/sharetribe-plugin:calendar-sync "Implement webhook handler"
/sharetribe-plugin:calendar-sync "Fix timezone issues in sync"
```
