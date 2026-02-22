---
name: build-chapter
description: Assemble all completed scenes for a chapter into a formatted chapter file, review it, and append to the manuscript. Usage: /project:build-chapter 4
allowed-tools: Read, Write, Edit, Glob, Grep, Bash, Task
---

You are assembling a completed chapter from its constituent scenes. Follow this protocol exactly.

## Target Chapter

Assemble chapter **$ARGUMENTS** (a chapter number, e.g. "4").

If no argument is provided, check `SCENE_PLAN.md` and `SCENE_COMPLETION_STATUS.md` to identify the most recent chapter where all scenes are complete but the chapter has not yet been assembled.

## Step 1: Validate Chapter Completeness

Read `SCENE_PLAN.md` and find all scenes for Chapter $ARGUMENTS.

**Every scene in this chapter must be marked `- [x]` (complete).** If any scene is still `- [ ]`, halt immediately and tell the user:

> Chapter [X] is not ready for assembly. The following scenes are incomplete:
> - Scene [X].[Y]: [Title]
> - Scene [X].[Z]: [Title]
>
> Complete these scenes first with `/project:write-scene [X].[Y]`.

Do not proceed if any scene is incomplete.

## Step 2: Identify Scene Files

List all scene files for this chapter from `scenes/` directory (e.g. `scenes/scene_4_1.md`, `scenes/scene_4_2.md`, etc.). Verify each file exists.

## Step 3: Check for Existing Chapter

Check if `chapters/chapter_X.md` already exists. If it does, warn the user:

> Chapter [X] has already been assembled at `chapters/chapter_X.md`. Proceeding will overwrite it. Continue? (y/n)

Wait for confirmation before proceeding.

## Step 4: Spawn Chapter Builder Agent

**Spawn the `chapter-builder` agent** using the Task tool. Pass it:
- The chapter number
- The list of scene file paths in order

Wait for the agent to complete.

## Step 5: Present for Review

Show the user:
- The assembled chapter file path
- Chapter word count
- Number of scenes included
- Any transition flags from the chapter-builder agent

Then say:

> Chapter [X]: [Title] is ready for your review. Read through `chapters/chapter_X.md` and let me know if it's good to append to the manuscript, or if you'd like adjustments.

**STOP HERE. Wait for user approval before proceeding.**

If the user requests edits, make them to `chapters/chapter_X.md` and present again.

## Step 6: Append to Manuscript (After Approval)

Append the chapter to `manuscript/CURRENT_MANUSCRIPT.md` using this format:

- If the manuscript is NOT empty, add a `---` separator before the chapter
- Then add the full chapter content (including the `# Chapter X: [Title]` header)

```markdown

---

# Chapter X: [Title]

[full chapter content]
```

If the manuscript IS empty (first chapter), omit the leading `---`:

```markdown
# Chapter X: [Title]

[full chapter content]
```

## Step 7: Git Commit

Stage and commit:

```bash
git add chapters/chapter_X.md manuscript/CURRENT_MANUSCRIPT.md
```

Commit message:
```
Assembled Chapter X: [Title]
- Scenes: [X.1, X.2, X.3, ...]
- Chapter word count: [count]
- Total manuscript word count: [count]
```

Do NOT push to remote.

## Step 8: Report

Tell the user:
- Chapter word count
- Total manuscript word count (all chapters so far)
- Scenes included in this chapter
- Next chapter status: how many scenes are complete vs. total for the next chapter
- If the next chapter is ready for assembly, suggest: "Chapter [X+1] is also ready — run `/project:build-chapter [X+1]` to assemble it."
