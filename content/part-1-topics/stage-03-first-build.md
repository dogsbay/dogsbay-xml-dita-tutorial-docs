---
title: "Stage 03: Publish the first guide"
description: Define an HTML deliverable, build the guide with DITA-OT, and inspect its generated pages.
type: tutorial
---

# Stage 03: Publish the first guide

Publish the map from stage 02. Keep this build throughout the course so you
can inspect every topic, link, and navigation change as you make it.

**You need:** stage 02 complete. **Time:** about 10 minutes.

## Define the deliverable

A deliverable names a map, a transformation, and an output folder. Define
one deliverable, named `full`, for the whole guide:

1. Choose **Project** > **Project Tools** > **Manage Deliverables...**. The
   table is empty.
2. Click **Add...**.
3. Enter these values:

   | Field | Value |
   |---|---|
   | **Name** | `full` |
   | **Input map** | `audacity-guide.ditamap` |
   | **DITAVAL (optional)** | (leave empty) |
   | **Transtype** | `html5` |
   | **Output (optional)** | `out/full` |
   | **Publication parameters** | `nav-toc` = `full` |

   To add the parameter, click **Add param**, double-click the **Name**
   cell and type `nav-toc`, then double-click the **value** cell and type
   `full`.
4. Click **Save...**. The editor asks whether to write the deliverable.
   Click **OK**.
5. Click **Close**.

The fields have these effects:

- **Input map** is the root map of the publication.
- **Transtype** `html5` selects the HTML5 transformation.
- **Output** is relative to the project root, so this deliverable builds
  into `out/full/`.
- The `nav-toc` parameter with the value `full` puts the navigation of the
  whole guide on each page.

The editor stores deliverables in `project.json` in the project folder. You
do not need to edit it.

The active deliverable is the one that **Project** > **Build Deliverables...** builds by
default. If the status bar does not show `full`, choose `full` in the status
bar's deliverable menu. You can also select the deliverable in **Manage
Deliverables** and click **Set active**.

## Build and inspect

Check your work. From now on, the check also builds every deliverable and
checks the built output. You do not need to install DITA-OT: the editor and
the command line include DITA-OT 4.3.5.

In the editor, choose **Project** > **Check Project**. The result appears in
the **Project Validation** panel. From `my-audacity-guide` on the command
line, run:

```bash
dogsbay-xml check .
```

The output looks like this example:

```
health   clean
build    full                 ok  /home/you/my-audacity-guide/out/full
output   clean (full)
Ready: the project is healthy, every deliverable built, and the output of full holds together.
```

The `build` line shows where the deliverable was built: the `out/full`
folder inside `my-audacity-guide`. To see it, open `out/full/index.html`
from the **Explorer** and choose **View** > **Preview in Tab**. The preview
shows the page as a browser does. Follow the topic link, and check the
heading, short description, list, and section.

Change the topic title, check again, and preview `index.html` again. Check
both the page heading and navigation label. Restore the title and check again when
you finish the exercise.

If the check fails, check the working directory and the map-relative paths.
The check stops at the first stage that fails. For details, see
[Check your work](/start-here/run-the-gate).

## Keep the output out of Git

If you track your project with Git, tell Git to ignore the build output. The
output is generated from your source, so it does not belong in the
repository.

1. In the **Explorer**, right-click `my-audacity-guide` and choose
   **New File**.
2. Enter `.gitignore` and press Enter. The file opens empty.
3. Type the following line and save the file:

   ```
   out/
   ```

Git now ignores everything in `out/`. If you do not use Git, skip this
section.

## Next lesson

**Checkpoint:** `tutorial/03-first-build`. If you use Git, commit your work.
The checkpoint's `.gitignore` has more rules than `out/`, for files that
this tutorial does not create.

Continue with [Stage 04: task and reference](/part-1-topics/stage-04-task-and-reference).

For the core course, use the [learning path](/start-here/learning-path) to skip optional modules.

[Compare this stage with its predecessor](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/02-first-map...tutorial/03-first-build).
