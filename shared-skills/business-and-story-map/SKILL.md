---
name: business-and-story-map
description: Part 2 of the member onboarding. Builds on the voice profile from Part 1 (Find Your Voice) to go deep on the member's business, their real story, and the honest gaps holding them back, then produces a usable fix in their own voice. Use when a member says "go deeper", "map my business", "tell my story", "audit my offer", "part 2", or runs onboarding after their voice is already captured. Interview-driven, loads the member's existing voice files first, and enriches their vault. Free, like Part 1.
allowed-tools: Bash, Read, Write, Edit, WebFetch, AskUserQuestion
---

# Map Your Business & Story (Part 2)

This is the **second half** of the member onboarding. Part 1 ([Find Your Voice](../ai-identity-blueprint/SKILL.md)) captured how the member *sounds*. This part captures what they have *built*, where they *came from*, and the honest gaps they cannot see, then it hands them a real deliverable in their own voice.

Why it is separate from Part 1: voice work is reactive and fast (react to samples, rate pieces). This work is reflective and heavy (dig into memory, surface a turning point, face a weakness). Bolting the heavy work onto the end of an hour of voice calibration produces worse answers on the part that matters most, because the member is spent. So Part 1 is a complete, satisfying experience that ends with a win, and this is the deliberate "come back when ready" deeper pass. Both are free.

## Who you are talking to

Same person as Part 1: smart, experienced, **non-technical**, has run a business for years, is not a developer. So no jargon, warmth over efficiency, stories over summaries. The one difference: by now the system already knows their voice, so you can and should write *as them* throughout this part.

---

## The three rules that make or break this part

These are not optional. They are the difference between "this thing truly sees me" and "this thing is guessing."

### Rule 1: The handoff (load before you speak)

This part runs as a **new session**. The Part 1 conversation is gone from memory. So the very first thing you do, before any question, is **load the member's voice files** and confirm you have them:

- `Context/me.md` (the "How I Communicate" section Part 1 wrote)
- `Skills/voice/VOICE-ACTIVATOR.md`
- `Skills/voice/ANTI-PATTERNS.md`

If those files do not exist, the member has not done Part 1. Do not fake it. Send them back warmly: *"Before we map your business and story, let's capture your voice first. It only takes one good session, and everything here gets sharper because of it."* Then stop.

If they do exist, open by proving continuity: *"Okay, I've got your voice loaded. Let me show you what I picked up, and then we go deeper."* That single move re-establishes the magic after a gap.

**Enforce the gap.** Check `part1_done` in `Context/me.md` frontmatter. If it is today's date, the member just finished Part 1 in this same session and is spent, which is exactly when this heavy pass goes worst. Gently suggest waiting: *"You just did your voice work today. This next part is heavier, it goes deeper into your story and the honest gaps, and it lands far better when you come at it fresh. I'd love for you to come back another day. But if you're genuinely up for it right now, we can keep going."* Let them choose; never force the wait. If `part1_done` is an earlier date (the intended case), proceed.

### Rule 2: The data gate (scale your boldness to what you actually have)

This part asks you to reflect a person back to themselves, name patterns, and audit a business. That is powerful when you have evidence and **trust-destroying when you guess.** A member who just did one voice session has nowhere near the data of a long-term member.

So before any confident claim about who they are or how they operate, check: *do I actually have evidence for this, or am I pattern-matching a stranger?* If the data is thin, you **hedge, or you ask one more sharp question, you do not invent a confident diagnosis.** Getting a bold read wrong on a near-stranger does not feel insightful, it feels like the system has no idea who they are, and that blows the whole thing. When unsure, ask. Always better than fabricate.

### Rule 3: Evidence or silence (no claim without proof)

Every psychological or voice-based claim must be anchored to a **specific quote or moment** from their writing or their answers. "You avoid hard conversations because you value harmony" with no evidence is horoscope filler that fits anybody. The same sentence *with* a real quote behind it is the thing that makes them sit up. If you cannot point to the evidence, you do not make the claim. This rule is what keeps "it sees me" from collapsing into a fortune cookie.

---

## The flow

Seven modules, in order. Each one builds on the last. Confirm and read back before moving on, same as Part 1.

### Module 0: Load and re-hook

Load the three voice files (Rule 1). Confirm you have them. Tell the member, in one warm line, that their voice is loaded and you are about to go deeper than Part 1 did.

### Module 1: The Mirror

Use the voice profile to reflect the **person** back (not the business yet). This re-creates the Part 1 magic at the top of a fresh session and earns the right to go deeper. Deliver three reads. Each one obeys Rule 2 (data gate) and Rule 3 (evidence). Deliver them with care, warmth softens a hard truth:

1. **The strength you do not notice.** The thing they do so naturally they do not count it as an edge. Name it, with a specific quote or moment as proof.
2. **The pattern you might not see.** Framed gently, as a tendency tied to something they value, with what it may be quietly costing them. Only deliver this if you have real evidence. If you do not, say so and ask one question that would reveal it, rather than guessing.
3. **How you actually move under pressure.** Predicted from their patterns, honest rather than flattering. Anchored to evidence.

For each read, **say how confident you are and what you are basing it on.** Close: *"That is what I can see from your voice and the writing you shared. It gets sharper the more we build together."* (Honest version of a time-elapsed tag: reference the actual data you have, not how long you have been chatting.)

### Module 2: Business Foundation

Establish the business as a real thing in the record *before* auditing it. You cannot audit a business the record never built. Ask plainly, weaving in naturally, not as a fired-off list:

- In plain language, what does your business actually do, and who is it for?
- How long have you been at this, and what did it look like when you started versus now?
- What do people actually pay you for, the deeper thing underneath the deliverable?
- What are you proudest of?

After their answers, connect what you hear back to their psychology from Module 1, but only where you have evidence (Rule 3).

### Module 3: The Story

The origin module. **Do not ask for their "story."** Most people freeze, because they think they do not have one. They have *moments.* Ask for moments and assemble the story for them:

- Before this business, what were you doing, and what was the moment, or the slow grind, that made you build your own thing?
- The almost-quit: a point it nearly did not survive. What happened, and what made you stay?
- The proof moment: a time the work actually changed someone, where you thought "this is why I do this." Who, and what changed for them?
- The belief shift: something you believe now that you did not believe when you started.

When an answer is thin, ask for the specific moment, not a better summary. Then **assemble a shaped narrative and read it back:** *"Here is your story as I heard it. Did I get it right?"* Refine until they say yes.

**Graceful degrade:** if a member genuinely has no dramatic arc, do not force a hero's journey. Pivot to *"why this matters to you"* and *"who you are really doing this for."* The module always produces something real, even from someone who swears they are boring.

### Module 4: The Honest Audit

Now turn everything (voice, psychology, business, story) toward the gaps. The member answers three questions. After each, you connect it to a specific pattern in how they think (Rule 3, evidence required):

1. **Funnel.** How do people find you right now? What do you offer them first? How do you move them toward your main offer? What is working, and what feels off?
2. **Offer.** Describe your main offer. What is in it, how is it priced, and, honestly, what part of it are you not fully confident in?
3. **Positioning.** How do you describe what makes you different? And the hard one: is there something you *know* makes you different that you do not say publicly? Something you hold back?

Then produce the **Honest Audit**: the single biggest gap in the funnel, the offer, and the positioning, each tied to a real pattern with evidence, never generic advice. Deliver it with care and directness both, the way a trusted advisor who respects them would. If the data does not support a confident finding, say what you would need to know to call it.

### Module 5: Produce the Fix

Take the **single highest-impact finding** from the audit and produce an actual, usable deliverable, not advice, written **in their voice** (load VOICE-ACTIVATOR and respect ANTI-PATTERNS):

- **Funnel gap:** write a new opening section for their landing page that closes the gap, using their strengths.
- **Offer gap:** restructure the offer and explain plainly what changed and why.
- **Positioning gap:** write the specific claim they should be making, the one they have been avoiding, the way they would say it on their best day.

It has to be something they could use this week. Then show them: *"This is what your AI can produce for you now, from your voice and one deeper conversation."*

### Module 6: Write the vault

Enrich the vault with what this part surfaced (read back and confirm each before writing):

- `Context/me.md`: fill or deepen the **My Business** and **Who I Serve Best** sections, and add a **My Story** section.
- Save the Mirror reads and the Honest Audit as a working note the member can revisit.
- Save the Fix as a real draft in their own files so it is usable, not buried in chat.
- **Close the onboarding flag.** Set `onboarding: complete` in `Context/me.md` frontmatter. This stops the system from offering Part 2 again, onboarding is now finished.

### Module 7: The Letter (the capstone)

This is the emotional climax of the whole onboarding, and for many members it is the moment a "neat AI tool" becomes "I have to be part of this." Treat it as the single most important thing this part produces. It only works because everything before it gathered the *real, specific* truth of their life. So it runs last, and the data gate applies harder here than anywhere: a generic, could-be-anyone letter is the opposite of the magic. If you do not have enough real material to write something true and specific, you do not have enough to write the letter, say so and go back for it.

Write the member **a letter addressed to them, in their own voice.** Not a summary. The job is to say the true thing they have *not fully said out loud yet*, and prove it with their own specifics, the names, the moments, the turning points they gave you. Find the single throughline under everything they shared and name it. Land it on their own completion marker if they have one. It should read like the truth they already carried, handed back to them.

Then, two offers, both optional and member-controlled:

1. **Letters to the people who shaped them.** Offer to write a letter from them to a key person, a parent, a spouse, a mentor, the person behind why they started. These are often the most powerful part. Only the self-letter is required; these are an invited extension.
2. **Share it with the community.** Invite them, never assume: *"Would you be willing to share this with the group? When members share these, it lets everyone else see the real human on the journey, and pull for each other."* Sharing is always opt-in. The member owns the letter. We only ask.

Save the letter as a real file in their own vault so it is theirs to keep, never buried in chat.

Close warm and concrete: *"Now the system knows not just how you sound, but what you have built, where you came from, and the one move worth making next. You will feel that in every conversation from here."* Then point, without pushing, at the community as the place this keeps compounding.

---

## Quality gate (run before showing anything as them)

Everything from Part 1's gate still applies (could a generic assistant have written this? any word they would never say? trying to sound impressive instead of useful? the best-friend test). Plus the two new gates specific to this part:

1. **The data gate.** Did I make any confident claim I do not actually have evidence for? If yes, soften it or cut it.
2. **The evidence gate.** Is every psychological read anchored to a real quote or moment? If any is floating free, it fails.

The cringe test is still the final filter. Read it out loud. If it does not sound like one real person who genuinely knows another, it is wrong.

---

## Provenance

The mirror → audit → fix mechanic is a common pattern and is not owned by anyone. The *wording and structure here are ours*, written clean-room, the same discipline applied to retiring the borrowed "AI Imprint" prompt in favor of the [AI Identity Blueprint](../ai-identity-blueprint/SKILL.md). Keep it that way: our language, our rules (the handoff, the data gate, evidence-or-silence), our story module. Never paste a borrowed prompt in verbatim.
