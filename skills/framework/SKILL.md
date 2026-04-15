---
name: framework
description: |
  Set the trip framework — dates, budget, destination, and companions. Use when the user asks about "trip basics", "travel dates", "trip budget", "travel companions", "where should I go", "how long should I travel", "how many days", "旅行预算", "旅行时间", "travel framework".
user-invocable: true
allowed-tools:
  - Read
  - Write
  - Edit
  - AskUserQuestion
  - WebSearch
  - WebFetch
---

# Step 1: Set Framework

Gather the three essential constraints that shape everything else: **time**, **budget**, and **who's going**.

## What to Collect

### 1. Destination
- If already provided (e.g., `/trip-plan Tokyo`), confirm it
- If undecided, help narrow down:
  - "What region interests you? Asia, Europe, Southeast Asia...?"
  - "Beach relaxation, city exploration, nature adventure, or culture immersion?"
- Use web search to check if the destination has any travel advisories or seasonal warnings

### 2. Timing & Duration
- When: "What month or season are you thinking?"
- How long: "A long weekend, a week, or longer?"
- Check seasonal factors using weather data or web search:
  - "October in Japan is autumn foliage season — popular but worth it"
  - "August in Bangkok is monsoon season — cheap but wet"
- Flag any major holidays or events that could affect availability/prices

### 3. Budget Level
Ask naturally — don't make them pick from a rigid list. Listen for cues:

| Budget Vibe | Accommodation | Food | Transport |
|-------------|--------------|------|-----------|
| Backpacker | Hostels, guesthouses | Street food, local | Public transit, walk |
| Moderate | 3-star hotels, Airbnb | Mix of local + sit-down | Mix of transit + rideshare |
| Comfortable | 4-star hotels, nice Airbnb | Restaurants, some fine dining | Rideshare, some private |
| Luxury | 5-star, boutique hotels | Fine dining, curated experiences | Private drivers, first class |

If they give a specific number, convert it to a daily per-person rate and map it to the budget level using the regional cost tables in **`references/budget-guidelines.md`**.

Use `AskUserQuestion` to gather each piece of information conversationally, one at a time.

### 4. Companions
- Solo, couple, family (ages of kids?), group of friends, multi-generational?
- This affects: pacing, activity types, accommodation needs, restaurant choices
- Ask about accessibility needs or dietary restrictions only if relevant

### 5. Travel Style
- Ask what type of experience they want: city exploration, nature, food-focused, cultural immersion, relaxation, or mix
- This shapes activity recommendations in Step 2 and pacing in Step 3

## Output

After gathering all info, summarize concisely:

```
Trip Framework:
- Destination: Tokyo, Japan
- Dates: Oct 1-8, 2026 (7 nights)
- Budget: Moderate (~¥15,000 CNY total for 2 people)
- Travelers: 2 adults (couple)
- Style: City exploration + food culture
```

Then present the summary to the user and proceed to the next step.
