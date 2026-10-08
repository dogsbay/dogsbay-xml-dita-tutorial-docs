---
title: "Stage 15: Subject scheme"
description: Define the allowed values of platform and audience in a subject scheme map, reference it from every root map, and check the project for a value outside it.
type: tutorial
---

# Stage 15: Subject scheme

Define the allowed platform and audience values in a subject scheme.
Without a controlled vocabulary, `platform="macos"` passes DTD validation
even though the filters expect `mac`. Before the scheme exists, the
controlled values check also passes every file, because nothing says
which values `@platform` may take. The unrecognized value leaves the
macOS content in Windows and Linux output, and no build fails.

Create `subject-scheme.ditamap`, reference it from each root map, and
check the values against it. DITA-OT only warns about a value outside the
scheme and builds anyway. **Check Project** treats it as an error, so a
typo cannot quietly change which content a build includes.

**Time:** about 20 minutes.
**You need:** stage 14 complete.


Recorded output below is an example. File counts, paths, and stage numbers can differ. [Check your work](/start-here/run-the-gate) to see the result for your own project.

## Step 1: The vocabulary

::::steps
1. **Create `subject-scheme.ditamap`**
   In the **Explorer**, right-click `my-audacity-guide`, choose
   **New File**, and enter `subject-scheme.ditamap`. In the
   **New XML Document** dialog, click **Subject Scheme**.
   The template binds `@audience` to two sample values:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE subjectScheme PUBLIC "-//OASIS//DTD DITA Subject Scheme Map//EN" "subjectScheme.dtd">

   <subjectScheme>
     <title>Controlled Values</title>
     <subjectdef keys="audience-values">
       <subjectdef keys="internal"/>
       <subjectdef keys="customer"/>
     </subjectdef>
     <enumerationdef>
       <attributedef name="audience"/>
       <subjectdef keyref="audience-values"/>
     </enumerationdef>
   </subjectScheme>
   ```

   Replace the title, the subject definitions, and the enumeration with
   the content of the listing. If you use another editor, create the file
   and type the finished listing.

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
     binds `@platform` to the values below `os`: `windows`, `mac`, and
     `linux`. The container `os` is excluded from the enumeration.
     The values come from the keys; `macOS` is a display label.
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
      <!-- Abbreviated forms link into this glossary group; toc="no" publishes it without a TOC entry. -->
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
   `<mapref href="subject-scheme.ditamap" type="subjectScheme"/>` identifies
   the referenced map as a subject scheme.
   A scheme applies to the map that references it and everything under
   it; a map that does not reference one has no controlled values, which
   is why each root map carries the line. Nothing about the scheme
   appears in the output.

3. **List the subjects**
   Choose **Project** > **Map** > **Controlled Values...**. The dialog lists
   each attribute that the scheme governs, with its **Allowed values**:
   `@audience` with `beginner` and `podcaster`, and `@platform` with
   `linux`, `mac` and `windows`.

   From the command line:

   ```bash
   dogsbay-xml list-subjects subject-scheme.ditamap
   ```

   Example output:

   ```
   @audience: beginner, podcaster
   @platform: linux, mac, windows
   ```

   The containers `os` and `readers` are not values: the enumeration
   binds each attribute to the subjects below them. See
   [Binding controlled values to an attribute](https://docs.oasis-open.org/dita/dita/v1.3/errata01/os/complete/part2-tech-content/archSpec/base/binding-controlled-values-to-attribute.html).
::::

## Step 3: Check the values and your work

::::steps
1. **Check the controlled values**
   In the editor, choose **Project** > **Validate Files** >
   **Controlled Values (Subject Scheme)...** and read the result in the
   **Project Validation** panel. From the command line, run:

   ```bash
   dogsbay-xml validate-conditions .
   ```

   Example output:

   ```
   29 file(s): 29 pass, 0 with violations.
   ```

   The command finds the scheme through the project's root map and its
   `<mapref type="subjectScheme">`. **Check Project** runs the same check
   as part of health; this command runs it on its own, which is quicker
   after you change a profiling attribute.

2. **Build and check your work**
   Rebuild the guides: choose **Project** > **Build Deliverables...**, and
   click **Build All**. The five guides build as before; the scheme adds
   no pages of its own. To check everything as well, choose **Project** >
   **Check Project** and read the result in the **Project Validation**
   panel. From the command line, run:

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
   output   clean (full, beginner-mac, beginner-windows, podcaster-linux, review)
   Ready: the project is healthy, every deliverable built, and every link in the pages of 5 deliverables leads somewhere. 3 unused keys above: worth knowing, and not treated as failures.
   ```

   The scheme is a map with its own DOCTYPE, and health validates it like
   any other file.

   The note lines count DITA-OT notes from the build. Run
   `dogsbay-xml check -v .` to print them, or read them in the
   **Project Validation** panel after **Check Project**. Here they are
   `DOTJ031I` notes. The `full` deliverable has no DITAVAL file, and the
   `review` DITAVAL file has no rule for values such as `platform=mac`, so
   DITA-OT uses the default action and includes the content.
::::

Test a value outside the controlled vocabulary. In
`topics/installing-audacity.dita`, change the macOS row to
`<chrow platform="macos">` and save. Choose **Project** >
**Validate Files** > **Controlled Values (Subject Scheme)...**. With the
scheme in place, the check fails on the install row and suggests `mac`.
Then choose **Project** > **Check Project**. The check stops at health
and builds nothing, so no guide ships with the typo. The output looks
like this example:

```
health   NOT CLEAN
  /home/you/my-audacity-guide/topics/installing-audacity.dita:54  @platform="macos" — “macos” is not a controlled value for @platform (did you mean “mac”?)
  (run project-health for the full report)
  unused key: start-here  [/home/you/my-audacity-guide/audacity-guide.ditamap:27] — nothing references it
  unused key: digital-audio  [/home/you/my-audacity-guide/audacity-guide.ditamap:34] — nothing references it
  unused key: podcast-workflow  [/home/you/my-audacity-guide/audacity-guide.ditamap:50] — nothing references it
Not ready: the project itself has faults. The build and the built output were not checked.
```

`dogsbay-xml validate-conditions .` reports the same line:

```
/home/you/my-audacity-guide/topics/installing-audacity.dita:54 — @platform="macos" — “macos” is not a controlled value for @platform (did you mean “mac”?)
29 file(s): 28 pass, 1 with violations.
```

The value is valid against the DTD, and DITA-OT on its own builds the
project anyway. It reports the value only as a warning. To see the
warning, build the `full` deliverable with the `-v` option, which prints
DITA-OT warnings and notes:

```bash
dogsbay-xml build . full -v
```

The build reports `OK`. Among the notes, the output includes these lines:

```
    INFO  [DOTJ031I] No rule for 'platform=macos' was found in the DITAVAL file. Using the default action, or a parent prop action if specified. To remove this message, specify a rule for 'platform=macos' in the DITAVAL file.
    WARN  [DOTJ049W] file:/home/you/my-audacity-guide/topics/installing-audacity.dita:54 The @platform attribute value 'macos' on the <chrow> element does not comply with the specified subject scheme. According to the subject scheme map, the following values are valid for the @platform attribute: 'linux,windows,mac'.
```

Because no DITAVAL has a rule for `macos`, the row would be included in
every filtered build: `beginner-windows`, `beginner-mac` and
`podcaster-linux` would all show the `.dmg` row. That is why the check
treats a value outside the scheme as an error.

After each error exercise, undo the deliberate change and check your work
again. Confirm that the check reports `Ready` before you continue.

## What you learned

- `<subjectScheme>` with nested `<subjectdef keys>` for the taxonomy,
  `<enumerationdef>` with `<attributedef name>` and `<subjectdef keyref>`
  to bind an attribute to it, `<hasNarrower>` to state the hierarchy.
- `<mapref type="subjectScheme">` in every root map; a scheme applies only
  where it is referenced.
- **Project** > **Map** > **Controlled Values...**, or `dogsbay-xml list-subjects`, shows the vocabulary;
  **Project** > **Validate Files** >
  **Controlled Values (Subject Scheme)...**, or
  `dogsbay-xml validate-conditions .`, fails on a value outside it;
  DITA-OT warns with `DOTJ049W` and builds anyway, and
  `dogsbay-xml build . full -v` prints the warning.
- **Check Project** fails on a value outside the scheme, before anything
  is built.

## Next lesson

**Checkpoint:** `tutorial/15-subject-scheme`. If you use Git, commit your
work.

Continue with [Stage 16: branch filtering](/part-3-conditions/stage-16-branch-filtering).

For the core course, use the [learning path](/start-here/learning-path) to skip optional modules.

[Compare this stage with its predecessor](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/14-conditional-text...tutorial/15-subject-scheme).
