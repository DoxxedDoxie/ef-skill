# EF Skill — Executive Function Support System for Claude

An ADHD executive function support system built as a Claude skill. It turns Claude into an external planning layer that uses your task manager (Apple Reminders, Google Tasks, etc.) as the dynamic state store.

You tell it about your life, it builds you a personalized system, and then it runs that system with you — planning your day the night before, running morning kickstarts, handling task transitions, adapting to your energy level, and getting better over time through a self-modification loop.

## Why this exists

Every feature in this system exists because something broke without it. The "one next action" principle exists because I kept stalling at decision points between tasks. The "lower the bar" principle exists because I kept not starting things that felt too big. The inbox zero rule exists because items I didn't triage once never got triaged at all.

This isn't a template someone thought up in an afternoon. It's a system that evolved through months of daily use by someone with ADHD, built specifically around how executive dysfunction actually works — not how productivity gurus think it should work.

I'm publishing it for free because I think it can help people, and I want to see what others do with it.

## How to install

1. Download `SKILL.md` and the `references/` folder from this repo
2. In Claude, go to your skill settings and add a new skill
3. Point it at the `SKILL.md` file (include the `references/` folder in the same directory)
4. Start a conversation with Claude and say something like "I want to set up a planning system" or "help me get organized"

Claude will run an onboarding interview — a ~15 minute conversation about how your brain works, what tools you use, where things fall apart, and what you're trying to get done. Then it generates a personalized executive function skill tailored to you.

## How it works

**The starter skill** (this repo's `SKILL.md`) runs an onboarding interview with five sections: what breaks down for you, your current tools, your daily rhythm, why past systems failed, and your current goals and projects.

**Based on that interview**, it generates a personalized skill — a single `SKILL.md` file that contains your principles, list definitions, session flows, and notes for Claude. This becomes *your* system. It uses your language, references your actual projects, and is weighted (light/standard/heavy) based on your history with productivity systems.

**The self-modification loop** is the secret weapon. As you use the system, you'll discover things that don't quite work. Instead of starting over, changes accumulate in a `SKILL UPDATE` entry in your context list and get batched into skill file updates over time. The system gets better through use.

## What makes this different

Most productivity systems are designed around "aspirational you" — the version of you that wakes up at 6 AM, journals, and time-blocks their calendar. This system is designed around "actual you" — the version that sometimes can't start tasks they know they need to do, loses track of what they were working on, and has a graveyard of abandoned productivity apps.

Key differences:

- **Claude does the planning heavy-lifting.** You don't maintain the system — Claude reads your lists, builds your plan, handles triage, and tells you what to do next. You just execute.
- **Night-before planning.** Your to-do list is already on your phone when you wake up. No morning decision fatigue.
- **Energy-aware.** The system passively reads your energy from conversation tone and adjusts — lighter load on bad days, more ambitious on good ones. No mood rating scales.
- **One next action.** When you finish something, the system tells you what to do next. No staring at a list of 15 tasks trying to pick one.
- **It adapts.** The self-modification loop means the system evolves with you instead of becoming another thing you abandon.

## The example

Check out [`examples/mature-system/`](examples/mature-system/) to see what a personalized skill looks like after months of daily use. It's been sanitized (no personal details), but it shows the full shape of a lived-in system — the list definitions, session flows, context format, and all the little refinements that came from actual use.

## Community

This is meant to be forked, tweaked, and shared. Some ideas:

- **Share your setup** — Once you've been using your generated skill for a while, share what worked and what you changed in [Discussions](../../discussions).
- **Contribute improvements** — If you find a pattern that makes the system better, open a PR or share it in Discussions.
- **Build on it** — The generation template is designed to be extended. Add new session types, new list definitions, new principles. Make it yours.

## Attribution

Created by [Ian Hunter](https://github.com/DoxxedDoxie).

Licensed under [CC BY 4.0](LICENSE) — do whatever you want with it, just say where it came from.
