## Overview

This guide is for the actual prose writing phase using the Draft Generation Prompt (Step 4). All planning stages (Concept, Outline, Chapter Planning, Scene Planning) have been completed elsewhere.

## Required Input Files

Before beginning any writing session, ensure these planning files are present in your project:

- `FINAL_NOVEL_CONCEPT.md` - Your finalized novel concept
- `WRITING_RULES.md` - Your established writing rules and style guide
- `STORY_RULES.md` - Your narrative constraints and conventions
- `WORLD_RULES.md` - The fundamental laws governing your story world
- `EXPANDED_OUTLINE.md` - Complete novel outline
- `CHAPTER_OUTLINE.md` - Novel implementation blueprint with chapter-by-chapter breakdowns
- `SCENE_PLAN.md` - Detailed scene-by-scene plan with checklist format
- `SUPPLEMENTARY_MATERIALS.md` *(optional)* - Additional world-building, lore, magic systems, technology, or reference material provided by the user. Common for sci-fi, fantasy, and other genres with deep world mechanics.

The following files are created during Project Setup and accumulate content as you write:

- `manuscript/CURRENT_MANUSCRIPT.md` - Assembled chapters (starts empty)
- `chapters/` - Individual assembled chapter files
- `scenes/` - Individual scene files (archive)
- `wiki/` - Story wiki: compiled, cross-referenced knowledge pages (see Story Wiki section)
- `MISC_STORY_NOTES.md` - For ongoing notes and decisions
- `continuity/CONTINUITY_TRACKER.md` - Established character, world, and timeline details
- `continuity/MODIFICATION_LOG.md` - Documented deviations from plan
- `SCENE_COMPLETION_STATUS.md` - Progress tracking

## Project Setup

### Initial Directory Structure

```bash
# Create project directories
mkdir -p {manuscript,scenes,session-notes,continuity,chapters,wiki}

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

## Creative Writing Philosophy

These principles govern how you approach scene generation. Internalize them — they are not a checklist to reference but a mindset to inhabit. You are not producing content. You are writing a novel.

### Voice Immersion

Before writing any scene, read the previous scene's final paragraphs and let the voice settle into your prose like a musician finding pitch. The novel's voice is not a style applied from outside — it is the way these characters perceive the world. Match vocabulary, sentence rhythm, and sensory emphasis to the POV character's consciousness. Each character should sound distinct — shaped by their history, temperament, and the way they process the world. The voice should feel inevitable, as though no other words were possible.

### Emotional Architecture

Every scene has an emotional shape — a starting feeling, a journey, and a landing. Before drafting, identify:

- **Entry state**: Where is the POV character emotionally when the scene begins?
- **Turn**: What shifts, deepens, or destabilizes during the scene?
- **Landing**: What does the reader feel when the scene ends? This may differ from what the character feels.

The gap between what the character understands and what the reader perceives is where the story's power lives. Design every scene around that gap.

### The Principle of Earned Emotion

The emotional impact of any scene is never arbitrary. Every powerful moment must be earned by the reality that precedes it. The reader's investment in what feels normal — the routines, the small comforts, the familiar rhythms — is what makes disruption devastating. Write the ordinary moments with the same care and specificity you bring to the dramatic ones. If the reader doesn't care about what's at stake, no amount of craft will make the moment land.

### Subtext Over Statement

The most important information in any scene is what goes unsaid. Characters don't name their emotions — they reveal them through action, gesture, and the details they notice. Trust the reader to assemble meaning from behavior, detail, and implication. If you can remove a line of interiority and the scene still communicates, remove it. The unsaid is louder.

### Sensory Primacy

This novel lives in the body. Anchor every scene in at least three senses, with emphasis on the senses most relevant to your genre and setting. The world registers physically before intellectually — characters feel shifts in their environment before they understand them. Let the body know before the mind catches up. Emotion is not a thought. It is sweat on a palm, a tightness behind the sternum, the inability to swallow.

### Restraint as Power

The most powerful moment in any scene is the one you almost didn't write. Resist the impulse to over-explain, over-describe, or resolve tension prematurely. Ambiguity, where the story calls for it, is not a failure of nerve — it is a deliberate choice. The reader's imagination, once activated, will always outperform your description. One carefully chosen detail does more than a paragraph of exposition. Trust the silence.

### Scene-Level Pacing

Within each scene, manipulate rhythm deliberately:

- **Approach**: Longer sentences, grounding detail, lulling specificity. Let the reader settle.
- **Disruption**: A shift — one sentence that doesn't fit the pattern. The reader's engagement changes before they know why.
- **Aftermath**: Short sentences. White space. The character processing what just shifted.

Not every scene follows this pattern. Some scenes are entirely quiet. Their impact is retrospective — the reader realizes later what was being set up. Some scenes begin in the aftermath and never explain what happened. Vary the structure so the reader cannot predict the shape of what's coming.

### Living Continuity

You are not generating isolated scenes — you are extending a living narrative. Every detail you establish becomes load-bearing. If a character puts a mug on the counter in Scene 3.2, that mug exists in Scene 3.3. If a character experienced something painful yesterday, their body carries it today. Track the physical and emotional state of every character across scenes with the fidelity of a novelist. Characters carry their previous scenes in their bodies. Experience accumulates. Fatigue compounds. The world remembers everything, and so must you.

### The Human Core

Above all else: this is a story about people. The plot serves the humanity, never the reverse. If a scene is technically accomplished but emotionally hollow, it has failed. If a scene makes the reader ache for these characters, it has succeeded — even if nothing dramatic happens in it. Write the characters first. The story is what happens to them.

## Story Wiki

The `wiki/` directory is a persistent, compiled knowledge base for your novel. Unlike the raw planning files (which are monolithic documents covering everything), the wiki breaks your story into focused, cross-referenced pages that are easy to look up and keep current.

### Why It Exists

The planning files from BookWeaverAI — particularly the Starter Template and Expanded Outline — can be tens of thousands of words each. When Claude needs to check a character's eye color, a faction's political stance, or when a plot thread was planted, it has to scan entire documents. The wiki compiles this knowledge once and keeps it organized, so lookups are fast and nothing gets lost as the manuscript grows.

The wiki works alongside `CONTINUITY_TRACKER.md`, not as a replacement. The tracker captures the *current state* of the story (where characters are now, what they know now). The wiki captures *permanent reference knowledge* (who characters are, how the world works, where plot threads connect).

### Wiki Structure

```
wiki/
├── characters/
│   ├── [character-name].md          # One page per major character
│   └── ...
├── locations/
│   ├── [location-name].md           # One page per significant location
│   └── ...
├── plot-threads/
│   ├── [thread-name].md             # One page per major plot thread
│   └── ...
├── world/
│   ├── [system-or-concept].md       # Magic systems, tech, politics, culture
│   └── ...
├── themes.md                        # Core themes and how they manifest
└── timeline.md                      # Chronological event sequence
```

### Character Page Format

```markdown
# [Character Name]

## Identity
- **Role**: [protagonist / antagonist / supporting]
- **Age**: [age]
- **Physical**: [key distinguishing features]
- **Speech patterns**: [voice notes — dialect, vocabulary level, verbal habits]

## Arc
- **Starting state**: [who they are at the story's beginning]
- **Core want / core need**: [external goal / internal need]
- **Key turning points**: [chapter/scene references]
- **Ending state**: [who they become]

## Relationships
- **[Character B]**: [nature of relationship, how it evolves]
- **[Character C]**: [nature of relationship, how it evolves]

## Plot Threads
- Connected to: [thread-name], [thread-name]

## Notes
- [Anything else relevant — secrets, knowledge gaps, symbolic associations]
```

### Building the Initial Wiki

After setting up the project with your BookWeaverAI exports, build the wiki before writing your first scene:

```
Read all planning files (FINAL_NOVEL_CONCEPT.md, STARTER_TEMPLATE.md, EXPANDED_OUTLINE.md,
CHAPTER_OUTLINE.md, SCENE_PLAN.md, STORY_RULES.md, WORLD_RULES.md, WRITING_RULES.md, and
SUPPLEMENTARY_MATERIALS.md if present).

Build the story wiki in the wiki/ directory:
- One page per major character in wiki/characters/
- One page per significant location in wiki/locations/
- One page per major plot thread in wiki/plot-threads/
- World mechanics pages in wiki/world/ (one per system — magic, politics, technology, etc.)
- wiki/themes.md covering core themes and how they manifest
- wiki/timeline.md with the chronological event sequence

Cross-reference between pages. Use the character and location page formats from CLAUDE.md.
After creating all pages, list what you built.
```

### Maintaining the Wiki

The wiki is updated as part of the post-scene workflow. After each scene is approved, the **file-updater** agent should:

1. Check if any wiki pages need updating based on what happened in the scene
2. Update character pages if relationships shifted, new details were established, or arc milestones were reached
3. Update location pages if new details about a setting were established
4. Update plot thread pages if a thread advanced, a new one was introduced, or one was resolved
5. Update the timeline with any new events
6. Add cross-references where new connections were established

Wiki updates are incremental — only touch what changed. The goal is compiled knowledge that stays current without manual effort.

### Using the Wiki During Writing

The **continuity-precheck** agent should read relevant wiki pages alongside the manuscript when producing the Scene Brief. For a scene featuring Character A at Location X involving Plot Thread Y, the agent pulls those specific wiki pages rather than scanning the full planning files.

This keeps the writing context focused: instead of loading 60K tokens of raw planning output, Claude gets the specific compiled pages it needs for the scene at hand.

## Writing Process

### Before Each Writing Session

1. **Create Session Branch**

```bash
git checkout -b writing-[DATE]-[SCENE-NUMBERS]
```

2. **Review Current Status**

- Check `SCENE_PLAN.md` for next incomplete scene (marked with `- [ ]`)
- Review `manuscript/CURRENT_MANUSCRIPT.md` and any unassembled scenes in `scenes/` to understand story state
- Check `continuity/CONTINUITY_TRACKER.md` for established details

3. **Prepare Context for Claude** Load all required files into Claude:

- FINAL_NOVEL_CONCEPT.md
- WRITING_RULES.md
- STORY_RULES.md
- WORLD_RULES.md
- EXPANDED_OUTLINE.md
- CHAPTER_OUTLINE.md
- SCENE_PLAN.md
- SUPPLEMENTARY_MATERIALS.md (if present)
- manuscript/CURRENT_MANUSCRIPT.md
- MISC_STORY_NOTES.md

### During Writing Session

1. **For Each Scene**

    - Use the CLAUDE.md exactly as provided
    - Read the previous scene if there is one, as well as all the context files (md files) in the project directory to make sure you are staying as consistent as possible with what has happened in the story, where the story is now, and where the story is going.
    - Claude will identify the next incomplete scene from SCENE_PLAN.md
    - Let Claude generate the complete scene text
2. **After Claude Generates Each Scene**

    - Save the scene text to: `scenes/scene_[X]_[Y].md`
    - Update `SCENE_PLAN.md`: Change `- [ ]` to `- [x]` for completed scene
3. **Update Tracking Files Based on Claude's Output**

    From **Craft Notes** section, update:

    - `MISC_STORY_NOTES.md` with character development insights
    - `continuity/CONTINUITY_TRACKER.md` with world-building details

    From **Consistency Tracking** section, update:

    - `continuity/MODIFICATION_LOG.md` with any story modifications
    - `continuity/CONTINUITY_TRACKER.md` with continuity checkpoints

    From **Story Modifications** section, update:

    - `continuity/MODIFICATION_LOG.md` with changes and rationale
    - Consider updating `EXPANDED_OUTLINE.md` if significant changes

    **Wiki Updates** — check and update relevant pages in `wiki/`:

    - `wiki/characters/` — update character pages if relationships changed, new details established, or arc milestones reached
    - `wiki/locations/` — update location pages if new setting details were described
    - `wiki/plot-threads/` — update thread pages if a thread advanced, was introduced, or resolved
    - `wiki/timeline.md` — add any significant events with scene references
    - Add cross-references between pages where new connections were established

### After Each Scene

1. **Commit Your Work**

```bash
git add .
git commit -m "Completed Scene [X].[Y]: [Scene Title]
- Character moments: [brief summary]
- Plot advancement: [what moved forward]
- Modifications: [any changes from plan]"
```

2. **Update Scene Completion Status** Update `SCENE_COMPLETION_STATUS.md`:

```markdown
## Chapter [X]
- [x] Scene [X].[Y]: [Scene Title] - [DATE] - [Word Count]
```

## File Update Checklists

### After Each Scene (`/project:post-scene`)

- [ ] `scenes/scene_[X]_[Y].md` - Individual scene saved
- [ ] `SCENE_PLAN.md` - Scene marked complete
- [ ] `MISC_STORY_NOTES.md` - Craft notes added
- [ ] `continuity/CONTINUITY_TRACKER.md` - New details tracked
- [ ] `continuity/MODIFICATION_LOG.md` - Changes documented (if deviations)
- [ ] `wiki/` - Relevant wiki pages updated (characters, locations, plot threads, timeline)
- [ ] `SCENE_COMPLETION_STATUS.md` - Progress updated
- [ ] Git commit completed

### After Each Chapter (`/project:build-chapter X`)

- [ ] All scenes in chapter marked `- [x]` in `SCENE_PLAN.md`
- [ ] `chapters/chapter_X.md` - Chapter file assembled from scenes
- [ ] `manuscript/CURRENT_MANUSCRIPT.md` - Chapter appended to manuscript
- [ ] Transition review completed (flags addressed or noted)
- [ ] Git commit completed

## Continuity Tracking Structure

### CONTINUITY_TRACKER.md Format

```markdown
# Continuity Tracker

## Character Details
### [Character Name]
- Physical: [established details]
- Possessions: [items they have]
- Knowledge: [what they know at this point]
- Relationships: [current status with others]
- Location: [where they are]

## World Details
### Locations
- [Location Name]: [established details]

### Timeline
- [Event]: [When it happened relative to story]

## Important Objects
- [Object]: [where it is, who has it]

## Planted Elements
- [Element]: [Where planted] → [Needs payoff in: Scene X]
```

### MODIFICATION_LOG.md Format

```markdown
# Story Modifications Log

## [DATE] - Scene [X].[Y]
### Change: [What was changed]
**Original Plan:** [From outline/scene plan]
**Actual Implementation:** [What was written]
**Rationale:** [Why the change improves the story]
**Impact on Future Scenes:** [What needs adjustment]
```

## Daily Writing Workflow

### START OF SESSION PROTOCOL

**CONTEXT PREPARATION PHASE:**
1. **Read SCENE_PLAN.md completely** to identify next incomplete scene (marked `- [ ]`)
2. **Review CONTINUITY_TRACKER.md** for current character locations, emotional states, timeline
3. **Check MISC_STORY_NOTES.md** for recent craft notes and story developments
4. **Read WRITING_RULES.md**, **STORY_RULES.md**, **WORLD_RULES.md**, **FINAL_NOVEL_CONCEPT.md**, **CHAPTER_OUTLINE.md**, and **SUPPLEMENTARY_MATERIALS.md** (if present) for voice/tone consistency, world/story constraints, supplementary lore/world-building, and chapter-level context
5. **Read relevant wiki pages** — identify which characters, locations, and plot threads are involved in the upcoming scene(s), then read their wiki pages in `wiki/characters/`, `wiki/locations/`, `wiki/plot-threads/`, and `wiki/world/` for compiled reference knowledge
6. **Read manuscript/CURRENT_MANUSCRIPT.md** and any unassembled scenes in `scenes/` — absorb the complete narrative state before writing
7. **Create git branch**: `git checkout -b writing-[DATE]-[SCENE-NUMBERS]`

### SCENE WRITING PROTOCOL

**MANDATORY SEQUENCE (NEVER SKIP STEPS):**
1. **Identify next scene** from SCENE_PLAN.md (first `- [ ]` incomplete scene)
2. **Pre-writing preparation** — before generating any prose:
   - Read the scene plan entry and the corresponding chapter context from CHAPTER_OUTLINE.md
   - Read the wiki pages for the POV character, any other characters in the scene, the scene's location, and any active plot threads — use these as compiled reference rather than re-scanning full planning files
   - Identify the scene's emotional architecture: entry state, turn, and landing
   - Note the POV character and settle into their voice by re-reading their most recent scene and their wiki character page (speech patterns, voice notes)
   - Identify what the reader should know, feel, and fear by the scene's end
   - Check CONTINUITY_TRACKER.md for the POV character's current physical location, emotional state, and knowledge
3. **Write scene** following all WRITING_RULES.md guidelines AND explicitly adhering to STORY_RULES.md and WORLD_RULES.md:
   - Take into account story past, present, and future from all context files
   - Maintain continuity with established details from CONTINUITY_TRACKER.md
   - Follow the voice and atmospheric requirements established in WRITING_RULES.md
   - Follow POV rules as defined in WRITING_RULES.md and STORY_RULES.md — respect each character's designated POV mode and never violate POV discipline
   - Inhabit the POV character's consciousness — filter all description, vocabulary, and rhythm through their specific way of perceiving the world
   - Prioritize subtext over statement; trust behavior, sensory detail, and implication to carry meaning
   - Anchor every scene in at least three senses, emphasizing sound, smell, and touch
   - Write the scene's warmth with as much precision as its dread — tenderness raises the stakes
   - Ensure all story conventions from STORY_RULES.md are followed
   - Ensure all world mechanics from WORLD_RULES.md are respected
   - Incorporate any relevant details from SUPPLEMENTARY_MATERIALS.md (if present) — lore, magic systems, technology, etc.
   - Create the scene in the scenes directory. See step 4 below. Do not create the scene in the claude window for review first.
4. **Create scene file** as `/scenes/scene_[X]_[Y].md`
5. **SHOW SCENE TO USER** and ask: "Please review and let me know if it's good or if you'd like me to edit anything"
6. **WAIT FOR USER APPROVAL** - do not proceed until confirmed good
7. **After approval, run `/project:post-scene`** — this handles all tracking updates:
   - Update `CONTINUITY_TRACKER.md` with new character positions/emotional states/timeline
   - Update `MISC_STORY_NOTES.md` with comprehensive craft analysis
   - Update relevant `wiki/` pages — character pages if relationships shifted or new details were established, location pages if setting details were added, plot thread pages if threads advanced or resolved, `wiki/timeline.md` if significant events occurred
   - Mark scene complete in `SCENE_PLAN.md` (change `- [ ]` to `- [x]`)
   - Update `SCENE_COMPLETION_STATUS.md`
   - Git commit with scene details
   - **Note**: Scenes are NOT appended to the manuscript individually. When all scenes in a chapter are complete, run `/project:build-chapter X` to assemble the chapter.
8. **If all chapter scenes are complete**, run `/project:build-chapter X` to assemble and append the chapter to the manuscript.

### CHAPTER ASSEMBLY PROTOCOL

When all scenes in a chapter are complete (all marked `- [x]` in SCENE_PLAN.md), the chapter is ready for assembly.

**Workflow**: `scenes → chapter file → manuscript`

1. **Run `/project:build-chapter X`** — validates completeness and spawns the chapter-builder agent
2. **Review the assembled chapter** at `chapters/chapter_X.md`
   - Check transitions between scenes for jarring shifts
   - Review any flags from the chapter-builder agent
   - Request edits if needed
3. **Approve** — the chapter is appended to `manuscript/CURRENT_MANUSCRIPT.md`
4. **Git commit** with chapter details (scenes included, word count)

**Manuscript format** — chapters are separated by `---`:

```markdown
# Chapter 1: [Title]

[scene 1 text]

* * *

[scene 2 text]

---

# Chapter 2: [Title]

[scene 1 text]

* * *

[scene 2 text]
```

**Key rules**:
- Individual scenes are NEVER appended directly to the manuscript
- Scene files in `scenes/` are kept as an archive (never deleted)
- Chapter files in `chapters/` are the intermediate assembly step
- The manuscript only grows by whole chapters

### END OF SESSION PROTOCOL

Run `/project:end-session` to automate all wrap-up tasks:
1. Checks for uncommitted work
2. Creates session summary in `session-notes/`
3. Merges to draft-v1 branch
4. Pushes to remote if configured
5. Reports final progress stats

### CRITICAL REMINDERS FOR CLAUDE

- **NEVER proceed without user scene approval**
- **ALWAYS follow the exact sequence** - do not skip or reorder steps
- **READ EVERYTHING**: Read all context files in full before writing. No shortcuts, no summaries, no skimming.
- **CONTEXT IS KING**: All context files inform story continuity and consistency
- **YOU ARE A NOVELIST, NOT A GENERATOR**: Every sentence must feel chosen, not produced. If a line could appear in any novel, it doesn't belong in this one.
- **PERFECT EXECUTION MATTERS** — every detail, every continuity point, every voice note

## Git Best Practices for Writing

### Branching Strategy

```bash
# Main branches
main              # Published/final draft
draft-v1         # Current working draft
writing-sessions # Daily writing branches

# Feature branches for revisions
revision/character-arc-fix
revision/pacing-adjustment
```

### Useful Git Commands

```bash
# See what scenes you wrote today
git log --oneline --since="1 day ago"

# Find when you wrote a specific scene
git log --grep="Scene X.Y"

# Revert a scene if needed
git revert [commit-hash]

# See word count growth
git diff --stat [earlier-commit] manuscript/CURRENT_MANUSCRIPT.md
```

## Handling Common Situations

### If Claude Identifies Inconsistencies

1. Note the inconsistency in `continuity/MODIFICATION_LOG.md`
2. Decide whether to fix now or in revision
3. If fixing now, update previous scenes and document changes

### If You Need to Skip a Scene

1. Mark it in `SCENE_PLAN.md` as `- [~] Scene X.Y: [Title] - SKIPPED`
2. Add note in `MISC_STORY_NOTES.md` about why and what's needed
3. Continue with next scene

### If a Scene Grows Too Long

1. Let Claude complete it naturally
2. Note in `continuity/MODIFICATION_LOG.md` that it expanded
3. Consider splitting in revision phase

## Progress Monitoring

### Create WRITING_PROGRESS.md

```markdown
# Writing Progress

## Daily Progress
| Date | Scenes Completed | Words Written | Total Words |
|------|-----------------|---------------|-------------|
| YYYY-MM-DD | X.Y, X.Z | XXXX | XXXXX |

## Chapter Progress
- Chapter 1: [X/Y scenes] - [STATUS]
- Chapter 2: [X/Y scenes] - [STATUS]
```

## Remember

- Trust Claude to maintain consistency by providing all context files
- The Draft Generation Prompt is designed to handle all aspects
- Document everything - your future self will thank you
- Commit frequently - every scene is a milestone
- Focus on forward progress - revision comes later
