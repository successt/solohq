---
name: os-setup
description: The SoloHQ vault setup. Bootstrap the SoloHQ vault structure and run personalized onboarding. Creates all directories, system files, Obsidian config, memory system, hooks, and output styles, then interviews the user to personalize everything. This is the SoloHQ setup (run this), NOT Anthropic's generic setup-cowork. Use when user says "set up my vault", "bootstrap", "initialize SoloHQ", "onboarding", or runs /os-setup.
---

# SoloHQ, Setup + Onboarding

USE WHEN the user runs `/os-setup` (the SoloHQ setup command, NOT Anthropic's generic `setup-cowork`) or asks to set up their vault, bootstrap the assistant, initialize the system, or configure SoloHQ.

> [!note] One product shape: solo
> SoloHQ is a solo-professional product. There is no Business/Teams mode. Every vault is built for one operator. (Removed 2026-06-09: the Business/Teams variant shipped a team-shaped vault and dashboard that did not fit the solo product.)

This is a two-phase process:
- **Phase A**: Bootstrap, Create the directory structure and system files
- **Phase B**: Onboarding, Interview the user and personalize everything

## Pre-flight Check

Check if `claude.md` or `CLAUDE.md` exists **only** in the current working directory (do NOT search subdirectories or parent directories, check only the exact CWD path).

- **If it exists**: The vault is already set up. Ask the user:
  - "This vault is already set up. Would you like to:"
  - **Re-run the interview**, Keep existing structure, update memory files based on new answers
  - **Full reset**, Delete everything and start fresh (confirm twice before proceeding)
  - **Cancel**, Do nothing
- **If it does NOT exist**: Proceed with full setup (Phase A + Phase B)

---

## Phase A: Bootstrap

Create the directory structure and write all system files. The vault is always built in solo (professional) shape; `CLAUDE.md` frontmatter is written as `os-mode: professional`.

### Resolving reference file paths

Every `references/<file>.md` mentioned below lives in the `references/` subdirectory next to **this SKILL.md**, not in the user's working directory. Two conventions matter:

- **Read paths** (`references/foo.md`) → resolve relative to this SKILL.md's directory.
- **Write paths** (`./Foo/CLAUDE.md`) → resolve relative to the user's current working directory (the vault root).

If the Read tool can't open a `references/...` path directly (some harnesses mount the skill at a path that differs between Read and Bash), run a quick discovery step **once** before Step A.2:

```bash
# Find the references directory; cache the result for the rest of Phase A.
find / -type d -path '*/setup/references' 2>/dev/null | head -1
```

Use that absolute path as the prefix for every reference read in Phase A and Phase B. Don't retry path resolution per-file, do it once and reuse.

### Step A.1: Create Directory Structure

```bash
mkdir -p .claude
mkdir -p Context
mkdir -p Clients
mkdir -p Projects
mkdir -p Daily
mkdir -p Resources
mkdir -p Skills
mkdir -p Memory
mkdir -p Intelligence/meetings/team-standups
mkdir -p Intelligence/meetings/client-calls
mkdir -p Intelligence/meetings/one-on-ones
mkdir -p Intelligence/meetings/general
mkdir -p Intelligence/competitors
mkdir -p Intelligence/market
mkdir -p Intelligence/decisions
mkdir -p Intelligence/archive
```

### Step A.2: Write System Files from References

Read each reference file and write it to the corresponding local path. The reference files contain the complete content for each system file.

**Shared system files:**

| Reference File | Creates at Local Path |
|---|---|
| `references/settings-json-template.md` | `./.claude/settings.local.json` (local scope: `autoMemoryDirectory` is ignored from project `settings.json`) |
| `references/claudeignore-template.md` | `./.claudeignore` |
| `references/gitignore-template.md` | `./.gitignore` |
| `references/memory-readme-template.md` | `./Memory/README.md` |
| `references/memory-index-template.md` | `./Memory/MEMORY.md` |

**Root OS instruction file** (written under two names so non-Claude agents read it too, see Step A.2b):

| Reference File | Creates at Local Path |
|---|---|
| `references/claude-md-template.md` | `./CLAUDE.md` |

**Per-folder routing indexes** (every major folder gets its own `CLAUDE.md`, matches production vault convention):

| Reference File | Creates at Local Path |
|---|---|
| `references/claude-md-context.md` | `./Context/CLAUDE.md` |
| `references/claude-md-projects.md` | `./Projects/CLAUDE.md` |
| `references/claude-md-daily.md` | `./Daily/CLAUDE.md` |
| `references/claude-md-intelligence.md` | `./Intelligence/CLAUDE.md` |
| `references/claude-md-resources.md` | `./Resources/CLAUDE.md` |
| `references/claude-md-skills.md` | `./Skills/CLAUDE.md` |

For each row: read the reference file, then write its content to the local path.

### Step A.2b: Create the `agents.md` twin

The root OS instruction file must exist under both names: `CLAUDE.md` (read automatically by Claude) and `agents.md` (the cross-tool standard read by other AI agents). They are the same physical file, kept in sync via a symlink, so editing one updates both.

After writing `./CLAUDE.md`, create the twin:

```bash
ln -sf CLAUDE.md agents.md
```

If the filesystem does not support symlinks (rare on macOS, possible on some Windows setups), fall back to a copy and tell the user the two files must be kept in sync manually:

```bash
ln -sf CLAUDE.md agents.md 2>/dev/null || cp CLAUDE.md agents.md
```

Verify `./agents.md` resolves to the same content as `./CLAUDE.md` before continuing.

### Step A.3: Initialize Starter Context Files

Create placeholder skill folders:

```bash
mkdir -p Skills/linkedin-writer/references
mkdir -p Skills/newsletter-writer/references
```

Then write placeholder files from references:
- Read `references/skills-placeholder-linkedin-notes.md` → write to `./Skills/linkedin-writer/notes.md`
- Read `references/skills-placeholder-linkedin-example.md` → write to `./Skills/linkedin-writer/references/example-post.md`
- Read `references/skills-placeholder-newsletter-strategy.md` → write to `./Skills/newsletter-writer/strategy.md`
- Read `references/skills-placeholder-newsletter-example.md` → write to `./Skills/newsletter-writer/references/example-edition.md`

Then write the starter context file:
- Read `references/context-me.md` → write to `./Context/me.md`

### Step A.4: Make Hooks Executable

```bash
chmod +x .claude/hooks/*.sh
```

### Step A.5: Confirm Bootstrap

Tell the user:
- "Vault structure created successfully."
- List the main folders created, including `Skills/`
- Recommend opening this folder as a vault in Obsidian
- Recommend installing **TaskNotes** community plugin if they want task management features
- Note that **Bases** (native database views) are built into Obsidian, no plugin needed for queries
- Mention `Resources/` for storing prompts, frameworks, swipe files, and templates
- "Now let's personalize it for you."

Then proceed to Phase B.

---

## Phase B: Onboarding, Guided Brain Dump

This skill runs **inside Cowork**. Phase B uses Cowork's rich-HTML widget tool, **not** AskUserQuestion, to render a real form with stacked categories, free-text textareas, and proper styling (matches the look of `os-optimizer`'s "Audit run details" form).

It's a guided brain dump across **12 categories** of the user's life and business, batched into **3 rich-HTML forms** (4 categories per form). Bullet points inside each category are **inspiration prompts**, riff on whatever lands.

The pitch to the user: *sit down for an hour or two, pour a beer, order a pizza, and brain-dump. It's not only for the assistant to feel personal on day one, it's a useful exercise in itself.*

### The tool: `mcp__visualize__show_widget` (Cowork-only)

Each of the 3 forms is **one** call to `mcp__visualize__show_widget`. The tool accepts:

| Field | Purpose |
|---|---|
| `title` | Internal widget identifier (e.g. `os_setup_form_1_you_business`) |
| `loading_messages` | Array of short strings shown while the form renders |
| `widget_code` | Raw HTML for the form (uses Cowork's `elicit-*` class conventions) |

The user fills in the form and submits. The submitted values come back to the agent as the tool result. The agent then proceeds to the next form. No AskUserQuestion. No radio buttons. No "Other" box.

### How the user should respond, per category

Inside each category's textarea, the user can:

1. **Paste a Whisper / dictation transcript**, open phone or Mac dictation, ramble for 2–5 minutes, paste the transcript.
2. **Paste documents**, links to PDFs, Notion pages, Google Docs, brand guides, About pages, LinkedIn profiles, OKR docs, decks. Or drop file paths.
3. **Point at connectors**, paste a Notion workspace URL, a wiki link, a Drive folder.
4. **Type long-form free text.**

Two kinds of knowledge: **what lives in your head** (Whisper it) and **what already lives online or in a tool** (paste links / docs). Mix freely per category. Leave a textarea blank to skip that category.

### Before Form 1, Send one orienting message

Send this verbatim (or close to it), no tool call yet:

> Three short forms, four categories each, twelve categories total. This isn't a quiz, it's a guided brain dump.
>
> Each category has three ways to give me context: a **brain-dump textarea**, a **links & file paths field**, and a **file upload**. Use any or all. Brain-dump anything around the bullet inspirations, you don't have to hit each one. Leave a category blank to skip it.
>
> Best inputs: a Whisper / dictation transcript, an About page URL, a brand guide PDF, an OKR doc, a LinkedIn profile, a Notion page. The more you give me, the less generic your vault will be on day one.
>
> Sit down for an hour or two. Pour a beer. Order a pizza. This is worth it.
>
> Submit each form when ready. Type "skip all" anytime to jump to defaults.

### Widget HTML template (every category in every form uses this shape)

Each category gets **three inputs**: a brain-dump textarea, a links/paths textarea, and a file upload input. Any or all can be filled. All blank = skip.

Inside `widget_code` for each form, build a `<form class="elicit">` containing one header and four `elicit-group` blocks. Per category:

```html
<div class="elicit-group">
  <label class="elicit-question">{N}/12, {Category name}</label>
  <div class="elicit-bullets" style="font-size:13px; color:var(--color-text-secondary); margin:8px 0">
    <ul style="margin:0; padding-left:18px">
      <li>{inspiration bullet 1}</li>
      <li>{inspiration bullet 2}</li>
      <li>{inspiration bullet 3}</li>
      <!-- etc -->
    </ul>
    <p style="margin-top:6px; font-style:italic">Brain-dump in the textarea below, OR paste links / file paths, OR upload docs. Any combination. Leave all blank to skip this category.</p>
  </div>

  <textarea class="elicit-textarea" name="cat{N}_braindump" rows="6"
    style="width:100%; border-radius:10px; padding:10px; border:1px solid var(--color-border-subtle); font-family:inherit; font-size:13px; margin-bottom:8px"
    placeholder="Brain dump, paste a Whisper transcript, or type long-form…"></textarea>

  <textarea class="elicit-textarea" name="cat{N}_links" rows="2"
    style="width:100%; border-radius:10px; padding:10px; border:1px solid var(--color-border-subtle); font-family:inherit; font-size:13px; margin-bottom:8px"
    placeholder="Links & file paths, one per line (Notion URL, LinkedIn profile, /path/to/file.pdf, etc.)"></textarea>

  <input class="elicit-file" type="file" name="cat{N}_files" multiple
    accept=".md,.txt,.pdf,.docx,.pptx,.xlsx,.csv,.json,.yaml,.yml,.png,.jpg,.jpeg"
    style="font-size:12px; color:var(--color-text-secondary)">
</div>
```

And one header at the top of `<form class="elicit">`:

```html
<div class="elicit-header">
  <svg viewBox="0 0 20 20" fill="currentColor" width="20" height="20"><!-- pencil/clipboard icon --></svg>
  <span>{Form title}</span>
</div>
<div class="elicit-body">
  <!-- 4 elicit-group blocks -->
</div>
```

Reuse the SVG icon pattern from the `os-optimizer` `Audit run details` widget (clipboard-with-marks icon). Form titles: "You & business", "Customer & brand", "How you operate".

### Reading form submissions

When the widget returns, the result is a record mapping each input's `name` to its value:

- `cat{N}_braindump` → string (the typed text / transcript)
- `cat{N}_links` → string (newline-separated URLs and file paths)
- `cat{N}_files` → array of file references (Cowork uploads these into the workspace folder; the result gives you the paths or signed URLs)

A category is "skipped" only when all three inputs are empty/blank.

### Ingestion between forms

After each form returns, for each category (N = 1..4 in this form):

1. **`cat{N}_braindump`**, if non-empty, tag and store raw in the working corpus under the category. Don't paraphrase.
2. **`cat{N}_links`**, split on newlines. For each line:
   - HTTP(S) URL → fetch with WebFetch / WebSearch.
   - Local file path → Read it.
   - Folder path → Glob, then Read each file.
3. **`cat{N}_files`**, for each uploaded file:
   - `.md`, `.txt`, `.json`, `.yaml`, `.csv` → Read directly
   - `.pdf` → Read with `pages` param if large
   - `.docx`/`.pptx`/`.xlsx` → use `pandoc` / `textutil` via Bash if available; otherwise note and continue
   - Images → Read (multimodal)

Merge everything into the corpus tagged by category. Then immediately fire the next form. No commentary or summarization between forms.

Bullet inspiration prompts are Oskar's prompt blocks verbatim, plus Ben's framing of "brain-dump anything around any of these bullets."

---

### The 3 forms × 4 categories

**Form 1, You & business**, one `mcp__visualize__show_widget` call. Title: `os_setup_form_1_you_business`. Contains Q1–Q4 as stacked `elicit-group` blocks.

**Q1. You.** Form header: `You`
Bullets:
- Name, role/title, location, industry
- When and how you do your best work (mornings? deep blocks? after a walk?)
- If someone you respected had to introduce you in a room of people you respected, how would you want them to describe you?
- 5 attributes that describe you (one or two words each)

**Q2. Your origin and POV.** Header: `POV`
Bullets:
- Why you started or joined what you're doing now
- A belief or POV you hold strongly, even when it's unpopular
- The "big idea" your work is built on (the wedge, the thesis)
- Who or what you're fighting against, a category, a behavior, a competitor archetype, a status quo

**Q3. What you sell.** Header: `Lines`
Bullets (one paragraph per revenue line, or skip if none yet):
- Name, what it does, who it's for, stage
- Current revenue baseline if applicable
- How it came to exist. What made you start it.

**Q4. The promise.** Header: `Offer`
Bullets:
- The 1–3 problems you solve for customers
- For each problem: are customers already aware they have it, or do you have to teach them?
- Your value proposition in one sentence
- The promise or guarantee you make (explicit or implicit)
- Why customers actually pick you, in their words if you've heard them say it

**Form 2, Customer & brand**, one `mcp__visualize__show_widget` call. Title: `os_setup_form_2_customer_brand`. Contains Q5–Q8 as stacked `elicit-group` blocks.

**Q5. The customer.** Header: `Customer`
Bullets:
- Title, role, industry, responsibilities
- What their day looks like, what tools they live in
- The language and words *they* use to describe their problem
- The dream outcome they want
- The situation they're in *before* they come to you, what triggered the search
- How long they typically take to decide to buy
- The media, podcasts, newsletters, or creators they follow
- 3–5 real examples (names, LinkedIn profiles, or company names)

**Q6. Your voice and look.** Header: `Voice`
Bullets:
- Tone descriptors that fit (direct, warm, dry, technical, playful, serious, supportive…)
- 5 attributes that describe how you sound
- Signature phrases you actually use
- Words or phrases you'd never use
- Topics you love talking about
- Topics you refuse to discuss publicly
- Brand colors, fonts, taglines if you have them
- The feeling people should carry away after reading your stuff
- Or: paste a writing sample / link and I'll extract from it

**Q7. Your positioning.** Header: `Position`
Bullets:
- The enemy you're fighting (the category, behavior, or competitor archetype)
- How you solve the problem *differently* from the obvious alternatives
- 3–4 distinct messages you want associated with your name or brand

**Q8. This year's priorities.** Header: `Priorities`
Bullets:
- 1–3 outcomes with a number attached (revenue, audience size, ship date)
- The *why* behind each
- What you're explicitly saying no to in order to focus here

**Form 3, How you operate**, one `mcp__visualize__show_widget` call. Title: `os_setup_form_3_how_you_operate`. Contains Q9–Q12 as stacked `elicit-group` blocks.

**Q9. Active projects.** Header: `Projects`
Bullets (for each project):
- Name, one-line purpose, status, deadline if any
- Which business it belongs to (if multiple)
- Who else is involved

**Q10. The people you work with.** Header: `People`
Bullets:
- Team, contractors, key external contacts
- For each: name, role, how you work together
- Skip if fully solo

**Q11. Your stack.** Header: `Stack`
Bullets:
- Stack across communication, meetings, CRM, content, finance, dev, automation
- Source of truth for each main workflow, where deals live, where decisions live, where writing actually happens, where the calendar lives

**Q12. Drains and workflows to automate.** Header: `Drains`
Bullets:
- Top 1–2 painful, repetitive workflows. Use this template:
  When **X** happens → I do **Y** → it takes **Z** time → output is **W** → what I want is **V**
- What's draining your attention right now, unclosed loops, decisions sitting unmade, things that should be done but aren't

---

The user submits each form with one click. Per-category response patterns:
- Type / paste a brain dump, transcript, links, docs, or file paths into the textarea
- Leave the textarea blank to skip that category
- Reply "skip all" between forms, stop asking and move to Phase B+

**Accept whatever they give.** Don't ask follow-ups inside or between forms. Extract what you can.

**If the user submits every form empty**, proceed to build with defaults only.

---

## Phase B+: Additional Context Drop

After Q12 (or "skip all") and **before** Phase B Build, ask one final `AskUserQuestion` to invite any leftover source material that didn't surface during the 12 categories. Most users still have brand decks, About pages, intake forms, LinkedIn URLs, Notion docs, PDFs, slide exports, voice/style guides, OKR docs, project briefs, etc. Always ask, even if Q1–Q12 looked rich.

**Call AskUserQuestion** (one question, header: `Context`):
- Question: "Anything else I should pull from before building? Upload files (PDFs, MDs, DOCXs), paste links (LinkedIn, websites, Notion pages, Google Docs), point me at a local folder, or paste raw text. The more I have, the more personalized your vault will be, instead of template scaffolds with placeholders."
- Options:
  - `Yes, I'll paste links / upload files`, "Walk me through it"
  - `Yes, point me at a folder on disk`, "I have local files"
  - `No, use just the answers above`, "Build with what we have"
  - `Skip`, "Skip this step"

**If the user picks a "Yes" option** (or pastes content directly):

1. Collect everything they share. Be greedy, accept anything they offer.
2. **For each link**: call `WebFetch` (or `WebSearch` if the URL is a search). Extract the relevant content.
3. **For each uploaded file or local file path**:
   - `.md`, `.txt`, `.json`, `.yaml`, `.csv` → read directly with `Read`
   - `.pdf` → read with `Read` (use `pages` parameter if >10 pages)
   - `.docx`, `.pptx`, `.xlsx` → use Bash with `pandoc` or `textutil` if available; otherwise tell the user to export as PDF or MD and re-share
   - Images / screenshots → read with `Read` (multimodal)
4. **For a local folder path**: use `Glob` to enumerate, then read each file.
5. **Maintain a context corpus** in working memory, every fact, name, number, quote you find. Tag each by likely target (`me.md`, `brand.md`, `icp.md`, `strategy.md`, `projects/{name}`, etc.).
6. After ingestion, briefly tell the user what you pulled (e.g., "Pulled 4 files: brand-guidelines.pdf, about-page.md, okrs-2026.md, team-roster.csv. 18 links fetched."). One sentence. Then proceed to Build.

**If the user picks `No` or `Skip`**: proceed straight to Build with only the Q1–Q12 answers from Phase B.

---

## Phase B Build: Personalize the Vault

After Q12 + the additional-context drop (or skips), build everything you can from what the user gave you. Work silently, don't narrate each step.

### CRITICAL: real personalization, not template scaffolds

The reference files in `references/` are **scaffolds**, they show the section structure to use. They are **not** the output. Do not copy a template verbatim with placeholders intact.

For every file you write:

1. **Read the reference template** to learn the section structure (headings, frontmatter shape, section order).
2. **Replace every placeholder** (anything in `[brackets]` or marked as TBD) with real data extracted from the 12 Phase B answers + the Phase B+ corpus.
3. **If a section has zero supporting data** after exhausting both Q answers and the corpus: **omit the entire section** rather than writing `[name]` or `TBD`. The output should never contain bracketed placeholders.
4. **If only some bullets in a section have data**: keep the section, drop the empty bullets.
5. **Use the user's actual words, names, numbers, URLs, and quotes** wherever the corpus contains them. Don't paraphrase facts, preserve specificity (exact company names, exact dollar figures, exact dates, exact phrases the user uses).
6. **Cross-reference**: a single fact may belong in multiple files (e.g., "we sell to RevOps leaders at Series B SaaS" belongs in both `icp.md` and `brand.md` positioning). Place it in each file where it's relevant.
7. **Frontmatter `updated:`** = today's date.

A finished context file should read as a real human-written document about the user. If it reads like a fillable form, you did it wrong, go back and fill it.

### Build Step 1: Create Context Files

For every file below, source data from BOTH the Q answers AND the Phase B+ corpus (uploaded files, fetched links, folder reads). The corpus typically contains the depth, Q answers are anchors. (Q1–Q12 = the solo brain-dump categories.)

- **`Context/me.md`**, Always created. Fill from Q1 (name, role, location, peer-intro line, attributes, working style) + Q2 (origin / POV / wedge / enemy) + Q12 (drains, unclosed loops) + corpus. Read `references/context-me.md` as scaffold.
- **`Context/business.md`**, Only if Q3 had content. Fill from Q3 (revenue lines: name, what it does, who it's for, stage, baseline, origin) + corpus (About page, business overview docs). Read `references/context-business.md` as scaffold.
- **`Context/services.md`**, Only if Q3 lists multiple revenue lines or corpus has product/service docs. Read `references/context-services.md` as scaffold.
- **`Context/pain-points.md`**, Only if Q4 named problems or Q2 surfaced one. Include awareness column (aware vs needs education) using Q4's awareness signal. Read `references/context-pain-points.md` as scaffold.
- **`Context/icp.md`**, Only if Q5 had content or corpus has ICP material. Fill role, day, language, dream outcome, trigger, decision time, media, examples. Read `references/context-icp.md` as scaffold.
- **`Context/brand.md`**, Only if Q6 (voice), Q7 (positioning), or Q4 (why-pick-you) had content, or corpus has brand material. From Q4 take value prop + why-pick-you. From Q6 take voice descriptors, signature phrases, words-to-avoid, feeling, colors/fonts. From Q7 take enemy, differentiation, key messages. Read `references/context-brand.md` as scaffold.
- **`Context/strategy.md`**, Only if Q8 had content. Fill priorities, why, and explicit nos. Read `references/context-strategy.md` as scaffold.
- **`Context/team.md`**, Only if Q10 had content (people / collaborators) or corpus has a team / contractor list. Read `references/context-team.md` as scaffold.
- **`Context/infrastructure.md`**, Only if Q11 (stack) or Q12 (workflows) had content, or corpus has a stack doc. Combine tool stack (Q11) + workflows-to-automate (Q12). Read `references/context-infrastructure.md` as scaffold.

### Build Step 2: Create Project Folders

From Q9 (active projects), plus any project briefs / Notion exports / project lists in the corpus. Intelligently structure each project based on what the user gave you.

**Analyze the info and decide the right structure:**
- Simple mention ("working on a podcast") → just a `README.md`
- Moderate detail (scope, deadlines, people) → `README.md` + relevant subdirs
- Rich info (briefs, specs, research, multiple workstreams) → full structure with subdirs and files

**Create subdirectories only when the content justifies them:**

| Content type | Goes to |
|---|---|
| Overview, status, deadlines, contacts | `README.md` |
| Research, competitor analysis, references | `research/{topic}.md` |
| Specs, requirements, briefs | `specs/{name}.md` or `briefs/{name}.md` |
| Drafts, scripts, written content | `drafts/{name}.md` |
| Ideas, brainstorms | `ideas/{name}.md` |
| Notes, working docs | `notes/{name}.md` |

**README.md is always the index:**
```markdown
---
type: project
status: active
owner: [name]
created: YYYY-MM-DD
updated: YYYY-MM-DD
---
## Overview
[What this project is]

## Current Status
[Where things stand]

## Key Resources
[Links, tools, contacts]

## Next Steps
[What needs to happen]
```

Don't create empty subdirs. Don't cram everything into the README. Distribute content into the right files based on what it actually is.

### Build Step 3: Create First Daily Note

Create `Daily/YYYY-MM-DD.md` (today's date):
```markdown
---
type: daily-note
date: YYYY-MM-DD
---
# YYYY-MM-DD

## Session
- **Focus**: Initial vault setup and onboarding
- **Completed**: Full vault bootstrap + personalized onboarding
- **Next Steps**: [based on what was discussed]
```

### Build Step 4: Confirm Completion

Tell the user:
- Quick summary of what was created (which context files, how many projects)
- "Open this folder in Obsidian to see your vault"
- "You can add more context anytime, just tell me and I'll update the right files."
- Suggest a next action based on what they told you
- **Invite the AI Identity Blueprint (the big one).** End by recommending they run it next. Say something like: "Your vault is ready. The one thing that makes this feel like magic is the AI Identity Blueprint, a short interview that teaches me your actual voice so I write like you, not like generic AI. Want to do that now? Just say build my blueprint." Keep `/setup` itself lean: do NOT run the full Blueprint inline. It is its own dedicated experience (the `ai-identity-blueprint` skill). This is the free first taste that makes the system click.

## Guidelines

- Phase A is fully automated, no user input needed
- Phase B is **12 categories** (Oskar's structure), batched into **3 rich-HTML forms** rendered via `mcp__visualize__show_widget` (Cowork-only). Each form has 4 stacked categories with title, bullet inspiration, and a single free-text textarea per category. It's a guided **brain dump**, not a Q&A box. The bullets are inspiration, not strict asks. Always recommend Whisper / dictation + pasting docs / links / file paths into the textarea
- No follow-ups, no drilling deeper between forms
- Phase B+ is one final AskUserQuestion (or visualize widget) inviting any leftover files / links / folders, always ask, even if Forms 1–3 looked rich
- Accept any format: typed brain dumps, Whisper transcripts, pasted docs, uploaded files, links (LinkedIn, websites, blog posts, Notion, Drive), local folder paths, or skips
- For every link the user pastes, fetch it (`WebFetch` / `WebSearch`); for every file or folder, read it (`Read` / `Glob`); merge into a single context corpus before building
- **Templates are scaffolds, not outputs.** Replace every `[bracketed placeholder]` with real user data. If a section has no data after exhausting Q answers + corpus, omit the section, never leave placeholders in the written file
- Preserve specificity: use the user's exact names, numbers, URLs, and phrasing
- Only create context files that have real content, don't create empty placeholder files
- Don't narrate every file you're creating, just build it and summarize at the end
