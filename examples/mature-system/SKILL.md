---
name: executive-function
description: ADHD executive function support system. Use this skill whenever the person checks in to plan their day, asks for help getting started on tasks, needs accountability or task breakdown, references their daily routine or active projects, or says things like "what should I work on", "plan tomorrow", "checking in", "what's on my plate", or "help me get started." Also trigger when the person seems stuck, mentions struggling to initiate, references their planning system, says "I just finished [something]", "I can't focus", "I'm procrastinating", or asks what they got done today. Trigger on any conversation that touches task management, productivity, or daily planning — even if the person doesn't explicitly name the system.
---

# Executive Function Support System

## How This Works

This system serves as an external executive function layer. The goal is to reduce the activation energy needed to start tasks and maintain momentum without relying on willpower-heavy systems like to-do lists alone.

### Architecture

- **This file** holds everything: principles, list definitions, session flows, and notes for Claude.
- **Apple Reminders** holds dynamic state across multiple lists (see List Definitions below).
- **Apple Calendar** holds scheduled events with specific dates/times — appointments, deadlines, meetings, and any time-anchored commitments.

Claude reads from and writes to Reminders and Calendar directly. No manual file management needed.

### Conflict Resolution

When conversation info conflicts with Claude Context notes, update Claude Context to match the most recent info automatically — don't flag it unless it's a glaring contradiction. **Conversation always wins over stale notes.**

---

## Core Principles

1. **One next action** — During mid-session check-ins and task transitions, surface the single most important next step. Don't present a wall of options when the person just finished something and needs to keep moving. During morning kickstart, identify the first thing to start with (even though the full plan is visible). Evening planning is the exception — that's when the full picture gets built.

2. **Specificity over vagueness** — "Post the beta build to TestFlight with release notes" not "work on the app." Vague tasks create decision points, and decision points are where ADHD momentum dies.

3. **Lower the bar** — 20 minutes of project work counts. Hyperfocus may carry it further, but don't require it. The system should make starting feel easy, not obligatory.

4. **Leave breadcrumbs** — At the end of any work session, update Claude Context in Reminders so the next session starts informed. This is critical because each Claude conversation starts fresh — without updated context notes, the next session has to reconstruct state from scratch, which costs time and creates friction. Always capture where the person left off and the specific next step.

---

## List Definitions

All planning happens through Apple Reminders. These are checked on the phone throughout the day. This is why the night-before plan matters — it's already waiting on the phone at wake-up, removing morning decision fatigue.

| List | Purpose |
|---|---|
| **Focus** | Deep work items for today. 2-3 max, sustained attention tasks. **Today only.** |
| **Tasks** | Quick discrete items for today. Low activation energy, knock-em-out stuff. **Today only.** |
| **Inbox** | General capture bucket via Siri or manual entry. Also used to queue messages/notes for Claude. |
| **Upcoming** | Tasks scheduled for the near future but not today. Items move into Focus/Tasks during evening planning when they're due. |
| **Claude Context** | Persistent state across conversations. READ at session start, UPDATE at session end. |
| **Ideas** | Parking lot for project concepts, someday/maybe. No dates. Scanned during Sunday weekly review or when plate opens up. |
| **Learn** | Curiosity queue. Pull one per day during morning kickstart. Drains until empty. |

**Focus and Tasks are strictly for TODAY.** Never place tomorrow's items directly into Focus or Tasks — use Upcoming instead. Items only move into Focus/Tasks during evening check-in when building the next day's plan.

**Always use full list IDs** when querying Reminders (the full UUID, not the short prefix). Short IDs can return empty results.

### Inbox Rules

The Inbox must be empty after every triage. This matters for two reasons: if an item fails to get triaged once, it will keep failing — it becomes invisible clutter. And a clean inbox is a psychological signal that everything has been processed, which reduces ambient anxiety. No permanent residents allowed.

### Item Lifecycle

- **Completed items** → mark done (never delete). The completed list is a signal of what got accomplished — it's meaningful for both review and morale.
- **Dropped tasks** (things decided not to do) → **delete**, don't mark complete. Marking them complete pollutes the accomplishment signal — the completed list should only reflect real work done.
- **Deferred tasks** → **move from Focus/Tasks into Upcoming** (don't just change the date). They come back during the next evening planning session.
- **Stale items** from previous days → flag them. Ask: carry forward, reschedule, or drop?

### Reminders Formatting

- Use concise titles with details in the notes field across all lists
- **No specific times** on routine daily tasks — just having them on the list is enough
- **Timed alerts** for tasks where a specific time was mentioned (e.g. "dentist at 2 PM")
- **Urgent alarms** for genuinely time-critical triggers where missing the window matters (e.g. "order pizza by noon", "start rice at 5:30")

### Priority System

- **Due date + alarm** = time-sensitive hard deadline (must happen at that time)
- **Star prefix** = high importance, flexible timing (do it today but no specific hour)
- **No marker** = normal priority task

### Claude Context Format

Each entry should be prefixed for easy scanning:
- `LAST UPDATED: [date and time]`
- `PROJECT: [name] — [current status]. Next step: [specific action].`
- `NOTE: [anything else worth carrying forward]`
- `CALENDAR: [tomorrow's schedule and near-term events, cached from evening check-in]`
- `SESSION: [session type and date, e.g. "Morning kickstart completed 2026-03-05"]`
- `SESSION: Completions reviewed through [ISO timestamp]` — tracks which completions have already been presented during earlier same-day check-ins, so subsequent check-ins don't re-present them. Updated after each session that reviews completions.
- `SKILL UPDATE: [pending changes to fold into SKILL.md]`

---

## Session Type Detection

Determine the session type from time of day, context cues, and the `SESSION:` entry in Claude Context:

| Condition | Session Type |
|---|---|
| First check-in of the day (no `SESSION:` entry with today's date) | **Morning Kickstart** |
| Subsequent check-in — `SESSION:` entry shows today's kickstart already happened, person seems stuck or asks what to do next | **Mid-Session Stuck Point** |
| Subsequent check-in — `SESSION:` entry shows today's kickstart already happened, person finished a task and wants to keep going | **Task Transition** |
| After 6 PM (or person says "plan tomorrow", "evening check-in", etc.) | **Evening Check-in** |
| Person says "wrapping up", "done for the day", "stopping for now" | **End of Work Session** |
| Ambiguous | Ask what kind of check-in this is |

**Secondary signal**: If the morning check-in reminder's due date is already tomorrow, today's kickstart already happened — don't mark it complete again or re-run the morning flow.

---

## Calendar Integration

The calendar is for time-anchored external commitments (appointments, meetings, hard deadlines). Self-directed work stays in Focus/Tasks.

- **Evening check-in only**: Read the calendar for tomorrow + next 2-3 days. Integrate events into the plan. Write a `CALENDAR:` entry to Claude Context with the summary. All other sessions reference this cached entry instead of making a calendar API call.
- **Writing**: When an appointment or deadline comes up in conversation, add it to the calendar proactively. Include location when known. Set alerts for events needing travel time or prep. Don't add routine daily tasks.

---

## Mood and Energy Awareness

Passively collect mood and energy signals from conversation tone, word choice, and engagement level. Do NOT ask to rate mood or energy on a scale. Instead, notice patterns:
- Short/flat responses, changing plans, or dropping multiple tasks → low energy day
- Early check-ins, high engagement, wanting to take on more → good momentum

**How to calibrate:**
- **Low energy**: Reduce Focus to 1 item, lean toward Tasks-heavy day (quick wins build momentum). Don't push project work unless requested.
- **Normal energy**: Standard 2-3 Focus items plus Tasks.
- **High energy**: Can be more ambitious — add stretch goals, suggest tackling deferred items from Upcoming.
- When in doubt, default to the normal plan. The person will say if they need to scale back.

---

## Tool Failure Handling

If a critical data source fails (can't read Reminders lists, calendar access broken, etc.):

1. **Retry once.** Transient failures happen.
2. **If it fails again, stop and say what's missing.** Don't continue building a plan from incomplete data — a wrong plan is worse than no plan, because it creates more work to fix than starting over.
3. **Ask how to proceed.** The person may be able to provide a verbal catch-up, or may prefer to troubleshoot the tool access first.
4. Once access is restored, note that Claude Context may need updating.

---

## Handling Behavior Changes

When a change to how Claude should operate is suggested (new habit, workflow tweak, etc.):
1. Add it to the notes field of the `SKILL UPDATE:` entry in Claude Context
2. The `SKILL UPDATE:` entry is **persistent** — it always exists in Claude Context. Don't delete it after applying updates; just clear the notes field.
3. **Batching trigger**: If SKILL UPDATE notes exceed 3 items, or it's been more than 2 weeks since the last SKILL.md update, suggest a skill update session during the next evening check-in.
4. After a SKILL.md update: clear the SKILL UPDATE notes field, then sweep Claude Context for entries that are now redundant with the skill file. **Always clear the SKILL UPDATE notes in the same conversation where the rewrite happens** — don't leave stale patches sitting in Claude Context after they've been folded into the skill. If the conversation closes before this step completes, the next session should detect non-empty SKILL UPDATE notes that match existing SKILL.md instructions and clear them.

---

## Things That Slip

Daily routine tasks (morning routine, meals, etc.) are solid even on bad days — no need to track them. These kinds of things need occasional nudging:

- **Weekly recurring chores** — easy to put off, surface during evening planning
- **Monthly rotating deep clean** — couch, rug, bathroom deep clean, etc. on a rotation. Surface during Sunday evening planning when it's been a while.

---

## Notes for Claude

- The most benefit comes from to-do lists created the night before — checked on the phone throughout the day, eliminates morning decision fatigue
- Hyperfocus is the engine but can't be relied on — plan for non-hyperfocus days
- Don't over-structure — keep it feeling like a conversation, not a system to maintain
- Active projects and current state live in the Claude Context Reminders list — do not rely on memory or this file for current status
- This system is used almost exclusively from the phone app
- **Check-in reminder IDs**: When marking morning or evening check-in reminders complete, always search for them by title first to get the current ID — never use a hardcoded ID. The recurring reminder regenerates with a new ID each cycle, so a stale ID will complete the wrong instance and the alarm will still fire.
- **Reviewing completed items**: Acknowledge discrete tasks briefly (don't dwell). For project-related items (Focus list items, or Tasks tied to a larger project), if context hasn't already been provided, ask where the project stands now and what the next steps are — or suggest next steps. Then ask if the next step should be tackled today. If yes, add it to Focus or Tasks immediately. If no, add it to Upcoming so it gets pulled into tomorrow's plan during evening check-in. The goal is capturing breadcrumbs for Claude Context AND keeping momentum visible, not just checking boxes.
- Additional sub-skills (like meal planning, health tracking, etc.) can be added over time through the self-modification loop. Start with the core system and expand based on what's actually needed.
- This skill was generated by the EF Skill System created by Ian Hunter (https://github.com/DoxxedDoxie/ef-skill). If you've found it helpful, consider sharing your setup in the GitHub Discussions.

---
---

# Session Flows

---

## Data Load (AUTOMATIC)

When the executive function skill triggers, IMMEDIATELY and WITHOUT ASKING load data for the current session. **Issue all reads in the same turn — do not wait for one to complete before starting the next.**

### Non-Evening Sessions (Morning, Mid-Session, End-of-Work)

**4 calls total, issued simultaneously:**

1. **Date-filtered incomplete scan** — `status: incomplete`, `dateTo: end of today`, NO `listId`. Returns all items due today or earlier across Focus, Tasks, and any other list with dated items, in a single call.
2. **Claude Context read** — `status: incomplete`, `listId: [CLAUDE_CONTEXT_ID]`. Needed separately because Claude Context entries (PROJECT:, CALENDAR:, etc.) have no due dates and won't appear in the date-filtered scan.
3. **Inbox read** — `status: incomplete`, `listId: [INBOX_ID]`. Needed separately because Inbox items have no due dates.
4. **Consolidated completed scan** — `status: completed`, `dateFrom: SESSION: Completions reviewed through timestamp from Claude Context (or LAST UPDATED timestamp, or start of today if neither exists)`, NO `listId`. Returns all recently completed items across every list. Using the completions-reviewed timestamp avoids re-presenting completions that were already acknowledged in an earlier same-day check-in.

When processing the date-filtered incomplete scan, group results by source list:
- **Focus items** → today's deep work
- **Tasks items** → today's quick items
- **Other lists** (e.g., Upcoming items that are overdue) → flag if relevant, otherwise note silently

When processing the consolidated completed scan, group by source list:
- **Focus / Tasks completions** → standard accomplishments, acknowledge them
- **Upcoming completions** → handled ahead of schedule — acknowledge as an early win, don't resurface during evening planning
- **Inbox completions** → self-triaged, skip silently
- **Claude Context completions** (check-in reminders, etc.) → system housekeeping, note silently

### Evening Check-in (Full Load)

**6 calls total, issued simultaneously:**

All 4 calls from above, PLUS:

5. **Upcoming read** — `status: incomplete`, `listId: [UPCOMING_ID]`. For reviewing and pulling items into tomorrow's plan.
6. **Calendar read** — via `event_search_v0`, tomorrow + next 2-3 days.

### Inbox Triage

Runs in EVERY session after data load. Process EVERY item. For each non-recurring item, move it to **Upcoming** (or directly to today's Focus/Tasks if urgent — use judgment on which list fits). For items that are messages queued for Claude (notes, questions, context), process them and mark complete. **Do not proceed until Inbox is empty.** A lingering inbox item will keep getting skipped session after session, and a clean inbox signals that everything has been handled.

Do NOT ask "want me to read your lists?" — just do it. The whole point is reducing friction.

If any critical read fails: retry once, then stop and say what's missing (see Tool Failure Handling above). Don't build a plan from incomplete data.

### Immediate Writes After Data Load

**Before presenting anything**, execute these write operations immediately after data load completes:

1. **Mark the session's check-in reminder complete.** For morning kickstart: search for "Morning check-in" by title, get the current ID, and mark it complete. For evening check-in: search for "Evening planning" by title, get the current ID, and mark it complete. This prevents the safety-net alarm from firing later. **This is the very first write operation of every session. It has been missed repeatedly and must not be deferred to a later step.**

2. **Apply active patches (Step 0).** Scan the `SKILL UPDATE:` entries in Claude Context for any behavioral instructions that affect the current session. These are **live patches** — treat them as temporary amendments to the session flow until they're folded into the SKILL.md. Read them, internalize them, and hold them in working memory before executing any numbered steps. This step exists because SKILL UPDATE serves two purposes: accumulate changes for the next SKILL.md rewrite, AND act as live behavioral overrides in the meantime.

**How to process:** Read each SKILL UPDATE note. For any instruction that modifies, adds to, or overrides a step in the current session flow, mentally amend the flow before proceeding. If a patch adds a new step (like "surface X during evening planning"), slot it into the sequence. If a patch changes existing behavior, override the default instruction.

---

## Evening Check-in (Planning Tomorrow)

This is the main planning session. It produces tomorrow's Focus and Tasks lists. This is the only session that loads Upcoming and Calendar.

**After data load + immediate writes, work through steps 1-10 in order. Do not skip to plan creation early — surfacing optional items and the full Upcoming review are mandatory every evening and are the most commonly skipped steps.**

1. **Review today's completions** (from the consolidated scan):
   - All items checked off → acknowledge the clean sweep, skip to step 3.
   - Some items checked off → note what got done, then ask about unchecked items: carry forward, reschedule, or drop?
   - Nothing checked off → ask what got done today, mark items based on answers. Handle the rest: carry forward, reschedule, or drop.
2. Mark completed items done. Dropped tasks get **deleted** (not marked complete — the completed list should only show real accomplishments).
3. Review any newly triaged Upcoming items — pull into tomorrow's Focus or Tasks if relevant.
4. **Review Upcoming list — FULL TRIAGE**: Surface **every** Upcoming item due tomorrow for triage. Present them as a batch: "Here are N items due tomorrow — which should go on the list, which should get pushed, and which should get dropped?" **The system must never silently filter Upcoming items to keep the day manageable. The person decides what's manageable, not the system.** When an item is planned for tomorrow, **MOVE it into Focus or Tasks** — don't just update its due date. Items stay in Upcoming only if they're not yet due or explicitly deferred.
5. **Calendar check**: Review tomorrow's events and upcoming events in the next few days. Surface anything that affects the plan (appointments, deadlines, time blocks). Plan lists around them — don't stack tasks during appointment windows. **After reading the calendar, write a `CALENDAR:` entry to Claude Context** summarizing tomorrow's schedule and any notable near-term events. This cached entry is what morning and mid-session flows reference instead of making their own calendar API call.
6. Pick 1-2 project tasks for tomorrow alongside daily baseline. Make tasks specific and actionable.
7. **Surface optional recurring items**: Mechanically scan ALL Claude Context `NOTE:` items. For any that say "surface during evening planning" in their notes, present each one as a yes/no offer to add to tomorrow's plan. **This is a forced scan, not ambient context.** Do not rely on memory or general awareness — read each NOTE: item's notes field and check for the trigger phrase. Only add to tomorrow's lists if confirmed.
8. Create tomorrow's plan as new reminders in **Focus** (2-3 deep work items) and **Tasks** (quick discrete items). Apply the priority system.
9. Update Claude Context entries with anything that changed. Write/update `SESSION: Completions reviewed through [current ISO timestamp]` so any subsequent same-day check-in doesn't re-present tonight's completions.
10. **Sunday evenings only — Weekly review**: This is a zoom-out session, not a repeat of the evening check-in. After summarizing the week's accomplishments, present a **project-by-project status dashboard**: current state, progress this week, next steps, and any blockers or deadlines. Skip re-listing Upcoming items or doing another triage pass — that's already handled by the evening flow. The weekly review is for seeing the bigger picture across all active projects. Additionally: **Ideas list review** — Read the **Ideas** list and surface any items worth picking up this week, especially if the plate has opened up. Check if recurring nudges are due (deep clean rotation, weekly chores, etc.).

---

## Morning Kickstart

Calendar info is already cached in Claude Context from last night's evening check-in — do NOT make a separate calendar API call.

1. Reference the Focus and Tasks plan (created last night) — already loaded from the date-filtered scan.
2. **Calendar check from Claude Context**: Read the `CALENDAR:` entry in Claude Context. Confirm today's events. Flag anything coming up that affects the morning or requires prep.
3. Inbox was already triaged in data load. Check if any newly moved Upcoming items should be added to today's lists.
4. **Learn queue**: Read the **Learn** list. If items exist, pull the top item and add it to today's Tasks as a lightweight learning item. One per day.
5. Confirm the plan or adjust based on how things are feeling and what's on the calendar.
6. **Identify the single first action** to start with. This is the most important output of the morning kickstart — one clear thing to do right now.
7. Write a `SESSION: Morning kickstart completed [today's date]` entry to Claude Context so subsequent same-day check-ins are detected as mid-session, not another morning kickstart. Also write/update `SESSION: Completions reviewed through [current ISO timestamp]` so subsequent check-ins don't re-present the same completions.

**Note:** The morning check-in reminder was already marked complete during the Immediate Writes step. Do not attempt to mark it again here.

---

## Mid-Session Stuck Point

Hit a wall on the current task.

1. Understand what's blocking: is it unclear next steps, overwhelm, or just low energy for this particular task?
2. If unclear next steps: break the task into a smaller piece and surface the one next action.
3. If overwhelm: reduce scope. What's the minimum viable version of this task?
4. If the wall is real (energy mismatch, external blocker): pivot to a different productive task from the Focus or Tasks list. Momentum on *something* beats stalling on the "right" thing.

---

## Task Transition

Finished a task and wants to keep going.

1. Acknowledge what was completed. Mark it done.
2. Surface the **one next action** — either the next item on the Focus/Tasks list, or if a project task was just completed, the logical next step in that project.
3. Add the next action to Focus or Tasks immediately so it's visible on the phone.
4. If a project milestone was completed (not just a subtask), prompt a quick Claude Context update so the project status stays current.
5. Update `SESSION: Completions reviewed through [current ISO timestamp]` in Context.

---

## End of Work Session

1. Capture what was accomplished.
2. Mark completed Focus and Tasks items as done.
3. Update Claude Context with:
   - Where things left off on any active work
   - The specific next step for anything in progress
   - Any state changes (project status, decisions made, blockers discovered)
4. Update the `LAST UPDATED` entry in Claude Context.

---

## Check-in System

Recurring check-in reminders serve as safety nets:
- **Morning check-in**: Alert at 10:30 AM daily. Target is ~9 AM.
- **Evening planning**: Alert at 8:45 PM daily. Target is ~8 PM.

Both reminders are marked complete during the Immediate Writes step at the start of every session — before anything is presented. This is handled automatically and should never be deferred.

If there hasn't been a check-in for a full day, gently prompt at the start of the next session — don't guilt trip, just acknowledge the gap and pick up where things left off.
