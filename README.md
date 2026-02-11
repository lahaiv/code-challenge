# OTA Mini Search Exercise

## Overview

This is a short, practical interview exercise designed to take 30–45 minutes.

You'll build a very small OTA-style search app that lets users search for property listings by city, dates, and guest count. The goal is to demonstrate clear thinking, correctness, and pragmatic tradeoffs — not polish or completeness.

## Tech Requirements (Required)

- React based (Next.js (App Router)/Tanstack)
- TypeScript
- Single container application
- API implemented using Next.js Route Handlers or use a stand alone language
- Frontend uses a template (shadcn/ui, material) components (components are already provided; no CLI setup required)
- App must run with Docker:

```bash
docker compose up --build

```

- App must be accessible at:

```
<http://localhost:3000>

```

## Provided Seed Data

You are provided a file:

```
lib/listings.seed.ts

```

It exports:

```tsx
export interface Listing {
  id: string
  name: string
  city: string
  nightly_price: number
  max_guests: number
  available_from: string // ISO date (YYYY-MM-DD)
  available_to: string   // ISO date (YYYY-MM-DD)
}

export const LISTINGS_SEED: Listing[]

```

### Important

The seed data intentionally includes edge cases, such as:

- listings with `max_guests = 1`
- listings where `available_from === available_to`
- listings that are only bookable for exactly one night

Your logic should handle these cases correctly.

## What to Build

### 1) API (Required)

Implement a single endpoint using a Next.js Route Handler or a standalone api:

```
GET /api/search

```

### Query Parameters

All parameters are required:

- `city` (string)
- `check_in` (ISO date: `YYYY-MM-DD`)
- `check_out` (ISO date: `YYYY-MM-DD`)
- `guests` (positive integer)

### Validation Rules

Return HTTP `400` with a clear error message if:

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

> Note: A same-day availability window (available_from === available_to) should not match any valid stay if check_out > check_in is enforced correctly.
> 

### Pricing

- `nights = number of nights between check_in and check_out`
- `total_price = nights * nightly_price`

### Response Format

```json
{
  "results": [
    {
      "id": "p1",
      "name": "Downtown Loft",
      "city": "Nashville",
      "nightly_price": 180,
      "max_guests": 4,
      "nights": 3,
      "total_price": 540
    }
  ]
}

```

### 2) Frontend (Required)

Build a single page at `/`.

### Search Form

Use shadcn/ui components (minimum):

- `Input`
- `Button`
- `Card`

Form fields:

- City
- Check-in (date)
- Check-out (date)
- Guests (number)

### Behavior

- Clicking Search calls `/api/search`
- Render results using Cards showing:
    - listing name
    - nightly price
    - number of nights
    - total price

### UI States (Required)

- Loading state (e.g. "Searching…")
- Empty state ("No results found")
- Error state (display API error message)

Styling beyond shadcn defaults is not required.

## Docker

The app must run using:

```bash
docker compose up --build

```

Only one service/container is expected.

## Deliverables

1. A working app that runs via Docker Compose
2. A short update to this `README.md` including:
    - any assumptions you made
    - 2–3 bullets describing what you would improve or add with more time (scaling, data modeling, caching, etc.)

## Timebox Guidance

This exercise is designed to take **30–45 minutes**. Please do not over-engineer.

