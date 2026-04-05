# CCWriting — Prompts & Workflow Guide

## Overview

This document explains the Claude Code novel-writing workflow, the custom skills (slash commands), and agents designed to maintain continuity as your manuscript grows.

## The Continuity Problem

As your novel grows beyond 30,000 words, the full manuscript plus all reference files no longer fit in Claude's context window. By chapter 10, you could have 80,000+ words of manuscript alone — well over half the available context — leaving no room for the outlines, rules, and scene plans Claude needs to write well.

This workflow solves that with a **layered context strategy**:

1. **Continuity Precheck Agent** — Before each scene, a subagent reads the full manuscript in its own context window and produces a compressed "Scene Brief" containing everything Claude needs
2. **Focused Writing Context** — The main writing session loads only what matters: the Scene Brief, the last 2-3 scenes (for voice matching), rules files, and the scene plan entry
3. **Post-Scene File Updater Agent** — After approval, a subagent handles all mechanical file updates so nothing gets missed
4. **Periodic Audits** — Every 5-10 scenes, a deep continuity audit catches drift before it compounds

## Available Commands

### `/project:write-scene [scene_number]`

Your primary writing command. Replaces manual prompting.

```
/project:write-scene 4.3
```

What happens:
1. Spawns the **continuity-precheck** and **voice-profiler** agents in parallel — one produces a Scene Brief (referencing relevant wiki pages), the other a Voice Profile for the POV character
2. Loads CLAUDE.md, rules files, scene plan, chapter outline, Scene Brief, and Voice Profile
3. Reads the previous 2-3 scenes for voice continuity
4. Identifies the emotional architecture (entry state, turn, landing) and voice anchors
5. Writes the complete scene directly to `scenes/scene_X_Y.md`
6. Spawns the **scene-validator** agent to check for continuity errors and rule violations — auto-fixes minor issues, flags major ones
7. Presents for your review with validation results
8. **Waits for your approval** — nothing else happens until you say it's good

### `/project:post-scene`

Run after you approve a scene. Handles all file updates and tracking.

```
/project:post-scene
```

What happens:
1. Updates `continuity/CONTINUITY_TRACKER.md` with every new detail
2. Updates `MISC_STORY_NOTES.md` with craft analysis
3. Documents deviations in `continuity/MODIFICATION_LOG.md` (if any)
4. Updates relevant `wiki/` pages (characters, locations, plot threads, timeline)
5. Marks the scene complete in `SCENE_PLAN.md`
6. Updates `SCENE_COMPLETION_STATUS.md`
7. Creates a structured git commit
8. Reports chapter progress — if all scenes in the chapter are complete, prompts you to run `/project:build-chapter X`

### `/project:build-chapter [chapter_number]`

Run when all scenes in a chapter are complete. Assembles scenes into a chapter file and appends to the manuscript.

```
/project:build-chapter 4
```

What happens:
1. Validates all scenes for the chapter are marked `- [x]` in SCENE_PLAN.md
2. Spawns the **chapter-builder** agent to assemble scene files into `chapters/chapter_X.md`
3. Adds chapter header (`# Chapter X: [Title]`) and scene breaks (`* * *`)
4. Flags any transition issues (jarring shifts, time gaps, tonal whiplash)
5. Presents the assembled chapter for your review
6. **Waits for your approval** — nothing is appended to the manuscript until you confirm
7. After approval, appends the chapter to `manuscript/CURRENT_MANUSCRIPT.md`
8. Creates a git commit with chapter details
9. Reports chapter and total manuscript word counts

### `/project:continuity-audit`

Run every 5-10 scenes, or at act/part breaks, or when something feels off.

```
/project:continuity-audit
```

What happens:
1. Spawns the **manuscript-auditor** agent to read everything
2. Cross-references characters, timeline, objects, locations, world rules
3. Checks for voice drift across POV characters
4. Reports planted elements (set up, paid off, or dangling)
5. Verifies `CONTINUITY_TRACKER.md` accuracy against the actual manuscript
6. Analyzes **outline drift** — compares manuscript trajectory against the original plan
7. Presents findings sorted by severity (Critical → Minor → Recommendations)

### `/project:end-session`

Run when you're done writing for the day. Automates all wrap-up tasks.

```
/project:end-session
```

What happens:
1. Checks for uncommitted work and prompts to commit if needed
2. Spawns the **session-closer** agent to summarize today's work
3. Creates a session notes file in `session-notes/` with scenes completed, key developments, craft observations, and tomorrow's starting point
4. Merges the current writing branch to `draft-v1`
5. Pushes to remote if configured
6. Reports final progress stats

## Agents

These run in **separate context windows** so they can read the full manuscript without crowding the main writing session.

| Agent | Purpose | Model | Spawned By |
|-------|---------|-------|------------|
| `continuity-precheck` | Pre-scene analysis → Scene Brief (reads wiki pages) | Opus | `write-scene` skill |
| `voice-profiler` | POV character voice analysis → Voice Profile | Opus | `write-scene` skill |
| `scene-validator` | Post-write continuity and rules check | Sonnet | `write-scene` skill |
| `file-updater` | Post-scene mechanical file + wiki updates | Sonnet | `post-scene` skill |
| `chapter-builder` | Assembles scenes into a chapter file | Sonnet | `build-chapter` skill |
| `manuscript-auditor` | Deep continuity, consistency, and outline drift audit | Opus | `continuity-audit` skill |
| `session-closer` | End-of-session summary and notes | Sonnet | `end-session` skill |

## Complete Session Workflow

### Starting a Writing Session

```
/project:write-scene 4.3
```

That's it. The skill handles context loading, continuity precheck, and scene generation.

### Reviewing and Iterating

After Claude presents the scene:
- Read through carefully
- Request specific edits: *"Make the dialogue in paragraph 5 more clipped — she's angry but hiding it"*
- Iterate until satisfied

### Approving and Updating

```
/project:post-scene
```

Or combine with your approval:
```
This is good. /project:post-scene
```

### Assembling Chapters

When post-scene reports that all scenes in a chapter are complete:
```
/project:build-chapter 4
```

Review the assembled chapter, then approve to append it to the manuscript. The manuscript grows by whole chapters, not individual scenes.

### Periodic Audits

Every 5-10 scenes (or at act breaks):
```
/project:continuity-audit
```

Review the findings. Fix critical issues immediately. Note minor issues for revision.

### End of Session

```
/project:end-session
```

Handles everything: creates session notes, merges to draft-v1, pushes if configured.

## Tips for Maximum Continuity

### The Tracker is Everything

`CONTINUITY_TRACKER.md` is the compressed memory of your entire novel. The more detailed it is, the better continuity you get. The post-scene agent updates it automatically, but review its updates occasionally to make sure nothing important was missed.

### Review the Scene Brief

The write-scene skill will show you the Scene Brief before writing. Scan it — if something critical is missing, mention it before Claude writes.

### Flag Issues in the Moment

If you notice a continuity error while reviewing a scene, flag it before approving. Fixing it now costs one edit. Fixing it retroactively costs reading back through dozens of scenes.

### Don't Skip Post-Scene Updates

Every skipped update is a compounding debt. The tracking files are what keep future scenes accurate. The `post-scene` command makes this painless.

### Run Audits at Story Beats

Natural audit points:
- End of Act 1
- Midpoint
- End of Act 2 / Start of Act 3
- Before the climax
- After any major revelation or plot twist

### Manual Prompting (Fallback)

If you prefer manual control, these are the improved versions of your original prompts:

**Scene generation:**
```
Read CLAUDE.md, WRITING_RULES.md, STORY_RULES.md, WORLD_RULES.md, CHAPTER_OUTLINE.md,
SCENE_PLAN.md, SUPPLEMENTARY_MATERIALS.md (if present), and continuity/CONTINUITY_TRACKER.md.
Then read the previous 2-3 scenes from scenes/ for voice continuity. Write scene [X.Y]
following all CLAUDE.md protocols. Create the file at scenes/scene_X_Y.md. Do not commit
until I approve.
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

## Why This Workflow Works

The traditional approach — loading everything into one context and hoping Claude tracks it — breaks down around 30K words. This workflow succeeds because:

1. **Separation of concerns**: Reading and compression (agents) vs. creative writing (main session)
2. **Persistent state**: Tracking files serve as external memory that survives across sessions
3. **Compiled knowledge**: The story wiki provides pre-organized, cross-referenced reference pages — Claude looks up specific character or plot details instead of re-scanning entire planning documents
4. **Structured updates**: Nothing gets forgotten because the post-scene process is systematic
5. **Chapter-level review**: Assembling scenes into chapters provides a natural editing boundary — you review transitions, pacing, and flow at the chapter level before it becomes part of the manuscript
6. **Periodic verification**: Audits catch what tracking files miss
7. **Voice preservation**: Always loading the last 2-3 full scenes keeps voice consistent even when the full manuscript can't be loaded
