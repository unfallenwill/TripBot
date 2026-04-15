---
name: itinerary
description: |
  Build a day-by-day travel itinerary with geographic clustering and rhythm balance. Use when the user asks to "build itinerary", "create schedule", "plan daily route", "organize days", "trip schedule", "day-by-day plan", "安排行程", "日程规划", "路线规划", "行程编排".
user-invocable: true
allowed-tools:
  - Read
  - Write
  - Edit
  - WebSearch
  - WebFetch
---

# Step 3: Build Itinerary

Arrange the Must-Go list into a logical day-by-day plan. The three principles: **geographic clustering**, **rhythm balance**, and **buffer time**.

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

**Use the day-by-day template in `references/itinerary-template.md`** for each day's layout, including arrival day, full days, and departure day formats, plus the summary stats section.

## Validation Checklist

After building the itinerary, verify:
- [ ] No backtracking across the city (geographic logic holds)
- [ ] Heavy days are followed by lighter days
- [ ] Each day has buffer time built in
- [ ] Restaurants/food areas are near activity clusters
- [ ] Must-Go items are all scheduled
- [ ] Maybe-Go items are slotted as flex options
- [ ] Opening hours are respected (flag if uncertain)
