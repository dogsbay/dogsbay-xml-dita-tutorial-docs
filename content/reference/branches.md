---
title: Branches
description: The stage branches, how to compare any two of them, and how to reset your clone to a stage.
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
| `tutorial/00-setup` | An empty project the tools recognise | [Stage 00](/part-1-topics/stage-00-setup) |
| `tutorial/01-concept` | One concept topic | [Stage 01](/part-1-topics/stage-01-concept) |
| `tutorial/02-task-and-reference` | A task and a reference topic | [Stage 02](/part-1-topics/stage-02-task-and-reference) |
| `tutorial/03-inline-and-block` | Inline semantics and block elements | [Stage 03](/part-1-topics/stage-03-inline-and-block) |
| `tutorial/04-rich-tasks` | The richer task elements | [Stage 04](/part-1-topics/stage-04-rich-tasks) |
| `tutorial/05-links` | Links between topics | [Stage 05](/part-1-topics/stage-05-links) |
| `tutorial/06-figures` | Figures, images, SVG and an equation | [Stage 06](/part-1-topics/stage-06-figures) |
| `tutorial/07-first-map` | The first map, `project.json` and the first build | [Stage 07](/part-2-maps/stage-07-first-map) |
| `tutorial/08-map-structure` | Hierarchy, sequences, link control and a reltable | [Stage 08](/part-2-maps/stage-08-map-structure) |
| `tutorial/09-keys` | Product keys and indirect links | [Stage 09](/part-2-maps/stage-09-keys) |
| `tutorial/10-reuse` | Conref, conkeyref, ranges and conref push from `shared/` | [Stage 10](/part-2-maps/stage-10-reuse) |
| `tutorial/11-glossary` | Glossary entries, a group, abbreviations and terms by key | [Stage 11](/part-2-maps/stage-11-glossary) |
| `tutorial/12-metadata-and-index` | Prolog metadata, index terms and the metadata policy | [Stage 12](/part-2-maps/stage-12-metadata-and-index) |
| `tutorial/13-conditional-text` | Platform, audience and revision conditions, DITAVAL filters and flags, a map and a deliverable per audience | [Stage 13](/part-3-conditions/stage-13-conditional-text) |
| `tutorial/14-subject-scheme` | A subject scheme for the conditional values, checked by the gate | [Stage 14](/part-3-conditions/stage-14-subject-scheme) |
| `tutorial/15-branch-filtering` | Three platform variants of one branch from a single map | [Stage 15](/part-3-conditions/stage-15-branch-filtering) |
| `tutorial/16-key-scopes` | Three guides in one collection under key scopes, and a peer map | [Stage 16](/part-3-conditions/stage-16-key-scopes) |
| `tutorial/17-chunking-and-output` | Chunking, copy-to, print-only topics, topicsets and outputclass | [Stage 17](/part-3-conditions/stage-17-chunking-and-output) |
| `tutorial/18-bookmap` | A bookmap and the PDF book: front matter, parts, chapters, appendices, glossary and index | [Stage 18](/part-4-books/stage-18-bookmap) |
| `tutorial/19-troubleshooting` | A troubleshooting topic with causes and remedies, and a trouble note | [Stage 19](/part-4-books/stage-19-troubleshooting) |
| `tutorial/20-hazards-and-safety` | Hazard statements with a message panel and a symbol, one reused by conkeyref | [Stage 20](/part-4-books/stage-20-hazards-and-safety) |
| `tutorial/21-software-domains` | The software and programming domains, a syntax diagram, and code pulled in by coderef | [Stage 21](/part-4-books/stage-21-software-domains) |
| `tutorial/22-learning` | A learning assessment, and the gate taught about the Learning and Training DTDs | [Stage 22](/part-4-books/stage-22-learning) |
| `tutorial/23-drafts-and-localization` | Language, translate flags, direction and sort keys; draft comments, required cleanup, status and a change history | [Stage 23](/part-5-governance/stage-23-drafts-and-localization) |
| `tutorial/24-house-rules` | The house style as Schematron, named in the project config and run by the gate; `AGENTS.md` and a skill; the ten violations resolved | [Stage 24](/part-5-governance/stage-24-house-rules) |
| `tutorial/25-final` | The complete guide: the README's full stage table and deliverable list, every deliverable built | [Stage 25](/part-5-governance/stage-25-final) |

`tutorial/25-final` is also tagged `tutorial/final`, a stable name for the
finished tree. The branch moves when a middle stage is edited and the chain
is rebased; the tag is moved to the new tip by hand once the ladder has been
re-checked, so `git show tutorial/final:README.md` always reads a tree that
passed the gate.

`main` is not part of the chain. It is the finished, deliberately broken demo
project that the DogsBay XML tutorial uses, and it shares no history with the
stage branches.

## Compare two stages

The diff between adjacent branches is the lesson. Locally:

```bash
git diff tutorial/03-inline-and-block tutorial/04-rich-tasks
git diff --stat tutorial/03-inline-and-block tutorial/04-rich-tasks     # files only
git diff tutorial/03-inline-and-block tutorial/04-rich-tasks -- topics/installing-audacity.dita
```

On GitHub, the compare URL is the two branch names joined by three dots:

```
https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/<from>...<to>
```

For example,
[compare 03 to 04](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/03-inline-and-block...tutorial/04-rich-tasks).
Non-adjacent branches work too: compare `tutorial/00-setup` to
`tutorial/06-figures` to see all of Part 1 at once, or `tutorial/06-figures`
to `tutorial/12-metadata-and-index` for all of Part 2, or
`tutorial/12-metadata-and-index` to `tutorial/17-chunking-and-output` for
all of Part 3, `tutorial/17-chunking-and-output` to `tutorial/22-learning`
for all of Part 4, or `tutorial/22-learning` to `tutorial/25-final` for all
of Part 5. The whole ladder is
[compare `tutorial/00-setup` to `tutorial/25-final`](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/00-setup...tutorial/25-final).

To read one file as it is on a branch without checking the branch out:

```bash
git show tutorial/06-figures:topics/what-is-digital-audio.dita
git ls-tree -r --name-only tutorial/06-figures                          # every file on the branch
git log --oneline tutorial/06-figures                                    # the lessons so far
```

## Reset to a stage

To put your clone at a stage, discarding your own work on it:

```bash
git checkout tutorial/04-rich-tasks
git status
```

`git status` should report nothing to commit. If it lists modified files, you
have uncommitted changes from your own work; `git stash` keeps them,
`git checkout -- .` discards them.

To keep your own work and still look at a stage, branch first:

```bash
git checkout -b my-stage-04 tutorial/03-inline-and-block   # start your work from stage 03
# ... write the stage ...
git diff tutorial/04-rich-tasks                             # compare with the answer
```

After any checkout, the "You are on" line near the top of `README.md` names
the stage:

```bash
head -9 README.md
```

## Editing a middle stage

For anyone maintaining the chain rather than following it: every later stage
must be rebased onto a change to a middle stage, in order, with
`git rebase --onto`, and the ladder re-checked with
`scripts/check-all-stages.sh` on `main`. The tutorial repository's
`docs-dev/stage-branches.md` has the procedure.
