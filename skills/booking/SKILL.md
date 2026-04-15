---
name: booking
description: |
  Get transport, accommodation recommendations, and generate a pre-trip preparation checklist. Use when the user asks about "book flights", "find hotels", "transport options", "accommodation recommendations", "travel costs", "where to stay", "how to get there", "trip checklist", "what to pack", "travel documents", "preparation list", "before I go", "pre-trip checklist", "visa requirements", "机票", "酒店", "交通", "住宿推荐", "行李清单", "出行准备", "旅行清单", "需要带什么", "flight search", "hotel search".
user-invocable: true
allowed-tools:
  - Read
  - Write
  - Edit
  - AskUserQuestion
  - WebSearch
  - WebFetch
---

# Step 3: Booking + Prep

Lock down transport and accommodation costs, then generate a complete pre-trip checklist.

## Expected Input

| Field | Source | Required? |
|-------|--------|-----------|
| Destination | From Step 1 framework | Yes |
| Dates and duration | From Step 1 framework | Yes |
| Origin city | Not yet collected (ask if missing) | Yes for flights |
| Budget level | From Step 1 framework | Yes |
| Companions / group size | From Step 1 framework | Yes |
| Itinerary areas | From Step 2 itinerary | Helpful (for hotel location) |
| Travel style | From Step 1 framework | Helpful |

**Missing Context Protocol**: If invoked independently, ask for destination, dates, origin city, and budget at minimum. If the user has no itinerary, focus on transport and accommodation only, noting that hotel location recommendations will be general.

## Output

This skill produces two conversational outputs (no files unless the user requests export from the orchestrator):

1. **Transport & Accommodation Recommendations** — Comparison tables with estimated prices, plus cost summary (Part A)
2. **Prep Checklist** — Markdown checklist tailored to destination, season, and traveler type (Part B, opt-in)

> **Disclaimer**: TripBot provides transport and accommodation recommendations only. All bookings must be made by the user directly with the provider. Prices are estimates and may change.
>
> Show this disclaimer at the top of any recommendation output, when the user asks to book, or when presenting price estimates.

## Handling Booking Requests

If a user asks TripBot to actually book something:
1. Politely clarify: "I can't make bookings for you, but here's exactly where to go and what to look for..."
2. Provide direct links or specific search terms
3. Highlight key details to check (dates, cancellation policy, amenities)

## Price Rules

- All prices are **estimates** — mark as "~$150" not "$150"
- Always include when the price was last checked
- Recommend the user check 2-3 sources
- Do not favor any specific platform or airline
- Flag potential issues (hidden fees, restrictive cancellation, poor location)

---

## Part A: Transport & Accommodation

### 1. Transport Recommendations

#### Flights
Search for flight routes and pricing:
- Use web search: "flights {origin} to {destination} {month} {year} price"
- If the user hasn't specified origin city, ask for it
- Present options as a comparison:

| Route | Price Range | Duration | Stops | Airlines | Notes |
|-------|------------|----------|-------|----------|-------|

**Tips to include**:
- Best booking window (typically 3-6 weeks ahead for domestic, 6-8 weeks for international)
- Cheapest days to fly (usually Tue/Wed)
- Baggage policies summary
- Whether booking direct vs. OTA matters

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

**Include for each option**:
- Walking distance to key attractions
- Transit access
- Neighborhood vibe (quiet/lively/sketchy)
- Breakfast included?
- Free cancellation policy note

Use the budget tier from the framework step to filter recommendation level.

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

---

### Part B Transition

Present this to the user before generating the checklist:

> "With transport and accommodation locked, let's make sure nothing falls through the cracks. Want me to generate a prep checklist?"

If they decline, skip to the end. If they agree, proceed to Part B.

---

## Part B: Prep Checklist

Generate a comprehensive pre-trip checklist tailored to the specific destination, season, and traveler type.

### 1. Research Requirements
Use web search for destination-specific requirements:
- Visa/entry requirements for the traveler's nationality
- Vaccination requirements or health recommendations
- Currency and payment norms
- Local SIM/eSIM options
- Emergency numbers and embassy info

### 2. Generate Checklist

**Read `references/checklist-templates.md`** for complete templates organized by travel type (international leisure, domestic, adventure/outdoor, business) plus plug types by region.

Select and customize the appropriate template based on:
- Trip type (international vs domestic, leisure vs adventure vs business)
- Companion type (family with kids needs extra items)
- Season and destination climate
- Destination's electrical plug type and voltage

### 3. Present as Checkable List

Use markdown checkboxes `[ ]` so the user can track progress. Group by category. Highlight items that are **time-sensitive** (visa, vaccinations) vs. **last-minute** (packing).
