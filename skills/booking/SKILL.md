---
name: booking
description: |
  Get transport and accommodation recommendations for a trip. Use when the user asks about "book flights", "find hotels", "transport options", "accommodation recommendations", "travel costs", "where to stay", "how to get there", "机票", "酒店", "交通", "住宿推荐", "flight search", "hotel search".
user-invocable: true
allowed-tools:
  - Read
  - Write
  - Edit
  - WebSearch
  - WebFetch
---

# Step 4: Lock Hard Costs — Recommendations Only

> **Disclaimer**: TripBot provides transport and accommodation recommendations only. All bookings must be made by the user directly with the provider. Prices are estimates and may change.
>
> **Read `references/booking-disclaimers.md`** for the full disclaimer protocol: when to show disclaimers, how to handle booking requests, price accuracy rules, and recommendation ethics. Follow these guidelines for every recommendation output.

## Process

### 1. Transport Recommendations

#### Flights
Search for flight routes and pricing:
- Use web search: "flights {origin} to {destination} {month} {year} price"
- If the user hasn't specified origin city, ask for it
- Present options as a comparison:

| Route | Price Range | Duration | Stops | Airlines | Notes |
|-------|------------|----------|-------|----------|-------|
| | | | | | |

**Tips to include**:
- Best booking window (typically 3-6 weeks ahead for domestic, 6-8 weeks for international)
- Cheapest days to fly (usually Tue/Wed)
- Baggage policies summary
- Whether booking direct vs. OTA (online travel agency) matters

### 2. Local Transport
Based on destination, recommend:
- Airport transfer options (train, bus, taxi, rideshare) with costs
- City transit passes (day pass, weekly pass, IC card)
- Inter-city transport if visiting multiple cities
- Whether to rent a car or rely on public transit

### 3. Accommodation Recommendations

Search based on budget level and itinerary areas:

- Use web search: "best hotels {area} {destination} {budget level} {year}"
- Cross-reference with the itinerary — recommend staying near Day 1-2 clusters

Present as comparison table:

| Name | Area | Price/Night | Rating | Pros | Cons |
|------|------|-------------|--------|------|------|
| | | | | | |

**Include for each option**:
- Walking distance to key attractions
- Transit access
- Neighborhood vibe (quiet/lively/sketchy)
- Breakfast included?
- Free cancellation policy note

Use the budget tier from Step 1 (framework) to filter recommendation level.

### 4. Cost Summary

Compile total estimated costs:

```
Transport:
- Flights: ~{amount} per person ({airline} range)
- Local transit: ~{amount} per person for {duration} days

Accommodation:
- {Hotel name}: ~{amount}/night x {N} nights = ~{total}

Total estimated hard costs: ~{amount} for {group_size} people
Remaining budget for food & activities: ~{amount}
```
