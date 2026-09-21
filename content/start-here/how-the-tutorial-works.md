---
title: How the tutorial works
description: The tutorial is a chain of git branches. Each branch is the previous one plus one lesson, and the diff between two branches is what the stage teaches.
type: explanation
---

# How the tutorial works

The tutorial is not a finished project that you read; it is a project that you
build. The finished states live in the
[tutorial repository](https://github.com/dogsbay/dogsbay-xml-dita-tutorial) as
a chain of branches, and this site tells you how to get from each one to the
next.

You can follow it two ways. Write every file yourself on your own clone and
use the branches to check your work, which is how the pages are written. Or
check out each branch in turn and read the diff, which is faster and still
teaches the elements. Either way, the gate tells you when a stage is done.

## The branch chain

Stage branches are named `tutorial/NN-slug`, from `tutorial/00-setup` to
`tutorial/25-final`. Each stage branch is the previous stage branch plus the
commits for that stage, so the chain is linear:

```
tutorial/00-setup
  └─ tutorial/01-concept
       └─ tutorial/02-task-and-reference
            └─ tutorial/03-inline-and-block
                 └─ …
                      └─ tutorial/25-final   (also tagged tutorial/final)
```

Commit messages start with `Stage NN:`, so `git log --oneline` on any branch
reads as the list of lessons so far.

## Diffs are lessons

Because each branch builds on the one before, the difference between two
adjacent branches is exactly one lesson:

```bash
git diff tutorial/03-inline-and-block tutorial/04-rich-tasks
```

GitHub shows the same thing as a compare view, and every stage page links to
its own:

```
https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/03-inline-and-block...tutorial/04-rich-tasks
```

Text is written once, correctly. Stages add structure; they do not plant
mistakes. When a page wants to show you a mistake, it shows what the gate
reports when you make it, and the branch itself stays clean.

## The orphan root

`tutorial/00-setup` is an *orphan* branch: it shares no history with `main`.
The `main` branch of the same repository is a finished, deliberately broken
demo project used by the
[DogsBay XML tutorial](https://dogsbay.ai/dogsbay-xml-docs/getting-started/tutorial-agentic),
and the step-by-step tutorial has to start empty. So `git log tutorial/00-setup`
shows one commit, and nothing on `main` is an ancestor of any stage branch.

The consequence for you: `git checkout tutorial/00-setup` from `main` replaces
the whole working tree. That is expected.

## The "You are on" line

Every stage branch carries the same `README.md`, with one line that changes at
each stage:

```
You are on **stage 03 — inline and block**: UI controls, shortcuts, terms, notes, lists, tables and code.
```

Read it after any checkout to confirm where you are. The README's stage table
is the whole ladder, so you can also see what is ahead.

## What every branch carries

Besides the topics, every stage branch has:

| File | Purpose |
|---|---|
| `README.md` | The "You are on" line, the stage table, the gate command |
| `LICENSE`, `NOTICE` | CC BY 4.0, and the credit to the Audacity Manual that the topic text is adapted from |
| `.dogsbay/config.xml` | Shared editor project settings: project type, framework, format style |
| `scripts/check-stage.sh` | The gate. See [Run the gate](/start-here/run-the-gate) |
| `.gitignore` | Keeps DITA-OT output out of git |

## The gate

`scripts/check-stage.sh` validates every file, runs a project health check
and, once the project has a `project.json`, builds every deliverable with
DITA-OT. A stage is done when it prints `STAGE OK`. Every branch in the chain
passes it, and every stage page ends by running it.

## Where to go next

:::cards
- **[Set up your tools](/start-here/set-up)** {icon="wrench"}
  DITA-OT 4.3.5, the DogsBay XML command line, and the clone.

- **[Run the gate](/start-here/run-the-gate)** {icon="check"}
  What the check does and what its output means.
:::
