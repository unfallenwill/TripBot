---
name: framework
description: |
  Set trip framework and research the destination — dates, budget, companions, then find attractions, activities, restaurants, and local tips. Use when the user asks about "trip basics", "travel dates", "trip budget", "travel companions", "where should I go", "how long should I travel", "research destination", "what to do in", "must-visit places", "things to see", "attractions", "local recommendations", "旅行预算", "旅行时间", "景点推荐", "必去的地方", "旅游攻略", "旅行研究", "travel framework".
user-invocable: true
argument-hint: "[destination]"
allowed-tools:
  - Read
  - AskUserQuestion
  - WebSearch
  - WebFetch
---

# Step 1: Framework + Research

First lock the trip constraints, then turn the destination into a prioritized list of things to do.

## Expected Input

When invoked from the orchestrator, the following may already be available in conversation context:

| Field | Source | Required? |
|-------|--------|-----------|
| Destination | User argument or prior conversation | Recommended |
| Dates / duration | Not yet collected | No |
| Budget | Not yet collected | No |
| Companions | Not yet collected | No |
| Travel style | Not yet collected | No |

**Missing Context Protocol**: If invoked independently (e.g., `/framework` without prior conversation), start fresh from Part A. Do not assume any prior context.

## Output

This skill produces two conversational outputs (no files):

1. **Trip Framework** — A concise summary of destination, dates, budget, companions, and style (see "Framework Output" template below)
2. **Research Results** — Prioritized Must-Go and Maybe-Go lists, food highlights, area overview, and practical tips (see "Research Output Format" below)

Both outputs live in conversation context only. The orchestrator will reaffirm them via its Context Checkpoint.

## Part A: Set Framework

Gather the three essential constraints that shape everything else: **time**, **budget**, and **who's going**.

### What to Collect

Ask in 3 conversational groups instead of 5 separate rounds. Combine related questions naturally.

#### Group 1: Destination

- If already provided (e.g., `/trip-plan Tokyo`), confirm it
- If undecided, help narrow down:
  - "What region interests you? Asia, Europe, Southeast Asia...?"
  - "Beach relaxation, city exploration, nature adventure, or culture immersion?"
- Use web search to check if the destination has any travel advisories or seasonal warnings

**Early teaser**: Once the destination is confirmed (even before collecting other constraints), share a quick seasonal insight to give immediate value:
> "Great choice — October is actually peak autumn foliage season there. The colors around the temples are supposed to be incredible."

This can be a one-sentence web-search fact. It builds trust and makes the rest of the questions feel worth answering.

#### Group 2: Timing & Duration

Combine into one conversational ask:
- "When are you thinking, and for how long? Even a rough idea like 'a week in October' works."
- Check seasonal factors using weather data or web search:
  - "October in Japan is autumn foliage season — popular but worth it"
  - "August in Bangkok is monsoon season — cheap but wet"
- Flag any major holidays or events that could affect availability/prices

#### Group 3: Budget, Companions & Style

Combine into 1-2 conversational asks. Listen for cues rather than asking each separately:

| Budget Vibe | Accommodation | Food | Transport |
|-------------|--------------|------|-----------|
| Backpacker | Hostels, guesthouses | Street food, local | Public transit, walk |
| Moderate | 3-star hotels, Airbnb | Mix of local + sit-down | Mix of transit + rideshare |
| Comfortable | 4-star hotels, nice Airbnb | Restaurants, some fine dining | Rideshare, some private |
| Luxury | 5-star, boutique hotels | Fine dining, curated experiences | Private drivers, first class |

If they give a specific number, convert it to a daily per-person rate and map it to the budget level using the regional cost tables in **`references/budget-guidelines.md`**.

Sample conversational prompt:
> "Who's going and what's the budget vibe — are we talking hostel life or hotel comfort? And what kind of trip is this: city exploration, food-focused, or a mix?"

**Conditional follow-ups** (ask only when triggered):
- Accessibility needs: ask if travelers include seniors or children
- Dietary restrictions: ask if the travel style is food-focused

### Framework Output

After gathering all info, summarize concisely:

```
Trip Framework:
- Destination: Tokyo, Japan
- Dates: Oct 1-8, 2026 (7 nights)
- Budget: Moderate (~¥15,000 CNY total for 2 people)
- Travelers: 2 adults (couple)
- Style: City exploration + food culture
```

Present the summary to the user, then proceed to research.

---

## Part B: Do Research

Turn the destination into a prioritized list of things to do. The goal is two lists: **Must-Go** (non-negotiable) and **Maybe-Go** (nice to have).

### 1. Gather Information

Use multiple sources to build a comprehensive picture:

**Primary sources** (use in order of reliability):
- **Web search**: "best things to do in {destination} {year}" — gets current info
- **Google Maps MCP** (if available): Search for POIs by category in the destination
- **If no MCP**: Use web search ("best things to do in {destination}", "top attractions {destination}") and WebFetch on travel blog articles for structured POI data
- **Travel sites**: Fetch content from travel blogs and guides for insider tips

**Categories to research**:
- Top attractions (temples, museums, landmarks)
- Food & dining (must-try local dishes, notable restaurants, markets)
- Neighborhoods/areas worth exploring
- Cultural experiences (festivals, workshops, shows)
- Nature & outdoor activities
- Shopping (local specialties, markets)
- Nightlife (include if budget is Moderate or above and companions are adults without children)

### 2. Organize Findings

Group results by **area/neighborhood** — this is critical for itinerary building. Use the structured format below for presenting Must-Go/Maybe-Go lists, food highlights, and practical tips.

### 3. Prioritize into Lists

**Must-Go criteria** (include if any apply):
- Iconic / symbolic of the destination (e.g., Senso-ji for Tokyo, Colosseum for Rome)
- Unique to this location (can't experience it elsewhere)
- Strongly aligns with the traveler's stated interests
- Highly rated across multiple sources (>4.5 stars)

**Maybe-Go criteria**:
- Interesting but not unique
- Weather-dependent or seasonal
- Requires extra effort (far from center, booking ahead)
- Good backup options

### 4. Present to User

Show both lists in a clean format:

#### Must-Go (ranked by priority)
1. **Senso-ji Temple** (Asakusa) — Tokyo's oldest temple, iconic
2. **Tsukiji Outer Market** (Tsukiji) — Fresh seafood paradise
3. **Meiji Shrine** (Shibuya) — Serene forest shrine in the city
...

#### Maybe-Go (by area)
- **teamLab Borderless** (Azabudai) — Immersive digital art museum
- **Akihabara** (Akihabara) — Electronics and anime culture
...

Then ask: "Should any Maybe-Go items be promoted to Must-Go? Or anything you'd remove?"

---

## Research Output Format

### Must-Go List

| # | Name | Area | Type | Time Needed | Cost | Why Visit |
|---|------|------|------|-------------|------|-----------|
| 1 | | | | | | |

### Maybe-Go List

| # | Name | Area | Type | Time Needed | Cost | Why Visit |
|---|------|------|------|-------------|------|-----------|
| 1 | | | | | | |

### Areas/Neighborhoods Overview

| Area | Vibe | Key Attractions | Good For |
|------|------|-----------------|----------|

### Food Highlights

| Dish/Restaurant | Area | Price Range | Must-Try? |
|-----------------|------|-------------|-----------|

### Practical Tips

- **Best time to visit**: {seasonal advice}
- **Getting around**: {transport overview}
- **Cultural notes**: {local customs, etiquette}
- **Common tourist traps**: {things to skip}
- **Hidden gems**: {off-the-beaten-path recommendations}

### Seasonal Considerations (for {dates})

- Weather: {expected conditions}
- Events: {festivals, holidays during visit}
- Crowds: {peak/off-peak assessment}
