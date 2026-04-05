# Writing Your Novel with Claude Code + Story Wiki

A complete guide for first-timers. This walks you through every step — from having your BookWeaverAI exports sitting in a ZIP file to writing your first scene or chapter with a fully built story wiki backing you up.

---

## Table of Contents

1. [The Big Picture](#the-big-picture)
2. [What You Need Before Starting](#what-you-need-before-starting)
3. [Choosing Your Workflow: Scenes vs. Chapters](#choosing-your-workflow-scenes-vs-chapters)
4. [Setting Up Your Project](#setting-up-your-project)
5. [Understanding the Story Wiki](#understanding-the-story-wiki)
6. [Building the Wiki](#building-the-wiki)
7. [Reviewing and Editing Wiki Pages](#reviewing-and-editing-wiki-pages)
8. [Your First Writing Session](#your-first-writing-session)
9. [How the Wiki Stays Current](#how-the-wiki-stays-current)
10. [The Continuity Tracker vs. the Wiki](#the-continuity-tracker-vs-the-wiki)
11. [Running Continuity Audits](#running-continuity-audits)
12. [Ending Your Writing Session](#ending-your-writing-session)
13. [Common Questions](#common-questions)
14. [Quick Reference](#quick-reference)

---

## The Big Picture

You've used BookWeaverAI to plot your novel. You went through all 5 steps — Novel Concept, Starter Template, Expanded Outline, Chapter Outline, and Scene Plan. Now you have a ZIP file full of markdown files that contain everything about your story: characters, world, plot structure, themes, chapter breakdowns, scene-by-scene plans.

Now it's time to actually write the book.

The problem is this: as your novel grows, it gets too big for Claude to hold in memory all at once. By chapter 10, you might have 80,000+ words of manuscript. Add in your outlines, rules files, and scene plans, and you've blown past what fits in a single conversation. Claude starts forgetting things. Characters change eye color. Plot threads get dropped. The voice drifts.

The CCWriting system solves this with **three layers of persistent memory**:

1. **Tracking files** — `CONTINUITY_TRACKER.md`, `MODIFICATION_LOG.md`, `MISC_STORY_NOTES.md` — these capture the *current state* of your story. Where is everyone right now? What do they know? What objects are where? These get overwritten with the latest state after every scene or chapter.

2. **The story wiki** — a directory of focused, cross-referenced pages about your characters, locations, plot threads, world mechanics, themes, and timeline. Unlike the tracking files, the wiki *accumulates* knowledge. A character's page grows richer with every scene they appear in. It's like a fan wiki for your novel, except Claude maintains it automatically.

3. **Specialized agents** — before Claude writes each scene or chapter, a separate agent reads the full manuscript (in its own context window) and produces a compressed "brief" that contains everything the writing session needs. That agent also reads relevant wiki pages so it can pull specific details instead of scanning your entire 20,000-word Starter Template.

The result: Claude writes with full knowledge of your story even when the manuscript is 200 pages long, because the knowledge has been compiled, organized, and kept current — not re-derived from scratch every time.

---

## What You Need Before Starting

### Software

- **Claude Code CLI** installed on your machine. This is the command-line version of Claude that can read and write files on your computer. Install it from [claude.com/claude-code](https://claude.com/claude-code).
- **A terminal** — Terminal.app (built into your Mac), iTerm2, or whatever you're comfortable with.
- **Git** — comes pre-installed on Mac. You'll use it to track every change to your manuscript. If you've never used git, don't worry — the system handles the git commands for you.

### Your BookWeaverAI Export

You need your planning files exported from BookWeaverAI:

1. Go to your project on [bookweaver.ai](https://bookweaver.ai)
2. Make sure all 5 steps are complete
3. Go to the **Export** page
4. Click **"Download All as ZIP"**

This gives you a file like `The_Crimson_Divide_BookWeaver_Export.zip` containing:

| File | What It Is |
|------|-----------|
| `FINAL_NOVEL_CONCEPT.md` | Your finalized novel concept (Step 1) |
| `STARTER_TEMPLATE.md` | Characters, world, plot structure (Step 2) |
| `EXPANDED_OUTLINE.md` | Full novel outline (Step 3) |
| `CHAPTER_OUTLINE.md` | Chapter-by-chapter breakdown (Step 4) |
| `SCENE_PLAN.md` | Scene-by-scene checklist (Step 5) |
| `WRITING_RULES.md` | Your prose style guide |
| `STORY_RULES.md` | Narrative constraints and conventions |
| `WORLD_RULES.md` | World mechanics and laws |

If you have additional world-building material (magic systems, technology specs, detailed histories), you can also create a `SUPPLEMENTARY_MATERIALS.md` file manually and add it to your project.

---

## Choosing Your Workflow: Scenes vs. Chapters

There are two writing systems. Pick the one that matches how you want to work:

### CCWriting (Scene-Based)

- You write **one scene at a time** (e.g., Scene 4.3 = Chapter 4, Scene 3)
- Scenes are saved individually in a `scenes/` directory
- When all scenes in a chapter are done, you assemble them into a chapter
- The chapter gets appended to the manuscript
- **Best for**: Writers who want granular control, detailed scene-level feedback, and the ability to rearrange scenes before committing to a chapter

### CCWritingChapters (Chapter-Based)

- You write **one chapter at a time**
- Chapters go directly into a `chapters/` directory and get appended to the manuscript
- No intermediate scene files
- **Best for**: Writers who prefer working at a higher level, faster throughput, or whose chapters are short enough to generate in one pass

Both systems use the same story wiki. The only difference is the unit of writing (scene vs. chapter) and the commands you use.

**This guide covers both.** Wherever the workflow differs, you'll see clearly labeled instructions for each.

---

## Setting Up Your Project

### Option A: Clone the Template (Recommended)

This is the fastest way. It gives you everything pre-configured.

Open your terminal and run one of the following depending on your chosen workflow:

**Scene-based (CCWriting):**
```bash
git clone https://github.com/dhwilliams/claudecodewriting.git YourNovelTitle
cd YourNovelTitle
```

**Chapter-based (CCWritingChapters):**
```bash
git clone https://github.com/dhwilliams/claudecodewritingchapters.git YourNovelTitle
cd YourNovelTitle
```

Replace `YourNovelTitle` with your actual book title (no spaces — use dashes or underscores, e.g., `The-Crimson-Divide`).

Now drop in your BookWeaverAI export:

```bash
# Unzip your export into the project directory
unzip ~/Downloads/Your_Project_BookWeaver_Export.zip -d .

# If you have supplementary materials, copy them in
# cp ~/path/to/SUPPLEMENTARY_MATERIALS.md .

# Commit everything
git add .
git commit -m "Writing project initialized with planning files"
```

That's it for setup. Skip to [Building the Wiki](#building-the-wiki).

### Option B: Set Up From Scratch

If you prefer to build it yourself:

```bash
# Create your project directory
mkdir YourNovelTitle && cd YourNovelTitle

# Unzip BookWeaverAI export
unzip ~/Downloads/Your_Project_BookWeaver_Export.zip -d .
```

Now create the directory structure. The commands differ slightly depending on your workflow:

**For scene-based (CCWriting):**

```bash
mkdir -p {manuscript,scenes,chapters,session-notes,continuity,wiki/{characters,locations,plot-threads,world}}

touch manuscript/CURRENT_MANUSCRIPT.md
touch MISC_STORY_NOTES.md
touch continuity/CONTINUITY_TRACKER.md
touch continuity/MODIFICATION_LOG.md
touch SCENE_COMPLETION_STATUS.md

git init
git add .
git commit -m "Writing project initialized"
```

**For chapter-based (CCWritingChapters):**

```bash
mkdir -p {manuscript,chapters,session-notes,continuity,wiki/{characters,locations,plot-threads,world}}

touch manuscript/CURRENT_MANUSCRIPT.md
touch MISC_STORY_NOTES.md
touch continuity/CONTINUITY_TRACKER.md
touch continuity/MODIFICATION_LOG.md
touch CHAPTER_COMPLETION_STATUS.md

git init
git add .
git commit -m "Writing project initialized"
```

You'll also need to copy the `CLAUDE.md` and `.claude/` directory from the template repository for the slash commands and agents to work.

### What Your Project Looks Like Now

After setup, your directory should look like this (scene-based shown — chapter-based is the same but without `scenes/`):

```
YourNovelTitle/
├── manuscript/
│   └── CURRENT_MANUSCRIPT.md          # Empty — your novel goes here
├── chapters/                          # Assembled chapter files
├── scenes/                            # Individual scene files (scene-based only)
├── wiki/                              # Empty — you'll build this next
│   ├── characters/
│   ├���─ locations/
│   ├── plot-threads/
│   └── world/
├── continuity/
│   ├── CONTINUITY_TRACKER.md          # Empty — grows as you write
│   └── MODIFICATION_LOG.md            # Empty — tracks deviations from plan
├── session-notes/                     # Session summaries
├── FINAL_NOVEL_CONCEPT.md             # From BookWeaverAI Step 1
├── STARTER_TEMPLATE.md                # From BookWeaverAI Step 2
├── EXPANDED_OUTLINE.md                # From BookWeaverAI Step 3
├── CHAPTER_OUTLINE.md                 # From BookWeaverAI Step 4
├── SCENE_PLAN.md                      # From BookWeaverAI Step 5
├── WRITING_RULES.md                   # Your prose style guide
├── STORY_RULES.md                     # Narrative constraints
├── WORLD_RULES.md                     # World mechanics
├── SUPPLEMENTARY_MATERIALS.md         # (Optional) Extra lore/world-building
├── MISC_STORY_NOTES.md                # Empty — grows as you write
├── SCENE_COMPLETION_STATUS.md         # (scene-based) or CHAPTER_COMPLETION_STATUS.md
├── CLAUDE.md                          # Writing philosophy and protocols for Claude
└── PROMPTS.md                         # Command reference
```

The planning files (everything from BookWeaverAI) are **read-only reference**. You never edit them directly during writing. The wiki, tracking files, and manuscript are the living documents that grow.

---

## Understanding the Story Wiki

### What It Is

The story wiki is a collection of focused markdown files in the `wiki/` directory. Each file covers one specific thing — a character, a location, a plot thread, a world system. Think of it like a Wikipedia for your novel.

### Why You Need It

Your BookWeaverAI exports are comprehensive but monolithic. The Starter Template alone might be 15,000+ words covering every character, every location, every plot thread, and every theme — all in one file. The Expanded Outline might be 20,000+ words.

When Claude needs to check something specific — "What color is Elena's hair?" or "What is the political structure of the northern territories?" — it has to scan these entire documents. That wastes context window space and sometimes Claude misses things buried deep in a long document.

The wiki solves this by **compiling knowledge once and keeping it organized**:

- Instead of scanning 15,000 words of Starter Template to find Elena's physical description, Claude reads her 500-word character page
- Instead of re-deriving the timeline from the Expanded Outline, Claude reads the pre-built timeline page
- Instead of searching for where a plot thread was planted, Claude reads the thread's dedicated page

This is the core insight: **compiled knowledge beats re-derived knowledge**. The wiki is built once and then kept current as you write, rather than being reconstructed from scratch every session.

### What Goes In the Wiki

| Directory | What It Contains | Example Files |
|-----------|-----------------|---------------|
| `wiki/characters/` | One page per major character | `elena-vasquez.md`, `marcus-chen.md`, `the-director.md` |
| `wiki/locations/` | One page per significant location | `the-observatory.md`, `old-town-market.md`, `sector-7.md` |
| `wiki/plot-threads/` | One page per major plot thread | `missing-artifact.md`, `elena-marcus-rivalry.md`, `uprising.md` |
| `wiki/world/` | World systems and mechanics | `magic-system.md`, `political-factions.md`, `technology.md` |
| `wiki/themes.md` | All themes in one file | Core themes, how each manifests through characters/events |
| `wiki/timeline.md` | Chronological event sequence | Every significant event with chapter/scene references |

The number of pages depends on your story's complexity. A simple contemporary romance might have 8-10 pages. An epic fantasy with multiple factions, a magic system, and dozens of characters could have 30-40+.

---

## Building the Wiki

This is a one-time step you do before your first writing session. It takes about 5-10 minutes.

### Step 1: Open Claude Code in Your Project

```bash
cd YourNovelTitle
claude
```

This opens Claude Code in your project directory. Claude can now read all your files and create new ones.

### Step 2: Ask Claude to Build the Wiki

Paste this prompt into Claude Code:

```
Read all planning files in the project root: FINAL_NOVEL_CONCEPT.md, STARTER_TEMPLATE.md,
EXPANDED_OUTLINE.md, CHAPTER_OUTLINE.md, SCENE_PLAN.md, STORY_RULES.md, WORLD_RULES.md,
WRITING_RULES.md, and SUPPLEMENTARY_MATERIALS.md (if present).

Build the story wiki in the wiki/ directory following the structure in CLAUDE.md:

1. One page per major character in wiki/characters/ — include identity (role, age, physical
   description, speech patterns), arc (starting state, core want/need, key turning points,
   ending state), relationships, connected plot threads, and notes
2. One page per significant location in wiki/locations/ — include physical description,
   atmosphere, which scenes/chapters happen there, symbolic meaning, connected characters
3. One page per major plot thread in wiki/plot-threads/ — include setup, escalation points,
   resolution, which chapters/scenes it touches, connected characters
4. World mechanics pages in wiki/world/ — one page per system (magic, politics, technology,
   culture, etc.)
5. wiki/themes.md — each core theme, how it manifests, which characters carry it
6. wiki/timeline.md — chronological event sequence with chapter/scene references

Cross-reference between pages where relevant. For example, a character page should mention
which plot threads they're involved in, and a plot thread page should mention which characters
drive it.

After creating all pages, give me a summary of what you built — how many pages, what they
cover, and anything you think might be missing.
```

### Step 3: Wait for Claude to Finish

Claude will read all your planning files, extract the key information, and create the wiki pages. This might take a few minutes depending on the complexity of your story. You'll see Claude creating files in real time.

When it's done, Claude will give you a summary like:

> Created 24 wiki pages:
> - 8 character pages (Elena, Marcus, The Director, ...)
> - 5 location pages (The Observatory, Old Town, ...)
> - 4 plot thread pages (Missing Artifact, The Uprising, ...)
> - 3 world pages (Magic System, Political Factions, Technology)
> - themes.md
> - timeline.md
>
> Note: Minor character "Sergeant Okafor" appears in 3 scenes but doesn't have their own page yet. Want me to create one?

### Step 4: Commit the Wiki

```bash
git add wiki/
git commit -m "Built initial story wiki from planning files"
```

---

## Reviewing and Editing Wiki Pages

Before you start writing, it's worth quickly scanning the wiki pages to make sure Claude extracted things correctly. You don't need to read every word — just spot-check:

### Quick Scan Checklist

- [ ] **Character pages**: Do the physical descriptions match what you intended? Are the arcs right? Are all major characters represented?
- [ ] **Location pages**: Are the key locations there? Do the descriptions feel accurate?
- [ ] **Plot threads**: Are the major threads identified? Do the escalation points look right?
- [ ] **World pages**: Are the mechanics correct? Did Claude capture the nuances of your magic system / political structure / technology?
- [ ] **Timeline**: Does the chronological order make sense?
- [ ] **Cross-references**: Do character pages mention their plot threads? Do location pages mention which scenes happen there?

### If Something Is Wrong or Missing

Just tell Claude:

```
Read wiki/characters/elena-vasquez.md. Her eye color should be green, not brown.
Also, add a note about her recurring nightmare — it's mentioned in STARTER_TEMPLATE.md
under her psychological profile. Fix it.
```

Or:

```
There's no wiki page for the abandoned mine. It appears in chapters 8, 12, and 19.
Create wiki/locations/abandoned-mine.md with details from the planning files.
```

Claude will make the edits and you can commit them.

### Don't Over-Polish

The wiki doesn't need to be perfect before you start writing. It's a living document — every scene you write makes it more detailed and accurate. Think of the initial build as a solid foundation, not a finished product.

---

## Your First Writing Session

Now the good stuff. Your project is set up, your wiki is built, and you're ready to write.

### Start Claude Code

```bash
cd YourNovelTitle
claude
```

### Create a Writing Branch

This is optional but recommended. It keeps your writing work separate from the main branch until you're happy with it:

**Scene-based:**
```bash
git checkout -b writing-2026-04-05-scenes-1.1-1.3
```

**Chapter-based:**
```bash
git checkout -b writing-2026-04-05-ch1
```

### Write Your First Scene or Chapter

**Scene-based — use the slash command:**
```
/project:write-scene 1.1
```

**Chapter-based — use the slash command:**
```
/project:write-chapter 1
```

If you don't specify a number, Claude picks up the next incomplete one automatically.

### What Happens Behind the Scenes

When you run the write command, here's what Claude does (you don't need to do any of this manually):

1. **Spawns the continuity-precheck agent** — this agent reads the full manuscript (empty for your first scene, but critical later), reads relevant wiki pages for the characters and locations involved, and produces a compressed "Scene Brief" or "Chapter Brief" containing everything the writing session needs to know

2. **Spawns the voice-profiler agent** (in parallel) — this agent analyzes how the POV character has sounded in previous scenes/chapters and produces a "Voice Profile" so the voice stays consistent

3. **Loads the writing context** — the main session gets the brief, the voice profile, the last 2-3 scenes (for voice continuity), the rules files, and the scene/chapter plan entry

4. **Pre-writing preparation** — Claude identifies the emotional architecture of the scene/chapter (entry state, turn, landing), settles into the POV character's voice, and checks continuity

5. **Writes the scene or chapter** — following all the rules in `WRITING_RULES.md`, `STORY_RULES.md`, and `WORLD_RULES.md`

6. **Spawns the validator agent** — checks for continuity errors and rule violations, auto-fixes minor issues, flags major ones

7. **Presents the scene/chapter for your review** — with validation results. **Claude waits here. It does NOT proceed until you approve.**

### Review and Iterate

Read through what Claude wrote. If something isn't right, just say so:

> The dialogue in the third paragraph feels too formal for Marcus. He'd use shorter sentences and more slang.

> The description of the market is too long. Cut it in half — focus on the smell of spices and the sound of the crowd, not the visual details.

> This scene needs more tension. Elena should notice something wrong earlier — maybe the silence where there should be noise.

Claude will revise and show you the updated version. Iterate as many times as you need.

### Approve and Update

When you're happy with the scene or chapter:

**Scene-based:**
```
/project:post-scene
```

Or combine approval with the command:
```
This is good. /project:post-scene
```

**Chapter-based:**
```
/project:post-chapter
```

Or combined:
```
This is good. /project:post-chapter
```

### What Happens When You Approve

The post-scene or post-chapter command triggers the **file-updater agent**, which does all of this automatically:

1. Updates `CONTINUITY_TRACKER.md` with current character positions, emotional states, and knowledge
2. Updates `MISC_STORY_NOTES.md` with craft analysis and development notes
3. Documents any deviations from the plan in `MODIFICATION_LOG.md`
4. **Updates relevant wiki pages** — if a character relationship shifted, if a new location detail was established, if a plot thread advanced, if a timeline event occurred
5. Marks the scene/chapter complete in the tracking file
6. Creates a git commit with details about what was written

For **scene-based** specifically: scenes are NOT appended to the manuscript individually. When all scenes in a chapter are done, you'll assemble them.

For **chapter-based**: the chapter is appended to `manuscript/CURRENT_MANUSCRIPT.md` immediately.

### Assembling Chapters (Scene-Based Only)

When the post-scene command reports that all scenes in a chapter are complete:

```
/project:build-chapter 1
```

This:
1. Validates all scenes are marked complete
2. Assembles the scene files into `chapters/chapter_1.md` with proper headers and `* * *` scene breaks
3. Flags any transition issues (jarring shifts between scenes)
4. Presents the assembled chapter for your review
5. After approval, appends it to the manuscript

### Keep Writing

Repeat the cycle:
- Write scene/chapter
- Review and iterate
- Approve
- (Assemble chapter if scene-based)
- Write the next one

---

## How the Wiki Stays Current

This is the part that makes the wiki genuinely useful instead of just a nice-to-have.

### Automatic Updates After Every Scene/Chapter

Every time you approve a scene or chapter, the file-updater agent checks whether any wiki pages need updating. Here's what it looks for:

| What Happened in the Scene/Chapter | Wiki Update |
|-------------------------------------|-------------|
| A character revealed something new about themselves | Character page gets the new detail |
| Two characters' relationship changed | Both character pages get updated relationship entries |
| A new location was described in detail | Location page gets the new details (or a new page is created) |
| A plot thread advanced or escalated | Plot thread page gets the new development |
| A new plot thread was introduced | New plot thread page created |
| A plot thread was resolved | Plot thread page marked as resolved |
| A significant event occurred | Timeline page gets the new event |
| A world mechanic was used or clarified | World page gets the new information |

### What This Looks Like in Practice

Say you write Scene 4.2 where Elena discovers that Marcus has been lying about his background. After you approve:

**wiki/characters/elena-vasquez.md** might get updated:
```markdown
## Relationships
- **Marcus Chen**: Initially trusted ally → discovered his background deception in Scene 4.2.
  Trust severely damaged. Now questioning everything he's told her.
```

**wiki/characters/marcus-chen.md** might get updated:
```markdown
## Notes
- Background deception revealed to Elena in Scene 4.2. His cover story about
  growing up in the northern territories was fabricated.
```

**wiki/plot-threads/marcus-deception.md** might get updated:
```markdown
## Escalation
- **Scene 2.3**: First hint — Marcus avoids questions about his childhood
- **Scene 3.1**: Elena notices inconsistencies in his stories
- **Scene 4.2**: Full revelation — Elena confronts him, he can't maintain the lie
```

**wiki/timeline.md** gets a new entry:
```markdown
- **Scene 4.2**: Elena discovers Marcus's background deception
```

You didn't do any of this manually. The agent read the scene, identified what changed, and updated the relevant pages.

### The Compounding Effect

This is the real power. By scene 30 or chapter 15:

- Character pages have detailed relationship arcs with timestamps
- Plot thread pages show the full escalation path
- The timeline is comprehensive
- Location pages have accumulated atmospheric details from every scene set there
- World pages reflect how mechanics have been used in practice, not just how they were planned

The wiki gets richer with every scene you write. When the continuity-precheck agent reads wiki pages for Scene 31, it has the benefit of everything that was compiled from the previous 30 scenes — not just what was in the original planning files.

---

## The Continuity Tracker vs. the Wiki

These are two different things that work together. Understanding the difference is important.

### CONTINUITY_TRACKER.md — "Where is everything right now?"

This file tracks the **current state** of your story. It gets overwritten (not appended) with the latest information after each scene or chapter.

Think of it like a save file in a video game. It tells you:
- Where each character physically is right now
- What each character knows at this moment
- What emotional state each character is in
- Where important objects are
- What time/date it is in the story

When Claude starts writing the next scene, it checks the tracker to know: "Elena is at the observatory, she just learned about Marcus's deception, it's Tuesday evening, and she has the artifact in her bag."

### The Wiki — "Who and what is everything?"

The wiki tracks **permanent reference knowledge** that doesn't change with every scene. It accumulates over time.

Think of it like an encyclopedia. It tells you:
- Who Elena is (her full identity, arc, speech patterns, relationships)
- What the observatory looks like (physical description, atmosphere, significance)
- How the magic system works (rules, limitations, costs)
- What the "Marcus deception" plot thread is (setup, escalation, all connected scenes)

When the continuity-precheck agent prepares the brief for the next scene, it reads relevant wiki pages to get the full picture — not just where Elena is now, but who she is, how she talks, what she cares about.

### Why You Need Both

| Scenario | Tracker | Wiki |
|----------|---------|------|
| "Where did Elena end the last scene?" | Check tracker | - |
| "What are Elena's speech patterns?" | - | Check wiki character page |
| "What does Elena know about the artifact?" | Check tracker (current knowledge) | Check wiki (artifact's full history) |
| "Has the uprising plot thread been resolved?" | - | Check wiki plot thread page |
| "Where is the artifact right now?" | Check tracker | - |
| "How does the magic system work?" | - | Check wiki world page |
| "What happened in the story so far?" | Check tracker (latest state) | Check wiki timeline (full history) |

---

## Running Continuity Audits

Every 5-10 scenes (scene-based) or every 2-3 chapters (chapter-based), run an audit. This is especially important at story beats like the end of Act 1, the midpoint, or before the climax.

```
/project:continuity-audit
```

The **manuscript-auditor** agent reads everything — the full manuscript, all tracking files, and all wiki pages — and checks for:

- Character inconsistencies (eye color changed, personality shifted without reason)
- Timeline errors (events in the wrong order, time gaps that don't make sense)
- Object tracking (items appearing or disappearing without explanation)
- Location details (descriptions that contradict earlier scenes)
- POV discipline (information the POV character shouldn't know)
- Voice drift (a character's speech patterns changing without reason)
- Rules violations (breaking your own world mechanics or story rules)
- Planted elements (setups that haven't been paid off, or payoffs without setups)
- **Outline drift** (how the manuscript's trajectory compares to the original plan)

The audit presents findings sorted by severity:
- **Critical**: Must fix now (contradictions, broken continuity)
- **Minor**: Should fix eventually (small inconsistencies)
- **Recommendations**: Nice to have (opportunities to strengthen connections)

The audit also checks whether the wiki pages are still accurate. If the manuscript has drifted from what a wiki page says, the audit flags it.

### Good Times to Audit

- After completing Act 1 (roughly the first 25% of scenes/chapters)
- At the midpoint
- After completing Act 2 (roughly the 75% mark)
- Before the climax
- After any major revelation or plot twist
- Whenever something feels "off" and you can't put your finger on it

---

## Ending Your Writing Session

When you're done for the day:

```
/project:end-session
```

This command:
1. Checks for any uncommitted work and prompts you to commit
2. Creates a session summary in `session-notes/` — what you wrote, key developments, craft observations, and where to pick up tomorrow
3. Merges your writing branch to `draft-v1`
4. Pushes to your remote repository if one is configured
5. Reports your progress stats (scenes/chapters completed, word count)

The session notes are useful when you come back tomorrow. Claude can read them to quickly orient itself on where you left off.

---

## Common Questions

### "Do I need to maintain the wiki manually?"

No. The file-updater agent handles it after every scene/chapter you approve. You might occasionally want to spot-check a page or ask Claude to add something specific, but day-to-day maintenance is automatic.

### "What if Claude gets something wrong in the wiki?"

Just tell Claude to fix it:
```
Read wiki/characters/marcus-chen.md. His age should be 34, not 28.
Also his relationship with Elena should note that they met at the academy, not at the market.
```

### "What if my story diverges from the original plan?"

That's expected and fine. The wiki evolves with your story, not with the original plan. If you decide in chapter 8 that Marcus is actually a double agent (not in the original outline), the wiki will reflect that after you approve the relevant scene. The continuity audit's "outline drift" section will note the divergence so you're aware of it.

### "Can I add wiki pages manually?"

Yes. If you realize there's a minor character or location that deserves its own page, just tell Claude:
```
Create a wiki page for Sergeant Okafor at wiki/characters/sergeant-okafor.md.
She appears in scenes 3.2, 5.1, and 7.4. Pull her details from those scenes and the
planning files.
```

### "How big does the wiki get?"

It depends on your story. Rough guide:
- **Simple story** (2-3 characters, few locations, 1-2 plot threads): 8-12 pages
- **Medium complexity** (5-8 characters, several locations, multiple threads): 15-25 pages
- **Epic/complex** (10+ characters, extensive world-building, many threads): 30-50+ pages

Each page is typically 200-800 words. The whole wiki might be 5,000-25,000 words — much smaller than your planning files but much more organized.

### "What if I'm using the chapter-based workflow — do I still get scene-level detail in the wiki?"

Yes. Even though you write at the chapter level, the wiki tracks details at whatever granularity matters. If a key moment happens in the middle of chapter 5, it goes in the wiki. The timeline page will reference chapter numbers instead of scene numbers, but the detail level is the same.

### "Can I browse the wiki outside of Claude?"

Absolutely. The wiki is just markdown files in a folder. You can open them in any text editor, VS Code, Obsidian, or any markdown viewer. Obsidian is particularly nice because it renders the cross-references as clickable links and has a graph view that shows how pages connect to each other.

### "What happens if I skip post-scene / post-chapter?"

The wiki doesn't get updated for that scene/chapter. The continuity tracker doesn't get updated either. This means the next scene/chapter will be written with stale information. Don't skip it — the automated updates are what make the whole system work.

### "I'm worried about context window limits. Does the wiki make things worse?"

The wiki makes things **better**, not worse. Without the wiki, the continuity-precheck agent has to scan your full planning files (potentially 60,000+ tokens) to find relevant details. With the wiki, it reads just the specific character, location, and plot thread pages it needs (maybe 2,000-5,000 tokens total). The wiki trades a one-time build cost for dramatically smaller lookups during every subsequent writing session.

---

## Quick Reference

### Commands

| Command | When to Use | Workflow |
|---------|------------|----------|
| `/project:write-scene [X.Y]` | Write the next scene | Scene-based |
| `/project:post-scene` | After approving a scene | Scene-based |
| `/project:build-chapter [X]` | When all scenes in a chapter are done | Scene-based |
| `/project:write-chapter [X]` | Write the next chapter | Chapter-based |
| `/project:post-chapter` | After approving a chapter | Chapter-based |
| `/project:continuity-audit` | Every 5-10 scenes / 2-3 chapters | Both |
| `/project:end-session` | When done writing for the day | Both |

### File Roles

| File / Directory | Role | Updated When |
|-----------------|------|-------------|
| `manuscript/CURRENT_MANUSCRIPT.md` | The growing novel | After chapter assembly (scene) or post-chapter (chapter) |
| `wiki/` | Compiled story knowledge base | After every post-scene / post-chapter |
| `continuity/CONTINUITY_TRACKER.md` | Current state of all characters, objects, timeline | After every post-scene / post-chapter |
| `continuity/MODIFICATION_LOG.md` | Deviations from the original plan | When deviations occur |
| `MISC_STORY_NOTES.md` | Craft notes, development insights | After every post-scene / post-chapter |
| `SCENE_PLAN.md` | Scene checklist | Scenes marked complete after post-scene |
| `SCENE_COMPLETION_STATUS.md` / `CHAPTER_COMPLETION_STATUS.md` | Progress tracking | After every post-scene / post-chapter |

### Typical Session Flow

```
1. Open terminal, cd to project, run `claude`
2. Create a writing branch (optional)
3. /project:write-scene 4.3        (or /project:write-chapter 4)
4. Review Claude's output
5. Request edits if needed
6. "This is good. /project:post-scene"   (or /project:post-chapter)
7. Repeat from step 3
8. /project:build-chapter 4          (scene-based only, when all scenes done)
9. /project:continuity-audit         (every 5-10 scenes or 2-3 chapters)
10. /project:end-session             (when done for the day)
```

### When to Audit

- End of Act 1
- Midpoint
- End of Act 2
- Before the climax
- After major revelations
- Whenever something feels off

---

## One Last Thing

The wiki is inspired by a pattern described by Andrej Karpathy: instead of an AI re-deriving knowledge from raw documents on every query, have it build and maintain a persistent, structured knowledge base that compounds over time. The tedious part of maintaining a knowledge base is the bookkeeping — updating cross-references, keeping pages current, noting contradictions. That's exactly what Claude is good at.

Your job is to write the story. Claude's job is everything else — the filing, the cross-referencing, the continuity tracking, the bookkeeping that would otherwise eat your creative energy. The wiki is what makes that possible at novel scale.

Now go write your book.
