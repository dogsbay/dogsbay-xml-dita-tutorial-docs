---
title: How the tutorial works
description: You build one project, stage by stage, in your own folder. Each lesson ends with the checkpoint branch of the sample project that matches its end.
type: explanation
---

# How the tutorial works

Build a DITA guide in 27 stages, in one project folder, `my-audacity-guide`.
Each lesson starts from the files of the previous lesson and adds one
feature. At the end of a lesson, you check your work.

The [learning path](/start-here/learning-path) separates the core course from
optional modules. Skipping a module means starting the next lesson from its
checkpoint, which already contains the prerequisite files.

## Checkpoints

The [sample project](/start-here/set-up#the-sample-project) has one branch
for each completed stage. Each lesson ends with the name of its branch, for
example:

> **Checkpoint:** `tutorial/01-concept`.

The branch contains the files that you have at the end of that lesson. The
branches are the reference: when your project and a checkpoint differ, the
checkpoint shows the intended result. The branches also carry files that
the lessons do not create, such as `README.md`, `LICENSE`, `NOTICE`, and
maintainer scripts. You do not need them in your project.

In a clone of the sample project, the branches are available as
`origin/tutorial/NN-slug`. In the editor, click the branch name in the
status bar and choose `origin/tutorial/NN-slug`: the editor creates the
local tracking branch and switches to it. See
[Move between stages](/start-here/set-up#move-between-stages).

If you track your project with Git, commit your work at the end of each
lesson.

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

`tutorial/00-setup` is an *orphan* branch: it shares no history with `main`,
because the step-by-step tutorial has to start empty. So
`git log tutorial/00-setup` shows one commit, and nothing on `main` is an
ancestor of any stage branch. `main` holds the finished guide, the same
files as `tutorial/26-final`, plus the maintainers' scripts and notes.

In a clone of the sample project, switching from `main` to
`tutorial/00-setup` replaces the whole working tree. That is expected.

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
  The DogsBay XML editor, your project folder, and the sample project.

- **[Check your work](/start-here/run-the-gate)** {icon="check"}
  What the check does and what its output means.
:::
