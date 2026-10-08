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
4. Click **Save...**. The editor asks whether to write the deliverable to
   `project.json`. Click **OK**.
5. Click **Close**.

The fields have these effects:

- **Input map** is the root map of the publication.
- **Transtype** `html5` selects the HTML5 transformation.
- **Output** is relative to the project root, so this deliverable builds
  into `out/full/`.
- The `nav-toc` parameter with the value `full` puts the navigation of the
  whole guide on each page.

The editor stores deliverables in `project.json` in the project folder. You
do not need to edit it. To see what the dialog wrote, open `project.json`
from the **Explorer**: it holds the deliverable's name, the input map, the
output folder, the transtype, and the `nav-toc` parameter. The editor reads
this file every time it builds.

The active deliverable is the one that **Project** > **Build Deliverables...** builds by
default. If the status bar does not show `full`, choose `full` in the status
bar's deliverable menu. You can also select the deliverable in **Manage
Deliverables** and click **Set active**.

## Build and inspect

Build the guide. In the editor, choose **Project** > **Build
Deliverables...** and click **Build All**. When the build finishes, click
**OK**. You do not need to install DITA-OT: the editor and the command line
include DITA-OT 4.3.5.

You can also choose **Project** > **Check Project**. From now on, the check
also builds every deliverable and checks the built output. The result
appears in the **Project Validation** panel. From `my-audacity-guide` on the
command line, run:

```bash
dogsbay-xml check .
```

The output looks like this example:

```
health   clean
build    full                 ok  /home/you/my-audacity-guide/out/full
output   clean (full)
Ready: the project is healthy, every deliverable built, and every link in the pages of full leads somewhere.
```

The `build` line shows where the deliverable was built: the `out/full`
folder inside `my-audacity-guide`. The `output   clean` line means that every
link in the built pages leads somewhere. To see the pages, open `out/full/index.html`
from the **Explorer** and choose **View** > **Preview in Tab**. The preview
shows the page as a browser does. Follow the topic link, and check the
heading, short description, list, and section. The navigation comes from
the map. You wrote no HTML: the build made it all.

Your markup says what each part is. The look of the pages comes from the
transform: DITA-OT's `html5` transtype, which the deliverable chose.

Before the next exercise, predict where on the page a change to the topic
title appears. Then change the topic title, build again, and preview
`index.html` again. Check the navigation label, then follow the link and
check the page heading. The heading of `index.html` is the map title, so it
does not change.
Restore the title and build again when you finish the exercise.

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
