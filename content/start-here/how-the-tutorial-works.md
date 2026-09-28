---
title: How the tutorial works
description: The tutorial is a chain of Git branches. Each branch is the previous one plus one lesson, and the diff between two branches is what the stage teaches.
type: explanation
---

# How the tutorial works

The [learning path](/start-here/learning-path) separates the core course from
optional modules. The numbered branches still form one complete reference
sequence. Skipping a module means starting the next lesson from its supplied
checkpoint, which already contains the prerequisite files.

Build a DITA guide in 27 stages. The
[tutorial repository](https://github.com/dogsbay/dogsbay-xml-dita-tutorial)
contains a branch for each completed stage. Each lesson explains how to
produce that stage from the previous one.

Choose one workflow:

- Build the guide in a separate `audacity-guide` repository, starting at
  stage 00. Fetch the reference branches there to compare your files and
  retrieve supplied images.
- Inspect completed stages in the tutorial clone. Compare adjacent branches
  and check your work on the stage that you switch to.

The command examples use `origin/tutorial/NN-slug` for fetched reference
branches. These refs are available after cloning or fetching; local stage
branches exist only after you create them. In the editor, click the branch
name in the status bar and choose `origin/tutorial/NN-slug`: the editor
creates the local tracking branch and switches to it. See
[Move between stages](/start-here/set-up#move-between-stages).

## The branch chain

Stage branches are named `tutorial/NN-slug`, from `tutorial/00-setup` to
`tutorial/26-final`. Each stage branch is the previous stage branch plus the
commits for that stage, so the chain is linear:

```
tutorial/00-setup
  └─ tutorial/01-concept
       └─ tutorial/02-first-map
            └─ tutorial/03-first-build
                 └─ tutorial/04-task-and-reference
                      └─ …
                           └─ tutorial/26-final   (also tagged tutorial/final)
```

Commit messages start with `Stage NN:`, so `git log --oneline` on any branch
reads as the list of lessons so far.

## Diffs are lessons

Because each branch builds on the one before, the difference between two
adjacent branches is exactly one lesson:

```bash
git diff origin/tutorial/05-inline-and-block origin/tutorial/06-rich-tasks
```

GitHub shows the same thing as a compare view, and every stage page links to
its own:

```
https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/05-inline-and-block...tutorial/06-rich-tasks
```

Each completed stage passes the check. Error exercises show the
diagnostics for a deliberate change. Undo that change and check again
before continuing. Later stages add checks, so an earlier passing stage can
still contain issues that those later checks detect.

## The orphan root

`tutorial/00-setup` is an *orphan* branch: it shares no history with `main`.
The `main` branch of the same repository is a finished, deliberately broken
demo project used by the
[DogsBay XML tutorial](https://dogsbay.ai/dogsbay-xml-docs/getting-started/tutorial-agentic),
and the step-by-step tutorial has to start empty. So `git log tutorial/00-setup`
shows one commit, and nothing on `main` is an ancestor of any stage branch.

The consequence for you: switching from `main` to `tutorial/00-setup`, in
the status bar or with `git switch`, replaces the whole working tree. That
is expected.

## The "You are on" line

Every stage branch carries the same `README.md`, with one line that changes at
each stage:

```
You are on stage 05: inline and block.
```

Read it after you switch branches to confirm where you are. The README's stage table
is the whole ladder, so you can also see what is ahead.

## What every branch carries

Besides the topics, every stage branch has:

| File | Purpose |
|---|---|
| `README.md` | The "You are on" line, the stage table, the maintainer check commands |
| `LICENSE`, `NOTICE` | CC BY 4.0, and the credit to the Audacity Manual that the topic text is adapted from |
| `.dogsbay/config.xml` | Shared editor project settings: project type, framework, format style |
| `scripts/check-stage.sh` | The maintainers' check for the branches. Readers do not need it. See [The stage gate](/reference/the-gate) |
| `.gitignore` | Keeps DITA-OT output out of Git |

## Check your work

**Project** > **Check Project** in the editor, or `dogsbay-xml check .` on
the command line, checks the project health and, once the project declares
deliverables in stage 03, builds every deliverable with DITA-OT and checks
the built output. From stage 03, a stage is done when the check reports
`Ready`. Before stage 03, look for `health   clean`. Every stage page ends by
checking your work.

## Where to go next

:::cards
- **[Set up your tools](/start-here/set-up)** {icon="wrench"}
  The DogsBay XML editor and command line, and the clone.

- **[Check your work](/start-here/run-the-gate)** {icon="check"}
  What the check does and what its output means.
:::
