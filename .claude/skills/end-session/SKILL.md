---
name: end-session
description: End your writing session. Creates session notes, merges to draft branch, and pushes if configured. Run this when you're done writing for the day.
allowed-tools: Read, Write, Edit, Glob, Grep, Bash, Task
---

The user is ending their writing session. Handle all wrap-up tasks.

## Step 1: Check for Uncommitted Work

Run `git status` to check for any uncommitted changes. If there are uncommitted scene files or tracking updates, warn the user:

> There are uncommitted changes. Would you like me to commit them before closing the session?

Wait for confirmation. If yes, stage and commit with an appropriate message. If no, proceed.

## Step 2: Spawn Session Closer Agent

**Spawn the `session-closer` agent** using the Task tool. Pass it:
- Today's date
- The current git branch name (from `git branch --show-current`)

The agent will:
- Identify all scenes written today
- Create a session summary at `session-notes/[DATE]-SCENES_[RANGE].md`
- Calculate progress stats

Wait for the agent to complete.

## Step 3: Commit Session Notes

Stage and commit the session notes file:

```bash
git add session-notes/
git commit -m "Session notes: [DATE] — Scenes [range]"
```

## Step 4: Merge to Draft Branch

Check if `draft-v1` branch exists:

```bash
git branch --list draft-v1
```

**If draft-v1 exists:**
1. Get the current branch name
2. Switch to draft-v1: `git checkout draft-v1`
3. Merge the writing branch: `git merge [current-branch]`
4. Switch back: `git checkout [current-branch]`

**If draft-v1 does NOT exist:**
Tell the user:

> The `draft-v1` branch doesn't exist. Would you like me to create it from the current state?

If yes, create it: `git branch draft-v1`
If no, skip the merge step.

## Step 5: Push to Remote (if configured)

Check if a remote is configured:

```bash
git remote -v
```

**If a remote exists:**
1. Push the current writing branch: `git push -u origin [current-branch]`
2. Push draft-v1: `git push origin draft-v1`

**If no remote:** Skip silently.

## Step 6: Final Report

Tell the user:
- Scenes completed today (count and list)
- Words written today
- Total manuscript progress (scenes completed / total, percentage)
- Session notes file path
- Git operations completed (branch merged, pushed, etc.)
- Next scene in the queue for tomorrow
- Any concerns or flags from the session closer agent
