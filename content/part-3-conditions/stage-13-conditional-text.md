---
title: "Stage 13: Conditional text"
description: Mark text for a platform or an audience, write DITAVAL filters that include, exclude or flag it, and ship one deliverable per audience from the same topics.
type: tutorial
---

# Stage 13: Conditional text

In this stage one set of topics starts serving several readers. A keyboard
shortcut is `Ctrl+A` on Windows and Linux and `Cmd+A` on macOS; a tip about
the effects chain matters to a podcaster and not to a beginner; a step is new
in version 3.4 and a reviewer wants to see it marked. DITA does this with
*profiling attributes*: `@platform`, `@audience` and `@rev` on the element
that varies. A DITAVAL file then says, per build, which values to include,
which to exclude and which to flag.

Two new maps, `beginner-guide.ditamap` and `podcaster-guide.ditamap`,
reuse the same topics in a different order, and `project.json` gains four
deliverables: a beginner's guide for Mac and for Windows, a podcaster's guide
for Linux, and a review build of the full guide with nothing removed and the
conditional text flagged.

**Time:** about 40 minutes.
**You need:** stage 12 complete.

## Step 1: Mark what varies

::::steps
1. **Edit `topics/installing-audacity.dita`**
   A paragraph for one platform, and one row of the choice table per
   platform.

   ```diff title="topics/installing-audacity.dita"
   --- a/topics/installing-audacity.dita
   +++ b/topics/installing-audacity.dita
   @@ -26,6 +26,7 @@
      <taskbody>
        <prereq>
          <p>You need about 250 MB of free disk space and an account that can install software.</p>
   +      <p platform="linux">Most distributions package <keyword keyref="product-name"/>, but the packaged version may be older than <keyword keyref="product-version"/>; the AppImage from the download page is always current.</p>
        </prereq>
        <context>
          <p><keyword keyref="product-name"/> is available for Windows, macOS and Linux.
   @@ -45,16 +46,16 @@
                <choptionhd>Operating system</choptionhd>
                <chdeschd>How to install</chdeschd>
              </chhead>
   -          <chrow>
   +          <chrow platform="windows">
                <choption>Windows</choption>
                <chdesc>Double-click the downloaded <filepath>.exe</filepath> file and follow the setup wizard.
                Accept the default location, <filepath>C:\Program Files\Audacity</filepath>, unless you have a reason to change it.</chdesc>
              </chrow>
   -          <chrow>
   +          <chrow platform="mac">
                <choption>macOS</choption>
                <chdesc>Open the downloaded <filepath>.dmg</filepath> file and drag <keyword keyref="product-name"/> to your <filepath>Applications</filepath> folder.</chdesc>
              </chrow>
   -          <chrow>
   +          <chrow platform="linux">
                <choption>Linux</choption>
                <chdesc>Install from your distribution's package manager, for example <codeph>sudo apt install audacity</codeph> on Ubuntu or Debian, <codeph>sudo dnf install audacity</codeph> on Fedora.</chdesc>
              </chrow>
   ```

2. **Edit `shared/common-steps.dita` and `shared/common-notes.dita`**
   The shortcut that differs by platform becomes two `<ph>` alternatives.
   The first carries two values, `platform="windows linux"`, because the
   shortcut is the same on both.

   ```diff title="shared/common-steps.dita"
   --- a/shared/common-steps.dita
   +++ b/shared/common-steps.dita
   @@ -25,7 +25,7 @@
            <cmd>Select the region of audio you want to work on.</cmd>
            <info>
              <p>Click and drag in the waveform to select a region; the selection is highlighted.
   -          To select the whole track, press <uicontrol>Ctrl+A</uicontrol> (Windows and Linux) or <uicontrol>Cmd+A</uicontrol> (macOS).</p>
   +          To select the whole track, press <ph platform="windows linux"><uicontrol>Ctrl+A</uicontrol></ph><ph platform="mac"><uicontrol>Cmd+A</uicontrol></ph>.</p>
            </info>
          </step>
          <step id="save-project">
   ```

   ```diff title="shared/common-notes.dita"
   --- a/shared/common-notes.dita
   +++ b/shared/common-notes.dita
   @@ -21,7 +21,7 @@
      </prolog>
      <body>
        <note id="backup-warning" type="warning">This effect changes the audio data.
   -    Save the project first; you can undo with <uicontrol>Ctrl+Z</uicontrol> (Windows and Linux) or <uicontrol>Cmd+Z</uicontrol> (macOS).</note>
   +    Save the project first; you can undo with <ph platform="windows linux"><uicontrol>Ctrl+Z</uicontrol></ph><ph platform="mac"><uicontrol>Cmd+Z</uicontrol></ph>.</note>
        <note id="quiet-room-tip" type="tip">For best results, record in a quiet room and position the microphone 15 to 30 cm from your mouth.</note>
        <p>Recommended starting settings for Noise Reduction:</p>
        <ul id="noise-reduction-settings">
   ```

3. **Edit `topics/removing-background-noise.dita`**
   The same `<ph>` pattern on a `<cmd>`, and a `<note>` for podcasters.

   ```diff title="topics/removing-background-noise.dita"
   --- a/topics/removing-background-noise.dita
   +++ b/topics/removing-background-noise.dita
   @@ -27,6 +27,8 @@
          <p>Background noise such as hum from electronics, hiss from a microphone or fan noise degrades a recording.
          The Noise Reduction effect learns what the noise sounds like from a section that contains nothing else, then removes it everywhere.</p>
          <note conkeyref="common-notes/backup-warning"/>
   +      <note type="tip" audience="podcaster">For podcast recordings, apply noise reduction before compression and equalization.
   +      Clean the audio first, then shape its dynamics and tone.</note>
        </context>
        <steps>
          <step>
   @@ -46,7 +48,7 @@
            </stepresult>
          </step>
          <step>
   -        <cmd>Select the whole track with <uicontrol>Ctrl+A</uicontrol> or <uicontrol>Cmd+A</uicontrol>.</cmd>
   +        <cmd>Select the whole track with <ph platform="windows linux"><uicontrol>Ctrl+A</uicontrol></ph><ph platform="mac"><uicontrol>Cmd+A</uicontrol></ph>.</cmd>
          </step>
          <step>
            <cmd>Open <uicontrol>Noise Reduction</uicontrol> again, adjust the settings and click <uicontrol>OK</uicontrol>.</cmd>
   ```

4. **Edit `topics/what-is-digital-audio.dita` and `topics/exporting-audio.dita`**
   Whole sections for one audience, a paragraph per audience inside a
   `<stepxmp>`, and `rev="3.4"` on the step that changed in that release.

   ```diff title="topics/what-is-digital-audio.dita"
   --- a/topics/what-is-digital-audio.dita
   +++ b/topics/what-is-digital-audio.dita
   @@ -33,6 +33,8 @@
        <p>Digital audio is sound that has been converted into a numerical representation.
        A few key concepts help you make better recordings and edits.
        If you would rather start recording, go straight to <xref href="recording-your-first-track.dita"/> and come back later.</p>
   +    <p audience="beginner">You do not need all of this to start recording: the default settings work well.
   +    Come back to this topic when you want to fine-tune a recording.</p>
        <section>
          <title>Waveforms</title>
          <p>Sound travels through air as a continuous <term keyref="gl-waveform">waveform</term> of pressure changes.
   @@ -57,7 +59,7 @@
            </svg-container>
          </fig>
        </section>
   -    <section>
   +    <section audience="podcaster">
          <title>Sample rate</title>
          <p>The <term keyref="gl-sample-rate">sample rate</term> is the number of samples captured per second, measured in <abbreviated-form keyref="gl-hertz"/>.
          Common sample rates are:</p>
   @@ -68,7 +70,7 @@
          </sl>
          <p>A higher sample rate captures more detail but produces larger files.<fn>File size grows in proportion to the sample rate: a 96 kHz file is more than twice the size of the same recording at 44.1 kHz.</fn></p>
        </section>
   -    <section>
   +    <section audience="podcaster">
          <title>Bit depth</title>
          <p><term keyref="gl-bit-depth">Bit depth</term> determines how precisely each sample's amplitude is recorded.</p>
          <simpletable>
   ```

   ```diff title="topics/exporting-audio.dita"
   --- a/topics/exporting-audio.dita
   +++ b/topics/exporting-audio.dita
   @@ -48,8 +48,9 @@
              <p>For MP3, the bit rate controls the trade-off between size and quality.</p>
            </info>
            <stepxmp>
   -          <p>A spoken-word podcast exports well at <uicontrol>128 kbps</uicontrol>, <uicontrol>Mono</uicontrol>.
   +          <p audience="podcaster">A spoken-word podcast exports well at <uicontrol>128 kbps</uicontrol>, <uicontrol>Mono</uicontrol>.
              Music needs <uicontrol>192 kbps</uicontrol> or more, <uicontrol>Stereo</uicontrol>.</p>
   +          <p audience="beginner">If in doubt, keep the defaults: they suit most recordings.</p>
            </stepxmp>
          </step>
          <step>
   @@ -59,7 +60,7 @@
            </steptroubleshooting>
          </step>
          <step importance="optional">
   -        <cmd>Fill in the <wintitle>Edit Metadata Tags</wintitle> dialog and click <uicontrol>OK</uicontrol>.</cmd>
   +        <cmd rev="3.4">Fill in the <wintitle>Edit Metadata Tags</wintitle> dialog and click <uicontrol>OK</uicontrol>.</cmd>
            <info>
              <p>Title, artist and album are shown by most players.</p>
            </info>
   ```

5. **Read the attributes**
   - `@platform`, `@audience`, `@product` and `@otherprops` are the
     profiling attributes; `@props` is their common base. They go on
     almost any element: here a `<p>`, a `<chrow>`, a `<section>`, a
     `<note>`, a `<ph>`. Nothing happens to the text until a DITAVAL
     says what to do with the value.
   - Several values in one attribute, space-separated, mean *any of
     these*: `platform="windows linux"` stays in a Windows build and in a
     Linux build.
   - Two alternatives side by side, `<ph platform="windows linux">` then
     `<ph platform="mac">`, are the idiom for text that differs: exactly
     one survives each filtered build. There is no space between them,
     because the survivor is followed by the full stop.
   - `@rev` is not a filter. It names the revision that changed the
     element, and a DITAVAL can flag it; nothing is ever excluded by
     `@rev`.
   - Content without a profiling attribute is for everyone. Mark the
     exceptions, not the rule.
::::

## Step 2: A topic for one audience

::::steps
1. **Create `topics/podcast-production-workflow.dita`**
   Three sections for podcasters and one for beginners, so the same topic
   reads differently in each guide.

   ```xml title="topics/podcast-production-workflow.dita"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE concept PUBLIC "-//OASIS//DTD DITA Concept//EN" "concept.dtd">

   <concept id="podcast-production-workflow">
     <title>Podcast production workflow</title>
     <shortdesc>A typical order of work for a spoken-word podcast: prepare, record, clean up, level and export.</shortdesc>
     <prolog>
       <author type="creator">The Audacity tutorial team</author>
       <critdates>
         <created date="2026-02-01"/>
       </critdates>
       <metadata>
         <audience type="user" experiencelevel="intermediate"/>
         <category>podcasting</category>
         <keywords>
           <keyword>podcast</keyword>
           <keyword>workflow</keyword>
           <keyword>noise reduction</keyword>
           <keyword>compression</keyword>
           <indexterm>podcasting<indexterm>workflow</indexterm></indexterm>
         </keywords>
       </metadata>
     </prolog>
     <conbody>
       <p><keyword keyref="product-name"/> is a popular choice for podcast production because it is free, cross-platform and capable of professional results.
       This topic outlines a typical workflow.</p>
       <section audience="podcaster">
         <title>Pre-production</title>
         <ol>
           <li>Test your microphone levels; aim for peaks around -12 <abbreviated-form keyref="gl-decibel"/> to leave headroom.</li>
           <li>Record in a quiet room with little echo.</li>
           <li>Use a pop filter to soften plosive sounds (p, b, t).</li>
           <li>Set the project to 44,100 Hz and 32-bit float for editing flexibility.</li>
         </ol>
       </section>
       <section audience="podcaster">
         <title>Recording</title>
         <ol>
           <li>Record a few seconds of silence at the start for a noise profile.</li>
           <li>Press <uicontrol>R</uicontrol> to start recording; monitor the meter and stay out of the red.</li>
           <li>If you make a mistake, pause briefly and say the sentence again; you will cut the first attempt later.</li>
         </ol>
       </section>
       <section audience="podcaster">
         <title>Post-production</title>
         <p>Process the audio in this order:</p>
         <ol>
           <li><b>Noise reduction</b>: remove background noise using the noise profile from the silence.</li>
           <li><b>Editing</b>: trim mistakes, long pauses and false starts.</li>
           <li><b><term keyref="gl-normalization">Normalization</term></b>: bring the peak level to -1.0 dB.</li>
           <li><b><term keyref="gl-compression">Compression</term></b>: even out volume differences (threshold -18 dB, ratio 3:1).</li>
           <li><b>Export</b>: MP3 at 128 kbps mono for distribution.</li>
         </ol>
       </section>
       <section audience="beginner">
         <title>Getting started with podcasting</title>
         <p>If you are new to podcasting, start simple: record, trim the beginning and end, and export as MP3.
         As you gain experience, explore noise reduction, compression and the other effects to polish your sound.</p>
       </section>
     </conbody>
   </concept>
   ```

2. **Edit `audacity-guide.ditamap`**
   The full guide gets the topic under a `<topichead>` marked
   `audience="podcaster"`, so a build that excludes podcasters drops the
   whole branch, heading and all. The topic gets a key, which stage 17
   uses.

   ```diff title="audacity-guide.ditamap"
   --- a/audacity-guide.ditamap
   +++ b/audacity-guide.ditamap
   @@ -45,6 +45,10 @@
        <topicref href="topics/removing-background-noise.dita" linking="targetonly"/>
      </topichead>
    
   +  <topichead navtitle="Podcast production" audience="podcaster">
   +    <topicref href="topics/podcast-production-workflow.dita" keys="podcast-workflow"/>
   +  </topichead>
   +
      <topichead navtitle="Reference" id="reference">
        <topicref href="topics/supported-audio-formats.dita" keys="formats" locktitle="yes">
          <topicmeta>
   ```

3. **Read the map**
   Profiling attributes on a `<topicref>` or `<topichead>` filter the
   branch: when the value is excluded, the topic is not in the table of
   contents and its page is not written. A `<p audience="beginner">`
   inside a topic removes one paragraph; a `<topichead audience="…">`
   removes a chapter.
::::

## Step 3: The filters

::::steps
1. **Create `filters/mac-beginner.ditaval`, `filters/windows-beginner.ditaval` and `filters/linux-podcaster.ditaval`**
   One file per deliverable. Each names every value of every attribute the
   topics use.

   ```text title="filters/mac-beginner.ditaval"
   <?xml version="1.0" encoding="UTF-8"?>

   <val>
     <prop att="platform" val="windows" action="exclude"/>
     <prop att="platform" val="mac" action="include"/>
     <prop att="platform" val="linux" action="exclude"/>
     <prop att="audience" val="beginner" action="include"/>
     <prop att="audience" val="podcaster" action="exclude"/>
   </val>
   ```

   ```text title="filters/windows-beginner.ditaval"
   <?xml version="1.0" encoding="UTF-8"?>

   <val>
     <prop att="platform" val="windows" action="include"/>
     <prop att="platform" val="mac" action="exclude"/>
     <prop att="platform" val="linux" action="exclude"/>
     <prop att="audience" val="beginner" action="include"/>
     <prop att="audience" val="podcaster" action="exclude"/>
   </val>
   ```

   ```text title="filters/linux-podcaster.ditaval"
   <?xml version="1.0" encoding="UTF-8"?>

   <val>
     <prop att="platform" val="windows" action="exclude"/>
     <prop att="platform" val="mac" action="exclude"/>
     <prop att="platform" val="linux" action="include"/>
     <prop att="audience" val="beginner" action="exclude"/>
     <prop att="audience" val="podcaster" action="include"/>
   </val>
   ```

2. **Create `filters/review.ditaval`**
   The review build excludes nothing. It flags podcaster content with a
   background colour and bracketed text, and draws a change bar beside
   anything marked `rev="3.4"`.

   ```text title="filters/review.ditaval"
   <?xml version="1.0" encoding="UTF-8"?>

   <!-- Review build: nothing is excluded; changed content is flagged instead. -->
   <val>
     <style-conflict background-conflict-color="yellow"/>
     <prop att="audience" val="podcaster" action="flag" backcolor="#e8f4ff">
       <startflag>
         <alt-text>[podcaster]</alt-text>
       </startflag>
       <endflag>
         <alt-text>[/podcaster]</alt-text>
       </endflag>
     </prop>
     <revprop val="3.4" action="flag" changebar="solid" color="#b00020">
       <startflag>
         <alt-text>[new in 3.4]</alt-text>
       </startflag>
     </revprop>
   </val>
   ```

3. **Read the DITAVAL**
   - The root is `<val>`. Each `<prop att="…" val="…" action="…"/>`
     is one rule: for this attribute and this value, `include`, `exclude`
     or `flag`. A `<prop>` without `val` sets the default for an
     attribute. Anything not covered is included, which is why the three
     filters spell out `exclude` for the other platform and the other
     audience.
   - `action="flag"` keeps the content and marks it. `backcolor` and
     `color` style it; `<startflag>` and `<endflag>` put text (or an
     image) before and after it, from an `<alt-text>`.
   - `<revprop val="…" action="flag">` flags elements with that `@rev`.
     `changebar="solid"` asks for a change bar; the html5 transform
     renders the colour and the start flag, and the change bar is for
     PDF.
   - `<style-conflict>` says what to do when two flags apply to one
     element: here, a yellow background.
::::

## Step 4: A map per audience

::::steps
1. **Create `beginner-guide.ditamap`**
   Seven topics in three chapters, no glossary, no reltable.

   ```xml title="beginner-guide.ditamap"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE map PUBLIC "-//OASIS//DTD DITA Map//EN" "map.dtd">

   <map>
     <title>Audacity Beginner's Guide</title>
     <mapref href="keydefs-product.ditamap"/>
     <mapref href="keydefs-glossary.ditamap"/>
     <keydef keys="common-notes" href="shared/common-notes.dita"/>
     <topicref href="shared/common-steps.dita" processing-role="resource-only"/>

     <topichead navtitle="Introduction">
       <topicref href="topics/what-is-audacity.dita"/>
       <topicref href="topics/installing-audacity.dita" keys="install"/>
     </topichead>
     <topichead navtitle="Your first recording">
       <topicref href="topics/recording-your-first-track.dita"/>
       <topicref href="topics/trimming-audio.dita"/>
       <topicref href="topics/exporting-audio.dita"/>
     </topichead>
     <topichead navtitle="Quick reference">
       <topicref href="topics/supported-audio-formats.dita" keys="formats"/>
     </topichead>
   </map>
   ```

2. **Create `podcaster-guide.ditamap`**

   ```xml title="podcaster-guide.ditamap"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE map PUBLIC "-//OASIS//DTD DITA Map//EN" "map.dtd">

   <map>
     <title>Audacity for Podcasters</title>
     <mapref href="keydefs-product.ditamap"/>
     <mapref href="keydefs-glossary.ditamap"/>
     <keydef keys="common-notes" href="shared/common-notes.dita"/>
     <topicref href="shared/common-steps.dita" processing-role="resource-only"/>

     <topichead navtitle="Getting started">
       <topicref href="topics/what-is-audacity.dita"/>
       <topicref href="topics/what-is-digital-audio.dita"/>
       <topicref href="topics/installing-audacity.dita" keys="install"/>
     </topichead>
     <topichead navtitle="Recording and editing">
       <topicref href="topics/preparing-to-record.dita"/>
       <topicref href="topics/recording-your-first-track.dita"/>
       <topicref href="topics/trimming-audio.dita"/>
       <topicref href="topics/removing-background-noise.dita"/>
       <topicref href="topics/exporting-audio.dita"/>
     </topichead>
     <topichead navtitle="Podcast production">
       <topicref href="topics/podcast-production-workflow.dita"/>
     </topichead>
     <topichead navtitle="Reference">
       <topicref href="topics/supported-audio-formats.dita" keys="formats"/>
       <topicref href="topics/glossary/audio-units.dita"/>
       <topicref href="topics/glossary/g-compression.dita"/>
       <topicref href="topics/glossary/g-normalization.dita"/>
     </topichead>
   </map>
   ```

3. **Read the maps**
   - A map is a view. The topics are the same files the full guide uses;
     what differs is which appear, in what order, under which headings.
     Combined with a filter, one topic set gives as many guides as there
     are audiences.
   - Each map defines every key its topics use. *What is Audacity?* has a
     `<link keyref="install"/>`, so both maps key the install task
     `install`; *Exporting audio* links to `formats/choosing`, so both key
     the formats reference `formats`; the conkeyrefs need `common-notes`.
     The product and glossary keys come in by `<mapref>`, and the steps
     warehouse is `resource-only`, as in the full guide. A map that
     misses one of these fails the build, as the demo below shows.
::::

## Step 5: Deliverables, README and the gate

::::steps
1. **Edit `project.json`**
   Four deliverables join `full`. Each names its map, its DITAVAL under
   `profiles.ditavals`, and its own output folder. The first hunk only
   reformats the existing `params` entry.

   ```diff title="project.json"
   --- a/project.json
   +++ b/project.json
   @@ -10,9 +10,88 @@
          "publication": {
            "transtype": "html5",
            "params": [
   -          { "name": "nav-toc", "value": "full" }
   +          {
   +            "name": "nav-toc",
   +            "value": "full"
   +          }
            ]
          }
   +    },
   +    {
   +      "name": "beginner-mac",
   +      "context": {
   +        "id": "beginner-mac",
   +        "input": "beginner-guide.ditamap",
   +        "profiles": {
   +          "ditavals": [
   +            "filters/mac-beginner.ditaval"
   +          ]
   +        }
   +      },
   +      "output": "out/beginner-mac",
   +      "publication": {
   +        "transtype": "html5",
   +        "params": [
   +          {
   +            "name": "nav-toc",
   +            "value": "partial"
   +          }
   +        ]
   +      }
   +    },
   +    {
   +      "name": "beginner-windows",
   +      "context": {
   +        "id": "beginner-windows",
   +        "input": "beginner-guide.ditamap",
   +        "profiles": {
   +          "ditavals": [
   +            "filters/windows-beginner.ditaval"
   +          ]
   +        }
   +      },
   +      "output": "out/beginner-windows",
   +      "publication": {
   +        "transtype": "html5",
   +        "params": [
   +          {
   +            "name": "nav-toc",
   +            "value": "partial"
   +          }
   +        ]
   +      }
   +    },
   +    {
   +      "name": "podcaster-linux",
   +      "context": {
   +        "id": "podcaster-linux",
   +        "input": "podcaster-guide.ditamap",
   +        "profiles": {
   +          "ditavals": [
   +            "filters/linux-podcaster.ditaval"
   +          ]
   +        }
   +      },
   +      "output": "out/podcaster-linux",
   +      "publication": {
   +        "transtype": "html5"
   +      }
   +    },
   +    {
   +      "name": "review",
   +      "context": {
   +        "id": "review",
   +        "input": "audacity-guide.ditamap",
   +        "profiles": {
   +          "ditavals": [
   +            "filters/review.ditaval"
   +          ]
   +        }
   +      },
   +      "output": "out/review",
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
    
   -You are on **stage 12 — metadata and index**: prolog metadata, index terms and a metadata policy the gate enforces.
   +You are on **stage 13 — conditional text**: platform and audience conditions, DITAVAL filters and flags, and one deliverable per audience.
    
    ## Stages
    
   @@ -36,7 +36,9 @@ override the defaults). A stage is done when it prints `STAGE OK`.
    ## Layout
    
    ```
   -audacity-guide.ditamap  the map (start here)
   +audacity-guide.ditamap  the main guide (start here)
   +beginner-guide.ditamap, podcaster-guide.ditamap  audience-specific guides that reuse the same topics
   +filters/              DITAVAL files: what each deliverable includes, excludes or flags
    keydefs-product.ditamap key definitions: product name, version, download URL, project extension
    keydefs-glossary.ditamap key definitions for the glossary entries
    topics/glossary/      glossary entries and a glossary group
   ````

3. **Format and check**

   ```bash
   dogsbay-xml format -i topics/*.dita shared/*.dita *.ditamap filters/*.ditaval
   scripts/check-stage.sh
   ```

   ```
   == validate-project  /home/you/audacity-guide ==
   28 file(s): 28 valid, 0 invalid.

   == project-health  /home/you/audacity-guide ==
   Root map: audacity-guide.ditamap (project config); house rules: none (none configured)
   Project is healthy: valid, no broken references, keys, orphans, or broken element ids.

   == build  project.json -> /tmp/check-stage-412907 ==
   all deliverables built

   STAGE OK
   ```

   The build step takes longer now: five deliverables instead of one.

4. **Read the output**
   Under `/tmp/check-stage-412907/out/` there is one folder per
   deliverable. Compare them:
   - `beginner-windows/topics/removing-background-noise.html` says
     `Ctrl+A` and contains no `Cmd+A`; the same page under `beginner-mac`
     says `Cmd+A` and contains no `Ctrl+A`. In `full`, the unfiltered
     build, both survive side by side.
   - `beginner-windows/topics/installing-audacity.html` has one row in
     the choice table, the `.exe` one; `beginner-mac` has the `.dmg` row,
     `podcaster-linux` the `apt install` row.
   - Neither beginner folder has a `podcast-production-workflow.html`;
     the topic is not in the beginner map, and in the full guide its
     branch is `audience="podcaster"`. `podcaster-linux/topics/` has it,
     without the *Getting started with podcasting* section.
   - `review/topics/exporting-audio.html` shows the optional step as
     `[new in 3.4]Fill in the Edit Metadata Tags dialog…` in the flag
     colour, and `review/topics/removing-background-noise.html` wraps
     the tip in `[podcaster]` … `[/podcaster]` on a pale blue background.
     Nothing is missing from the review build.
::::

Now leave a key out. In `beginner-guide.ditamap`, remove `keys="install"`
from the install topicref and run the gate. Validation and health pass, and
the build fails:

```
== build  project.json -> /tmp/check-stage-413350 ==
Error: file:/home/you/audacity-guide/topics/what-is-audacity.dita:49:29: [DOTX028E]: Link or cross reference must contain a valid @href or @keyref attribute; no link target is specified.
FAIL: DITA-OT build reported errors (full log: /tmp/check-stage-413350.log)

STAGE FAILED
```

`project-health` analyses the project's default root map, the full guide,
where `install` is defined. Every root map needs every key its topics use;
the build of the beginner guide is where a missing one shows.

And break a topic. Add a `<p>` after the last `<section>` of the podcast
workflow and validation fails:

```
== validate-project  /home/you/audacity-guide ==
/home/you/audacity-guide/topics/podcast-production-workflow.dita:
  61:13  error: The content of element type "conbody" does not match its content model.
28 file(s): 27 valid, 1 invalid.
FAIL: validation errors
```

In a `<conbody>`, block elements come first and sections last; a paragraph
that belongs after a section goes inside it.

## What you learned

- `@platform`, `@audience`, `@product`, `@otherprops` and `@props` on any
  element; space-separated values; two `<ph>` alternatives for text that
  differs; `@rev` to mark a change without filtering.
- A DITAVAL: `<val>`, `<prop att val action>` with `include`, `exclude` or
  `flag`, `<revprop>`, `<startflag>`/`<endflag>` with `<alt-text>`,
  `<style-conflict>`.
- A profiling attribute on a `<topicref>` or `<topichead>` filters a branch
  of the map.
- One topic set, several maps, one DITAVAL per deliverable in
  `project.json`, under `profiles.ditavals`.
- Every root map defines every key its topics use; `project-health` checks
  the default root map, the build checks each map.

## Where to go next

:::cards
- **[Stage 14: Subject scheme](/part-3-conditions/stage-14-subject-scheme)** {icon="arrow-right"}
  A controlled vocabulary for `platform` and `audience`, so that `macos`
  is an error and not a silent no-op.

- **[Compare 12 to 13 on GitHub](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/12-metadata-and-index...tutorial/13-conditional-text)** {icon="github"}
  Exactly what this stage added.
:::
