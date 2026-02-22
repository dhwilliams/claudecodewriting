# CCWriting — Claude Code Novel Writing System

A structured workflow for writing a novel with Claude Code, designed to maintain perfect continuity as your manuscript grows beyond what fits in a single context window.

## How It Works

You write one scene at a time. After each scene is approved, tracking files are updated automatically. When all scenes in a chapter are done, you assemble them into a chapter file and append it to the manuscript. Specialized agents run in separate context windows to handle manuscript reading, file updates, and continuity auditing — keeping the main writing session focused on craft.

```
write scene → approve → update tracking → (repeat)
                                            ↓
              all chapter scenes done → build chapter → approve → append to manuscript
```

## Getting Your Planning Files from BookWeaverAI

The planning files used by this writing system come from the **BookWeaverAI** web app. BookWeaverAI guides you through 5 AI-powered steps to develop your story from initial concept to a detailed scene-by-scene plan. Once all 5 steps are complete, you export everything and bring it here.

### Export from BookWeaverAI

1. Complete all 5 steps in the BookWeaverAI app:
   - **Step 1**: Novel Concept
   - **Step 2**: Starter Template
   - **Step 3**: Expanded Outline
   - **Step 4**: Chapter Outline
   - **Step 5**: Scene Plan

2. Go to your project's **Export** page

3. Click **"Download All as ZIP"** — this downloads a `.zip` file named `{YourProjectTitle}_BookWeaver_Export.zip`

4. The ZIP contains these markdown files:

| File in ZIP | Source | Used By CCWriting |
|-------------|--------|-------------------|
| `FINAL_NOVEL_CONCEPT.md` | Step 1 — Novel Concept | Story reference |
| `STARTER_TEMPLATE.md` | Step 2 — Starter Template | Background reference |
| `EXPANDED_OUTLINE.md` | Step 3 — Expanded Outline | Full outline |
| `CHAPTER_OUTLINE.md` | Step 4 — Chapter Outline | Chapter titles, scene context |
| `SCENE_PLAN.md` | Step 5 — Scene Plan | Scene-by-scene checklist |
| `WRITING_RULES.md` | Project Settings | Prose style guide, voice rules |
| `STORY_RULES.md` | Project Inputs | Narrative constraints and conventions |
| `WORLD_RULES.md` | Project Inputs | World mechanics and laws |

All 8 files are included in the ZIP. You can also download the 5 step files individually from the export page.

## Setup

### Prerequisites

- [Claude Code](https://claude.com/claude-code) CLI installed
- A completed planning phase (all 5 BookWeaverAI steps done and exported)

### Initialize Project

1. **Unzip your export** into the CCWriting directory:

```bash
cd CCWriting

# Unzip the BookWeaverAI export
unzip ~/Downloads/Your_Project_BookWeaver_Export.zip -d .
```

This extracts all 8 planning files (5 steps + 3 rules files) into the directory.

2. **Create the writing directories and tracking files**:

```bash
# Create directories
mkdir -p {manuscript,scenes,chapters,session-notes,continuity}

# Create tracking files
touch manuscript/CURRENT_MANUSCRIPT.md
touch MISC_STORY_NOTES.md
touch continuity/CONTINUITY_TRACKER.md
touch continuity/MODIFICATION_LOG.md
touch SCENE_COMPLETION_STATUS.md

# Initialize git
git init
git add .
git commit -m "Writing project initialized"
```

### Required Planning Files

After setup, these files must all be present before you start writing. All come from the BookWeaverAI ZIP export:

| File | Purpose |
|------|---------|
| `FINAL_NOVEL_CONCEPT.md` | Finalized novel concept |
| `STARTER_TEMPLATE.md` | Character/world reference |
| `EXPANDED_OUTLINE.md` | Complete novel outline |
| `CHAPTER_OUTLINE.md` | Chapter breakdown with titles |
| `SCENE_PLAN.md` | Scene checklist (`- [ ]` / `- [x]`) |
| `WRITING_RULES.md` | Prose style guide, voice rules |
| `STORY_RULES.md` | Narrative constraints and conventions |
| `WORLD_RULES.md` | World mechanics and laws |

## Commands

### `/project:write-scene [scene_number]`

Your primary writing command. Handles everything from continuity analysis to scene generation.

```
/project:write-scene 4.3
```

What happens:
1. Spawns the **continuity-precheck** agent to read the full manuscript and produce a Scene Brief
2. Loads all rules files, scene plan, chapter outline, and the Scene Brief
3. Reads the previous 2-3 scenes for voice continuity
4. States the emotional architecture (entry state, turn, landing) before writing
5. Writes the scene directly to `scenes/scene_X_Y.md`
6. Presents for your review — **waits for approval before anything else happens**

If no scene number is given, it picks up the next incomplete scene from `SCENE_PLAN.md`.

### `/project:post-scene`

Run after you approve a scene. Handles all mechanical file updates.

```
/project:post-scene
```

What happens:
1. Spawns the **file-updater** agent to update all tracking files
2. Updates `continuity/CONTINUITY_TRACKER.md` with every new detail from the scene
3. Updates `MISC_STORY_NOTES.md` with craft analysis
4. Documents any deviations in `continuity/MODIFICATION_LOG.md`
5. Marks the scene complete in `SCENE_PLAN.md`
6. Updates `SCENE_COMPLETION_STATUS.md`
7. Git commits all changes
8. Reports chapter progress — if all scenes in the chapter are done, prompts you to build the chapter

You can combine approval and post-scene in one message:
```
This is good. /project:post-scene
```

### `/project:build-chapter [chapter_number]`

Run when all scenes in a chapter are complete. Assembles scenes into a chapter file and appends it to the manuscript.

```
/project:build-chapter 4
```

What happens:
1. Validates every scene in the chapter is marked `- [x]` — halts if any are incomplete
2. Spawns the **chapter-builder** agent to assemble scene files
3. Creates `chapters/chapter_4.md` with chapter header and `* * *` scene breaks
4. Flags any transition issues (jarring shifts, time gaps, tonal whiplash)
5. Presents the assembled chapter for your review — **waits for approval**
6. After approval, appends the chapter to `manuscript/CURRENT_MANUSCRIPT.md`
7. Git commits with chapter details
8. Reports chapter word count, total manuscript word count, and next chapter status

### `/project:continuity-audit`

Run every 5-10 scenes, at act breaks, or whenever something feels off.

```
/project:continuity-audit
```

What happens:
1. Spawns the **manuscript-auditor** agent to read the entire manuscript plus all tracking files
2. Audits character consistency, timeline integrity, object tracking, location details, POV discipline, voice drift, rules compliance, planted elements, and chapter assembly integrity
3. Presents findings sorted by severity: Critical > Minor > Recommendations

Good times to run an audit:
- End of Act 1
- Midpoint
- End of Act 2
- Before the climax
- After any major revelation or plot twist

## Agents

These run in **separate context windows** so they can load the full manuscript without crowding your writing session.

| Agent | Model | Purpose | Spawned By |
|-------|-------|---------|------------|
| `continuity-precheck` | Opus | Reads full manuscript, produces compressed Scene Brief | `write-scene` |
| `file-updater` | Sonnet | Updates all tracking files after scene approval | `post-scene` |
| `chapter-builder` | Sonnet | Assembles scene files into a formatted chapter | `build-chapter` |
| `manuscript-auditor` | Opus | Deep continuity and consistency audit | `continuity-audit` |

## Writing Session Workflow

### Start

```
/project:write-scene 4.3
```

That's it. The skill handles context loading, continuity analysis, and scene generation.

### Review and Iterate

After Claude presents the scene, read through carefully. Request edits:

> Make the dialogue in paragraph 5 more clipped — she's angry but hiding it.

Iterate until you're satisfied.

### Approve and Update

```
/project:post-scene
```

### Assemble Chapters

When post-scene reports all chapter scenes are complete:

```
/project:build-chapter 4
```

Review the assembled chapter. Approve to add it to the manuscript.

### Audit Periodically

```
/project:continuity-audit
```

### End of Session

- Session notes are created in `session-notes/`
- Merge to your draft branch: `git checkout draft-v1 && git merge [branch]`
- Push if configured

## File Structure

```
CCWriting/
├── manuscript/
│   └── CURRENT_MANUSCRIPT.md        # The growing novel (assembled chapters)
├── chapters/
│   ├── chapter_1.md                  # Assembled chapter files
│   ├── chapter_2.md
│   └── ...
├── scenes/
│   ├── scene_1_1.md                  # Individual scene archive
│   ├── scene_1_2.md
│   └── ...
├── continuity/
│   ├── CONTINUITY_TRACKER.md         # Character states, objects, timeline
│   └── MODIFICATION_LOG.md           # Deviations from plan
├── session-notes/                    # Session summaries
├── FINAL_NOVEL_CONCEPT.md            # Story concept
├── WRITING_RULES.md                  # Prose style guide
├── STORY_RULES.md                    # Narrative constraints
├── WORLD_RULES.md                    # World mechanics
├── EXPANDED_OUTLINE.md               # Full outline
├── CHAPTER_OUTLINE.md                # Chapter breakdown with titles
├── SCENE_PLAN.md                     # Scene checklist
├── SCENE_COMPLETION_STATUS.md        # Progress tracking
├── MISC_STORY_NOTES.md               # Craft notes and decisions
├── CLAUDE.md                         # Writing philosophy and protocols
└── PROMPTS.md                        # Command reference and workflow guide
```

### How Files Flow

```
Planning files (read-only reference)
    ↓
scenes/scene_X_Y.md          ← written one at a time
    ↓
chapters/chapter_X.md         ← assembled from scenes
    ↓
manuscript/CURRENT_MANUSCRIPT.md  ← built from chapters
```

Tracking files (`CONTINUITY_TRACKER.md`, `MISC_STORY_NOTES.md`, `MODIFICATION_LOG.md`, `SCENE_COMPLETION_STATUS.md`, `SCENE_PLAN.md`) are updated after every scene and serve as persistent memory across sessions.

## Manuscript Format

Chapters are separated by `---`. Scenes within a chapter are separated by `* * *`.

```markdown
# Chapter 1: The Arrival

The truck rattled up the gravel drive...

* * *

By evening, the boxes were stacked in the hallway...

---

# Chapter 2: First Night

She couldn't say what woke her...

* * *

The kitchen light was already on when she came downstairs...
```

## Creating a Synopsis / Book Jacket Blurb

To generate a synopsis or book jacket blurb at any point during or after writing, prompt Claude with:

```
Read FINAL_NOVEL_CONCEPT.md, EXPANDED_OUTLINE.md, CHAPTER_OUTLINE.md, STORY_RULES.md,
and WORLD_RULES.md. If writing is in progress, also read manuscript/CURRENT_MANUSCRIPT.md
for tone reference.

Write a book jacket blurb (150-200 words) that:
- Opens with a hook that captures the core tension
- Introduces the protagonist and their situation without naming the antagonist
- Escalates to the central conflict
- Ends with a question or unresolved tension that makes the reader need to know more
- Matches the novel's tone and voice
- Never reveals the ending or major twists
- Avoids cliches like "nothing is as it seems" or "will they survive?"

Then write a longer synopsis (500-800 words) suitable for query letters that:
- Covers the full arc including the ending
- Highlights the emotional journey, not just plot events
- Names all major characters and their roles
- Identifies the genre, comparable titles, and target audience
- Includes word count and genre classification

Save the blurb to SYNOPSIS_BLURB.md and the full synopsis to SYNOPSIS_FULL.md.
```

You can also ask for variations:

- **Back cover copy**: More atmospheric, focuses on mood and premise
- **Query letter synopsis**: Includes the ending, aimed at agents/editors
- **Elevator pitch**: 1-2 sentences capturing the core hook
- **Comparative pitch**: "X meets Y" format with comp titles

Example prompt for a quick elevator pitch:

```
Read FINAL_NOVEL_CONCEPT.md. Write a one-sentence elevator pitch and a
two-sentence version. Save to SYNOPSIS_PITCH.md.
```

## Manual Prompting (Fallback)

If you prefer not to use the slash commands, these prompts replicate the workflow:

**Scene generation:**
```
Read CLAUDE.md, WRITING_RULES.md, STORY_RULES.md, WORLD_RULES.md, CHAPTER_OUTLINE.md,
SCENE_PLAN.md, and continuity/CONTINUITY_TRACKER.md. Then read the previous 2-3 scenes
from scenes/ for voice continuity. Write scene [X.Y] following all CLAUDE.md protocols.
Create the file at scenes/scene_X_Y.md. Do not commit until I approve.
```

**Post-scene:**
```
Follow CLAUDE.md "After Each Scene" protocol exactly. Update all tracking files
(continuity tracker, modification log, misc notes, scene plan, completion status).
Do NOT append to manuscript — scenes are assembled into chapters first.
Then commit with a structured message. Do not push.
```

**Chapter assembly:**
```
All scenes for Chapter [X] are complete. Read all scene files for the chapter from scenes/,
get the chapter title from CHAPTER_OUTLINE.md, and assemble them into chapters/chapter_X.md
with "# Chapter X: [Title]" header and "* * *" scene breaks. Flag any transition issues.
After I approve, append the chapter to manuscript/CURRENT_MANUSCRIPT.md and commit.
```

## Tips

- **The tracker is everything.** `CONTINUITY_TRACKER.md` is the compressed memory of your novel. The more detailed it is, the better your continuity.
- **Review the Scene Brief.** The write-scene skill shows you the Scene Brief before writing. If something critical is missing, mention it.
- **Flag issues immediately.** Fixing a continuity error during review costs one edit. Fixing it later costs reading back through dozens of scenes.
- **Don't skip post-scene.** Every skipped update is compounding debt. The tracking files keep future scenes accurate.
- **Run audits at story beats.** Act breaks, midpoints, and major revelations are natural checkpoints.
- **Scene files are your archive.** They're never deleted. If you need to revisit what was originally written vs. what ended up in a chapter, the scene files are your source of truth.
