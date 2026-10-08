---
title: "Stage 00: Create your project folder"
description: Open the empty my-audacity-guide folder in DogsBay XML, add a topics folder, and optionally start tracking it with Git.
type: tutorial
---

# Stage 00: Create your project folder

Prepare the folder for the guide. At the end of this lesson, the
`my-audacity-guide` folder contains an empty `topics` folder, where the
topics of later lessons go.

**Time:** about 5 minutes.
**You need:** the DogsBay XML editor, and the empty `my-audacity-guide`
folder open in it. To create and open the folder, use **File** >
**Open Project Folder...** as described in
[Set up your tools](/start-here/set-up#create-and-open-your-project-folder).

## Step 1: Create the topics folder

A DITA project is a folder of topics and maps. Maps stay in the project
folder, and topics go in a folder of their own, so that the project stays
readable as it grows.

:::steps
1. **Create the folder**
   In the **Explorer**, right-click `my-audacity-guide` and choose
   **New Folder**. Type `topics` and press Enter.

2. **Confirm the result**
   The **Explorer** shows `topics` inside `my-audacity-guide`.
:::

## Step 2: Optional: start tracking with Git

If you want to track your work with Git, initialize the repository now, as
described in [Track your work with Git](/start-here/set-up#optional-track-your-work-with-git).
Git does not track empty folders, so there is nothing to commit until
stage 01 adds the first topic.

## Step 3: Check your work

The project has no topics yet, so there is nothing for the editor to
check. You check your work in the editor from stage 01, when the project has
its first topic.

If you installed the command line, you can confirm that it works on your
project. Open the editor's **Terminal** panel and run this command from
`my-audacity-guide`:

```bash
dogsbay-xml check .
```

The output looks like this example:

```
health   clean
Project health is clean — no deliverables yet, so nothing here speaks for the output.
```

`health   clean` means that the check found nothing wrong. With no topics,
there is nothing to find. "No deliverables yet" means that the project does
not publish anything yet. You add the first deliverable in stage 03. If the
command is not found, see [Set up your tools](/start-here/set-up).

## What you learned

- A DITA project is a folder of topics and maps.
- Topics go in a `topics` folder of their own.
- Git is optional. If you use it, you commit at the end of each stage.

## Next lesson

**Checkpoint:** `tutorial/00-setup`. The checkpoint branch also contains
files that you do not create, such as `README.md` and `LICENSE`.

Continue with [Stage 01: concept](/part-1-topics/stage-01-concept).

For the core course, use the [learning path](/start-here/learning-path) to skip optional modules.
