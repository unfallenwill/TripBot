---
name: itinerary
description: |
  Build a day-by-day travel itinerary with geographic clustering and rhythm balance. Use when the user asks to "build itinerary", "create schedule", "plan daily route", "organize days", "trip schedule", "day-by-day plan", "安排行程", "日程规划", "路线规划", "行程编排".
user-invocable: true
allowed-tools:
  - Read
  - AskUserQuestion
  - WebSearch
  - WebFetch
---

# Step 2: Build Itinerary

Arrange the Must-Go list into a logical day-by-day plan. The three principles: **geographic clustering**, **rhythm balance**, and **buffer time**.

## Expected Input

| Field | Source | Required? |
|-------|--------|-----------|
| Destination | From Step 1 framework | Yes |
| Dates and duration | From Step 1 framework | Yes |
| Must-Go list | From Step 1 research | Yes |
| Maybe-Go list | From Step 1 research | Helpful |
| Travel style | From Step 1 framework | Helpful |
| Companions | From Step 1 framework | Helpful (affects pacing) |
| Budget level | From Step 1 framework | Helpful (affects activity choices) |

**Missing Context Protocol**: If invoked independently, ask the user for the destination and duration at minimum. If they lack a Must-Go list, offer to do a quick research pass or ask them to name their top 3-5 priorities.

## Output

This skill produces one conversational output (no files):

1. **Day-by-Day Itinerary** — Structured schedule using the Arrival Day / Full Day / Departure Day templates, plus Summary Stats

Use `AskUserQuestion` to present the draft itinerary and iterate based on user feedback before finalizing. The orchestrator will reaffirm the result via its Context Checkpoint.

## Building the Itinerary

### 1. Cluster by Geography

Group attractions by area/neighborhood. The goal: minimize travel time between stops.

**If Google Maps MCP is available**:
- Use `distance_matrix` to measure travel times between clusters
- Assign each day to one geographic area

**If no MCP**:
- Use web search: "{destination} neighborhoods map" or "{attraction A} to {attraction B} distance"
- Group by proximity based on known geography

### 2. Assign Days

- Number of days = end_date - start_date
- Assign each geographic cluster to a day
- Consider opening hours (museums closed Mondays, markets best in morning, etc.)

### 3. Apply Rhythm Balance

Alternate between high-energy and lighter days. **Read `references/rhythm-balance.md`** for the full methodology including day classification, sequencing rules, buffer time guidelines, and adjustments for different traveler types (solo, couple, family, group, seniors).

Key rules: arrival/departure days are always light. After a heavy day (3-4 attractions, ~8h), schedule a light day. Leave 20% padding between activities. Apply buffer time rules from `references/rhythm-balance.md` (15-30 min between activities, 2-3 hours daily flex).

### 4. Sequence Within Each Day

Order activities considering:
- **Time of day**: Markets in morning, temples early (beat crowds), nightlife at night
- **Energy curve**: Active things when energy is high (morning), passive things when tired (afternoon)
- **Meal timing**: Schedule near good food areas around lunch/dinner
- **Lighting**: Outdoor sightseeing in golden hour, indoor museums in midday

## Output Format

Use the following templates for each day type:

### Arrival Day

```
### Day 1 — {Date} | Arrival & {Area}

**Theme**: Settle in, light exploration

| Time | Activity | Location | Duration | Notes |
|------|----------|----------|----------|-------|
| Afternoon | Check in, rest | Hotel | ~1h | Recover from travel |
| Late afternoon | Walk around neighborhood | {Area} | ~2h | Get bearings |
| Evening | Welcome dinner | {Restaurant} | ~1.5h | Try local cuisine |

**Flex time**: 3+ hours
**Getting around**: Walk / taxi from airport
```

### Full Day

```
### Day {N} — {Date} | {Area Focus}

**Theme**: {Theme}

| Time | Activity | Location | Duration | Notes |
|------|----------|----------|----------|-------|
| Morning | {Must-go #1} | {Area A} | ~2h | Go early to beat crowds |
| Late morning | {Must-go #2} | {Area A} | ~1.5h | Walking distance from #1 |
| Lunch | {Local food spot} | {Area A} | ~1h | |
| Afternoon | {Must-go #3} | {Area B} | ~2h | Short transit ride |
| Free time | Explore / Maybe-go item | {Area B} | 2-3h | No fixed plan |
| Evening | {Activity or restaurant} | {Area B} | ~1.5h | |

**Flex time**: 2-3 hours
**Getting around**: {Metro / walk / bus}
```

### Departure Day

```
### Day {N} — {Date} | Departure

**Theme**: Last impressions

| Time | Activity | Location | Duration | Notes |
|------|----------|----------|----------|-------|
| Morning | Souvenir shopping / final sight | {Central area} | ~2h | |
| Late morning | Pack up, check out | Hotel | ~1h | |
| Afternoon | Head to airport/station | — | — | Allow extra time |
```

### Summary Stats

After all days, include:

```
## Summary Stats

- Total attractions: {N} must-go + {N} maybe-go
- Heavy days: {N} | Light days: {N}
- Estimated daily walking: ~{N} km average
- Estimated daily transit cost: ~{currency amount}
```

## Validation Checklist

After building the itinerary, verify:
- [ ] No backtracking across the city (geographic logic holds)
- [ ] Heavy days are followed by lighter days
- [ ] Each day has buffer time built in
- [ ] Restaurants/food areas are near activity clusters
- [ ] Must-Go items are all scheduled
- [ ] Maybe-Go items are slotted as flex options
- [ ] Opening hours are respected (flag if uncertain)
