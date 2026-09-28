---
title: "Stage 09: Map structure"
description: Nest topics, declare families and sequences, control linking, lock a navigation title, and replace hand-written related links with a relationship table.
type: tutorial
---

# Stage 09: Map structure

Organize topics into a hierarchy and a sequence. Set the navigation title
and link behavior for selected topics, then add a relationship table to
connect concepts, tasks, and references.

DITA-OT generates links from the map. Remove the corresponding links from
six topics so that each relationship is maintained in one place.

**Time:** about 25 minutes.
**You need:** stage 08 complete.


Recorded diagnostic examples below come from earlier runs. File counts, paths, and stage numbers can differ. Run the gate for your current checkout.

## Step 1: Structure the map

::::steps
1. **Edit `audacity-guide.ditamap`**

   ```diff title="audacity-guide.ditamap"
   --- a/audacity-guide.ditamap
   +++ b/audacity-guide.ditamap
   @@ -2,14 +2,63 @@
    <!DOCTYPE map PUBLIC "-//OASIS//DTD DITA Map//EN" "map.dtd">
    
    <map>
   -  <title>Audio user guide</title>
   -  <topicref href="topics/exporting-audio.dita"/>
   -  <topicref href="topics/installing-audacity.dita"/>
   -  <topicref href="topics/preparing-to-record.dita"/>
   -  <topicref href="topics/recording-your-first-track.dita"/>
   -  <topicref href="topics/removing-background-noise.dita"/>
   -  <topicref href="topics/supported-audio-formats.dita"/>
   -  <topicref href="topics/trimming-audio.dita"/>
   -  <topicref href="topics/what-is-audacity.dita"/>
   -  <topicref href="topics/what-is-digital-audio.dita"/>
   +  <title>Audacity User Guide</title>
   +  <topicmeta>
   +    <shortdesc>Record, edit and export audio with Audacity, from your first track to a finished file.</shortdesc>
   +  </topicmeta>
   +
   +  <topichead navtitle="Getting started">
   +    <topicref href="topics/what-is-audacity.dita" collection-type="family">
   +      <topicref href="topics/what-is-digital-audio.dita"/>
   +    </topicref>
   +    <topicref href="topics/installing-audacity.dita"/>
   +  </topichead>
   +
   +  <topichead navtitle="Recording and editing">
   +    <topicgroup collection-type="sequence">
   +      <topicref href="topics/preparing-to-record.dita"/>
   +      <topicref href="topics/recording-your-first-track.dita"/>
   +      <topicref href="topics/trimming-audio.dita"/>
   +      <topicref href="topics/exporting-audio.dita"/>
   +    </topicgroup>
   +    <topicref href="topics/removing-background-noise.dita" linking="targetonly"/>
   +  </topichead>
   +
   +  <topichead navtitle="Reference" id="reference">
   +    <topicref href="topics/supported-audio-formats.dita" locktitle="yes">
   +      <topicmeta>
   +        <navtitle>Audio formats</navtitle>
   +      </topicmeta>
   +    </topicref>
   +  </topichead>
   +  <reltable>
   +    <title>Concept, task and reference links</title>
   +    <relheader>
   +      <relcolspec type="concept"/>
   +      <relcolspec type="task"/>
   +      <relcolspec type="reference"/>
   +    </relheader>
   +    <relrow>
   +      <relcell>
   +        <topicref href="topics/what-is-digital-audio.dita"/>
   +      </relcell>
   +      <relcell>
   +        <topicref href="topics/preparing-to-record.dita"/>
   +        <topicref href="topics/recording-your-first-track.dita"/>
   +      </relcell>
   +      <relcell>
   +        <topicref href="topics/supported-audio-formats.dita"/>
   +      </relcell>
   +    </relrow>
   +    <relrow>
   +      <relcell/>
   +      <relcell>
   +        <topicref href="topics/exporting-audio.dita"/>
   +        <topicref href="topics/trimming-audio.dita"/>
   +      </relcell>
   +      <relcell>
   +        <topicref href="topics/supported-audio-formats.dita"/>
   +      </relcell>
   +    </relrow>
   +  </reltable>
    </map>
   ```

2. **Read the hierarchy**

   - A `<topicref>` inside a `<topicref>` is a child topic. The output nests
     it in the table of contents and generates a "Parent topic" link on the
     child. *What is digital audio?* is now a child of *What is Audacity?*.
   - `@collection-type` on the parent says how its children relate.
     `family` means the children link to each other and to the parent;
     `sequence` means they are read in order, and each child gets
     "Previous topic" and "Next topic" links.
   - `<topicgroup>` groups `<topicref>`s without adding a heading, which is
     how the four recording tasks become a sequence without a fifth entry
     in the table of contents. Compare `<topichead>`, which is a heading.
   - `@linking="targetonly"` on *Removing background noise* means other
     topics may link to it, but it generates no links of its own. The
     other values are `sourceonly`, `normal` (the default) and `none`.
   - `@id="reference"` on the `<topichead>` gives the heading an id, so it
     can be referenced like any element with an id; the map is XML too.
   - `@locktitle="yes"` with a `<navtitle>` inside `<topicmeta>` makes the
     table of contents show *Audio formats* while the page keeps its own
     title, *Supported audio formats*. Without `@locktitle`, the navigation
     title is ignored and the topic's title is used.

3. **Read the relationship table**

   - `<reltable>` is a table of relationships. Each `<relcolspec>` in the
     `<relheader>` names a column, here typed `concept`, `task` and
     `reference`, so the generated links are grouped as "Related concepts",
     "Related tasks" and "Related reference".
   - Each `<relrow>` says that everything in its cells is related. In the
     first row the concept links to both tasks and the reference, and each
     of them links back. An empty `<relcell/>` in the second row means no
     concept is involved.
   - Links are generated in both directions from one statement, and a
     topic can be related to different neighbors in a different map,
     which is what a hand-written `<link>` cannot do.
::::

## Step 2: Remove the links the map now generates

::::steps
1. **Edit the six topics**
   Each deletion is a link that a parent-child relationship, the sequence
   or the reltable now produces. Only the links with no map equivalent stay:
   the external ones, the `<linklist>` with its own title in *What is
   digital audio?*, and two links with a `<desc>` or to a topic outside the
   reltable.

   ```diff title="topics/what-is-audacity.dita"
   --- a/topics/what-is-audacity.dita
   +++ b/topics/what-is-audacity.dita
   @@ -23,8 +23,6 @@
        </section>
      </conbody>
      <related-links>
   -    <link href="what-is-digital-audio.dita"/>
   -    <link href="installing-audacity.dita"/>
        <link href="https://www.audacityteam.org/" scope="external" format="html">
          <linktext>The Audacity website</linktext>
          <desc>Downloads, the manual and the user forum.</desc>
   ```

   ```diff title="topics/what-is-digital-audio.dita"
   --- a/topics/what-is-digital-audio.dita
   +++ b/topics/what-is-digital-audio.dita
   @@ -113,7 +113,6 @@
          <link href="preparing-to-record.dita"/>
        </linklist>
        <linkpool>
   -      <link href="supported-audio-formats.dita"/>
          <link href="https://en.wikipedia.org/wiki/Sampling_(signal_processing)" scope="external" format="html">
            <linktext>Sampling (signal processing)</linktext>
          </link>
   ```

   ```diff title="topics/recording-your-first-track.dita"
   --- a/topics/recording-your-first-track.dita
   +++ b/topics/recording-your-first-track.dita
   @@ -68,7 +68,6 @@
        </postreq>
      </taskbody>
      <related-links>
   -    <link href="preparing-to-record.dita"/>
        <link href="removing-background-noise.dita"/>
      </related-links>
    </task>
   ```

   ```diff title="topics/trimming-audio.dita"
   --- a/topics/trimming-audio.dita
   +++ b/topics/trimming-audio.dita
   @@ -45,8 +45,4 @@
          <p>The unwanted audio is gone and the track plays through without a gap.</p>
        </result>
      </taskbody>
   -  <related-links>
   -    <link href="recording-your-first-track.dita"/>
   -    <link href="exporting-audio.dita"/>
   -  </related-links>
    </task>
   ```

   ```diff title="topics/exporting-audio.dita"
   --- a/topics/exporting-audio.dita
   +++ b/topics/exporting-audio.dita
   @@ -57,7 +57,6 @@
        </example>
      </taskbody>
      <related-links>
   -    <link href="supported-audio-formats.dita"/>
        <link href="trimming-audio.dita">
          <desc>Clean up the recording before you export it.</desc>
        </link>
   ```

   ```diff title="topics/supported-audio-formats.dita"
   --- a/topics/supported-audio-formats.dita
   +++ b/topics/supported-audio-formats.dita
   @@ -71,7 +71,6 @@
        </table>
      </refbody>
      <related-links>
   -    <link href="exporting-audio.dita"/>
        <link href="https://manual.audacityteam.org/man/file_formats.html" scope="external" format="html">
          <linktext>File formats in the Audacity Manual</linktext>
        </link>
   ```

2. **Read why**
   A hand-written `<link>` states a relationship inside one of the two
   topics, once, in one direction. The map states it once for both
   directions, keeps the topic free of assumptions about its neighbors,
   and lets the same topic sit in another guide with other neighbors.
   Retain topic-level links where they provide useful context, such as the
   titled `<linklist>` and its descriptions. Maps can also define external
   links and link descriptions.
::::

## Step 3: Update the README, run the gate and look at the output

::::steps
1. **Change the "You are on" line**

   ```diff title="README.md"
   --- a/README.md
   +++ b/README.md
   @@ -2,7 +2,7 @@
    
    Build a guide through 27 stages. Maps begin at stage 02; HTML publication begins at stage 03.
    
   -You are on stage 08: figures.
   +You are on stage 09: map structure.
    
    ## Stages
    
   ```

2. **Format and check**

   ```bash
   dogsbay-xml format -i topics/*.dita
   scripts/check-stage.sh
   ```

   ```
   == validate-project  /home/you/audacity-guide ==
   10 file(s): 10 valid, 0 invalid.

   == project-health  /home/you/audacity-guide ==
   Root map: audacity-guide.ditamap (project config); house rules: none (none configured)
   Project is healthy: valid, no broken references, keys, orphans, or broken element ids.

   == build  project.json -> /tmp/check-stage-357688 ==
   all deliverables built

   STAGE OK
   ```

3. **Read the generated links**
   Build with `dita --project=project.json` and open the pages under
   `out/full/topics/`. The link blocks at the foot of each page are now
   generated. The text of three of them, as DITA-OT 4.3.5 writes it:

   | Page | Generated links |
   |---|---|
   | *Recording your first track* | Previous topic: Preparing to record. Next topic: Trimming audio. Related concepts: What is digital audio? Related tasks: Removing background noise. Related reference: Supported audio formats |
   | *Supported audio formats* | Related concepts: What is digital audio? Related tasks: Preparing to record, Recording your first track, Exporting audio, Trimming audio. Related information: File formats in the Audacity Manual |
   | *Removing background noise* | none |

   The sequence gives the previous and next links, the reltable gives the
   typed groups, the hand-written external link survives as "Related
   information", and `@linking="targetonly"` leaves the noise task with no
   links at all while the recording task still links to it.
::::

`project-health` checks the reltable like any other reference. Misspell
`topics/exporting-audio.dita` in the second `<relrow>` as
`topics/export-audio.dita` and run the gate:

```
== project-health  /home/you/audacity-guide ==
Root map: audacity-guide.ditamap (project config); house rules: none (none configured)
Broken references (1):
  /home/you/audacity-guide/audacity-guide.ditamap:56  @href="topics/export-audio.dita"

Summary
  Broken references             1
FAIL: project-health found issues
```

After each error exercise, undo the deliberate change and rerun the gate.
Confirm that it prints `STAGE OK` before continuing.

## What you learned

- Nested `<topicref>`s make a hierarchy; `@collection-type="family"` and
  `"sequence"` say how children relate, and the output links accordingly.
- `<topicgroup>` groups without a heading; `<topichead>` is a heading.
- `@linking` controls which links a topic gives and receives;
  `@locktitle="yes"` with `<navtitle>` changes the table of contents but
  not the page.
- `<reltable>`, `<relheader>`, `<relcolspec type>`, `<relrow>`, `<relcell>`
  state relationships once for both directions, typed by information type.
- Use `<related-links>` for relationships that belong with the topic.

## Next lesson

Continue with [Stage 10: keys](/part-2-maps/stage-10-keys).

For the core course, use the [learning path](/start-here/learning-path) to skip optional modules.

[Compare this stage with its predecessor](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/08-figures...tutorial/09-map-structure).
