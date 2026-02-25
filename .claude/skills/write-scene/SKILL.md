---
name: write-scene
description: Begin writing a specific scene. Runs continuity precheck, loads context, and writes the scene following all CLAUDE.md protocols. Usage: /project:write-scene 4.3
allowed-tools: Read, Glob, Grep, Write, Edit, Task, Bash
---

You are about to write a scene for this novel. Follow this protocol exactly. Do not skip steps.

## Target Scene

Write scene **$ARGUMENTS** (format: chapter.scene — e.g. "4.3" means Chapter 4, Scene 3).

If no argument is provided, read `SCENE_PLAN.md` and identify the next incomplete scene (first `- [ ]` entry).

## Phase 1: Continuity Precheck

**Spawn the `continuity-precheck` agent** using the Task tool. Pass it the target scene number. This agent reads the full manuscript in its own context window and produces a Scene Brief — a compressed document with everything you need for continuity.

Wait for the Scene Brief before proceeding. If the manuscript is empty or very short (fewer than 3 scenes written), skip this phase and proceed to Phase 2.

## Phase 2: Context Loading

Read these files completely. Do not skim. Do not summarize. Read every line:

1. `CLAUDE.md` — Writing philosophy and all protocols
2. `WRITING_RULES.md` — Prose style guide
3. `STORY_RULES.md` — Narrative constraints and conventions
4. `WORLD_RULES.md` — World mechanics and laws
5. `SUPPLEMENTARY_MATERIALS.md` *(if present)* — Additional lore, magic systems, technology, or world-building reference
6. `CHAPTER_OUTLINE.md` — Find the chapter containing the target scene. Read the full chapter context.
7. `SCENE_PLAN.md` — Find and read the specific scene plan entry plus the entries for the scenes immediately before and after it
8. `continuity/CONTINUITY_TRACKER.md` — Current state of all characters, world, timeline
9. `MISC_STORY_NOTES.md` — Recent craft notes and story developments

Then read the **previous 2-3 scenes** from `scenes/` for voice continuity. If writing scene 4.3, read scenes 4.2 and 4.1 (and 3.6 if 4.1 is the first scene in the chapter).

Finally, incorporate the **Scene Brief** from the continuity-precheck agent.

## Phase 3: Pre-Writing Preparation

Before generating any prose, state explicitly in the chat:

- **POV Character**: Who narrates this scene
- **Entry State**: Where they are emotionally and physically when the scene opens
- **Turn**: What shifts, deepens, or disrupts during the scene
- **Landing**: What the reader should feel when the scene ends (which may differ from what the character feels)
- **Key Continuity Points**: Established details that must appear or be honored
- **Active Threads**: Plot threads, subtext, or planted elements that surface here

This is not optional. Writing without this preparation produces generic prose.

## Phase 4: Write the Scene

Write the complete scene following all guidelines in CLAUDE.md and WRITING_RULES.md.

**Create the file directly** at `scenes/scene_X_Y.md` (e.g. `scenes/scene_4_3.md`). Do NOT write the scene into the chat first. Write it to the file.

While writing:
- Inhabit the POV character's consciousness completely
- Anchor in at least three senses (emphasize sound, smell, touch)
- Prioritize subtext over statement — the most important things go unsaid
- Match vocabulary and sentence rhythm to this specific character's voice
- Honor every established detail from CONTINUITY_TRACKER.md and the Scene Brief
- Follow STORY_RULES.md and WORLD_RULES.md absolutely — no exceptions
- Let the emotional architecture from Phase 3 guide the scene's shape

## Phase 5: Present for Review

After writing the scene, tell the user:

> Scene [X].[Y]: [Scene Title] is ready for your review. Read through and let me know if it's good or if you'd like any adjustments.

**STOP HERE. Do not proceed with any file updates, commits, or tracking changes until the user explicitly approves the scene.** If the user requests edits, make them to the scene file and present again. Repeat until approved.
