---
title: Set up your tools
description: Install DITA-OT 4.3.5 and the DogsBay XML command line, point the gate at them, and clone the tutorial repository at its first stage.
type: how-to
---

# Set up your tools

The tutorial needs two tools: DITA-OT, which publishes the guide, and the
DogsBay XML command line `dogsbay-xml`, which validates the files and checks
the project. The gate script finds both on your `PATH` or through two
environment variables.

You also need git and a text editor. The DogsBay XML editor is the recommended
editor: it validates as you type against the same DTDs the gate uses, and it
ships the command line. Any editor that can save UTF-8 text works.

## Install DITA-OT 4.3.5

The gate and the tutorial are verified against DITA-OT 4.3.5. Other 4.x
releases probably work; 4.3.5 is known to.

:::steps
1. **Download the release**
   Get `dita-ot-4.3.5.zip` from the
   [DITA-OT releases](https://github.com/dita-ot/dita-ot/releases/tag/4.3.5)
   and unpack it somewhere you keep tools, such as `~/Downloads/dita-ot-4.3.5`
   or `/opt/dita-ot-4.3.5`.

2. **Check it runs**
   DITA-OT needs a Java runtime, 17 or later.

   ```bash
   ~/Downloads/dita-ot-4.3.5/bin/dita --version
   ```

3. **Tell the gate where it is**
   The gate looks for `dita` on your `PATH`, then at `DITA_HOME`, then at the
   default `~/Downloads/dita-ot-4.3.5`. If you unpacked it elsewhere, set the
   variable in your shell profile:

   ```bash
   export DITA_HOME=/opt/dita-ot-4.3.5
   ```
:::

## Install the DogsBay XML command line

`dogsbay-xml` is installed with the DogsBay XML editor. The installer puts the
executable beside the editor, and the command line needs neither a running
editor nor a separate JDK.

:::steps
1. **Install the editor**
   Follow [Installing](https://dogsbay.ai/dogsbay-xml-docs/getting-started/install)
   in the DogsBay XML documentation: download the package for your platform
   from the [releases page](https://github.com/dogsbay/dogsbay-xml/releases)
   and install it the usual way.

2. **Put the command line on your PATH**
   The same page shows the directory for each platform. On Linux:

   ```bash
   export PATH="/opt/dogsbay-xml-editor/bin:$PATH"
   dogsbay-xml --version
   ```

3. **Or point the gate at it**
   If you would rather not change your `PATH`, set `DOGSBAY_XML` to the
   executable. The gate checks the variable first, then the `PATH`.

   ```bash
   export DOGSBAY_XML=/opt/dogsbay-xml-editor/bin/dogsbay-xml
   ```
:::

> [!NOTE]
> `xmllint` cannot stand in for `dogsbay-xml`. libxml2 refuses the DITA 1.3
> DTDs with "Maximum entity amplification factor exceeded", even with
> `--huge`. `dogsbay-xml` validates with Xerces and the DITA-OT catalog.

## Clone the repository

:::steps
1. **Clone it**

   ```bash
   git clone https://github.com/dogsbay/dogsbay-xml-dita-tutorial.git
   cd dogsbay-xml-dita-tutorial
   ```

   The clone opens on `main`, which is a different, finished project. Do not
   start from it.

2. **Check out the first stage**

   ```bash
   git checkout tutorial/00-setup
   ```

   The working tree is replaced: `tutorial/00-setup` shares no history with
   `main`. See [How the tutorial works](/start-here/how-the-tutorial-works).

3. **Confirm where you are**

   ```bash
   head -9 README.md
   ```

   The last line printed is the "You are on" line for stage 00.
:::

## Open the project in the editor

If you use the DogsBay XML editor, open the clone as a project. The
`.dogsbay/config.xml` on every stage branch sets the project type to DITA, the
framework to DITA-OT 4.3.5 and the format style, so the editor and the gate
agree on what a valid, well-formatted file is.

## Check the tools

Run the gate once on stage 00. There is nothing to validate yet, so it should
finish at once:

```bash
scripts/check-stage.sh
```

The next page explains what it printed.

## Where to go next

:::cards
- **[Run the gate](/start-here/run-the-gate)** {icon="check"}
  What the check does, what it prints and the two switches.

- **[Stage 00: Set up the project](/part-1-topics/stage-00-setup)** {icon="play"}
  The empty project, file by file.
:::
