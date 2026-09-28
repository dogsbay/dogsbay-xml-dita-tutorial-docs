---
title: Choose a learning path
description: Build your first guide in stages 00 to 03, then choose the core course or optional DITA modules.
type: explanation
---

# Choose a learning path

Start with a topic, a map, and a working HTML build. Keep publishing as you
learn: every authoring lesson after stage 03 adds to a buildable guide.

## Core course

| Module | Lessons | Result |
|---|---|---|
| Plan and publish | Plan your topics, stages 00 to 03 | A content plan, concept topic, map, and HTML output |
| Write topics | Stages 04 to 07 | Tasks, references, semantic markup, and working links |
| Organize and reuse | Stages 09 to 11 | Map hierarchy, keys, and shared content |
| Publish variants | Stages 14 and 15 | Audience and platform variants with controlled values |
| Check and complete | Stage 25, accessibility review, capstone, stage 26 | Reviewed output and a completed project |

Stage 11 introduces pull reuse with `conref` and `conkeyref`. Its range and
push examples are optional. Use the stage 13 checkpoint before continuing
with stage 14 if you skip those examples.

## Continue across a skipped stage

Reference branches form one cumulative sequence. Skipping a lesson means
using its completed source before starting the next lesson. Your authored
work stays in its existing directory.

| Next core lesson | Starting checkpoint |
|---|---|
| Stage 09 after skipping figures | `origin/tutorial/08-figures` |
| Stage 14 after skipping glossary and metadata | `origin/tutorial/13-metadata-and-index` |
| Stage 25 after skipping specialized modules | `origin/tutorial/24-drafts-and-localization` |

From the tutorial clone, create a practice branch and worktree, for example:

```bash
git worktree add -b practice-conditions ../audacity-conditions origin/tutorial/13-metadata-and-index
cd ../audacity-conditions
scripts/check-stage.sh
```

Use a new branch and directory name if either already exists. The checkpoint
includes the skipped examples so later diffs apply cleanly.

## Optional modules

| Module | Lessons | Starting checkpoint |
|---|---|---|
| Figures and equations | Stage 08 | `origin/tutorial/07-links` |
| Glossary, metadata, and index | Stages 12 and 13 | `origin/tutorial/11-reuse` |
| Branch filtering, key scopes, and chunking | Stages 16 to 18 | `origin/tutorial/15-subject-scheme` |
| PDF books | Stage 19 | `origin/tutorial/18-chunking-and-output` |
| Troubleshooting and hazards | Stages 20 and 21 | `origin/tutorial/19-bookmap` |
| Software documentation | Stage 22 | `origin/tutorial/21-hazards-and-safety` |
| Learning and Training | Stage 23 | `origin/tutorial/22-software-domains` |
| Review and translation | Stage 24 | `origin/tutorial/23-learning` |

To author every example, follow stages 00 to 26 in order. Complete the
[practice exercises](/practice/exercises) after each core module and the
[capstone](/practice/capstone) before tagging your finished guide.
