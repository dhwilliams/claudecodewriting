---
name: continuity-audit
description: Deep continuity audit across the manuscript. Run every 5-10 scenes or at act breaks to catch inconsistencies, timeline issues, and drift. No arguments needed.
allowed-tools: Read, Glob, Grep, Task
---

Perform a comprehensive continuity audit of the novel manuscript.

## Step 1: Spawn Auditor Agent

**Spawn the `manuscript-auditor` agent** using the Task tool. This agent reads the full manuscript and all tracking files in its own context window, then produces a detailed audit report.

Wait for the agent to complete and return its findings.

## Step 2: Present Findings

Present the audit results to the user, organized by severity:

### Critical Issues
Contradictions or errors that would break reader immersion. These need fixing before writing more scenes.

### Minor Issues
Slight inconsistencies or drift that should be noted but won't derail the story. Good candidates for revision phase.

### Planted Elements Status
- Elements set up and paid off (confirmed)
- Elements set up and still open (track these)
- Elements that may have been forgotten (flag for attention)

### Tracker Accuracy
Discrepancies between what `CONTINUITY_TRACKER.md` says and what the manuscript actually contains. The tracker must stay accurate — it's what future scenes rely on.

### Recommendations
- Specific updates to tracking files
- Scenes that may need revision
- Patterns to watch for going forward

## Step 3: Fix Discrepancies

If the audit found discrepancies, ask the user:

> The audit found [N] discrepancies. Would you like me to fix them now?

If approved, make corrections. **Critical rule: any text fix must be applied across ALL locations where that text exists.** The same prose may live in up to three places:

1. `scenes/scene_X_Y.md` — the scene archive file
2. `chapters/chapter_X.md` — the assembled chapter file (if the chapter has been built)
3. `manuscript/CURRENT_MANUSCRIPT.md` — the full manuscript (if the chapter has been appended)

When fixing a text discrepancy:
- Use Grep to find the text in all three locations
- Apply the fix to every file where it appears
- Verify all copies match after the fix
- If only the tracker or scene-number references need correction (not prose text), those only exist in their respective files

Similarly, if fixing `CONTINUITY_TRACKER.md` data (scene numbers, details, etc.), verify the correction against the actual manuscript text before committing.
