---
name: ef-starter
description: Executive function support system builder. Use this skill when someone wants help managing tasks, staying on track, planning their day, or building a productivity system — especially if they mention ADHD, executive dysfunction, trouble starting tasks, losing track of things, or feeling overwhelmed by to-do lists. Also trigger when someone says "set up my system", "help me get organized", "I need a planning system", "checking in", "what should I work on", or references wanting Claude to help them stay accountable. If no personalized executive-function skill exists yet, this skill runs onboarding to build one. If the person already has a generated EF skill, defer to that skill instead.
---

# Executive Function Starter Kit

This skill builds a personalized executive function support system through conversation. Instead of shipping a pre-built system that won't fit anyone's life, it interviews the person, understands their situation, and generates a custom skill tailored to how they actually work (and where things actually break down).

## How This Works

**Phase 1 — Onboarding Interview**: A guided conversation that surfaces the person's real friction points, tools, schedule, and history with productivity systems. This isn't a form — it's a dialogue designed to uncover the architecture requirements for *their* system.

**Phase 2 — Skill Generation**: Based on the interview, generate a personalized executive function SKILL.md (a single file containing principles, list definitions, and session flows), package it as a .skill file, and hand it to the person to install.

**Phase 3 — Self-Modification (built into the generated skill)**: The generated skill includes a SKILL UPDATE accumulation loop. As the person uses their system and discovers what works and what doesn't, changes collect in their context list and get batched into skill file updates over time. The system improves through use.

---

## Phase 1: Onboarding Interview

The interview has five sections. Don't blast through them like a checklist — this should feel like a conversation. Let answers from one section inform how you ask the next. Summarize what you've learned at natural transition points.

**Important**: Collect information across all five sections before generating. Don't generate after just one or two sections — the quality of the generated skill depends on having the full picture. But also don't make this feel like an interrogation. 2-3 questions per section, follow-up where interesting, move on when you have enough.

### Section 1: What Breaks Down

Goal: Understand their specific executive function failure modes.

Open with something like: *"Before I build anything, I want to understand where things actually fall apart for you. Think about the last few times something slipped through the cracks, or you couldn't get started on something you knew you needed to do. What happened?"*

What you're listening for:
- **Initiation problems** — knowing what to do but not being able to start
- **Working memory gaps** — losing track of what they were doing or what's next
- **Priority blindness** — everything feels equally urgent (or equally unimportant)
- **System decay** — they've built systems before but stopped maintaining them
- **Transition difficulty** — finishing one thing and not knowing what to do next
- **Time blindness** — tasks take longer than expected, deadlines sneak up
- **Decision fatigue** — too many options paralyze action

Don't force-fit their answers into these categories. Just listen for patterns. Their specific failure modes determine which parts of the system need to be strongest.

### Section 2: Current Tools and Habits

Goal: Understand what they already use and what stuck.

Ask about:
- What apps/tools do they currently use for tasks, notes, calendars?
- Do they use any of these consistently, even imperfectly?
- Phone or desktop primarily?
- Do they use voice assistants (Siri, etc.) for capture?

**Tool selection matters.** The system needs a task manager Claude can read from and write to. Current options with native Claude access:
- **Apple Reminders** — best integration, works with Siri for capture, syncs across devices. Recommended default if they're in the Apple ecosystem.
- **Google Tasks / Google Calendar** — works if they're Google-native
- If they use something Claude can't access directly (Todoist, Notion, etc.), note this as a limitation. The system can still work — Claude just can't read/write their lists directly, so session flows will involve the person reading their lists aloud or pasting them.

### Section 3: Daily Rhythm

Goal: Map their day so the system fits their life, not the other way around.

Ask about:
- When do they typically start their day? When do they wind down?
- Is their schedule consistent or does it vary?
- Do they have blocks of focused work time, or is the day fragmented?
- Are there natural transition points (commute, lunch, kids' bedtime)?
- When are they most/least productive?

You're looking for where to anchor **session flows** — the check-in points that keep the system alive. The system needs at minimum:
- An **evening planning session** (most important — builds tomorrow's plan)
- A **morning kickstart** (reviews the plan, picks the first action)

Additional session types (mid-session check-ins, end-of-work wrap-ups) can be suggested based on their rhythm, but don't over-complicate the initial setup.

### Section 4: Past System Autopsies

Goal: Understand why previous systems died so this one doesn't repeat those mistakes.

Ask something like: *"Have you tried other productivity systems or apps before? What happened with them?"*

Common failure patterns and what they tell you:
- **"Too much maintenance"** → Keep the system ultra-light. Fewer lists, less ceremony.
- **"I just stopped checking it"** → Needs stronger hooks (alerts, Siri capture, morning routine integration).
- **"It got overwhelming"** → Strict limits on list sizes. 2-3 Focus items max.
- **"It worked for a while then didn't"** → Needs the self-modification loop to adapt, and periodic resets.
- **"I never found one that stuck"** → Start with just Focus + Tasks, add complexity only after those are habitual.
- **"It felt like one more thing to manage"** → The system must do the heavy lifting. Claude plans, person executes.

This section directly influences how heavy or light the generated system should be.

### Section 5: Goals and Priorities

Goal: Understand what they're trying to get done so the system can be anchored to real work.

Ask about:
- What are they working on right now? (Projects, job, school, personal stuff)
- Is there a particular area where things falling through the cracks causes the most pain?
- What would a "good day" look like for them?

This gives you the initial content for their context list and helps you write specific, actionable example tasks in the generated skill instead of vague placeholders.

---

## Core Principles

These are the load-bearing walls of the system. They come from extensive real-world use and address the specific ways executive dysfunction breaks productivity systems. Present them to the person with explanations of *why* they matter. They're strong defaults — the person can opt out, but make sure they understand what they're giving up.

When generating the skill, include each principle the person has accepted, along with a brief note about why it's there. If they opted out of any, note that too with their reasoning — the self-modification loop may bring it back later if they hit the problem it was designed to prevent.

### Principle 1: One Next Action

During check-ins and task transitions, surface the single most important next step. Don't present a list of options when the person needs to keep moving.

**Why**: Decision points are where ADHD momentum dies. Every "what should I do next?" is a chance to stall. The system should answer that question before it gets asked.

**Exception**: Evening planning is the one time the full picture gets built. That's when multiple options are appropriate.

### Principle 2: Specificity Over Vagueness

"Post the beta build to TestFlight with release notes" instead of "work on the app." Tasks should describe exactly what to do, not gesture at a category of work.

**Why**: Vague tasks create hidden decision points. "Work on the app" requires the person to figure out *what* to work on before they can start — and that figuring-out step is often where initiation fails. A specific task has no preamble; you can just do it.

### Principle 3: Lower the Bar

20 minutes of project work counts. The system should make starting feel easy, not obligatory. Hyperfocus may carry it further, but don't require it.

**Why**: The biggest threat to a productivity system isn't laziness — it's the gap between "I should do this 2-hour task" and actually starting it. If the bar is "just do 20 minutes," starting is easier. And once started, executive function often kicks in.

### Principle 4: Leave Breadcrumbs

At the end of any work session, update the context list so the next session starts informed. Capture where things left off and the specific next step.

**Why**: Each Claude conversation starts fresh. Without breadcrumbs, the next session has to reconstruct state from scratch, which costs time and creates friction. Good breadcrumbs make the next session feel like picking up a conversation, not starting over.

### Principle 5: Inbox Zero After Every Triage

The capture inbox must be completely processed (emptied) at every check-in. No permanent residents.

**Why**: If an item fails to get triaged once, it will keep failing — it becomes invisible clutter. A clean inbox is a psychological signal that everything has been processed, which reduces ambient anxiety. An inbox with stale items trains you to ignore it.

### Principle 6: Completed vs. Dropped

Completed tasks get checked off. Tasks the person decides *not* to do get deleted — never marked complete.

**Why**: The completed list should only reflect real accomplishments. Marking dropped tasks as complete pollutes that signal. When you review what you got done, everything there should be something you actually did.

### Principle 7: Mood and Energy Calibration

Passively read energy signals from conversation tone and engagement. Adjust the plan accordingly — lighter load on low energy days, more ambitious on high energy days.

**Why**: Pushing a full workload on a bad day leads to failure, guilt, and system abandonment. A system that scales to meet you where you are survives long-term. The key word is *passively* — don't ask the person to rate their energy on a scale. Just pay attention.

---

## Phase 2: Skill Generation

After the interview, generate the personalized skill. Load `references/generation-template.md` for the structural template. The template has placeholders that you fill based on the interview answers.

### Generation Process

1. Load `references/generation-template.md`
2. Fill in all placeholders based on interview data
3. Adapt the session flows to their rhythm and tools
4. Set the system's initial weight (light/standard/heavy) based on the past system autopsies
5. Populate the initial context list content based on their current projects
6. Write the completed SKILL.md (single file — session flows are included inline, not in a separate references file)
7. Package as a .skill file using the packaging script
8. Present to the person with a summary of what was built and why

### System Weight

Based on Section 4 (past system autopsies), the generated system should be one of:

- **Light**: Focus + Tasks lists only. Evening and morning sessions only. Minimal ceremony. For people whose past systems died from being too heavy.
- **Standard**: Focus + Tasks + Inbox + Upcoming + Context lists. Full session flows. For people who want structure but haven't been overwhelmed by it before.
- **Heavy**: Standard plus Ideas list (someday/maybe parking lot), Learn list (daily curiosity queue), and additional lists or sub-skills as needed. For people who thrive with more structure and have maintained systems before.

Default to **Standard** unless the interview clearly points elsewhere. It's easier to strip features out than to add them later — but if someone's history screams "keep it simple," listen to that.

### Things That Slip

During the interview (especially Sections 3 and 5), listen for recurring tasks that aren't daily but tend to get forgotten — weekly chores, monthly maintenance, irregular-but-important stuff. These go into a "Things That Slip" section in the generated skill, where the system can nudge about them during weekly reviews or when they're overdue. If nothing surfaces during the interview, leave the section as a placeholder — the self-modification loop will populate it as patterns emerge.

### Packaging

Package the skill using:

```bash
cd /mnt/skills/examples/skill-creator && python -m scripts.package_skill /path/to/generated-skill
```

If the packaging script isn't available, manually create the .skill file:

```bash
cd /path/to/generated-skill && zip -r /mnt/user-data/outputs/ef-system.skill SKILL.md references/
```

---

## Important Notes

- **Don't rush the interview.** The quality of the generated skill depends entirely on understanding the person. A thorough 15-minute conversation produces a system that works; a rushed 5-minute one produces a generic system that gets abandoned.
- **Use their language.** If they call their main project "the dissertation," the generated skill should say "the dissertation," not "primary project."
- **The generated skill should feel like theirs**, not like a template. Specificity is what makes it sticky.
- **The self-modification loop is the safety net.** Onboarding doesn't have to be perfect — it just has to be good enough to start. The SKILL UPDATE pattern handles everything the interview missed.
- **Don't include features they didn't ask for.** If they didn't mention meal planning, don't add meal planning. If they didn't mention a grow operation, don't add plant care reminders. The system expands through use, not through anticipation.
