---
title: "Stage 18: Chunking and output"
description: Combine output pages, test splitting in a separate example, and control publication names and visibility.
type: tutorial
---

# Stage 18: Chunking and output

Control how topics become pages. Combine topics with `chunk="to-content"`,
test `chunk="by-topic"` with an isolated example, and assign an output name
with `copy-to`.

**Optional module.** See [Choose a learning path](/start-here/learning-path).
**You need:** stage 17 complete. **Time:** about 35 minutes.

Recorded output below is an example. File counts, paths, and stage numbers can differ. [Check your work](/start-here/run-the-gate) to see the result for your own project.

## Add the production topics

Create the following files. The effects reference and presets use separate
source files. Each gets an explicit topicref in the production maps.
The effect-order concept supports the reusable cleanup branch. The guide
information topic is published in print output.

```xml title="topics/effects-reference.dita"
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE reference PUBLIC "-//OASIS//DTD DITA Reference//EN" "reference.dtd">

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
</reference>
```

```xml title="topics/effect-presets.dita"
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE reference PUBLIC "-//OASIS//DTD DITA Reference//EN" "reference.dtd">

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
```

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

## Update the production maps

Apply the map changes below. `to-content` combines the introductory topics.
`print="printonly"` restricts the guide information topic to print output;
`toc="no"` and `search="no"` control navigation and search where supported.

The `cleanup` topicset names a reusable branch. The podcaster map includes
that branch with `topicsetref`. The beginner map uses `copy-to` to give the
audio-formats topic a different output filename.

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
+    <topicref href="topics/effects-reference.dita" keys="effects"/>
+    <topicref href="topics/effect-presets.dita"/>
   </topichead>
   <topichead navtitle="Glossary">
     <topicref href="topics/glossary/audio-units.dita"/>
```

```diff title="beginner-guide.ditamap"
--- a/beginner-guide.ditamap
+++ b/beginner-guide.ditamap
@@ -23,6 +23,7 @@
     <topicref href="topics/exporting-audio.dita"/>
   </topichead>
   <topichead navtitle="Quick reference">
-    <topicref href="topics/supported-audio-formats.dita" keys="formats"/>
+    <!-- The same source topic, published under a different file name in this guide. -->
+    <topicref href="topics/supported-audio-formats.dita" keys="formats" copy-to="topics/formats-quick-reference.dita"/>
   </topichead>
 </map>
```

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
@@ -31,6 +31,8 @@
   </topichead>
   <topichead navtitle="Reference">
     <topicref href="topics/supported-audio-formats.dita" keys="formats"/>
+    <topicref href="topics/effects-reference.dita" keys="effects"/>
+    <topicref href="topics/effect-presets.dita"/>
     <topicref href="topics/glossary/audio-units.dita"/>
     <topicref href="topics/glossary/g-compression.dita"/>
     <topicref href="topics/glossary/g-normalization.dita"/>
```

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

## Compare combined and split pages

Create these files under `examples/chunking/`. This example has its own maps
and is excluded from the deliverables in `project.json`.

```xml title="examples/chunking/topics/nested.dita"
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE reference PUBLIC "-//OASIS//DTD DITA Reference//EN" "reference.dtd">

<reference id="chunk-parent">
  <title>Chunking demonstration</title>
  <shortdesc>Compare a combined page with separate pages for nested topics.</shortdesc>
  <prolog>
    <author>The tutorial team</author>
    <metadata>
      <keywords>
        <keyword>chunking</keyword>
      </keywords>
    </metadata>
  </prolog>
  <refbody>
    <section>
      <p>The parent and child share one source file.</p>
    </section>
  </refbody>
  <reference id="chunk-child">
    <title>Nested topic</title>
    <shortdesc>The child can be published on its own page.</shortdesc>
    <prolog>
      <author>The tutorial team</author>
      <metadata>
        <keywords>
          <keyword>chunking</keyword>
        </keywords>
      </metadata>
    </prolog>
    <refbody>
      <section>
        <p>Check the generated parent and child links.</p>
      </section>
    </refbody>
  </reference>
</reference>
```

```xml title="examples/chunking/combined.ditamap"
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE map PUBLIC "-//OASIS//DTD DITA Map//EN" "map.dtd">

<map>
  <title>Chunking demonstration</title>
  <topicref href="topics/nested.dita"/>
</map>
```

```xml title="examples/chunking/split.ditamap"
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE map PUBLIC "-//OASIS//DTD DITA Map//EN" "map.dtd">

<map>
  <title>Chunking demonstration</title>
  <topicref href="topics/nested.dita" chunk="by-topic"/>
</map>
```

The example is not a deliverable, so **Check Project** does not build it.
To build both maps with the DITA-OT that the editor includes, give the
example folder a `project.json` of its own. Create
`examples/chunking/project.json`:

```json
{
  "deliverables": [
    {
      "name": "chunk-combined",
      "context": { "id": "chunk-combined", "input": "combined.ditamap" },
      "output": "out/chunk-combined",
      "publication": { "transtype": "html5", "params": [ { "name": "nav-toc", "value": "full" } ] }
    },
    {
      "name": "chunk-split",
      "context": { "id": "chunk-split", "input": "split.ditamap" },
      "output": "out/chunk-split",
      "publication": { "transtype": "html5", "params": [ { "name": "nav-toc", "value": "full" } ] }
    }
  ]
}
```

The stage branch does not contain this file. Then check the example folder
as a project of its own:

```bash
dogsbay-xml check examples/chunking
```

The output looks like this example:

```
health   clean
build    chunk-combined       ok  /home/you/audacity-guide/examples/chunking/out/chunk-combined
build    chunk-split          ok  /home/you/audacity-guide/examples/chunking/out/chunk-split
output   2 broken link(s)
  /home/you/audacity-guide/examples/chunking/out/chunk-split/97fc308b7a258ea5c2fbc53def61887c5203182b.html:12  @href="602efa29ceccc1cbad72daac54430cff43f917d2.html#chunk-parent" — there is no 602efa29ceccc1cbad72daac54430cff43f917d2.html
  /home/you/audacity-guide/examples/chunking/out/chunk-split/topics/chunk-parent.html:13  @href="97fc308b7a258ea5c2fbc53def61887c5203182b.html" — there is no topics/97fc308b7a258ea5c2fbc53def61887c5203182b.html
Not ready: 2 links in the built output lead nowhere.
```

Open each output's `index.html`. The combined version,
`out/chunk-combined/topics/nested.html`, keeps the parent and child on one
page. The split version writes two pages: `topics/chunk-parent.html` for
the parent, and a page with a generated name at the top of the output
folder for the child. The table of contents links to both pages correctly.
The links that DITA-OT 4.3.5 generates between parent and child do not:
the child link on the parent page looks for the child page in `topics/`,
and the parent link on the child page names another generated file that
was never written. Generated file names can differ between runs.

Nested topics with `chunk="by-topic"` are valid DITA. The broken links
come from how this processor version writes the split pages. Record the
results for your version and transformation, and check generated links
before you publish split pages. When you finish, delete
`examples/chunking/project.json` and `examples/chunking/out/`.

## Check the guide

In the editor, choose **Project** > **Check Project** and read the result
in the **Project Validation** panel. From the command line, run:

```bash
dogsbay-xml check .
```

Example output:

```
health   clean
build    full                 ok  /home/you/audacity-guide/out/full
build    beginner-mac         ok  /home/you/audacity-guide/out/beginner-mac
build    beginner-windows     ok  /home/you/audacity-guide/out/beginner-windows
build    podcaster-linux      ok  /home/you/audacity-guide/out/podcaster-linux
build    review               ok  /home/you/audacity-guide/out/review
build    install-variants     ok  /home/you/audacity-guide/out/install-variants
build    collection           ok  /home/you/audacity-guide/out/collection
output   clean (full, beginner-mac, beginner-windows, podcaster-linux, review, install-variants, collection)
Ready: the project is healthy, every deliverable built, and the output of full, beginner-mac, beginner-windows, podcaster-linux, review, install-variants, collection holds together.
```

The unused-key warnings are gone: *About this guide* refers to
`start-here`, `digital-audio` and `podcast-workflow`.

Check that the effects and presets pages have stable filenames
(`out/full/topics/effects-reference.html` and `effect-presets.html`), that
`out/full/topics/` has no `what-is-digital-audio.html` because
`to-content` merged it into `what-is-audacity.html`, and that the beginner
guides have `topics/formats-quick-reference.html`.
`outputclass="compact"` on the effects table supplies a CSS class; a
stylesheet must define its visual effect.

## What you learned

- `to-content` combines topics into an output document.
- `by-topic` splits nested topics; inspect generated links as well as pages.
- `copy-to` assigns an alternate output filename in a map.
- Print, navigation, and search attributes control different output features.
- `topicset` and `topicsetref` reuse map branches.

## Next lesson

Continue with [Stage 19: bookmap](/part-4-books/stage-19-bookmap).

For the core course, use the [learning path](/start-here/learning-path) to skip optional modules.

[Compare this stage with its predecessor](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/17-key-scopes...tutorial/18-chunking-and-output).
