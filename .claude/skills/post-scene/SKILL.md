---
name: post-scene
description: After scene approval, update all tracking files, mark complete, and commit. Run this after you approve a scene.
allowed-tools: Read, Write, Edit, Glob, Grep, Bash, Task
---

The user has approved the most recently written scene. Execute all post-scene updates following the CLAUDE.md protocol. Do every step in order. Do not skip any.

## Step 1: Identify the Scene

Determine which scene was just written. Check conversation context or find the most recently modified file in `scenes/`. Note the scene number (X.Y) and title.

## Step 2: Spawn File Updater Agent

**Spawn the `file-updater` agent** using the Task tool. Pass it:
- The scene number (X.Y)
- The scene file path

The agent handles all mechanical updates in its own context:
- Updating continuity tracker
- Updating story notes
- Updating modification log (if deviations exist)
- Marking the scene complete in scene plan
- Updating scene completion status
- Checking chapter progress

Wait for the agent to complete.

## Step 3: Verify Updates

After the agent finishes, do a quick verification:
- Confirm `SCENE_PLAN.md` shows the scene as `- [x]`
- Confirm `continuity/CONTINUITY_TRACKER.md` was updated with new details
- Confirm `SCENE_COMPLETION_STATUS.md` was updated

If anything was missed, fix it directly.

## Step 4: Check Chapter Completion

Read `SCENE_PLAN.md` and check whether ALL scenes in this scene's chapter are now marked `- [x]`.

## Step 5: Report

Tell the user:
- Which files were updated
- Current progress: scenes completed / total scenes
- **Chapter [X] progress: [Y/Z scenes complete]**
- Next scene in the queue (first remaining `- [ ]` in SCENE_PLAN.md)
- Any continuity notes or concerns that emerged during the update

**If all scenes in the chapter are now complete**, add:

> All scenes for Chapter [X] are complete. Run `/project:build-chapter [X]` to assemble the chapter and add it to the manuscript.
