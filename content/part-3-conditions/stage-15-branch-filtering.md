---
title: "Stage 15: Branch filtering"
description: Publish the install task three times from one map, once per platform, with a ditavalref on the branch and a prefix or suffix on the copies.
type: tutorial
---

# Stage 15: Branch filtering

Stage 13 filtered a whole deliverable: one DITAVAL, one map, one build. This
stage filters a *branch* of a map. `installation-variants.ditamap` refers
to the install task once and hangs three `<ditavalref>`s under it, one per
platform. DITA-OT copies the branch three times, filters each copy with its
DITAVAL, and gives the copies distinct file names from a prefix or suffix
named in `<ditavalmeta>`. One map, one build, and the reader of the output
sees *Installing Audacity* for Windows, for macOS and for Linux side by
side.

Three small DITAVALs, one map, one deliverable.

**Time:** about 20 minutes.
**You need:** stage 14 complete.

## Step 1: A DITAVAL per platform

::::steps
1. **Create `filters/platform-windows.ditaval`, `filters/platform-mac.ditaval` and `filters/platform-linux.ditaval`**
   Platform only; no audience rules, because the variants map has no
   audience-specific content.

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

   ```xml title="installation-variants.ditamap"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE map PUBLIC "-//OASIS//DTD DITA Map//EN" "map.dtd">

   <map>
     <title>Installing Audacity: one topic, three platform variants</title>
     <mapref href="subject-scheme.ditamap" type="subjectScheme"/>
     <mapref href="keydefs-product.ditamap"/>
     <mapref href="keydefs-glossary.ditamap"/>
     <keydef keys="common-notes" href="shared/common-notes.dita"/>
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
     `linux.install`, and the same for `first-track`. Stage 16 is about
     key scopes.
   - The rest of the map is what any root map needs: the scheme, the
     product and glossary keys, the warehouses.

3. **List the branches**

   ```bash
   dogsbay-xml list-branches installation-variants.ditamap
   ```

   ```
   win-                 filters topics/installing-audacity.dita ditaval=filters/platform-windows.ditaval  keyscope=win.
   mac-                 filters topics/installing-audacity.dita ditaval=filters/platform-mac.ditaval  keyscope=mac.
   platform-linux.ditaval filters topics/installing-audacity.dita ditaval=filters/platform-linux.ditaval  keyscope=linux.
   3 branch variant(s).
   ```

   Each line is one variant: its prefix, the topic it filters, the
   DITAVAL and the key scope. A variant with no prefix is labelled by its
   DITAVAL file.
::::

## Step 3: The deliverable, the README and the gate

::::steps
1. **Edit `project.json`**
   No `profiles` entry: the filtering is in the map.

   ```diff title="project.json"
   --- a/project.json
   +++ b/project.json
   @@ -92,6 +92,17 @@
          "publication": {
            "transtype": "html5"
          }
   +    },
   +    {
   +      "name": "install-variants",
   +      "context": {
   +        "id": "install-variants",
   +        "input": "installation-variants.ditamap"
   +      },
   +      "output": "out/install-variants",
   +      "publication": {
   +        "transtype": "html5"
   +      }
        }
      ]
    }
   ```

2. **Change the "You are on" line and the layout**

   ````diff title="README.md"
   --- a/README.md
   +++ b/README.md
   @@ -5,7 +5,7 @@ branch is the previous branch plus one lesson, from a single concept topic to a
    complete, publish-ready **Audacity User Guide**. The diff between two branches
    is the lesson.
    
   -You are on **stage 14 — subject scheme**: a controlled vocabulary for the conditional attributes, checked by the tools.
   +You are on **stage 15 — branch filtering**: three platform variants of one topic from a single map.
    
    ## Stages
    
   @@ -37,6 +37,7 @@ override the defaults). A stage is done when it prints `STAGE OK`.
    
    ```
    audacity-guide.ditamap  the main guide (start here)
   +installation-variants.ditamap  branch filtering: one install topic, three platform variants
    beginner-guide.ditamap, podcaster-guide.ditamap  audience-specific guides that reuse the same topics
    filters/              DITAVAL files: what each deliverable includes, excludes or flags
    keydefs-product.ditamap key definitions: product name, version, download URL, project extension
   ````

3. **Check**

   ```bash
   scripts/check-stage.sh
   ```

   ```
   == validate-project  /home/you/audacity-guide ==
   33 file(s): 33 valid, 0 invalid.

   == project-health  /home/you/audacity-guide ==
   Root map: audacity-guide.ditamap (project config); house rules: none (none configured)
   Project is healthy: valid, no broken references, keys, orphans, or broken element ids.

   == validate-conditions  /home/you/audacity-guide ==
   33 file(s): 33 pass, 0 with violations.

   == build  project.json -> /tmp/check-stage-421655 ==
   all deliverables built

   STAGE OK
   ```

4. **Read the output**
   `/tmp/check-stage-421655/out/install-variants/topics/` has three copies
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
   `win-installing-audacity.html` and the choice table has the `.exe`
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

Now collide two variants. Change the Linux branch's `<dvrResourceSuffix>`
to `<dvrResourcePrefix>mac-</dvrResourcePrefix>`, the same prefix as the
Mac branch, and list the branches:

```
win-                 filters topics/installing-audacity.dita ditaval=filters/platform-windows.ditaval  keyscope=win.
mac-                 filters topics/installing-audacity.dita ditaval=filters/platform-mac.ditaval  keyscope=mac.
mac-                 filters topics/installing-audacity.dita ditaval=filters/platform-linux.ditaval  keyscope=linux.
3 branch variant(s).
```

Two variants now want the same file names. The gate still says `STAGE OK`:
DITA-OT logs no error and no warning. But the output has no Linux pages at
all. `mac-installing-audacity.html` is the Linux copy (its choice table has
the `apt install` row), the Mac copy is gone, and the table of contents
links the second variant to `350611f215aad75b9ab4b6d1c51e836229c8b605-1.html`
and `1260618b1518ef4be91b7a2f9576e1da3ce3bd89-1.html`, generated names for
pages that were never written. A branch collision is silent; check the
`list-branches` output, and the output folder, whenever you add a variant.

## What you learned

- `<ditavalref>` under a `<topicref>`: one filters the branch in place,
  several copy it once per DITAVAL.
- `<ditavalmeta>` with `<dvrResourcePrefix>`, `<dvrResourceSuffix>` and
  `<dvrKeyscopePrefix>` names the copies and their key scopes; the
  prefixes must differ.
- A branch-filtered map needs no `profiles` in `project.json`; a
  collision between variants breaks the output without an error.
- `dogsbay-xml list-branches <map>` enumerates the variants.

## Where to go next

:::cards
- **[Stage 16: Key scopes](/part-3-conditions/stage-16-key-scopes)** {icon="arrow-right"}
  Three guides in one collection, each with its own `start-here`, and a
  peer map for links between deliverables.

- **[Compare 14 to 15 on GitHub](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/14-subject-scheme...tutorial/15-branch-filtering)** {icon="github"}
  Exactly what this stage added.
:::
