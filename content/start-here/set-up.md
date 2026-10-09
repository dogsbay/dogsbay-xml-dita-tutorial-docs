---
title: Set up your tools
description: Install the DogsBay XML editor, create and open an empty my-audacity-guide folder, and optionally track your work with Git.
type: how-to
---

# Set up your tools

Install the DogsBay XML editor and open an empty folder, `my-audacity-guide`,
where you build the guide. You do not need Git or the command line. Git is
optional and recommended.

The editor includes the `dogsbay-xml` command line and DITA-OT 4.3.5. You
check your work with either one: **Project** > **Check Project** in the
editor, or `dogsbay-xml check` on the command line. Both validate the
files, check the project, and build the guide with the included DITA-OT.
You do not need to install DITA-OT or set any environment variables.

## Install the DogsBay XML editor

:::steps
1. **Install the editor**
   Follow [Installing](https://dogsbay.ai/dogsbay-xml-docs/getting-started/install)
   in the DogsBay XML documentation: download the package for your platform
   from the [releases page](https://github.com/dogsbay/dogsbay-xml/releases)
   and install it the usual way.

2. **Optional: put the command line on your PATH**
   The lessons show a command-line equivalent for each check. To use it,
   `dogsbay-xml` must be on your `PATH`. The installation page shows the
   directory for each platform. On Linux:

   ```bash
   export PATH="/opt/dogsbay-xml-editor/bin:$PATH"
   dogsbay-xml --version
   ```

   The second command prints the version of the command line. Add the
   export to your Bash profile to use it in future sessions. The command
   line needs neither a running editor nor a separate JDK. If you check your
   work only in the editor, skip this step.
:::

The shell examples use Bash. On Windows, use a Bash environment.

> [!NOTE]
> Use `dogsbay-xml` to check your work from the command line. In the
> environment used for the recorded runs, `xmllint` rejected the DITA 1.3
> DTDs with "Maximum entity amplification factor exceeded", including with
> `--huge`. `dogsbay-xml` uses Xerces and an XML catalog to resolve the DTDs.

## Create and open your project folder

Your guide lives in one folder, `my-audacity-guide`. Create it from the
editor:

:::steps
1. **Open the folder chooser**
   Choose **File** > **Open Project Folder...**. On the welcome screen, you
   can click **Open Project Folder…** instead. A folder chooser opens.

2. **Create the folder**
   Go to the folder where you want to keep your work, for example
   double-click your Documents folder. Click **Create New Folder** (the
   folder icon at the top of the chooser). A new folder appears with its
   name ready to edit. Type `my-audacity-guide` and press Enter.

3. **Open it**
   Select `my-audacity-guide` and click **Open**.
:::

The **Explorer** shows `my-audacity-guide`, which is empty.
[Stage 00](/part-1-topics/stage-00-setup) adds the first folder.

## Optional: track your work with Git

Git records each version of your files. With Git, you can compare your work
with an earlier stage and undo changes that you do not want. The tutorial
works without it.

To use Git, initialize a repository in your project folder once:

:::steps
1. **Open the terminal**
   Open the editor's **Terminal** panel. If it does not start in
   `my-audacity-guide`, change to that folder with `cd`.

2. **Initialize the repository**
   Run this command:

   ```bash
   git init
   ```

   For now, the editor has no command that initializes a repository. An
   editor action is planned.
:::

At the end of each stage, commit your work in the **Git** panel in the
sidebar: enter a message, such as `Stage 01: concept`, and click
**Commit**. To keep each stage on its own branch, create a branch from the
**…** menu of the **Git** panel.

## The sample project

The [sample project](https://github.com/dogsbay/dogsbay-xml-dita-tutorial)
on GitHub is the finished tutorial, with one branch for each stage, from
`tutorial/00-setup` to `tutorial/26-final`. The tag `tutorial/final` marks
the completed guide. Each lesson ends with the name of the branch, the
*checkpoint*, that matches your project at the end of that lesson.

You do not need the sample project to follow the lessons. Use it to:

- Compare a file with the checkpoint when your check fails and you cannot
  find the cause.
- Copy files that you cannot type, such as the images in stage 08.
- Start a later stage from its checkpoint when you skip a lesson. See
  [Choose a learning path](/start-here/learning-path).

The editor's **File** > **Open Sample Project...** opens the finished
guide, the same files as `tutorial/26-final`, without the stage branches.

### Get the sample project

Keep the sample project in its own folder, next to `my-audacity-guide`,
not inside it. In the editor, choose **Project** > **Manage Projects...**,
click **Clone from GitHub**, and clone this address:

```
https://github.com/dogsbay/dogsbay-xml-dita-tutorial.git
```

From the command line, run this command from the folder where you keep
your work instead:

```bash
git clone https://github.com/dogsbay/dogsbay-xml-dita-tutorial.git
```

The copy opens on `main`, which is the finished guide. The stages are on
the `tutorial/NN-slug` branches.

### Move between stages

To see a checkpoint, open the sample project in the editor and click the
branch name in the status bar. The list shows the stage branches as
`origin/tutorial/NN-slug`. Choose one, for example
`origin/tutorial/01-concept`. The editor creates a local branch,
`tutorial/01-concept`, that tracks it, and switches to it. From the command
line, run `git switch tutorial/01-concept` in the sample project.

Switching replaces the files in the sample project folder with the files of
that stage. Your own project in `my-audacity-guide` does not change.

Each branch also has files that you do not create, such as `README.md`,
`LICENSE`, `NOTICE`, and a `scripts/` folder. They are for the maintainers
and visitors on GitHub. Compare only the files that the lessons create.

## Where to go next

:::cards
- **[Stage 00: Create your project folder](/part-1-topics/stage-00-setup)** {icon="play"}
  Add the first folder to `my-audacity-guide`.

- **[Check your work](/start-here/run-the-gate)** {icon="check"}
  What the check does, what it prints, and where to find the details.
:::
