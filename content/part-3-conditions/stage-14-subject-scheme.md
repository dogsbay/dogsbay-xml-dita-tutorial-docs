---
title: "Stage 14: Subject scheme"
description: Define the allowed values of platform and audience in a subject scheme map, reference it from every root map, and make the gate reject a value outside it.
type: tutorial
---

# Stage 14: Subject scheme

Stage 13 left a hole. Write `platform="macos"` instead of `platform="mac"`
and nothing complains: the file validates, the DITAVAL has no rule for
`macos`, so the element is included in every build, and the Windows guide
quietly gains a macOS row. A *subject scheme* closes the hole. It is a map
of a special kind that lists the values each profiling attribute may take,
and once a root map references it, DITA-OT warns on any other value and
`dogsbay-xml validate-conditions` fails on it.

One new file, `subject-scheme.ditamap`, one line in each of the three maps,
and one more step in the gate.

**Time:** about 20 minutes.
**You need:** stage 13 complete.

## Step 1: The vocabulary

::::steps
1. **Create `subject-scheme.ditamap`**

   ```xml title="subject-scheme.ditamap"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE subjectScheme PUBLIC "-//OASIS//DTD DITA Subject Scheme Map//EN" "subjectScheme.dtd">

   <subjectScheme>
     <title>Controlled values for the Audacity guide</title>
     <!-- The vocabulary: a taxonomy of subjects. -->
     <subjectdef keys="os">
       <topicmeta>
         <navtitle>Operating systems</navtitle>
       </topicmeta>
       <subjectdef keys="windows">
         <topicmeta>
           <navtitle>Windows</navtitle>
         </topicmeta>
       </subjectdef>
       <subjectdef keys="mac">
         <topicmeta>
           <navtitle>macOS</navtitle>
         </topicmeta>
       </subjectdef>
       <subjectdef keys="linux">
         <topicmeta>
           <navtitle>Linux</navtitle>
         </topicmeta>
       </subjectdef>
     </subjectdef>

     <subjectdef keys="readers">
       <topicmeta>
         <navtitle>Audiences</navtitle>
       </topicmeta>
       <subjectdef keys="beginner">
         <topicmeta>
           <navtitle>Beginners: first recording, defaults, no jargon</navtitle>
         </topicmeta>
       </subjectdef>
       <subjectdef keys="podcaster">
         <topicmeta>
           <navtitle>Podcasters: spoken word, levels, effects chain</navtitle>
         </topicmeta>
       </subjectdef>
     </subjectdef>
     <!-- The binding: which attribute may take which subjects. -->
     <enumerationdef>
       <attributedef name="platform"/>
       <subjectdef keyref="os"/>
     </enumerationdef>
     <enumerationdef>
       <attributedef name="audience"/>
       <subjectdef keyref="readers"/>
     </enumerationdef>
     <!-- Relationship: every OS subject is narrower than "os" (already implied
          by nesting; stated explicitly so tools can walk the hierarchy). -->
     <hasNarrower>
       <subjectdef keyref="os">
         <subjectdef keyref="windows"/>
         <subjectdef keyref="mac"/>
         <subjectdef keyref="linux"/>
       </subjectdef>
     </hasNarrower>
   </subjectScheme>
   ```

2. **Read the elements**
   - `<subjectScheme>` is the root, with its own DOCTYPE. Its `<title>`
     is for the editor; it is never published.
   - `<subjectdef keys="…">` defines a subject. Nested `<subjectdef>`s
     form a taxonomy: `os` has the narrower subjects `windows`, `mac` and
     `linux`; `readers` has `beginner` and `podcaster`. The `<navtitle>`
     in `<topicmeta>` is the human name, and for the audiences it doubles
     as a one-line definition of who the audience is.
   - `<enumerationdef>` binds an attribute to a subject:
     `<attributedef name="platform"/>` with `<subjectdef keyref="os"/>`
     says that `@platform` may take `os` or anything under it. The
     controlled values are the keys, not the navtitles: `mac`, not
     `macOS`.
   - `<hasNarrower>` states a relationship between subjects that the
     nesting already implies. Tools that walk the hierarchy can use it.
     `<hasRelated>`, `<hasPart>` and `<hasInstance>` are its siblings.
   - The scheme is a map, so `dogsbay-xml keys` lists its subjects as
     definition-only keys, and it takes a `<mapref>` like any map.
::::

## Step 2: Reference it from every root map

::::steps
1. **Edit `audacity-guide.ditamap`, `beginner-guide.ditamap` and `podcaster-guide.ditamap`**
   The same line in each, first among the maprefs.

   ```diff title="audacity-guide.ditamap"
   --- a/audacity-guide.ditamap
   +++ b/audacity-guide.ditamap
   @@ -21,6 +21,7 @@
        </keywords>
      </topicmeta>
    
   +  <mapref href="subject-scheme.ditamap" type="subjectScheme"/>
      <mapref href="keydefs-product.ditamap"/>
      <mapref href="keydefs-glossary.ditamap"/>
      <keydef keys="start-here" href="topics/what-is-audacity.dita"/>
   ```

   ```diff title="beginner-guide.ditamap"
   --- a/beginner-guide.ditamap
   +++ b/beginner-guide.ditamap
   @@ -3,6 +3,7 @@
    
    <map>
      <title>Audacity Beginner's Guide</title>
   +  <mapref href="subject-scheme.ditamap" type="subjectScheme"/>
      <mapref href="keydefs-product.ditamap"/>
      <mapref href="keydefs-glossary.ditamap"/>
      <keydef keys="common-notes" href="shared/common-notes.dita"/>
   ```

   ```diff title="podcaster-guide.ditamap"
   --- a/podcaster-guide.ditamap
   +++ b/podcaster-guide.ditamap
   @@ -3,6 +3,7 @@
    
    <map>
      <title>Audacity for Podcasters</title>
   +  <mapref href="subject-scheme.ditamap" type="subjectScheme"/>
      <mapref href="keydefs-product.ditamap"/>
      <mapref href="keydefs-glossary.ditamap"/>
      <keydef keys="common-notes" href="shared/common-notes.dita"/>
   ```

2. **Read the reference**
   `<mapref href="subject-scheme.ditamap" type="subjectScheme"/>` tells
   the processor that the referenced map is a scheme and not content.
   A scheme applies to the map that references it and everything under
   it; a map that does not reference one has no controlled values, which
   is why each root map carries the line. Nothing about the scheme
   appears in the output.

3. **List the subjects**

   ```bash
   dogsbay-xml list-subjects subject-scheme.ditamap
   ```

   ```
   @audience: beginner, podcaster, readers
   @platform: linux, mac, os, windows
   ```

   The parent subjects, `os` and `readers`, are valid values too.
::::

## Step 3: The gate checks the values

::::steps
1. **Edit `scripts/check-stage.sh`**
   A step between health and build, run only once a root map references a
   scheme.

   ```diff title="scripts/check-stage.sh"
   --- a/scripts/check-stage.sh
   +++ b/scripts/check-stage.sh
   @@ -5,6 +5,8 @@
    #   1. dogsbay-xml validate-project   every topic and map validates against its DOCTYPE
    #   2. dogsbay-xml project-health     links, keys, element ids, conref pushes, index,
    #                                     metadata policy and house rules are clean
   +#   2b. dogsbay-xml validate-conditions (once a subject scheme is referenced) every
   +#                                     platform/audience value is a controlled value
    #   3. dita --project=project.json    every deliverable actually publishes
    #
    # Exit code is non-zero when any gate fails. Usage:
   @@ -40,6 +42,13 @@ else
      "$DOGSBAY_XML" project-health --severity=error "$ROOT" || fail "project-health found errors"
    fi
    
   +scheme=$(grep -l '<subjectScheme' "$ROOT"/*.ditamap 2>/dev/null | head -1 || true)
   +if [ -n "$scheme" ]; then
   +  # Without -S (or -m) the command discovers no scheme and passes vacuously.
   +  say "validate-conditions  $ROOT  (scheme: ${scheme#$ROOT/})"
   +  "$DOGSBAY_XML" validate-conditions -S "$scheme" "$ROOT" || fail "profiling values outside the subject scheme"
   +fi
   +
    if [ "$SKIP_BUILD" = "1" ]; then
      say "build  skipped (SKIP_BUILD=1)"
    elif [ -f "$ROOT/project.json" ]; then
   ```

2. **Change the "You are on" line and the layout**

   ```diff title="README.md"
   --- a/README.md
   +++ b/README.md
   @@ -5,7 +5,7 @@ branch is the previous branch plus one lesson, from a single concept topic to a
    complete, publish-ready **Audacity User Guide**. The diff between two branches
    is the lesson.
    
   -You are on **stage 13 — conditional text**: platform and audience conditions, DITAVAL filters and flags, and one deliverable per audience.
   +You are on **stage 14 — subject scheme**: a controlled vocabulary for the conditional attributes, checked by the tools.
    
    ## Stages
    
   @@ -43,6 +43,7 @@ keydefs-product.ditamap key definitions: product name, version, download URL, pr
    keydefs-glossary.ditamap key definitions for the glossary entries
    topics/glossary/      glossary entries and a glossary group
    project.json          DITA-OT project file: the deliverables this guide ships
   +subject-scheme.ditamap  the controlled values for platform and audience
    .dogsbay/config.xml   shared editor project settings (project type, framework, default map and deliverable, format style)
    scripts/              the gate
    topics/               topics
   ```

3. **Check**

   ```bash
   scripts/check-stage.sh
   ```

   ```
   == validate-project  /home/you/audacity-guide ==
   29 file(s): 29 valid, 0 invalid.

   == project-health  /home/you/audacity-guide ==
   Root map: audacity-guide.ditamap (project config); house rules: none (none configured)
   Project is healthy: valid, no broken references, keys, orphans, or broken element ids.

   == validate-conditions  /home/you/audacity-guide  (scheme: subject-scheme.ditamap) ==
   29 file(s): 29 pass, 0 with violations.

   == build  project.json -> /tmp/check-stage-418211 ==
   all deliverables built

   STAGE OK
   ```

   The 29th file is the scheme itself, which validates against its own
   DTD.
::::

Now make the mistake the stage exists for. In
`topics/installing-audacity.dita`, change the macOS row to
`<chrow platform="macos">` and check the values against the scheme of the
full guide:

```bash
dogsbay-xml validate-conditions -m audacity-guide.ditamap .
```

```
/home/you/audacity-guide/topics/installing-audacity.dita:54 — @platform="macos" — “macos” is not a controlled value for @platform (did you mean “mac”?)
29 file(s): 28 pass, 1 with violations.
```

The `-m` names the map whose scheme applies; the command discovers the
scheme from the map's closure, and with `-S subject-scheme.ditamap` you can
name a scheme directly. Run without either, the scan has no scheme to check
against and every file passes. That is why the gate finds the scheme map
itself and passes it with `-S`: an early version of the gate called the
command bare and passed vacuously.

DITA-OT sees the same thing. Build the full guide and the log carries a
warning:

```
Warning: file:/home/you/audacity-guide/topics/installing-audacity.dita:54:35: [DOTJ049W] The @platform attribute value 'macos' on the <chrow> element does not comply with the specified subject scheme. According to the subject scheme map, the following values are valid for the @platform attribute: 'linux,windows,mac'.
```

It is a warning, so the build goes on, and because no DITAVAL has a rule
for `macos`, the row is included in every filtered build.
The gate greps the log for errors only; the `validate-conditions` step is
what turns this into a failure.

## What you learned

- `<subjectScheme>` with nested `<subjectdef keys>` for the taxonomy,
  `<enumerationdef>` with `<attributedef name>` and `<subjectdef keyref>`
  to bind an attribute to it, `<hasNarrower>` to state the hierarchy.
- `<mapref type="subjectScheme">` in every root map; a scheme applies only
  where it is referenced.
- `dogsbay-xml list-subjects` shows the vocabulary;
  `dogsbay-xml validate-conditions -m <rootmap> .` fails on a value outside
  it; DITA-OT warns with `DOTJ049W` and builds anyway.
- The gate gains a step that names the scheme with `-S`, guarded so that
  earlier stages without a scheme still pass.

## Where to go next

:::cards
- **[Stage 15: Branch filtering](/part-3-conditions/stage-15-branch-filtering)** {icon="arrow-right"}
  Three platform variants of the install task from one map, with
  `<ditavalref>` instead of three deliverables.

- **[Compare 13 to 14 on GitHub](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/13-conditional-text...tutorial/14-subject-scheme)** {icon="github"}
  Exactly what this stage added.
:::
