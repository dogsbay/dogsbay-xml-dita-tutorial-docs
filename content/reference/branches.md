---
title: Branches
description: Compare tutorial branches and inspect completed stages in a separate worktree while keeping your own work.
type: reference
---

# Branches

The tutorial repository is
[dogsbay/dogsbay-xml-dita-tutorial](https://github.com/dogsbay/dogsbay-xml-dita-tutorial).
Stage branches are named `tutorial/NN-slug` and form one chain from an orphan
root; see [How the tutorial works](/start-here/how-the-tutorial-works).

## The branches

| Branch | Stage | Page |
|---|---|---|
| `tutorial/00-setup` | An empty project the tools recognize | [Stage 00](/part-1-topics/stage-00-setup) |
| `tutorial/01-concept` | One concept topic | [Stage 01](/part-1-topics/stage-01-concept) |
| `tutorial/02-first-map` | The first map | [Stage 02](/part-1-topics/stage-02-first-map) |
| `tutorial/03-first-build` | The first HTML deliverable | [Stage 03](/part-1-topics/stage-03-first-build) |
| `tutorial/04-task-and-reference` | A task and a reference topic | [Stage 04](/part-1-topics/stage-04-task-and-reference) |
| `tutorial/05-inline-and-block` | Inline semantics and block elements | [Stage 05](/part-1-topics/stage-05-inline-and-block) |
| `tutorial/06-rich-tasks` | The richer task elements | [Stage 06](/part-1-topics/stage-06-rich-tasks) |
| `tutorial/07-links` | Links between topics | [Stage 07](/part-1-topics/stage-07-links) |
| `tutorial/08-figures` | Figures, images, SVG and an equation | [Stage 08](/part-1-topics/stage-08-figures) |
| `tutorial/09-map-structure` | Hierarchy, sequences, link control and a reltable | [Stage 09](/part-2-maps/stage-09-map-structure) |
| `tutorial/10-keys` | Product keys and indirect links | [Stage 10](/part-2-maps/stage-10-keys) |
| `tutorial/11-reuse` | Conref, conkeyref, ranges and conref push from `shared/` | [Stage 11](/part-2-maps/stage-11-reuse) |
| `tutorial/12-glossary` | Glossary entries, a group, abbreviations and terms by key | [Stage 12](/part-2-maps/stage-12-glossary) |
| `tutorial/13-metadata-and-index` | Prolog metadata, index terms and the metadata policy | [Stage 13](/part-2-maps/stage-13-metadata-and-index) |
| `tutorial/14-conditional-text` | Platform, audience and revision conditions, DITAVAL filters and flags, a map and a deliverable per audience | [Stage 14](/part-3-conditions/stage-14-conditional-text) |
| `tutorial/15-subject-scheme` | A subject scheme for the conditional values | [Stage 15](/part-3-conditions/stage-15-subject-scheme) |
| `tutorial/16-branch-filtering` | Three platform variants of one branch from a single map | [Stage 16](/part-3-conditions/stage-16-branch-filtering) |
| `tutorial/17-key-scopes` | Three guides in one collection under key scopes, and a peer map | [Stage 17](/part-3-conditions/stage-17-key-scopes) |
| `tutorial/18-chunking-and-output` | Chunking, copy-to, print-only topics, topicsets and outputclass | [Stage 18](/part-3-conditions/stage-18-chunking-and-output) |
| `tutorial/19-bookmap` | A bookmap and the PDF book: front matter, parts, chapters, appendixes, glossary and index | [Stage 19](/part-4-books/stage-19-bookmap) |
| `tutorial/20-troubleshooting` | A troubleshooting topic with causes and remedies, and a trouble note | [Stage 20](/part-4-books/stage-20-troubleshooting) |
| `tutorial/21-hazards-and-safety` | Hazard statements with a message panel and a symbol, one reused by conkeyref | [Stage 21](/part-4-books/stage-21-hazards-and-safety) |
| `tutorial/22-software-domains` | The software and programming domains, a syntax diagram, and code pulled in by coderef | [Stage 22](/part-4-books/stage-22-software-domains) |
| `tutorial/23-learning` | A learning assessment, validated against the Learning and Training DTDs | [Stage 23](/part-4-books/stage-23-learning) |
| `tutorial/24-drafts-and-localization` | Language, translate flags, direction and sort keys; draft comments, required cleanup, status and a change history | [Stage 24](/part-5-governance/stage-24-drafts-and-localization) |
| `tutorial/25-house-rules` | The house style as Schematron, named in the project config and run by the check; `AGENTS.md` and a skill; the ten violations resolved | [Stage 25](/part-5-governance/stage-25-house-rules) |
| `tutorial/26-final` | The complete guide: the README's full stage table and deliverable list, every deliverable built | [Stage 26](/part-5-governance/stage-26-final) |

`tutorial/26-final` is also tagged `tutorial/final`, a stable name for the
finished tree. The branch moves when a middle stage is edited and the chain
is rebased; the tag is moved to the new tip by hand once the ladder has been
re-checked, so `git show tutorial/final:README.md` always reads a tree that
passed the checks.

`main` is not part of the chain. It is the finished, deliberately broken demo
project that the DogsBay XML tutorial uses, and it shares no history with the
stage branches.

## Compare two stages

The diff between adjacent branches is the lesson. Locally:

```bash
git diff origin/tutorial/05-inline-and-block origin/tutorial/06-rich-tasks
git diff --stat origin/tutorial/05-inline-and-block origin/tutorial/06-rich-tasks
git diff origin/tutorial/05-inline-and-block origin/tutorial/06-rich-tasks -- topics/installing-audacity.dita
```

On GitHub, the compare URL is the two branch names joined by three dots:

```
https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/<from>...<to>
```

For example,
[compare these stages](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/05-inline-and-block...tutorial/06-rich-tasks).
Non-adjacent branches work too: compare `tutorial/00-setup` to
`tutorial/08-figures` to see all of Part 1 at once, or `tutorial/08-figures`
to `tutorial/13-metadata-and-index` for all of Part 2, or
`tutorial/13-metadata-and-index` to `tutorial/18-chunking-and-output` for
all of Part 3, `tutorial/18-chunking-and-output` to `tutorial/23-learning`
for all of Part 4, or `tutorial/23-learning` to `tutorial/26-final` for all
of Part 5. The whole ladder is
[compare `tutorial/00-setup` to `tutorial/26-final`](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/00-setup...tutorial/26-final).

To read one file as it is on a branch without checking the branch out:

```bash
git show origin/tutorial/08-figures:topics/what-is-digital-audio.dita
git ls-tree -r --name-only origin/tutorial/08-figures
git log --oneline origin/tutorial/08-figures
```

## Inspect a stage and keep your work

To switch the tutorial clone to a completed stage in the editor, click the
branch name in the status bar and choose the stage's
`origin/tutorial/NN-slug` branch. The editor creates a local tracking branch
and switches to it. See
[Move between stages](/start-here/set-up#move-between-stages).

From the command line, you can instead open a completed stage in a separate
directory. Run these commands in the tutorial clone, and choose a directory
that does not already exist:

```bash
git worktree add --detach ../audacity-stage-04 origin/tutorial/06-rich-tasks
cd ../audacity-stage-04
dogsbay-xml check .
```

The worktree contains the reference stage. Your original working directory
retains its files and uncommitted changes. Switching branches with
`git checkout` can carry compatible uncommitted changes across branches;
it does not reset your work.

To start a lesson from the preceding reference stage, create a branch in a
clean clone or worktree:

```bash
git switch -c my-stage-04 origin/tutorial/05-inline-and-block
# ... write the stage ...
git diff origin/tutorial/06-rich-tasks
```

After you switch branches, the "You are on" line near the top of `README.md` names
the stage:

```bash
grep 'You are on' README.md
```

`git diff` compares tracked files. Use `git status --short` as well to find
new files that Git has not yet tracked. Review and commit your lesson files
when you reach a passing checkpoint.

## Editing a middle stage

For anyone maintaining the chain rather than following it: every later stage
must be rebased onto a change to a middle stage, in order, with
`git rebase --onto`, and the ladder re-checked with
`scripts/check-all-stages.sh` on `main`. The tutorial repository's
`docs-dev/stage-branches.md` has the procedure.
