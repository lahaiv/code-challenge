# OTA Mini Search Exercise

## Overview

This is a short, practical interview exercise designed to take 30–45 minutes.
You'll build a very small OTA-style search app that lets users search for property listings by city, dates, and guest count. The goal is to demonstrate clear thinking, correctness, and pragmatic tradeoffs — not polish or completeness.

## Tech Requirements (Required)

- React based (Next.js/Tanstack)
- TypeScript
- Frontend is custom css/tailwindcss or uses a template library (shadcn/ui, material)
- App must be accessible at:

```
http://localhost:3003
```

## Provided Seed Data

You are provided a file:

```
listings.json
```

It contains an array of listing objects with the following shape:

```ts
interface Listing {
  id: string
  name: string
  city: string
  nightly_price: number
  max_guests: number
  available_from: string // ISO date (YYYY-MM-DD)
  available_to: string   // ISO date (YYYY-MM-DD)
}
```

Import and use this file directly as your data source — no API or server required.

## What to Build

### 1) Search Logic (Required)

Implement a `searchListings` function (e.g. in `lib/search.ts`) that accepts the following parameters and filters the listings array accordingly:

### Parameters

All parameters are required:

- `city` (string)
- `check_in` (ISO date: `YYYY-MM-DD`)
- `check_out` (ISO date: `YYYY-MM-DD`)
- `guests` (positive integer)

### Validation Rules

Surface a clear error message to the user if:

- any required parameter is missing
- a date is invalid
- `check_out <= check_in`
- `guests` is not a positive integer

### Availability Rules

A listing matches if all of the following are true:

- `city` matches (case-insensitive is fine)
- `guests <= max_guests`
- `available_from <= check_in`
- `check_out <= available_to`

> Note: A same-day availability window (`available_from === available_to`) should not match any valid stay if `check_out > check_in` is enforced correctly.

### Pricing

- `nights = number of nights between check_in and check_out`
- `total_price = nights * nightly_price`

### Result Shape

Each matched listing should include:

```ts
{
  id: string
  name: string
  city: string
  nightly_price: number
  max_guests: number
  nights: number
  total_price: number
}
```

---

### 2) Frontend (Required)

Build a single page at `/`.

### Search Form

Components (minimum):

- `Input`
- `Button`
- `Card`

Form fields:

- City
- Check-in (date)
- Check-out (date)
- Guests (number)

### Behavior

- Clicking Search runs your `searchListings` function against the imported JSON
- Render results using Cards showing:
  - listing name
  - nightly price
  - number of nights
  - total price

### UI States (Required)

- Loading state (e.g. "Searching…") — even a short artificial delay is fine if it helps demonstrate the state
- Empty state ("No results found")
- Error state (display the validation error message)

Styling beyond shadcn defaults is not required.

---

## Deliverables

1. A working app
2. A short update to this `README.md` including:
   - any assumptions you made
   - 2–3 bullets describing what you would improve or add with more time (e.g. real API layer, pagination, caching, date range UX, etc.)

## Timebox Guidance

This exercise is designed to take **30–45 minutes**. Please do not over-engineer.
