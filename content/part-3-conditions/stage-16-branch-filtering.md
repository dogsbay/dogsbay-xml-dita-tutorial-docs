---
title: "Stage 16: Branch filtering"
description: Publish the install task three times from one map, once per platform, with a ditavalref on the branch and a prefix or suffix on the copies.
type: tutorial
---

# Stage 16: Branch filtering

Apply three DITAVAL files to one branch of
`installation-variants.ditamap`. DITA-OT creates a filtered copy for
Windows, macOS, and Linux in a single build.

Use `<ditavalmeta>` to assign distinct output names and key scopes to the
copies. Add the map as a deliverable in the editor.

**Optional module.** See [Choose a learning path](/start-here/learning-path)
for the starting checkpoint and the next core lesson.

**Time:** about 20 minutes.
**You need:** stage 15 complete.


Recorded output below is an example. File counts, paths, and stage numbers can differ. [Check your work](/start-here/run-the-gate) to see the result for your own project.

## Step 1: A DITAVAL per platform

::::steps
1. **Create `filters/platform-windows.ditaval`, `filters/platform-mac.ditaval` and `filters/platform-linux.ditaval`**
   In the **Explorer**, right-click the `filters` folder, choose
   **New File**, enter the file name, and choose the **DITAVAL** template.
   Replace the template's rule with the rules below. The files filter by
   platform only; they have no audience rules, because the variants map
   has no audience-specific content.

   ```text title="filters/platform-windows.ditaval"
   <?xml version="1.0" encoding="UTF-8"?>
   
   <val>
     <prop att="platform" val="windows" action="include"/>
     <prop att="platform" val="mac" action="exclude"/>
     <prop att="platform" val="linux" action="exclude"/>
   </val>
   ```

   ```text title="filters/platform-mac.ditaval"
   <?xml version="1.0" encoding="UTF-8"?>
   
   <val>
     <prop att="platform" val="windows" action="exclude"/>
     <prop att="platform" val="mac" action="include"/>
     <prop att="platform" val="linux" action="exclude"/>
   </val>
   ```

   ```text title="filters/platform-linux.ditaval"
   <?xml version="1.0" encoding="UTF-8"?>
   
   <val>
     <prop att="platform" val="windows" action="exclude"/>
     <prop att="platform" val="mac" action="exclude"/>
     <prop att="platform" val="linux" action="include"/>
   </val>
   ```
::::

## Step 2: The map with three branches

::::steps
1. **Create `installation-variants.ditamap`**
   Right-click `my-audacity-guide` in the **Explorer**, choose
   **New File**, enter `installation-variants.ditamap`, and choose the
   **Map** template. Replace the template's content with this map:

   ```xml title="installation-variants.ditamap"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE map PUBLIC "-//OASIS//DTD DITA Map//EN" "map.dtd">
   
   <map>
     <title>Installing Audacity: one topic, three platform variants</title>
     <mapref href="subject-scheme.ditamap" type="subjectScheme"/>
     <mapref href="keydefs-product.ditamap"/>
     <mapref href="keydefs-glossary.ditamap"/>
     <!-- Abbreviated forms link into this glossary group; toc="no" publishes it without a TOC entry. -->
     <topicref href="topics/glossary/audio-units.dita" toc="no"/>
     <keydef keys="common-notes" href="shared/common-notes.dita"/>
     <!-- The exporting task links to the formats topic by key; toc="no" publishes it without a TOC entry. -->
     <topicref href="topics/supported-audio-formats.dita" keys="formats" toc="no"/>
     <topicref href="shared/common-steps.dita" processing-role="resource-only"/>
     <!-- Branch filtering (DITA 1.3): each ditavalref makes DITA-OT copy this branch
          and filter the copy for one platform. The copies get the prefix and suffix
          named in ditavalmeta; the keys defined inside the branch get a key scope
          prefix so the three copies do not collide. -->
     <topicref href="topics/installing-audacity.dita" keys="install">
       <ditavalref href="filters/platform-windows.ditaval">
         <ditavalmeta>
           <dvrResourcePrefix>win-</dvrResourcePrefix>
           <dvrKeyscopePrefix>win.</dvrKeyscopePrefix>
         </ditavalmeta>
       </ditavalref>
       <ditavalref href="filters/platform-mac.ditaval">
         <ditavalmeta>
           <dvrResourcePrefix>mac-</dvrResourcePrefix>
           <dvrKeyscopePrefix>mac.</dvrKeyscopePrefix>
         </ditavalmeta>
       </ditavalref>
       <ditavalref href="filters/platform-linux.ditaval">
         <ditavalmeta>
           <dvrResourceSuffix>-linux</dvrResourceSuffix>
           <dvrKeyscopePrefix>linux.</dvrKeyscopePrefix>
         </ditavalmeta>
       </ditavalref>
       <topicref href="topics/recording-your-first-track.dita" keys="first-track"/>
     </topicref>
   </map>
   ```

2. **Read the elements**

   - `<ditavalref href="…">` inside a `<topicref>` applies that DITAVAL
     to the topicref and everything under it. One `<ditavalref>` filters
     the branch in place; two or more make one copy of the branch per
     DITAVAL. Here the branch is the install task and, nested under it,
     *Recording your first track*, so each platform gets both pages.
   - `<ditavalmeta>` holds the naming. `<dvrResourcePrefix>` is put
     before the file name of every copied topic, `<dvrResourceSuffix>`
     after it (before the extension). The Windows and Mac branches use a
     prefix, the Linux branch a suffix, so you see both forms in the
     output; in a real project pick one.
   - `<dvrKeyscopePrefix>` puts the copies in separate key scopes. The
     branch defines `install` and `first-track`; without the prefixes,
     three copies would define the same keys, and the first would win.
     With them the keys are `win.install`, `mac.install` and
     `linux.install`, and the same for `first-track`. Stage 17 is about
     key scopes.
   - The rest of the map is what any root map needs: the scheme, the
     product and glossary keys, the shared topics.
   - The `<topicref toc="no">` to `audio-units.dita` is the same one the
     beginner map got in stage 14. *Recording your first track* links to
     pages that use `<abbreviated-form>`, and a `<keydef>` to a fragment
     such as `audio-units.dita#gl-decibel` does not make DITA-OT publish
     the glossary group. The topicref publishes the group so that those
     links have a page to land on, and keeps it out of the table of
     contents.
   - The `<topicref toc="no">` to `supported-audio-formats.dita` does the
     same for the formats topic, and defines the key `formats`. The
     exporting task, which this map reaches through links, says "see
     `<xref keyref="formats/choosing"/>`". Without the key in this map,
     that reference resolves to nothing and the output reads "see )";
     with only a `<keydef>`, the link resolves but the page is not
     published.

3. **List the branches**

   ```bash
   dogsbay-xml list-branches installation-variants.ditamap
   ```

   ```
   win-                 filters topics/installing-audacity.dita ditaval=filters/platform-windows.ditaval  keyscope=win.
   mac-                 filters topics/installing-audacity.dita ditaval=filters/platform-mac.ditaval  keyscope=mac.
   -linux               filters topics/installing-audacity.dita ditaval=filters/platform-linux.ditaval  keyscope=linux.
   3 branch variant(s).
   ```

   Each line is one variant: its prefix, the topic it filters, the
   DITAVAL and the key scope. A variant with no prefix is labeled by its
   suffix.
::::

## Step 3: The deliverable and the check

::::steps
1. **Add the deliverable**
   Choose **Project** > **Project Tools** > **Manage Deliverables...** and
   click **Add...**. Enter these values:

   | Field | Value |
   |---|---|
   | **Name** | `install-variants` |
   | **Input map** | `installation-variants.ditamap` |
   | **DITAVAL (optional)** | (leave empty) |
   | **Transtype** | `html5` |
   | **Output (optional)** | `out/install-variants` |
   | **Publication parameters** | (none) |

   Leave **DITAVAL** empty: the `<ditavalref>` elements in the map do the
   filtering. Click **Save...**, click **OK** to write the deliverable, and
   click **Close**.

2. **Preview a branch**
   In the status bar's deliverable menu, choose `install-variants`. Open
   `topics/installing-audacity.dita` and choose **View** > **Preview in
   Tab**. Because the map has branches, the bar above the preview adds
   **Branch**, set to **Whole map**. Choose **win-** and scroll to the
   install step: the preview applies that branch's filter,
   **platform-windows.ditaval**, and keeps only the Windows install row.
   Choose **mac-**, and only the macOS row is left. The topic has not
   changed; the filter does the work. Close the preview, and choose `full`
   in the status bar again before you continue.

3. **Build**
   Choose **Project** > **Build Deliverables...** and click **Build All**.
   **Project** > **Check Project** also builds every deliverable, and
   checks the project and the built pages as well. From the command line,
   the same full check is:

   ```bash
   dogsbay-xml check .
   ```

   Example output:

   ```
   health   clean, with warnings
     unused key: start-here  [/home/you/my-audacity-guide/audacity-guide.ditamap:27] — nothing references it
     unused key: digital-audio  [/home/you/my-audacity-guide/audacity-guide.ditamap:34] — nothing references it
     unused key: podcast-workflow  [/home/you/my-audacity-guide/audacity-guide.ditamap:50] — nothing references it
   build    full                 ok  /home/you/my-audacity-guide/out/full
     5 note(s) — run with --verbose to see them
   build    beginner-mac         ok  /home/you/my-audacity-guide/out/beginner-mac
   build    beginner-windows     ok  /home/you/my-audacity-guide/out/beginner-windows
   build    podcaster-linux      ok  /home/you/my-audacity-guide/out/podcaster-linux
   build    review               ok  /home/you/my-audacity-guide/out/review
     4 note(s) — run with --verbose to see them
   build    install-variants     ok  /home/you/my-audacity-guide/out/install-variants
     5 note(s) — run with --verbose to see them
   output   clean (full, beginner-mac, beginner-windows, podcaster-linux, review, install-variants)
   Ready: the project is healthy, every deliverable built, and every link in the pages of 6 deliverables leads somewhere. 3 unused keys above: worth knowing, and not treated as failures.
   ```

   The new filters use only controlled values: the full check includes
   the controlled values from stage 15, and it reads the DITAVAL files
   too.

4. **Read the output**
   `out/install-variants/topics/` has three copies
   of each of the two topics:

   ```
   win-installing-audacity.html
   win-recording-your-first-track.html
   mac-installing-audacity.html
   mac-recording-your-first-track.html
   installing-audacity-linux.html
   recording-your-first-track-linux.html
   ```

   and the table of contents in `index.html` lists *Installing Audacity*
   and *Recording your first track* three times, in that order. Open
   `win-installing-audacity.html` and choose **View** > **Preview in Tab**
   to see it as a browser shows it: the choice table has the `.exe`
   row only; `installing-audacity-linux.html` has the `apt install` row,
   and inside a variant the links stay in it:
   `win-recording-your-first-track.html` links to
   `win-installing-audacity.html`.

   The folder also holds pages the map never lists,
   `what-is-digital-audio.html`, `trimming-audio.html` and the rest, and
   an unprefixed `installing-audacity.html` with all three rows. The two
   branch topics link to other topics by `@href`, and DITA-OT writes every
   link target it can reach, once and unfiltered. A link by `@href` into
   the branch reaches the original, not a variant.
::::

Test conflicting output names. Change the Linux branch's `<dvrResourceSuffix>`
to `<dvrResourcePrefix>mac-</dvrResourcePrefix>`, the same prefix as the
Mac branch, and list the branches:

```
win-                 filters topics/installing-audacity.dita ditaval=filters/platform-windows.ditaval  keyscope=win.
mac-                 filters topics/installing-audacity.dita ditaval=filters/platform-mac.ditaval  keyscope=mac.
mac-                 filters topics/installing-audacity.dita ditaval=filters/platform-linux.ditaval  keyscope=linux.
3 branch variant(s).
```

Two variants now want the same file names. DITA-OT logs no error and no
warning, and the output has no Linux pages at all.
`mac-installing-audacity.html` is the Linux copy (its choice table has the
`apt install` row), the Mac copy is gone, and the table of contents links
the Mac variant to generated names for pages that were never written.
A build alone does not show the problem, so run the full check: choose
**Project** > **Check Project**, or check this deliverable only with
`dogsbay-xml check --deliverable=install-variants .`. The build succeeds,
and the output check finds the two table of contents links. The output
looks like this example:

```
health   clean, with warnings
  unused key: start-here  [/home/you/my-audacity-guide/audacity-guide.ditamap:27] — nothing references it
  unused key: digital-audio  [/home/you/my-audacity-guide/audacity-guide.ditamap:34] — nothing references it
  unused key: podcast-workflow  [/home/you/my-audacity-guide/audacity-guide.ditamap:50] — nothing references it
build    install-variants     ok  /home/you/my-audacity-guide/out/install-variants
  7 note(s) — run with --verbose to see them
output   2 broken link(s)
  /home/you/my-audacity-guide/out/install-variants/index.html:23  @href="e676c532f446de854f19d505d53ab754fc11517b-1.html" — there is no e676c532f446de854f19d505d53ab754fc11517b-1.html
  /home/you/my-audacity-guide/out/install-variants/index.html:24  @href="397e15314df15a1ce6ffe81cf005134f385e5428-1.html" — there is no 397e15314df15a1ce6ffe81cf005134f385e5428-1.html
Not ready: 2 links in the built output lead nowhere.
```

The generated names and line numbers can differ between runs. The check reports the symptom,
two links that lead nowhere, and not the cause. Check the `list-branches`
output, and the output folder, whenever you add a variant.

After each error exercise, undo the deliberate change and check again.
Confirm that the check reports `Ready` before you continue.

## What you learned

- `<ditavalref>` under a `<topicref>`: one filters the branch in place,
  several copy it once per DITAVAL.
- `<ditavalmeta>` with `<dvrResourcePrefix>`, `<dvrResourceSuffix>` and
  `<dvrKeyscopePrefix>` names the copies and their key scopes; the
  prefixes must differ.
- A branch-filtered map needs no DITAVAL in its deliverable; a
  collision between variants builds without an error, and only the
  output check sees the links it breaks.
- `dogsbay-xml list-branches <map>` enumerates the variants.

## Next lesson

**Checkpoint:** `tutorial/16-branch-filtering`. If you use Git, commit your
work.

Continue with [Stage 17: key scopes](/part-3-conditions/stage-17-key-scopes).

For the core course, use the [learning path](/start-here/learning-path) to skip optional modules.

[Compare this stage with its predecessor](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/15-subject-scheme...tutorial/16-branch-filtering).
