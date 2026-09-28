---
title: "Stage 10: Keys"
description: Define the product name, version and download URL once as keys, key the topics in the map, and make text and links resolve through the key space.
type: tutorial
---

# Stage 10: Keys

Define keys for the product name, version, download URL, and selected topics.
A `<keyword keyref="product-name"/>` uses text from a key definition; an
`<xref keyref="install"/>` uses a key to identify a link target. Updating
the definition changes each reference that resolves to it.

Store product values in `keydefs-product.ditamap` and include that map
with `<mapref>`. Define topic keys with `@keys` on the guide's existing
`<topicref>` elements.

**Time:** about 30 minutes.
**You need:** stage 09 complete.


Recorded output below is an example. File counts, paths, and stage numbers can differ. [Check your work](/start-here/run-the-gate) to see the result for your own project.

## Step 1: Define the product keys

::::steps
1. **Create `keydefs-product.ditamap`** in the project root.

   ```xml title="keydefs-product.ditamap"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE map PUBLIC "-//OASIS//DTD DITA Map//EN" "map.dtd">
   
   <map>
     <title>Product key definitions</title>
   
     <keydef keys="product-name">
       <topicmeta>
         <keywords>
           <keyword>Audacity</keyword>
         </keywords>
       </topicmeta>
     </keydef>
     <keydef keys="product-version">
       <topicmeta>
         <keywords>
           <keyword>3.4</keyword>
         </keywords>
       </topicmeta>
     </keydef>
     <keydef keys="download-url" href="https://www.audacityteam.org/download/" scope="external" format="html">
       <topicmeta>
         <linktext>the Audacity download page</linktext>
       </topicmeta>
     </keydef>
     <keydef keys="project-extension">
       <topicmeta>
         <keywords>
           <keyword>.aup3</keyword>
         </keywords>
       </topicmeta>
     </keydef>
   </map>
   ```

2. **Read the elements**

   - `<keydef>` is a `<topicref>` specialized for defining keys. `@keys` is
     the key name. A `<keydef>` with no `@href` defines text only.
   - The text comes from `<keywords>` inside `<topicmeta>`: the `<keyword>`
     is what a `<keyword keyref>` in a topic renders as.
   - `download-url` has an `@href` with `@scope="external"` and
     `@format="html"`, exactly as an external `<xref>` in stage 07. Its
     `<linktext>` is the text an empty `<xref keyref="download-url"/>`
     shows, because a web page has no DITA title to borrow.
   - A keydef map is an ordinary map with a `<title>`. It is never
     published on its own; the guide references it.
::::

## Step 2: Bring the keys into the guide and key the topics

::::steps
1. **Edit `audacity-guide.ditamap`**

   ```diff title="audacity-guide.ditamap"
   --- a/audacity-guide.ditamap
   +++ b/audacity-guide.ditamap
   @@ -7,11 +7,14 @@
        <shortdesc>Record, edit and export audio with Audacity, from your first track to a finished file.</shortdesc>
      </topicmeta>
    
   +  <mapref href="keydefs-product.ditamap"/>
   +  <keydef keys="start-here" href="topics/what-is-audacity.dita"/>
   +
      <topichead navtitle="Getting started">
        <topicref href="topics/what-is-audacity.dita" collection-type="family">
   -      <topicref href="topics/what-is-digital-audio.dita"/>
   +      <topicref href="topics/what-is-digital-audio.dita" keys="digital-audio"/>
        </topicref>
   -    <topicref href="topics/installing-audacity.dita"/>
   +    <topicref href="topics/installing-audacity.dita" keys="install"/>
      </topichead>
    
      <topichead navtitle="Recording and editing">
   @@ -25,7 +28,7 @@
      </topichead>
    
      <topichead navtitle="Reference" id="reference">
   -    <topicref href="topics/supported-audio-formats.dita" locktitle="yes">
   +    <topicref href="topics/supported-audio-formats.dita" keys="formats" locktitle="yes">
          <topicmeta>
            <navtitle>Audio formats</navtitle>
          </topicmeta>
   ```

2. **Read the map**

   - `<mapref href="keydefs-product.ditamap"/>` includes another map. Its
     key definitions join the guide's key space; a `<keydef>` that is not
     reachable from the root map defines nothing.
   - `<keydef keys="start-here" href="topics/what-is-audacity.dita"/>`
     gives a topic a key without adding it to the table of contents: a
     keydef is `@processing-role="resource-only"` by default.
   - `@keys` on a `<topicref>` that is already in the guide, such as
     `keys="install"`, keys the topic in place. The key space is
     `start-here`, `digital-audio`, `install`, `formats` and the four
     product keys.
::::

## Step 3: Use the keys in the topics

::::steps
1. **Edit the topics**
   Every literal "Audacity" becomes `<keyword keyref="product-name"/>`,
   the install task gains the version and the download link, and three
   links go through keys.

   ```diff title="topics/installing-audacity.dita"
   --- a/topics/installing-audacity.dita
   +++ b/topics/installing-audacity.dita
   @@ -2,19 +2,19 @@
    <!DOCTYPE task PUBLIC "-//OASIS//DTD DITA Task//EN" "task.dtd">
    
    <task id="installing-audacity">
   -  <title>Installing Audacity</title>
   -  <shortdesc>Download Audacity from the official website, or install it with your package manager, and launch it once to finish setup.</shortdesc>
   +  <title>Installing <keyword keyref="product-name"/></title>
   +  <shortdesc>Download <keyword keyref="product-name"/> from the official website, or install it with your package manager, and launch it once to finish setup.</shortdesc>
      <taskbody>
        <prereq>
          <p>You need about 250 MB of free disk space and an account that can install software.</p>
        </prereq>
        <context>
   -      <p>Audacity is available for Windows, macOS and Linux.
   +      <p><keyword keyref="product-name"/> is available for Windows, macOS and Linux.
          The steps differ by operating system only at the install step.</p>
        </context>
        <steps>
          <step>
   -        <cmd>Download Audacity from <filepath>https://www.audacityteam.org/download/</filepath>.</cmd>
   +        <cmd>Download <keyword keyref="product-name"/> <keyword keyref="product-version"/> from <xref keyref="download-url"/>.</cmd>
            <info>
              <p>Select the installer for your operating system.</p>
            </info>
   @@ -33,7 +33,7 @@
              </chrow>
              <chrow>
                <choption>macOS</choption>
   -            <chdesc>Open the downloaded <filepath>.dmg</filepath> file and drag Audacity to your <filepath>Applications</filepath> folder.</chdesc>
   +            <chdesc>Open the downloaded <filepath>.dmg</filepath> file and drag <keyword keyref="product-name"/> to your <filepath>Applications</filepath> folder.</chdesc>
              </chrow>
              <chrow>
                <choption>Linux</choption>
   @@ -42,10 +42,10 @@
            </choicetable>
          </step>
          <step>
   -        <cmd>Launch Audacity.</cmd>
   +        <cmd>Launch <keyword keyref="product-name"/>.</cmd>
            <substeps>
              <substep>
   -            <cmd>If Audacity asks to scan for audio plugins, click <uicontrol>OK</uicontrol>.</cmd>
   +            <cmd>If <keyword keyref="product-name"/> asks to scan for audio plugins, click <uicontrol>OK</uicontrol>.</cmd>
                <info>
                  <p>This happens once, on first launch.</p>
                </info>
   @@ -60,7 +60,7 @@
          </step>
        </steps>
        <result>
   -      <p>Audacity is installed and ready to use.</p>
   +      <p><keyword keyref="product-name"/> <keyword keyref="product-version"/> is installed and ready to use.</p>
        </result>
        <postreq>
          <p>Connect a microphone, then continue with <xref href="recording-your-first-track.dita"/>.</p>
   ```

   ```diff title="topics/exporting-audio.dita"
   --- a/topics/exporting-audio.dita
   +++ b/topics/exporting-audio.dita
   @@ -7,7 +7,7 @@
      <taskbody>
        <prereq>
          <p>Finish editing and save the project.
   -      A project file (<filepath>.aup3</filepath>) can only be opened by Audacity; exporting creates an ordinary audio file.</p>
   +      A project file (<filepath>.aup3</filepath>) can only be opened by <keyword keyref="product-name"/>; exporting creates an ordinary audio file.</p>
        </prereq>
        <steps>
          <step>
   @@ -17,7 +17,7 @@
            </stepresult>
          </step>
          <step>
   -        <cmd>Choose a format (see <xref href="supported-audio-formats.dita#supported-audio-formats/choosing"/>).</cmd>
   +        <cmd>Choose a format (see <xref keyref="formats/choosing"/>).</cmd>
            <choices>
              <choice>Choose <uicontrol>WAV</uicontrol> or <uicontrol>FLAC</uicontrol> for an archive copy with no quality loss.</choice>
              <choice>Choose <uicontrol>MP3</uicontrol> or <uicontrol>OGG</uicontrol> for a small file to share.</choice>
   ```

   ```diff title="topics/what-is-audacity.dita"
   --- a/topics/what-is-audacity.dita
   +++ b/topics/what-is-audacity.dita
   @@ -2,12 +2,12 @@
    <!DOCTYPE concept PUBLIC "-//OASIS//DTD DITA Concept//EN" "concept.dtd">
    
    <concept id="what-is-audacity">
   -  <title>What is Audacity?</title>
   -  <shortdesc>Audacity is a free, open-source audio editor and recorder for Windows, macOS and Linux.</shortdesc>
   +  <title>What is <keyword keyref="product-name"/>?</title>
   +  <shortdesc><keyword keyref="product-name"/> is a free, open-source audio editor and recorder for Windows, macOS and Linux.</shortdesc>
      <conbody>
   -    <p>Audacity records live audio, imports and exports the common audio formats, and edits sound with cut, copy, paste and a library of effects.
   +    <p><keyword keyref="product-name"/> records live audio, imports and exports the common audio formats, and edits sound with cut, copy, paste and a library of effects.
        It is free to download and use, and it runs on Windows, macOS and Linux.</p>
   -    <p>With Audacity you can:</p>
   +    <p>With <keyword keyref="product-name"/> you can:</p>
        <ul>
          <li>Record live audio through a microphone or mixer.</li>
          <li>Import and export audio in many formats, including WAV, MP3, OGG and FLAC.</li>
   @@ -19,12 +19,13 @@
          <p>Podcasters record and clean up their episodes.
          Archivists digitize vinyl records and cassettes.
          Video makers edit sound effects and voice-overs.
   -      Whatever the project, Audacity provides the tools at no cost.</p>
   +      Whatever the project, <keyword keyref="product-name"/> provides the tools at no cost.</p>
        </section>
      </conbody>
      <related-links>
   +    <link keyref="install"/>
        <link href="https://www.audacityteam.org/" scope="external" format="html">
   -      <linktext>The Audacity website</linktext>
   +      <linktext>The <keyword keyref="product-name"/> website</linktext>
          <desc>Downloads, the manual and the user forum.</desc>
        </link>
      </related-links>
   ```

   ```diff title="topics/recording-your-first-track.dita"
   --- a/topics/recording-your-first-track.dita
   +++ b/topics/recording-your-first-track.dita
   @@ -3,7 +3,7 @@
    
    <task id="recording-your-first-track">
      <title>Recording your first track</title>
   -  <shortdesc>Make your first recording in Audacity: pick a microphone, record, stop and play it back.</shortdesc>
   +  <shortdesc>Make your first recording in <keyword keyref="product-name"/>: pick a microphone, record, stop and play it back.</shortdesc>
      <taskbody>
        <context>
          <p>This task walks you through making your first audio recording.
   @@ -13,9 +13,9 @@
        </context>
        <steps>
          <step>
   -        <cmd>Open Audacity.</cmd>
   +        <cmd>Open <keyword keyref="product-name"/>.</cmd>
            <info>
   -          <p>On first launch Audacity opens an empty project window with a gray waveform area.</p>
   +          <p>On first launch <keyword keyref="product-name"/> opens an empty project window with a gray waveform area.</p>
            </info>
          </step>
          <step>
   @@ -44,7 +44,7 @@
          <step>
            <cmd>Click <uicontrol>Record</uicontrol>, or press <uicontrol>R</uicontrol>, to start recording.</cmd>
            <info>
   -          <p>A blue waveform appears as Audacity captures audio from your microphone, like <xref href="what-is-digital-audio.dita#what-is-digital-audio/fig-waveform"/>.</p>
   +          <p>A blue waveform appears as <keyword keyref="product-name"/> captures audio from your microphone, like <xref href="what-is-digital-audio.dita#what-is-digital-audio/fig-waveform"/>.</p>
              <note type="warning">A flat line instead of a waveform means the microphone is not selected correctly.
              Click <uicontrol>Stop</uicontrol> and check the recording device.</note>
            </info>
   ```

   ```diff title="topics/supported-audio-formats.dita"
   --- a/topics/supported-audio-formats.dita
   +++ b/topics/supported-audio-formats.dita
   @@ -3,10 +3,10 @@
    
    <reference id="supported-audio-formats">
      <title>Supported audio formats</title>
   -  <shortdesc>The audio formats Audacity imports and exports, and when to use each one.</shortdesc>
   +  <shortdesc>The audio formats <keyword keyref="product-name"/> imports and exports, and when to use each one.</shortdesc>
      <refbody>
        <section>
   -      <p>Audacity imports and exports audio in several formats.
   +      <p><keyword keyref="product-name"/> imports and exports audio in several formats.
          <term>Lossless</term> formats keep every sample of the source; <term>lossy</term> formats discard detail to make smaller files.
          Exported files are named after the project, so <filepath>podcast-episode-1.aup3</filepath> exports as <filepath>podcast-episode-1.mp3</filepath>.</p>
        </section>
   @@ -72,7 +72,7 @@
      </refbody>
      <related-links>
        <link href="https://manual.audacityteam.org/man/file_formats.html" scope="external" format="html">
   -      <linktext>File formats in the Audacity Manual</linktext>
   +      <linktext>File formats in the <keyword keyref="product-name"/> Manual</linktext>
        </link>
      </related-links>
    </reference>
   ```

   ```diff title="topics/trimming-audio.dita"
   --- a/topics/trimming-audio.dita
   +++ b/topics/trimming-audio.dita
   @@ -37,7 +37,7 @@
          <step>
            <cmd>Save the project with <menucascade><uicontrol>File</uicontrol><uicontrol>Save Project</uicontrol><uicontrol>Save Project</uicontrol></menucascade>.</cmd>
            <info>
   -          <p>Audacity saves the project as a single <filepath>.aup3</filepath> file in the folder you choose; the <wintitle>Save Project</wintitle> dialog suggests your Documents folder.</p>
   +          <p><keyword keyref="product-name"/> saves the project as a single <filepath>.aup3</filepath> file in the folder you choose; the <wintitle>Save Project</wintitle> dialog suggests your Documents folder.</p>
            </info>
          </step>
        </steps>
   ```

   ```diff title="topics/removing-background-noise.dita"
   --- a/topics/removing-background-noise.dita
   +++ b/topics/removing-background-noise.dita
   @@ -25,7 +25,7 @@
          <step>
            <cmd>Choose <menucascade><uicontrol>Effect</uicontrol><uicontrol>Noise Removal and Repair</uicontrol><uicontrol>Noise Reduction</uicontrol></menucascade>, then click <uicontrol>Get Noise Profile</uicontrol>.</cmd>
            <stepresult>
   -          <p>Audacity captures the character of the noise and closes the dialog.</p>
   +          <p><keyword keyref="product-name"/> captures the character of the noise and closes the dialog.</p>
            </stepresult>
          </step>
          <step>
   ```

   ```diff title="topics/what-is-digital-audio.dita"
   --- a/topics/what-is-digital-audio.dita
   +++ b/topics/what-is-digital-audio.dita
   @@ -65,7 +65,7 @@
            <strow>
              <stentry>32-bit float</stentry>
              <stentry>effectively unlimited</stentry>
   -          <stentry>editing inside Audacity</stentry>
   +          <stentry>editing inside <keyword keyref="product-name"/></stentry>
            </strow>
          </simpletable>
        </section>
   ```

2. **Read the four forms of reference**

   - `<keyword keyref="product-name"/>`: variable text. The element is
     empty; the processor fills in the `<keyword>` from the keydef. It
     works in a `<title>` and a `<shortdesc>` as well as in a `<p>`.
   - `<xref keyref="download-url"/>`: a link through a key to an external
     page. The text is the keydef's `<linktext>`.
   - `<xref keyref="formats/choosing"/>`: a link to an element inside a
     keyed topic. `key/element-id` replaces `file#topic/element-id`; the
     topic id is not needed because the key already names the topic.
   - `<link keyref="install"/>`: a related link through a key, with the
     text taken from the target's title as before.
   - Keep the example file name `<filepath>podcast-episode-1.aup3</filepath>`
     literal. Stage 11 uses `project-extension` for the extension itself.
::::

## Step 4: List the key space

::::steps
1. **Run the keys command**
   The DogsBay XML command line resolves the key space of a root map. Pass
   the root map as the argument.

   ```bash
   dogsbay-xml keys audacity-guide.ditamap
   ```

   ```
   start-here  →  topics/what-is-audacity.dita   [/home/you/audacity-guide/audacity-guide.ditamap:11]  file: /home/you/audacity-guide/topics/what-is-audacity.dita
   digital-audio  →  topics/what-is-digital-audio.dita   [/home/you/audacity-guide/audacity-guide.ditamap:15]  file: /home/you/audacity-guide/topics/what-is-digital-audio.dita
   install  →  topics/installing-audacity.dita   [/home/you/audacity-guide/audacity-guide.ditamap:17]  file: /home/you/audacity-guide/topics/installing-audacity.dita
   formats  →  topics/supported-audio-formats.dita   [/home/you/audacity-guide/audacity-guide.ditamap:31]  file: /home/you/audacity-guide/topics/supported-audio-formats.dita
   product-name  →  "Audacity"   [/home/you/audacity-guide/keydefs-product.ditamap:7]
   product-version  →  "3.4"   [/home/you/audacity-guide/keydefs-product.ditamap:14]
   download-url  →  "the Audacity download page"   [/home/you/audacity-guide/keydefs-product.ditamap:21]
   project-extension  →  ".aup3"   [/home/you/audacity-guide/keydefs-product.ditamap:26]
   8 key(s).
   ```

   A topic key shows the file it resolves to; a text key shows its text;
   each shows the map and line that defines it. Keys from the `<mapref>`
   are listed as part of the guide's key space.
::::

## Step 5: Update the README and check your work

::::steps
1. **Change the "You are on" line and the layout**

   ````diff title="README.md"
   --- a/README.md
   +++ b/README.md
   @@ -2,7 +2,7 @@
    
    Build a guide through 27 stages. Maps begin at stage 02; HTML publication begins at stage 03.
    
   -You are on stage 09: map structure.
   +You are on stage 10: keys.
    
    ## Stages
    
   ````

2. **Format and check your work**

   Format the topics:

   ```bash
   dogsbay-xml format -i topics/*.dita
   ```

   Then check the project. In the editor, choose **Project** >
   **Check Project** and read the result in the **Project Validation**
   panel. From the command line, run:

   ```bash
   dogsbay-xml check .
   ```

   Example output:

   ```
   health   clean, with warnings
     unused key: start-here  [/home/you/audacity-guide/audacity-guide.ditamap:11] — nothing references it
     unused key: digital-audio  [/home/you/audacity-guide/audacity-guide.ditamap:15] — nothing references it
     unused key: project-extension  [/home/you/audacity-guide/keydefs-product.ditamap:26] — nothing references it
   build    full                 ok  /home/you/audacity-guide/out/full
   output   clean (full)
   Ready: the project is healthy, every deliverable built, and the output of full holds together. 3 unused keys above: worth knowing, and not treated as failures.
   ```

   The three unused keys are defined for later stages: stage 11 uses
   `project-extension` in a reused step, and stage 18 uses `start-here`
   and `digital-audio` in a new topic.

   The check builds the guide into `out/full/`.
   Open `out/full/topics/installing-audacity.html`: the
   title reads *Installing Audacity*, the download step reads "Download
   Audacity 3.4 from the Audacity download page" with the link on the last
   five words, after DITA-OT resolves the key references.
::::

Test an undefined key reference.
Change both occurrences of `keyref="product-version"` in the install task to
`product-verson` and check your work. The check stops at health and
names both references. Example output:

```
health   NOT CLEAN
  /home/you/audacity-guide/topics/installing-audacity.dita:17  @keyref="product-verson" — key 'product-verson' not defined
  /home/you/audacity-guide/topics/installing-audacity.dita:63  @keyref="product-verson" — key 'product-verson' not defined
  (run project-health for the full report)
  unused key: start-here  [/home/you/audacity-guide/audacity-guide.ditamap:11] — nothing references it
  unused key: digital-audio  [/home/you/audacity-guide/audacity-guide.ditamap:15] — nothing references it
  unused key: product-version  [/home/you/audacity-guide/keydefs-product.ditamap:14] — nothing references it
  unused key: project-extension  [/home/you/audacity-guide/keydefs-product.ditamap:26] — nothing references it
Not ready: stopped at health — the project itself has faults, so nothing was built and no output was read.
```

The **Project Validation** panel lists the same problems. From the command
line, `dogsbay-xml project-health .` prints the full report:

```
Root map: audacity-guide.ditamap (project config); house rules: none (none configured)
Undefined keys (2):
  /home/you/audacity-guide/topics/installing-audacity.dita:17  key 'product-verson' not defined
  /home/you/audacity-guide/topics/installing-audacity.dita:63  key 'product-verson' not defined
Unused keys (4):
  start-here  [/home/you/audacity-guide/audacity-guide.ditamap:11]
  digital-audio  [/home/you/audacity-guide/audacity-guide.ditamap:15]
  product-version  [/home/you/audacity-guide/keydefs-product.ditamap:14]
  project-extension  [/home/you/audacity-guide/keydefs-product.ditamap:26]

Summary
  Undefined keys                2
  Unused keys                   4
```

The file validates, since `@keyref` is only a
name to the DTD, and DITA-OT would build the page with the version missing.
The unused keys are warnings. `product-version` appears in the list now
because the misspelled references no longer use it.

Undo the change and check again. Confirm that the check reports `Ready`
before you continue.

## What you learned

- `<keydef keys="…">` with `<keywords>`/`<keyword>` defines variable text;
  with `@href` it defines a link target, and `<linktext>` names an external
  one.
- `<mapref>` includes a map, keys and all; `@keys` on a `<topicref>` keys a
  topic in place; a `<keydef>` is resource-only and stays out of the table
  of contents.
- `<keyword keyref>`, `<xref keyref>`, `<link keyref>`, and
  `key/element-id` for an element inside a keyed topic.
- `dogsbay-xml keys <rootmap>` lists the key space; `project-health`
  reports undefined keys.

## Next lesson

Continue with [Stage 11: reuse](/part-2-maps/stage-11-reuse).

For the core course, use the [learning path](/start-here/learning-path) to skip optional modules.

[Compare this stage with its predecessor](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/09-map-structure...tutorial/10-keys).
