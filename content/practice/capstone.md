---
title: Add an interview workflow
description: Extend the guide with an interview-recording workflow and verify beginner and podcaster deliverables using the core DITA skills.
type: tutorial
---

# Add an interview workflow

Extend the guide for a reader who wants to record and review an interview.
Plan the content, implement it, and verify two audience variants. Use the
requirements below without copying an existing topic in full.

**Time:** about 60 to 90 minutes.
**You need:** the core course complete, including stage 25. Start from your
completed work or `origin/tutorial/25-house-rules` in a practice worktree.

## Requirements

1. Identify the reader's goal, prerequisites, and successful result. Plan
   a focused task and any supporting concept or reference that it needs.
2. Add an interview task with a descriptive title, short description,
   prerequisites, steps, and result. Use product keys and semantic UI markup.
3. Reference existing background or format topics where they meet the need.
   Reuse an existing shared step only if its meaning is appropriate here.
4. Add the task to the beginner and podcaster maps. Define every key the
   topic uses in both publication contexts.
5. Include one beginner explanation and one podcaster note with the
   controlled audience values. Explain your choice of content boundaries.
6. Include the required metadata and satisfy the existing house rules.
7. Build the deliverables in `project.json`. Inspect the new page in
   `out/beginner-windows/` and `out/podcaster-linux/`, including its links,
   resolved keys, reused content, and audience-specific text.

Use placeholder interview details that do not include personal information.
The exercise tests documentation design and publishing; you do not need to
make a real recording.

## Acceptance checks

| Check | Passing result |
|---|---|
| Content plan | Each topic has a distinct reader purpose |
| DTD and house rules | The new content validates and the check reports `Ready` |
| Map integration | Both intended guides expose the new task in navigation |
| Key and reuse resolution | No empty product values, missing steps, or unresolved links |
| Filtering | Each audience sees its intended explanation or note |
| Output review | Links reach the correct page or fragment; markup renders as intended |
| Accessibility | Link text, reading order, and any image or table alternatives are reviewed |
| Change review | The diff contains only the intended additions and required map changes |

Use the [automated checks](/reference/verification) alongside manual output
inspection. Record the commands, output directories, and any limitations
you found. If the check reports `Ready` but output is wrong, investigate the affected
map and transform before accepting the result.

The reference guide checks clean (see
[Known output issues](/reference/known-output-issues)), so any failure
that the check reports in your project comes from your changes. Your new
topic and its links must pass the acceptance checks, and the check must
report `Ready` before you hand off your work.

## Explain your design

Write a short handoff note describing one topic-boundary decision, one reuse
decision, and how you verified filtering. Include any tradeoff you would
revisit if the guide grew. Finish with [Stage 26](/part-5-governance/stage-26-final)
to record and tag your completed work.
