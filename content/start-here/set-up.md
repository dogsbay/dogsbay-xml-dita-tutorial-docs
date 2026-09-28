---
title: Set up your tools
description: Install the DogsBay XML editor, which includes the dogsbay-xml command line and DITA-OT 4.3.5, and clone the tutorial repository at its first stage.
type: how-to
---

# Set up your tools

The tutorial needs two tools: the DogsBay XML editor and Git. The editor
includes the `dogsbay-xml` command line and DITA-OT 4.3.5. You check your
work with either one: **Project** > **Check Project** in the editor, or
`dogsbay-xml check` on the command line. Both validate the files, check the
project, and build the guide with the included DITA-OT. You do not need to
install DITA-OT or set any environment variables.

The shell examples use Bash and Unix utilities. On Windows, use a Bash
environment. The editor validates as you type against the same DTDs that
the check uses. Any editor that can save UTF-8 text works for writing the
files, but you need the DogsBay XML editor or its command line to check your
work.

## Install the DogsBay XML editor

The command line is installed with the editor. It needs neither a running
editor nor a separate JDK. The recorded examples use the included DITA-OT
4.3.5.

:::steps
1. **Install the editor**
   Follow [Installing](https://dogsbay.ai/dogsbay-xml-docs/getting-started/install)
   in the DogsBay XML documentation: download the package for your platform
   from the [releases page](https://github.com/dogsbay/dogsbay-xml/releases)
   and install it the usual way.

2. **Put the command line on your PATH**
   If you work from the command line, `dogsbay-xml` must be on your `PATH`.
   The installation page shows the directory for each platform. On Linux:

   ```bash
   export PATH="/opt/dogsbay-xml-editor/bin:$PATH"
   dogsbay-xml --version
   ```

   Add the export to your Bash profile to use it in future sessions. If you
   check your work only in the editor, you can skip this step.
:::

> [!NOTE]
> Use `dogsbay-xml` to check your work. In the environment used for
> the recorded runs, `xmllint` rejected the DITA 1.3 DTDs with "Maximum
> entity amplification factor exceeded", including with `--huge`.
> `dogsbay-xml` uses Xerces and an XML catalog to resolve the DTDs.

## Clone the repository

:::steps
1. **Clone it**

   ```bash
   git clone https://github.com/dogsbay/dogsbay-xml-dita-tutorial.git
   cd dogsbay-xml-dita-tutorial
   ```

   The clone opens on `main`, which contains the separate repair exercise.
   Use the stage branches for this tutorial.

2. **Check out the first stage**

   ```bash
   git checkout tutorial/00-setup
   ```

   The working tree is replaced: `tutorial/00-setup` shares no history with
   `main`. See [How the tutorial works](/start-here/how-the-tutorial-works).

3. **Confirm where you are**

   ```bash
   grep 'You are on' README.md
   ```

   The command prints the "You are on" line for stage 00:

   ```
   You are on stage 00: setup.
   ```
:::

## Open the project in the editor

For the authoring workflow, stage 00 creates a separate `audacity-guide`
repository. Open that folder after creating it. Keep the clone as a
reference. For the inspection workflow, open the clone directly.

If you use the DogsBay XML editor, open the selected folder as a project. The
`.dogsbay/config.xml` on every stage branch sets the project type to DITA, the
framework to DITA-OT 4.3.5 and the format style, so the editor and the
command line agree on what a valid, well-formatted file is.

## Check the tools

Check stage 00 once. There is nothing to validate yet, so the check should
finish at once. In the editor, choose **Project** > **Check Project**. From
the command line, run this command from the project root:

```bash
dogsbay-xml check --no-build .
```

The next page explains what it printed.

## Where to go next

:::cards
- **[Check your work](/start-here/run-the-gate)** {icon="check"}
  What the check does, what it prints, and where to find the details.

- **[Stage 00: Set up the project](/part-1-topics/stage-00-setup)** {icon="play"}
  The empty project, file by file.
:::
