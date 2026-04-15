---
name: research
description: |
  Research a travel destination — find attractions, activities, restaurants, and local tips. Use when the user asks to "research destination", "what to do in", "must-visit places", "things to see", "attractions", "local recommendations", "景点推荐", "必去的地方", "旅游攻略", "旅行研究".
user-invocable: true
argument-hint: "[destination]"
allowed-tools:
  - Read
  - WebSearch
  - WebFetch
---

# Step 2: Do Research

Turn a destination into a prioritized list of things to do. The goal is two lists: **Must-Go** (non-negotiable) and **Maybe-Go** (nice to have).

## Research Process

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
- Nightlife (if relevant to the traveler)

### 2. Organize Findings

Group results by **area/neighborhood** — this is critical for Step 3 (itinerary building). **Use the structured format in `references/research-template.md`** for presenting Must-Go/Maybe-Go lists, food highlights, and practical tips.

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
