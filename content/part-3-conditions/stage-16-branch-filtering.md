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
copies. Add the map as a deliverable in `project.json`.

**Optional module.** See [Choose a learning path](/start-here/learning-path)
for the starting checkpoint and the next core lesson.

**Time:** about 20 minutes.
**You need:** stage 15 complete.


Recorded diagnostic examples below come from earlier runs. File counts, paths, and stage numbers can differ. Run the gate for your current checkout.

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
     `linux.install`, and the same for `first-track`. Stage 17 is about
     key scopes.
   - The rest of the map is what any root map needs: the scheme, the
     product and glossary keys, the shared topics.

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
   DITAVAL and the key scope. A variant with no prefix is labeled by its
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
   @@ -2,7 +2,7 @@
    
    Build a guide through 27 stages. Maps begin at stage 02; HTML publication begins at stage 03.
    
   -You are on stage 15: subject scheme.
   +You are on stage 16: branch filtering.
    
    ## Stages
    
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
   Under your gate's build directory, `out/install-variants/topics/` has three copies
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

Test conflicting output names. Change the Linux branch's `<dvrResourceSuffix>`
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

After each error exercise, undo the deliberate change and rerun the gate.
Confirm that it prints `STAGE OK` before continuing.

## What you learned

- `<ditavalref>` under a `<topicref>`: one filters the branch in place,
  several copy it once per DITAVAL.
- `<ditavalmeta>` with `<dvrResourcePrefix>`, `<dvrResourceSuffix>` and
  `<dvrKeyscopePrefix>` names the copies and their key scopes; the
  prefixes must differ.
- A branch-filtered map needs no `profiles` in `project.json`; a
  collision between variants breaks the output without an error.
- `dogsbay-xml list-branches <map>` enumerates the variants.

## Next lesson

Continue with [Stage 17: key scopes](/part-3-conditions/stage-17-key-scopes).

For the core course, use the [learning path](/start-here/learning-path) to skip optional modules.

[Compare this stage with its predecessor](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/15-subject-scheme...tutorial/16-branch-filtering).
