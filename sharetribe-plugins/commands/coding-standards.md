---
name: coding-standards
description: Enforce Sharetribe FTW coding standards and patterns on your code
agent: coding-standards
---

# Sharetribe Coding Standards Command

Invoke the Sharetribe coding standards agent to analyze and enforce FTW patterns.

## Usage

```
/sharetribe-plugin:coding-standards
/sharetribe-plugin:coding-standards [file or description]
```

## What This Does

This command invokes the `coding-standards` agent which:

1. Analyzes code against Sharetribe FTW patterns
2. Checks component architecture (container/presentational separation)
3. Validates Redux Ducks pattern usage
4. Reviews CSS Modules conventions
5. Checks React Final Form patterns
6. Validates i18n usage

## Examples

```
/sharetribe-plugin:coding-standards src/components/MyComponent
/sharetribe-plugin:coding-standards "Review my new page component"
```
