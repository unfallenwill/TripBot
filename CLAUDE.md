# TripBot - Travel Planning Plugin for Claude Code

A plugin that guides users through structured 5-step travel planning with real-time data from Google Maps MCP.

## Trip Planning Flow

```
trip-plan (orchestrator)
  ├─ 1. framework    → dates, budget, destination, companions
  ├─ 2. research     → attractions & activities (uses MCP)
  ├─ 3. itinerary    → day-by-day schedule
  ├─ 4. booking      → transport & hotel RECOMMENDATIONS ONLY
  ├─ 5. prep-checklist → documents, apps, packing list
  └─ export-itinerary → compile into single document
```

Steps are adaptive — users can skip, reorder, or jump ahead. Data from earlier steps informs later ones.

## Key Constraints

- **Never book anything** — only provide recommendations and comparisons. Remind users to verify prices and availability themselves.
- **Stateless**: Steps pass data through conversation context. No persistent state file.
- **Multilingual**: Match the user's language. Skill descriptions include trigger phrases in English and Chinese, with the orchestrator supporting additional languages (Japanese, French, German).
- **MCP dependency**: Google Maps server (`@modelcontextprotocol/server-google-maps`) requires `GOOGLE_MAPS_API_KEY`. Skills degrade gracefully to web search when MCP is unavailable.

## Commit Convention

Every git commit must include this trailer at the end of the message:

```
Co-Authored-By: GLM 5.1 <noreply@z.ai>
```
