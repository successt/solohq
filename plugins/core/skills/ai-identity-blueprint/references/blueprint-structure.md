# Blueprint output structure

The exact shape of the three files the [AI Identity Blueprint](../SKILL.md) writes. Keep the member's own words wherever possible. Write in their voice, not a corporate template voice. No em dashes anywhere.

---

## File 1, `Context/me.md` (the Blueprint)

This is the file the assistant loads at the start of every session. Frontmatter, then four sections. Open with a one-line note that this is the real version of them and the AI should never ask them to re-explain it.

```yaml
---
type: identity
status: active
tags: [me, identity, blueprint, voice]
date: <YYYY-MM-DD>
onboarding: part2-pending
part1_done: <YYYY-MM-DD>
---
```

The `onboarding` and `part1_done` fields are the onboarding-state flag. Part 1 sets `onboarding: part2-pending` and `part1_done` to today's date. Part 2 sets `onboarding: complete` when it finishes. The member `CLAUDE.md` startup rule reads these to know whether (and when) to gently offer Part 2. Do not remove them.

### Section 1, How I Communicate
The voice portrait, built from Phase 3 plus the gap interview. Cover, in prose (not bullets where a paragraph belongs):
- **My natural rhythm**, how I actually build a sentence and a paragraph.
- **My go-to phrases**, the words and tics that are unmistakably me, and the words I avoid.
- **How I say hard things**, my way of delivering bad news or pushback.
- **How I treat people**, the relationship stance underneath my words.
- **My energy and my humor**, what my tone feels like, where humor shows up.
- **What I would never say**, the line I do not cross.
- **Where I contradict myself**, the honest tensions (the systems person who leads with story, etc.). These make it real.
- **Me on my best day**, a short, vivid picture of me at my sharpest.

### Section 2, My Business
Pulled from their real business docs plus the interview:
- **What we actually do**, in plain language, not a tagline.
- **The turning point**, the real story that set the direction.
- **What people pay for**, the deeper thing under the deliverable.
- **What I am proudest of.**
- **What makes us different.**
- **What I will not compromise.**
- **How clients go deeper**, the relationship model that works.
- **Where this is headed.**

### Section 3, Who I Serve Best
Built from their real client language (testimonials, intake forms, case studies) plus the interview:
- **Who they are**, beyond demographics.
- **The problem they say out loud.**
- **The problem underneath.**
- **What they would say venting at midnight**, the exhausted, unpolished version. This is the single most valuable line in the file. If the first answer sounds too clean, push for the raw one.
- **What already failed them.**
- **The real transformation** they are actually buying.
- **Who is NOT my client.**
- **What almost stops them.**

### Section 4, How To Work With Me
A short, practical list the AI can act on: how to explain things back to them, what energy to match, what to never do, how to help them when they are sitting on a decision. End with whatever their personal completion or sign-off marker is.

---

## File 2, `Skills/voice/VOICE-ACTIVATOR.md`

A standalone profile any AI can load to write *as them*, in any tool. Include:
- **The voice in one sentence.**
- **Tonal dimensions rated to ten** (formality, warmth, directness, confidence, humor, vulnerability, energy), each with a word of evidence.
- **Top 15 signature phrases.**
- **Structural patterns**, how they open, close, and transition; paragraph vs list; length tendency.
- **Persuasion playbook**, how they actually move someone.
- **Micro-patterns**, sentence rhythm (name it precisely), punctuation personality, top emphasis tools, time orientation, gain-vs-pain pull.
- **Context rules**, how the voice shifts for customers vs team vs friends vs under pressure.
- **3 to 5 of the best calibrated sentences** from Phase 5, as live examples.
- **Operating rules:** match, do not improve (if they write fragments, write fragments); default to their dominant pattern when unsure; respect the anti-patterns; shift by context; pass the best-friend test every time.

## File 3, `Skills/voice/ANTI-PATTERNS.md`

What any AI must never do when writing as them. Include:
- **Universal AI tells**, the phrases every AI overuses that no real person writes ("I hope this finds you well," "Let's dive in," "In today's fast-paced world," and the like). Banned outright.
- **Their specific banned tones**, with evidence from their writing.
- **Their specific banned phrases.**
- **Structural patterns to avoid.**
- **Persuasion tactics they would never deploy** (manufactured urgency, income claims, guru gatekeeping, etc.).
- **Context boundaries**, what changes between marketing, client, team, and personal writing.
- **The core failure mode**, the specific way their voice goes wrong when it misses, plus the cringe test: if the output triggers "I would never say it like that," it failed.

---

## Why these three files

`me.md` makes the system *know* them (it auto-loads every session). The voice activator lets any AI *speak* as them. The anti-patterns keep it *real*. Together they are the difference between an assistant that sounds like a tool and one that sounds like them. That difference is the free taste that earns the rest of the system.
