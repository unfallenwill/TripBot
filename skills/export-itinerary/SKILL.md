---
name: export-itinerary
description: |
  Export the complete trip plan as a structured Markdown document. Use when the user asks to "export itinerary", "download trip plan", "save my itinerary", "get trip summary", "export to file", "导出行程", "保存行程单", "下载攻略".
user-invocable: true
allowed-tools:
  - Read
  - Write
  - Edit
  - AskUserQuestion
---

# Export Itinerary

Compile all 5 steps into a single, shareable Markdown document that the user can take with them on their trip.

## Process

### 1. Collect Trip Data
Gather all trip information from the current conversation context:
- Framework (destination, dates, budget, companions)
- Must-Go and Maybe-Go lists
- Day-by-day itinerary
- Transport and accommodation notes
- Any prep checklist items already noted

### 2. Compile the Document

**Read `references/export-template.md`** for the complete export template with all sections: Quick Reference, Day-by-Day Itinerary, Transport, Accommodation, Budget Breakdown, Prep Checklist, and Important Contacts.

Fill in the template with data collected from the conversation.

**Language**: The template is a structural reference — all headings, labels, table headers, and instructional text **must be written in the same language the user is using**. For example, if the user speaks Chinese, use "## 快速参考" instead of "## Quick Reference"; if Japanese, use "## 日程表" instead of "## Day-by-Day Itinerary". Content values (place names, addresses) stay in their original language.

### 3. Save the File

Write the compiled document to:
- `tripbot-{destination}-{start_date}.md` in the current project directory (use ISO date format, e.g., `tripbot-tokyo-2026-10-01.md`)
- Or use `AskUserQuestion` to let the user specify a custom path

### 4. Confirm

Tell the user:
- Where the file was saved
- File size / section count
- Suggest: "You can convert this to PDF with any Markdown renderer, or just keep it on your phone for offline reference."
