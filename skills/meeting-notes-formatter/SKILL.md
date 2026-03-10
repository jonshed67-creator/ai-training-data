---
name: meeting-notes-formatter
description: >
  TRIGGER: Use this skill when the user asks to format meeting notes, clean up meeting
  notes, organize meeting notes, structure meeting notes, "format these notes", "clean up
  my meeting notes", "organize these notes from a meeting", "structure my notes", "format
  this transcript", "turn this into meeting minutes", "meeting summary", or when the user
  pastes messy bullet points, stream-of-consciousness text, or voice transcript dumps
  that appear to be from a meeting and wants them restructured into a clean, actionable
  format.
---

# Meeting Notes Formatter

Transform messy meeting notes — bullet points, stream-of-consciousness dumps, voice transcripts — into clean, structured meeting minutes with attendees, decisions, action items, and follow-ups.

## Step-by-Step Process

### 1. Analyze the Raw Input

Read through the entire input first to identify:

- **Meeting topic/purpose**: What was this meeting about?
- **Attendees**: Names mentioned, speakers in a transcript
- **Date**: Mentioned date, or ask the user
- **Key discussions**: Topics that were debated or explored
- **Decisions**: Conclusions that were reached
- **Action items**: Tasks assigned to specific people
- **Open questions**: Unresolved items needing follow-up

### 2. Extract Attendees

Look for attendees in:
- Explicit lists: "Present: Alice, Bob, Carol"
- Transcript speakers: "Alice: I think we should..."
- Mentions in context: "Bob agreed to handle..."
- Email signatures or @mentions

If attendees aren't clear, list the names that appear and mark the list as "Mentioned" rather than "Attendees."

### 3. Identify Action Items

An action item has three components:
1. **What**: The task to be done
2. **Who**: The person responsible (owner)
3. **When**: The deadline or timeframe

Look for signals:
- "X will do Y by Z"
- "Action: ..."
- "TODO: ..."
- "We need to..."
- "Someone should..."
- "Let's make sure..."
- Verb phrases assigned to a person

If an action item is missing an owner, flag it as `[Owner TBD]`.
If it's missing a deadline, flag it as `[Deadline TBD]`.

### 4. Identify Decisions

Decisions are conclusions the group agreed on. Look for:
- "We decided to..."
- "The decision is..."
- "We're going with..."
- "Agreed: ..."
- "Everyone agreed that..."
- Consensus statements after a discussion

Distinguish decisions from action items — a decision is a conclusion, an action item is a task.

### 5. Format the Output

Use this structure:

```markdown
# Meeting: [Topic/Title]

**Date:** [Date]
**Attendees:** [Names, comma-separated]
**Note-taker:** [Name, if known]

---

## Summary

[2-3 sentence executive summary of the meeting — what was discussed and the key outcomes]

## Discussion Points

### [Topic 1]

[Summary of the discussion around this topic. Include key arguments,
concerns raised, and context that led to any decisions.]

### [Topic 2]

[Summary of discussion]

## Decisions

| # | Decision | Context |
|---|----------|---------|
| 1 | Use PostgreSQL instead of MongoDB for the user service | Better suited for relational data; team has more Postgres experience |
| 2 | Launch beta to internal users first | Reduces risk; gives us a feedback loop before public launch |

## Action Items

| # | Task | Owner | Deadline | Status |
|---|------|-------|----------|--------|
| 1 | Set up PostgreSQL staging instance | Alice | Mar 15 | Pending |
| 2 | Draft beta announcement email | Bob | Mar 12 | Pending |
| 3 | Update API docs for new auth flow | Carol | Mar 18 | Pending |
| 4 | Schedule follow-up design review | [TBD] | Next week | Pending |

## Open Questions

- How will we handle data migration from the current MongoDB instance?
- Do we need legal review before the beta launch?

## Next Meeting

[Date/time if mentioned, or "TBD"]

---

*Notes formatted from raw meeting transcript on [today's date]*
```

### 6. Formatting Rules

- **Be concise**: Summarize discussions, don't transcribe them verbatim
- **Preserve nuance**: If there was disagreement, note it. Don't flatten debates into false consensus.
- **Use active voice**: "Alice will set up the database" not "The database will be set up by Alice"
- **Quantify when possible**: "3 of 5 team members preferred option A" is better than "most people liked option A"
- **Don't invent information**: If something is ambiguous in the notes, mark it as `[unclear]` rather than guessing
- **Maintain original meaning**: Don't editorialize or add interpretation. Restructure, don't rewrite.
- **Attribute opinions**: "Bob raised concerns about timeline" not "There were concerns about timeline"

## Handling Different Input Types

### Bullet Points
```
- talked about new homepage design
- alice showed mockups
- bob didn't like the header
- going with version 2
- need to finish by friday
```
These are easy — group into topics, extract decisions and action items.

### Stream of Consciousness
```
So we talked about the api and john thinks we should use graphql but
sarah pointed out rest is simpler and we already have rest endpoints
so we decided to stick with rest for now but maybe revisit graphql
for the mobile app later...
```
Break into sentences, identify the discussion (GraphQL vs REST), the decision (stick with REST), and any action items (revisit for mobile).

### Voice Transcripts
```
Speaker 1: Okay so um let's talk about the the deployment timeline
Speaker 2: Yeah I think we can we should aim for next Friday
Speaker 1: That's a bit aggressive don't you think
Speaker 2: Maybe but if we if we cut the admin panel from this release
Speaker 1: Yeah that makes sense let's do that
```
Remove filler words (um, uh, like), fix fragments, identify speakers by name if possible, and extract the decision (cut admin panel, aim for Friday).

## Edge Cases

- **No clear action items**: Some meetings are purely informational. Note that no action items were identified and focus on the discussion summary.
- **Very long meetings**: For meetings over an hour with many topics, add a table of contents at the top.
- **Multiple meetings in one dump**: If the notes clearly cover separate meetings, split them into separate formatted documents.
- **Unclear attendees**: List everyone mentioned and mark the list as approximate.
- **Non-English notes**: Format in the same language as the input. Don't translate unless asked.
- **Sensitive content**: Don't flag or redact content — format it as-is. The user shared it intentionally.
