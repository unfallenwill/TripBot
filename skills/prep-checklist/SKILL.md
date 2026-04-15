---
name: prep-checklist
description: |
  Generate a pre-trip preparation checklist covering documents, payment, apps, and essentials. Use when the user asks about "trip checklist", "what to pack", "travel documents", "preparation list", "before I go", "pre-trip checklist", "visa requirements", "行李清单", "出行准备", "旅行清单", "需要带什么".
user-invocable: true
allowed-tools:
  - Read
  - Write
  - Edit
  - WebSearch
  - WebFetch
---

# Step 5: Prep Checklist

Generate a comprehensive pre-trip checklist tailored to the specific destination, season, and traveler type.

## Process

### 1. Research Requirements
Use web search for destination-specific requirements:
- Visa/entry requirements for the traveler's nationality
- Vaccination requirements or health recommendations
- Currency and payment norms
- Local SIM/eSIM options
- Emergency numbers and embassy info

### 2. Generate Checklist

**Read `references/checklist-templates.md`** for complete checklist templates organized by travel type (international leisure, domestic, adventure/outdoor, business) plus a plug types by region table.

Select and customize the appropriate template based on:
- Trip type (international vs domestic, leisure vs adventure vs business)
- Companion type (family with kids needs extra items)
- Season and destination climate
- Destination's electrical plug type and voltage

### 3. Present as Checkable List

Use markdown checkboxes `[ ]` so the user can track progress. Group by category. Highlight items that are **time-sensitive** (visa, vaccinations) vs. **last-minute** (packing).
