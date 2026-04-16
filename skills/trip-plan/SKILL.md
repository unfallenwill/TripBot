---
name: trip-plan
description: |
  "plan a trip", "travel planning", "organize my trip", "create a trip", "build itinerary", "make travel plan", "schedule my trip", "TripBot", "帮我规划旅行", "旅行计划", "旅行攻略", "做行程", "planifier un voyage", "Reise planen", "export itinerary", "download trip plan", "save my itinerary", "get trip summary", "导出行程", "保存行程单", "下载攻略". Covers framework, research, itinerary, booking recommendations, and export. DO NOT TRIGGER when: asking a single travel question, searching for one hotel/flight, translating text, or asking about visa requirements in isolation.
argument-hint: "[destination]"
user-invocable: true
allowed-tools:
  - Read
  - Write
  - Edit
  - AskUserQuestion
  - WebSearch
  - WebFetch
  - TaskCreate
  - TaskUpdate
---

## On Activation

1. **Parse arguments**: If destination provided (e.g., `/trip-plan Tokyo`), pre-fill it.
2. **Greet**: Match user language.
3. **Create tasks**: Call `TaskCreate` for each:
   - "Phase 1: Framework + Research"
   - "Phase 2: Build Itinerary"
   - "Phase 3: Booking + Prep"
   - "Export Itinerary"
4. Set Phase 1 `in_progress` via `TaskUpdate`.

## Navigation Rules

- **NEVER book anything** — only provide recommendations and comparisons.
- Adaptive order: skip completed parts, jump ahead on request.
- No repetition: never re-ask answered questions.
- After each phase: 1-2 sentence summary of decisions, then ask to proceed.

## Context Checkpoint

After each phase, reaffirm the key fields in 2-3 sentences (not a table):
- After Phase 1: destination, dates, duration, budget level, companions, travel style
- After Phase 2: day themes, total attraction count, unresolved slots
- After Phase 3: flight cost range, accommodation choice, remaining budget, checklist status

---

## Phase 1: Framework + Research

Mark task `in_progress` via `TaskUpdate` on start. Mark `completed` when done.

### Part A: Set Framework

Collect constraints in 3 groups. Ask related questions together.

**Group 1: Destination**
- If provided as argument, confirm it.
- If undecided, narrow by region or travel type (beach, city, nature, culture).
- Web-search for travel advisories or seasonal warnings.
- Share one seasonal insight immediately upon destination confirmation.

**Group 2: Timing & Duration**
- Ask in one prompt: "When are you thinking, and for how long?"
- Check seasonal factors (weather, events, holidays) via web search.

**Group 3: Budget, Companions & Style**
- Ask in 1-2 prompts. Infer from cues.
- Map specific numbers to budget level using `references/budget-guidelines.md`.
- Conditional follow-ups only: accessibility (seniors/children), dietary (food-focused trips).

Present a concise summary:

```
Trip Framework:
- Destination: Tokyo, Japan
- Dates: Oct 1-8, 2026 (7 nights)
- Budget: Moderate (~¥15,000 CNY total for 2 people)
- Travelers: 2 adults (couple)
- Style: City exploration + food culture
```

---

### Part B: Do Research

Build Must-Go and Maybe-Go lists.

1. **Gather**: Web search "{destination} best things to do {year}". Use Google Maps MCP if available; otherwise WebFetch travel blogs. Research categories: attractions, food, neighborhoods, culture, nature, shopping, nightlife (skip if budget is low or children present).
2. **Organize**: Group results by area/neighborhood.
3. **Prioritize**: Must-Go = iconic + unique + matches traveler interests + >4.5 stars. Maybe-Go = interesting but not unique / weather-dependent / far.
4. **Present**: Show both ranked lists. Ask user to promote or remove items.

**Read `references/research-output-format.md`** for the output table templates.

### Phase 1 Complete When

- [ ] Destination confirmed
- [ ] Dates and duration set
- [ ] Budget level mapped
- [ ] Must-Go list reviewed and approved by user
- [ ] Maybe-Go list presented

---

## Phase 2: Build Itinerary

Mark task `in_progress` via `TaskUpdate` on start. Mark `completed` when done.

1. **Cluster by geography**: Group attractions by area. Use Google Maps MCP `distance_matrix` if available; otherwise web search for distances.
2. **Assign days**: One geographic area per day. Check opening hours.
3. **Apply rhythm balance**: **Read `references/rhythm-balance.md`**. Key rules: arrival/departure days are light; after a heavy day, schedule a light day; leave 20% padding.
4. **Sequence within each day**: Morning = active + beat crowds. Afternoon = passive. Meals near activity clusters.

**Read `references/itinerary-templates.md`** for day templates (Arrival, Full, Departure) and Summary Stats format.

### Validation Checklist

- [ ] No backtracking across the city
- [ ] Heavy days followed by lighter days
- [ ] Each day has buffer time
- [ ] Restaurants near activity clusters
- [ ] All Must-Go items scheduled
- [ ] Maybe-Go items slotted as flex
- [ ] Opening hours respected

Present draft via `AskUserQuestion`. Iterate, then finalize.

---

## Phase 3: Booking + Prep

Mark task `in_progress` via `TaskUpdate` on start. Mark `completed` when done.

If invoked without prior context, ask for destination, dates, origin city, and budget at minimum. Offer quick research if no Must-Go list exists.

### Part A: Transport & Accommodation

**Flights**: Web search "{origin} to {destination} flights {month} {year} price". Present as comparison (route, price range, duration, stops, notes). Include booking tips (best window 3-8 weeks ahead, cheapest days Tue/Wed, baggage policies).

**Local transport**: Airport transfer options + costs. City transit passes. Whether to rent a car.

**Accommodation**: Web search "best hotels {area} {destination} {budget level} {year}". Cross-reference with itinerary areas. Present comparison (name, area, price/night, rating, pros, cons). Include: walking distance to attractions, transit access, neighborhood vibe, breakfast, cancellation policy.

**Cost summary**: Flights + local transit + accommodation = total hard costs. Show remaining budget.

Before proceeding to Part B, ask: "Want me to generate a prep checklist?"

### Part B: Prep Checklist

1. Web search for destination-specific: visa, vaccinations, currency, local SIM/eSIM, emergency numbers.
2. **Read `references/checklist-templates.md`**. Select template by trip type (international/domestic/adventure/business). Customize by: companion type, season, climate, electrical plug type.
3. Present as markdown `[ ]` checkboxes grouped by category. Mark time-sensitive vs. last-minute items.

### Phase 3 Complete When

- [ ] Transport comparison presented
- [ ] Accommodation comparison presented
- [ ] Cost summary with remaining budget shown
- [ ] Prep checklist generated and reviewed

---

## Export Itinerary

Mark task `in_progress` via `TaskUpdate` on start. Mark `completed` when done.

1. Collect all trip data from conversation context.
2. **Read `references/export-template.md`** for the complete template.
3. Fill template. Write all headings and labels in the user's language. Place names stay in original language.
4. Save to `tripbot-{destination}-{YYYY-MM-DD}.md` in current directory, or ask user for custom path.
5. Confirm: file location, section count. Suggest offline rendering options.
