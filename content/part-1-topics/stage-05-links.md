---
title: "Stage 05: Links"
description: Add cross-references inside the text and related links at the end of each topic, link to a section by id, and link out to the web.
type: tutorial
---

# Stage 05: Links

In this stage the nine topics start pointing at each other. There are two
kinds of link in DITA. An `<xref>` is a cross-reference inside the text: "see
*Supported audio formats*". A `<link>` in `<related-links>` sits at the end of
the topic, where output collects it under "Related information", and says
what else the reader might want next.

No new topics; every change is an edit, and the diffs below are the whole
stage. The last section explains why most of these links will move into the
map in stage 08, and why writing them by hand first is still worth it.

**Time:** about 20 minutes.
**You need:** stage 04 complete.

## Step 1: Cross-references in the text

::::steps
1. **Edit `topics/installing-audacity.dita`**
   The postrequisite gets an `<xref>` to the recording task.

   ```diff title="topics/installing-audacity.dita"
   --- a/topics/installing-audacity.dita
   +++ b/topics/installing-audacity.dita
   @@ -63,7 +63,7 @@
          <p>Audacity is installed and ready to use.</p>
        </result>
        <postreq>
   -      <p>Connect a microphone before you continue to recording.</p>
   +      <p>Connect a microphone, then continue with <xref href="recording-your-first-track.dita"/>.</p>
        </postreq>
      </taskbody>
    </task>
   ```

2. **Edit `topics/exporting-audio.dita`**
   The format step links to a *section* of the reference. The target of an
   `<xref>` is `file#topic-id/element-id`; the section needs an `@id` for
   that, which the next step adds.

   ```diff title="topics/exporting-audio.dita"
   --- a/topics/exporting-audio.dita
   +++ b/topics/exporting-audio.dita
   @@ -17,7 +17,7 @@
            </stepresult>
          </step>
          <step>
   -        <cmd>Choose a format.</cmd>
   +        <cmd>Choose a format (see <xref href="supported-audio-formats.dita#supported-audio-formats/choosing"/>).</cmd>
            <choices>
              <choice>Choose <uicontrol>WAV</uicontrol> or <uicontrol>FLAC</uicontrol> for an archive copy with no quality loss.</choice>
              <choice>Choose <uicontrol>MP3</uicontrol> or <uicontrol>OGG</uicontrol> for a small file to share.</choice>
   @@ -56,4 +56,10 @@
          A 30-minute episode exports to a file of about 27 MB.</p>
        </example>
      </taskbody>
   +  <related-links>
   +    <link href="supported-audio-formats.dita"/>
   +    <link href="trimming-audio.dita">
   +      <desc>Clean up the recording before you export it.</desc>
   +    </link>
   +  </related-links>
    </task>
   ```

3. **Edit `topics/supported-audio-formats.dita`**
   `id="choosing"` on the section makes it a link target. The related links
   include an external one with `@scope="external"`, `@format="html"` and its
   own `<linktext>`, because there is no DITA title to take the text from.

   ```diff title="topics/supported-audio-formats.dita"
   --- a/topics/supported-audio-formats.dita
   +++ b/topics/supported-audio-formats.dita
   @@ -10,7 +10,7 @@
          <term>Lossless</term> formats keep every sample of the source; <term>lossy</term> formats discard detail to make smaller files.
          Exported files are named after the project, so <filepath>podcast-episode-1.aup3</filepath> exports as <filepath>podcast-episode-1.mp3</filepath>.</p>
        </section>
   -    <section>
   +    <section id="choosing">
          <title>Choosing a format</title>
          <p>Keep a lossless master and export a lossy copy for distribution:</p>
          <ol>
   @@ -70,4 +70,10 @@
          </tgroup>
        </table>
      </refbody>
   +  <related-links>
   +    <link href="exporting-audio.dita"/>
   +    <link href="https://manual.audacityteam.org/man/file_formats.html" scope="external" format="html">
   +      <linktext>File formats in the Audacity Manual</linktext>
   +    </link>
   +  </related-links>
    </reference>
   ```

4. **Read the elements**
   - `<xref href="…"/>` with no content: output supplies the target's
     `<title>` as the link text. This is the usual form for a link to a DITA
     topic, and it means a retitled topic never leaves stale link text
     behind.
   - `href="file.dita"` links to the topic. `href="file.dita#topic/element"`
     links to an element inside it that has an `@id`; the topic id is
     required in the fragment because one file can hold several topics.
   - `@scope="external"` says the target is outside the project and is not
     to be resolved or copied. `@format="html"` says what kind of thing it
     is, so the processor does not try to parse it as DITA.
   - `<link>` is the same reference as `<xref>` but as a list entry in
     `<related-links>`. It takes `<linktext>` for the text when the target
     has no title, and `<desc>` for a short description shown under the
     link.
::::

## Step 2: Related links on the remaining topics

::::steps
1. **Edit `topics/recording-your-first-track.dita`**
   A cross-reference in the context, a new `<postreq>` with two links, and a
   `<related-links>` block after `</taskbody>`.

   ```diff title="topics/recording-your-first-track.dita"
   --- a/topics/recording-your-first-track.dita
   +++ b/topics/recording-your-first-track.dita
   @@ -7,7 +7,8 @@
      <taskbody>
        <context>
          <p>This task walks you through making your first audio recording.
   -      You need a microphone connected to your computer.</p>
   +      You need a microphone connected to your computer.
   +      If you are new to audio terms such as sample rate and clipping, read <xref href="what-is-digital-audio.dita"/> first.</p>
          <note type="tip">For best results, record in a quiet room and position the microphone 15 to 30 cm from your mouth.</note>
        </context>
        <steps>
   @@ -46,5 +47,12 @@
          <p>You now have a recorded audio track in your project.
          The waveform shows the shape of the recorded audio: louder sounds appear as taller waves.</p>
        </result>
   +    <postreq>
   +      <p>Trim the false starts (<xref href="trimming-audio.dita"/>), then export the result (<xref href="exporting-audio.dita"/>).</p>
   +    </postreq>
      </taskbody>
   +  <related-links>
   +    <link href="preparing-to-record.dita"/>
   +    <link href="removing-background-noise.dita"/>
   +  </related-links>
    </task>
   ```

2. **Edit `topics/trimming-audio.dita`**

   ```diff title="topics/trimming-audio.dita"
   --- a/topics/trimming-audio.dita
   +++ b/topics/trimming-audio.dita
   @@ -45,4 +45,8 @@
          <p>The unwanted audio is gone and the track plays through without a gap.</p>
        </result>
      </taskbody>
   +  <related-links>
   +    <link href="recording-your-first-track.dita"/>
   +    <link href="exporting-audio.dita"/>
   +  </related-links>
    </task>
   ```

3. **Edit `topics/what-is-audacity.dita`**

   ```diff title="topics/what-is-audacity.dita"
   --- a/topics/what-is-audacity.dita
   +++ b/topics/what-is-audacity.dita
   @@ -22,4 +22,12 @@
          Whatever the project, Audacity provides the tools at no cost.</p>
        </section>
      </conbody>
   +  <related-links>
   +    <link href="what-is-digital-audio.dita"/>
   +    <link href="installing-audacity.dita"/>
   +    <link href="https://www.audacityteam.org/" scope="external" format="html">
   +      <linktext>The Audacity website</linktext>
   +      <desc>Downloads, the manual and the user forum.</desc>
   +    </link>
   +  </related-links>
    </concept>
   ```

4. **Edit `topics/what-is-digital-audio.dita`**
   This one groups its links. A `<linklist>` has a `<title>` and keeps its
   links in the order written; a `<linkpool>` has no title and lets the
   processor sort and merge its links with the ones the map generates later.

   ```diff title="topics/what-is-digital-audio.dita"
   --- a/topics/what-is-digital-audio.dita
   +++ b/topics/what-is-digital-audio.dita
   @@ -6,7 +6,8 @@
      <shortdesc>Digital audio is sound stored as numbers: samples taken thousands of times a second at a chosen precision.</shortdesc>
      <conbody>
        <p>Digital audio is sound that has been converted into a numerical representation.
   -    A few key concepts help you make better recordings and edits.</p>
   +    A few key concepts help you make better recordings and edits.
   +    If you would rather start recording, go straight to <xref href="recording-your-first-track.dita"/> and come back later.</p>
        <section>
          <title>Waveforms</title>
          <p>Sound travels through air as a continuous <term>waveform</term> of pressure changes.
   @@ -72,4 +73,17 @@
          <lq reftitle="The Audacity Manual">Audio is measured in decibels because the ear hears ratios, not differences.</lq>
        </section>
      </conbody>
   +  <related-links>
   +    <linklist>
   +      <title>Put it into practice</title>
   +      <link href="recording-your-first-track.dita"/>
   +      <link href="preparing-to-record.dita"/>
   +    </linklist>
   +    <linkpool>
   +      <link href="supported-audio-formats.dita"/>
   +      <link href="https://en.wikipedia.org/wiki/Sampling_(signal_processing)" scope="external" format="html">
   +        <linktext>Sampling (signal processing)</linktext>
   +      </link>
   +    </linkpool>
   +  </related-links>
    </concept>
   ```

5. **Read the placement**
   `<related-links>` is a child of the topic, after the body, not inside it.
   In a task it follows `</taskbody>`; in a concept, `</conbody>`. The
   validator rejects it anywhere else.
::::

## Step 3: Update the README and run the gate

::::steps
1. **Change the "You are on" line**

   ```diff title="README.md"
   --- a/README.md
   +++ b/README.md
   @@ -5,7 +5,7 @@ branch is the previous branch plus one lesson, from a single concept topic to a
    complete, publish-ready **Audacity User Guide**. The diff between two branches
    is the lesson.
    
   -You are on **stage 04 — rich tasks**: prerequisites, choices, substeps, examples, troubleshooting and unordered steps.
   +You are on **stage 05 — links**: cross-references, related links, link lists and external links.
    
    ## Stages
    
   ```

2. **Format and check**

   ```bash
   dogsbay-xml format -i topics/*.dita
   scripts/check-stage.sh
   ```

   ```
   == validate-project  /home/you/audacity-guide ==
   9 file(s): 9 valid, 0 invalid.

   == project-health  /home/you/audacity-guide ==
   Root map: none (none configured); house rules: none (none configured)
   Project is healthy: valid, no broken references, keys, orphans, or broken element ids.

   == build  skipped (no project.json yet) ==

   STAGE OK
   ```
::::

This is the first stage where `project-health` has something to check besides
validity: every `@href` now has to resolve. Misspell the target of the
`<link>` to *Trimming audio* in *Exporting audio* as `trimming-audo.dita` and
run the gate. The file still validates, because a broken link is not a
grammar error, but the health check reports it and the stage fails:

```
== project-health  /home/you/audacity-guide ==
Root map: none (none configured); house rules: none (none configured)
Broken references (1):
  /home/you/audacity-guide/topics/exporting-audio.dita:61  @href="trimming-audo.dita"
…
FAIL: project-health found issues
```

That is why the gate has two steps. Note what neither step catches: a wrong
fragment such as `#supported-audio-formats/choose` passes the health check
and, once there is a map, the DITA-OT build as well. The link renders and
points at the top of the topic instead of the section. Check fragments by
clicking them in the built output, or run `dogsbay-xml conref-audit`, which
resolves element ids for conref targets.

## Why links belong in the map

Every `<related-links>` block you wrote here is a statement about how topics
relate, stored inside one of the two topics. When the map arrives in stage 07
and relationship tables in stage 08, those statements move to the map, where
one `<reltable>` row says "these topics are related" once and the processor
generates the links on both sides. Topics then carry only the `<xref>`s that
the text needs, and the same topic can be related to different neighbours in
different guides.

Hand-written links first, because they show what the map generates, and
because a topic that is published on its own, or reused in a guide with no
reltable, still needs them.

## What you learned

- `<xref>` in the text; `<link>` in `<related-links>` after the body.
- Empty link elements take their text from the target's title.
- `file#topic/element` reaches a section or any element with an `@id`.
- `@scope="external"` and `@format` for links out of the project;
  `<linktext>` when there is no title to borrow; `<desc>` for a blurb.
- `<linklist>` keeps order and has a title; `<linkpool>` lets the processor
  sort and merge.
- `project-health` catches broken references that validation cannot.

## Where to go next

:::cards
- **[Stage 06: Figures](/part-1-topics/stage-06-figures)** {icon="arrow-right"}
  Images with alt text, an inline SVG, an image map and an equation.

- **[Compare 04 to 05 on GitHub](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/04-rich-tasks...tutorial/05-links)** {icon="github"}
  Exactly what this stage added.
:::
