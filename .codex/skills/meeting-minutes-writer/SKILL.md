---
name: meeting-minutes-writer
description: Write concise startup meeting minutes focused on decision traceability and accountability. Use when the user asks to draft, summarize, or log meeting outcomes with date, attendees, decisions, concise conclusion, and signatures/sign-off.
---

# Meeting Minutes Writer

## Goal
Capture what was decided, by whom, and who approved it in under one page.

## Output Rules
- Keep it brief and factual.
- Keep the conclusion to 2-3 lines.
- Keep each decision to one line when possible.
- Include explicit sign-off names and titles.

## Required Fields
- Meeting date
- Attendees
- Decisions made
- Short conclusion
- Signatures

## Template
Use this exact structure:

```markdown
# Minutes of Meeting

Date: [YYYY-MM-DD]
Meeting: [Topic]

Attendees:
- [Name - Role]
- [Name - Role]

Decisions:
1. [Decision] - Owner: [Name] - Reason: [short reason]
2. [Decision] - Owner: [Name] - Reason: [short reason]

Conclusion (Summary):
[2-3 short lines with final outcome and next action]

Sign-Off:
- [Name, Title, Signature/Approval]
- [Name, Title, Signature/Approval]
```

## Missing Data Handling
If input is incomplete, ask only for missing required fields, then produce the final minutes.
