---
title: "Stage 17: Chunking and output"
description: Decide which topics become which pages with chunk and copy-to, keep a licence topic out of the web output, and reuse a map branch by topicsetref.
type: tutorial
---

# Stage 17: Chunking and output

So far one topic has meant one page. This stage takes control of that.
`chunk="to-content"` merges *What is digital audio?* into the *What is
Audacity?* page; `chunk="by-topic"` splits a file that holds two topics into
two pages; `copy-to` publishes the formats reference under a second file
name in the beginner guide; `print="printonly"` with `toc="no"` and
`search="no"` keeps a licence topic for the PDF and out of the web output
altogether. A `<topicset>` in the full guide becomes a named branch that
the podcaster guide pulls in with `<topicsetref>`, and an `outputclass` on
a table reaches the CSS.

Three new topics, and the three keys that have been idle since they were
defined finally get used. End of Part 3.

**Time:** about 35 minutes.
**You need:** stage 16 complete.

## Step 1: Two topics, one page

::::steps
1. **Edit `audacity-guide.ditamap`**
   The whole diff of the map; the other steps come back to the rest of it.

   ```diff title="audacity-guide.ditamap"
   --- a/audacity-guide.ditamap
   +++ b/audacity-guide.ditamap
   @@ -28,9 +28,12 @@
      <!-- Warehouses: pulled in by conref and conkeyref, never pages of their own. -->
      <keydef keys="common-notes" href="shared/common-notes.dita"/>
      <topicref href="shared/common-steps.dita" processing-role="resource-only"/>
   +  <!-- Print only: the licence page belongs in the PDF, not in the web TOC or search. -->
   +  <topicref href="topics/about-this-guide.dita" print="printonly" toc="no" search="no"/>
    
      <topichead navtitle="Getting started">
   -    <topicref href="topics/what-is-audacity.dita" collection-type="family">
   +    <!-- Two small topics, one page: to-content merges the child into the parent. -->
   +    <topicref href="topics/what-is-audacity.dita" collection-type="family" chunk="to-content">
          <topicref href="topics/what-is-digital-audio.dita" keys="digital-audio"/>
        </topicref>
        <topicref href="topics/installing-audacity.dita" keys="install"/>
   @@ -43,7 +46,11 @@
          <topicref href="topics/trimming-audio.dita"/>
          <topicref href="topics/exporting-audio.dita"/>
        </topicgroup>
   -    <topicref href="topics/removing-background-noise.dita" linking="targetonly"/>
   +    <!-- A topicset: a named, reusable branch other maps can pull in by topicsetref. -->
   +    <topicset id="cleanup" navtitle="Cleaning up a recording">
   +      <topicref href="topics/removing-background-noise.dita" linking="targetonly"/>
   +      <topicref href="topics/effect-order.dita"/>
   +    </topicset>
      </topichead>
    
      <topichead navtitle="Podcast production" audience="podcaster">
   @@ -56,6 +63,8 @@
            <navtitle>Audio formats</navtitle>
          </topicmeta>
        </topicref>
   +    <!-- One file, two topics: by-topic gives each its own page. -->
   +    <topicref href="topics/effects-reference.dita" keys="effects" chunk="by-topic"/>
      </topichead>
      <topichead navtitle="Glossary">
        <topicref href="topics/glossary/audio-units.dita"/>
   ```

2. **Read `chunk="to-content"`**
   On the *What is Audacity?* topicref, `chunk="to-content"` tells the
   processor to write the topicref and everything under it as one page.
   *What is digital audio?* is still its own file and its own topic; in
   the html5 output it is a second-level heading on
   `what-is-audacity.html`, and every link to it, from the reltable, the
   related links and the `<xref>`s, is rewritten to
   `what-is-audacity.html#what-is-digital-audio`. There is no
   `what-is-digital-audio.html` in `out/full/topics/` any more. The
   `keys="digital-audio"` still resolves; a key names a topic, not a page.

   Merge only topics that nothing links into by key from outside the
   chunk. The demo at the end shows what happens otherwise.
::::

## Step 2: One file, two pages

::::steps
1. **Create `topics/effects-reference.dita`**
   A reference with a second reference nested inside it.

   ```xml title="topics/effects-reference.dita"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE reference PUBLIC "-//OASIS//DTD DITA Reference//EN" "reference.dtd">

   <!-- Nested topics: two topics in one file. The map decides whether they publish
        as one page (the default) or one page each (chunk="by-topic"). -->
   <reference id="effects-reference">
     <title>Effects reference</title>
     <shortdesc>The built-in effects, what each one does, and the parameters that matter.</shortdesc>
     <prolog>
       <author type="creator">The Audacity tutorial team</author>
       <critdates>
         <created date="2026-02-10"/>
       </critdates>
       <metadata>
         <audience type="user"/>
         <category>reference</category>
         <keywords>
           <keyword>effects</keyword>
           <keyword>Amplify</keyword>
           <keyword>Compressor</keyword>
           <keyword>Normalize</keyword>
           <indexterm>effects<indexterm>reference</indexterm></indexterm>
         </keywords>
       </metadata>
     </prolog>
     <refbody>
       <section>
         <p><keyword keyref="product-name"/> includes several built-in effects.
           Select a region of audio, then choose an effect from the <uicontrol>Effect</uicontrol> menu.</p>
       </section>
       <table outputclass="compact">
         <title>Built-in audio effects</title>
         <tgroup cols="3">
           <colspec colname="effect" colwidth="1*"/>
           <colspec colname="description" colwidth="2*"/>
           <colspec colname="parameters" colwidth="2*"/>
           <thead>
             <row>
               <entry>Effect</entry>
               <entry>Description</entry>
               <entry>Key parameters</entry>
             </row>
           </thead>
           <tbody>
             <row>
               <entry>Amplify</entry>
               <entry>Increases or decreases the volume of the selection.</entry>
               <entry>Amplification (dB); <uicontrol>Allow clipping</uicontrol>.</entry>
             </row>
             <row>
               <entry>Noise Reduction</entry>
               <entry>Removes steady background noise such as hum, hiss or fan noise.
                 Needs a noise profile.</entry>
               <entry>Noise reduction (dB): 6 to 12 for mild noise, 12 to 24 for heavy; sensitivity; frequency smoothing.</entry>
             </row>
             <row>
               <entry>Compressor</entry>
               <entry>Reduces the dynamic range: quiet parts louder, loud parts quieter.</entry>
               <entry>Threshold (dB), ratio (for example 3:1), attack and release times.</entry>
             </row>
             <row>
               <entry>Normalize</entry>
               <entry>Sets the peak amplitude to a target level and optionally removes DC offset.</entry>
               <entry>Peak amplitude (dB), typically -1.0; <uicontrol>Remove DC offset</uicontrol>.</entry>
             </row>
             <row>
               <entry>Fade In, Fade Out</entry>
               <entry>Ramps the volume of the selection up from silence, or down to it.</entry>
               <entry>None: a linear fade over the selection.</entry>
             </row>
           </tbody>
         </tgroup>
       </table>
     </refbody>
     <reference id="effect-presets">
       <title>Presets</title>
       <shortdesc>Most effect dialogs can save their settings as a named preset.</shortdesc>
       <prolog>
         <author type="creator">The Audacity tutorial team</author>
         <metadata>
           <keywords>
             <keyword>presets</keyword>
           </keywords>
         </metadata>
       </prolog>
       <refbody>
         <section>
           <p>In an effect dialog, choose <menucascade><uicontrol>Presets and settings</uicontrol><uicontrol>Save Preset</uicontrol></menucascade> and give the settings a name.
             The preset then appears in the same menu for that effect in every project.</p>
         </section>
       </refbody>
     </reference>
   </reference>
   ```

2. **Read the nested topic**
   - A topic may contain topics of the same or a compatible type: a
     `<reference>` may nest a `<reference>`, after its `<refbody>`. Both
     have their own `<title>`, `<shortdesc>` and `<prolog>`, and the
     metadata policy applies to each. The DOCTYPE stays `reference`.
   - DITA also has `<dita>`, a composite root for unrelated topics in one
     file. It has no `<prolog>` of its own, and the metadata policy from
     stage 12 reports a composite as a topic without keywords, as the
     demo below shows. Nest under a real topic instead.
   - `outputclass="compact"` on the `<table>` becomes
     `class="table compact"` in the HTML, for a stylesheet to pick up.
     `@outputclass` is allowed on every element and means nothing until a
     transform or a CSS rule gives it a meaning.

3. **Read `chunk="by-topic"`**
   The full guide and, in step 4, the podcaster guide reference the file
   with `chunk="by-topic"`: one page per topic in the file, however they
   are nested. Without it, the two topics would be one page, the default
   for a nested topic. In the output, `effects-reference.html` holds the
   table, and the table of contents links it as
   `effects-reference.html#effects-reference`; *Presets* is a page of its
   own, and because it was split off from a file, DITA-OT names it by a
   hash, `9e1b93c9483b674c92c214dbfd4943686304ccb1.html`, at the output
   root. Give a nested topic that you chunk `by-topic` a `copy-to` if you
   want a readable file name.

4. **Create `topics/effect-order.dita`**
   A small concept for the topicset of step 4.

   ```xml title="topics/effect-order.dita"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE concept PUBLIC "-//OASIS//DTD DITA Concept//EN" "concept.dtd">

   <concept id="effect-order">
     <title>Why the order of effects matters</title>
     <shortdesc>Clean first, then shape: noise reduction before compression, and normalization last.</shortdesc>
     <prolog>
       <author type="creator">The Audacity tutorial team</author>
       <metadata>
         <audience type="user" experiencelevel="intermediate"/>
         <category>concept</category>
         <keywords>
           <keyword>effects</keyword>
           <keyword>order</keyword>
         </keywords>
       </metadata>
     </prolog>
     <conbody>
       <p>Every effect works on whatever the previous one left.
       Compression raises quiet passages, so background noise compressed before it is removed becomes louder and harder to profile.
       Normalization sets the final peak, so anything applied after it can push the audio into <term keyref="gl-clipping">clipping</term>.</p>
       <p>A safe order for spoken word: noise reduction, editing, compression, normalization, export.</p>
     </conbody>
   </concept>
   ```

5. **Edit `topics/podcast-production-workflow.dita`**
   The workflow now links to the effects reference by its new key.

   ```diff title="topics/podcast-production-workflow.dita"
   --- a/topics/podcast-production-workflow.dita
   +++ b/topics/podcast-production-workflow.dita
   @@ -43,7 +43,7 @@
        </section>
        <section audience="podcaster">
          <title>Post-production</title>
   -      <p>Process the audio in this order:</p>
   +      <p>Process the audio in this order (see <xref keyref="effects"/> for each effect's parameters):</p>
          <ol>
            <li><b>Noise reduction</b>: remove background noise using the noise profile from the silence.</li>
            <li><b>Editing</b>: trim mistakes, long pauses and false starts.</li>
   ```
::::

## Step 3: A topic for print only

::::steps
1. **Create `topics/about-this-guide.dita`**
   A generic `<topic>`: front matter, not a concept, task or reference.

   ```xml title="topics/about-this-guide.dita"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE topic PUBLIC "-//OASIS//DTD DITA Topic//EN" "topic.dtd">

   <topic id="about-this-guide">
     <title>About this guide</title>
     <shortdesc>Who wrote this guide, the licence it is published under, and the material it adapts.</shortdesc>
     <prolog>
       <author type="creator">The Audacity tutorial team</author>
       <metadata>
         <audience type="user"/>
         <category>front matter</category>
         <keywords>
           <keyword>licence</keyword>
           <keyword>credits</keyword>
         </keywords>
       </metadata>
     </prolog>
     <body>
       <p>This guide is published under the Creative Commons Attribution 4.0 licence.
       Its topics are adapted from the Audacity Manual, copyright the Audacity Team and the Manual's authors, under the Creative Commons Attribution 3.0 licence.</p>
       <p>New to <keyword keyref="product-name"/>? Start with <xref keyref="start-here"/>.
       New to audio? Read <xref keyref="digital-audio"/> first.
       Here for podcasting? Go straight to <xref keyref="podcast-workflow"/>.</p>
       <p><keyword keyref="product-name"/> is a registered trademark of Dominic Mazzoni.
       The Audacity Team does not endorse this guide and is not affiliated with it.</p>
     </body>
   </topic>
   ```

2. **Read the topicref**
   In the map, the licence topic is referenced with
   `print="printonly" toc="no" search="no"`:
   - `print="printonly"` means the topic is for print deliverables. The
     html5 transform does not write it: there is no `about-this-guide.html`
     in any output folder, and the PDF in stage 18 will have it.
   - `toc="no"` keeps a topic out of the table of contents, and
     `search="no"` out of the search index, when a transform does publish
     it. They are here so the intent holds if the topic is ever published
     to the web after all.
   - The topic uses `keyref="start-here"`, `keyref="digital-audio"` and
     `keyref="podcast-workflow"`, keys the full guide defines and that
     nothing has used until now. `dogsbay-xml health`
     on stage 16 lists all three under *Unused keys*, while
     `project-health` reports the project healthy: it lists unused keys,
     and recommended-metadata warnings, only alongside another finding.
     A key that nothing uses is a key that goes stale unnoticed; use
     every key you define, and run `dogsbay-xml health` when you want to
     see the idle ones.
::::

## Step 4: A second file name, and a branch shared by reference

::::steps
1. **Edit `beginner-guide.ditamap`**

   ```diff title="beginner-guide.ditamap"
   --- a/beginner-guide.ditamap
   +++ b/beginner-guide.ditamap
   @@ -21,6 +21,7 @@
        <topicref href="topics/exporting-audio.dita"/>
      </topichead>
      <topichead navtitle="Quick reference">
   -    <topicref href="topics/supported-audio-formats.dita" keys="formats"/>
   +    <!-- The same source topic, published under a different file name in this guide. -->
   +    <topicref href="topics/supported-audio-formats.dita" keys="formats" copy-to="topics/formats-quick-reference.dita"/>
      </topichead>
    </map>
   ```

2. **Read `copy-to`**
   `copy-to="topics/formats-quick-reference.dita"` publishes
   `supported-audio-formats.dita` under a new name, in this map only.
   The beginner build has `formats-quick-reference.html`, its table of
   contents links it, and the `formats/choosing` link from *Exporting
   audio* resolves to
   `formats-quick-reference.html#supported-audio-formats__choosing`. The
   source file is untouched, and the full guide still publishes it as
   `supported-audio-formats.html`. Use `copy-to` when one topic must be
   two pages, or when a generated file name (a `by-topic` chunk, a
   branch-filter copy) needs a readable one.

3. **Edit `podcaster-guide.ditamap`**

   ```diff title="podcaster-guide.ditamap"
   --- a/podcaster-guide.ditamap
   +++ b/podcaster-guide.ditamap
   @@ -23,7 +23,7 @@
        <topicref href="topics/preparing-to-record.dita"/>
        <topicref href="topics/recording-your-first-track.dita"/>
        <topicref href="topics/trimming-audio.dita"/>
   -    <topicref href="topics/removing-background-noise.dita"/>
   +    <topicsetref href="audacity-guide.ditamap#cleanup"/>
        <topicref href="topics/exporting-audio.dita"/>
      </topichead>
      <topichead navtitle="Podcast production">
   @@ -31,6 +31,7 @@
      </topichead>
      <topichead navtitle="Reference">
        <topicref href="topics/supported-audio-formats.dita" keys="formats"/>
   +    <topicref href="topics/effects-reference.dita" keys="effects" chunk="by-topic"/>
        <topicref href="topics/glossary/audio-units.dita"/>
        <topicref href="topics/glossary/g-compression.dita"/>
        <topicref href="topics/glossary/g-normalization.dita"/>
   ```

4. **Read the topicset**
   - In the full guide, `<topicset id="cleanup" navtitle="…">` is a
     `<topicref>` with an id that other maps can reference: a named,
     reusable branch. It holds the noise task and the new concept.
   - `<topicsetref href="audacity-guide.ditamap#cleanup"/>` in the
     podcaster guide pulls that branch in, in place. The podcaster table
     of contents lists *Removing background noise* and *Why the order of
     effects matters* under *Cleaning up a recording*, and the guide
     never mentions the two topics by file. Change the branch in the full
     guide and the podcaster guide follows.
   - A topicset is the map-level counterpart of a conref: reuse of
     structure, not of text.
::::

## Step 5: README and the gate

::::steps
1. **Change the "You are on" line**

   ```diff title="README.md"
   --- a/README.md
   +++ b/README.md
   @@ -5,7 +5,7 @@ branch is the previous branch plus one lesson, from a single concept topic to a
    complete, publish-ready **Audacity User Guide**. The diff between two branches
    is the lesson.
    
   -You are on **stage 16 — key scopes**: three guides in one collection without key collisions, and a peer map.
   +You are on **stage 17 — chunking and output control**: pages, file names, print-only and search-hidden topics, composite files and topicsets.
    
    ## Stages
    
   ```

2. **Format and check**

   ```bash
   dogsbay-xml format -i topics/*.dita *.ditamap
   scripts/check-stage.sh
   ```

   ```
   == validate-project  /home/you/audacity-guide ==
   37 file(s): 37 valid, 0 invalid.

   == project-health  /home/you/audacity-guide ==
   Root map: audacity-guide.ditamap (project config); house rules: none (none configured)
   Project is healthy: valid, no broken references, keys, orphans, or broken element ids.

   == validate-conditions  /home/you/audacity-guide ==
   37 file(s): 37 pass, 0 with violations.

   == build  project.json -> /tmp/check-stage-428890 ==
   all deliverables built

   STAGE OK
   ```

   And `dogsbay-xml health .` now agrees with it:

   ```
   Project is clean: no broken references, undefined keys, unused keys, or orphan topics.
   ```
::::

Now chunk the wrong branch. Put `chunk="to-content"` on the Glossary
`<topichead>` of `audacity-guide.ditamap`, so that the seven glossary
topics become one page, and run the gate:

```
== build  project.json -> /tmp/check-stage-429316 ==
Error: file:/home/you/audacity-guide/topics/what-is-digital-audio.dita:124:40: [DOTX031E]: The 'acbf71b544d7a300de3bdf64e8942257c37cc438.dita' resource is not available to resolve link information.
Error: file:/home/you/audacity-guide/topics/what-is-digital-audio.dita:40:79: [DOTX031E]: The 'c4fd876753b3dcca401f0eb415584b7133b5b3f6.dita' resource is not available to resolve link information.
Error: file:/home/you/audacity-guide/topics/what-is-digital-audio.dita:64:44: [DOTX031E]: The 'a6be0f3ada5eb185f7e449ba536a3de1bb106e21.dita' resource is not available to resolve link information.
Error: file:/home/you/audacity-guide/topics/what-is-digital-audio.dita:75:38: [DOTX031E]: The '076da363ff2582f3503502f5ee84bd84f2a8d521.dita' resource is not available to resolve link information.
FAIL: DITA-OT build reported errors (full log: /tmp/check-stage-429316.log)

STAGE FAILED
```

Every `<term keyref="gl-…">` in *What is digital audio?* now points at a
glossary entry that has been merged into a chunk, and DITA-OT 4.3.5 cannot
resolve a key link into a to-content chunk. The glossary stays as separate
files; merge only branches that nothing links into by key.

And write the effects reference as a composite. Change its DOCTYPE to
`<!DOCTYPE dita PUBLIC "-//OASIS//DTD DITA Composite//EN" "ditabase.dtd">`,
make `<dita>` the root and the two references siblings inside it. It
validates, and the gate fails at health:

```
== project-health  /home/you/audacity-guide ==
Root map: audacity-guide.ditamap (project config); house rules: none (none configured)
Metadata policy (1 of 30 files):
  /home/you/audacity-guide/topics/effects-reference.dita — [warning] recommended <author> is missing
  /home/you/audacity-guide/topics/effects-reference.dita — [error] missing required <keyword>

Summary
  Metadata policy               2  in 1 of 30 files (1 error(s), 1 warning(s))
    recommended <author> is missing                                   1
    missing required <keyword>                                        1
FAIL: project-health found issues
```

The `<dita>` root has no `<prolog>`, so the policy finds no author and no
keyword on it, whatever the topics inside carry.

## What you learned

- `chunk="to-content"` merges a branch into one page and rewrites links
  into it; `chunk="by-topic"` splits a file of nested topics into a page
  per topic, with generated names for the nested ones.
- A topic may nest a topic of a compatible type; `<dita>` is the composite
  root, and the metadata policy has no prolog to check on it.
- `copy-to` publishes one topic under another file name in one map.
- `print="printonly"`, `toc="no"`, `search="no"` on a topicref.
- `<topicset id>` names a branch; `<topicsetref href="map#id">` reuses it.
- `@outputclass` reaches the HTML `class`.
- `project-health` lists unused keys only beside another finding;
  `dogsbay-xml health` lists them on their own. Use every key.
- End of Part 3: conditions, a subject scheme, branch filtering, key
  scopes and chunking, on the same topics as Part 2.

## Where to go next

:::cards
- **[Stage 18: Bookmap](/part-4-books/stage-18-bookmap)** {icon="arrow-right"}
  Part 4 starts with a bookmap over the same topics and the PDF book it
  publishes.

- **[Compare 16 to 17 on GitHub](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/16-key-scopes...tutorial/17-chunking-and-output)** {icon="github"}
  Exactly what this stage added.
:::
