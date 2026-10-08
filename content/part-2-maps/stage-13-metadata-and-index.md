---
title: "Stage 13: Metadata and index"
description: Give every topic a prolog with author, dates, audience, keywords and index terms, add publication metadata to the map, and enforce a metadata policy.
type: tutorial
---

# Stage 13: Metadata and index

Add author, date, audience, keyword, and index metadata to the topics.
Place it in `<prolog>`, between the short description and the body. Add
publication metadata to the map's `<topicmeta>`.

DITA makes all of this metadata optional. To require fields, set a
metadata policy that requires keywords on topics and creation dates on
tasks. The metadata policy is a DogsBay XML feature, not part of DITA:
the policy is stored with the project, and the DogsBay XML check enforces
it.

The examples show each topic type; expand the remaining examples to
apply the same structure.

**Optional module.** See [Choose a learning path](/start-here/learning-path)
for the starting checkpoint and the next core lesson.

**Time:** about 40 minutes.
**You need:** stage 12 complete.


Recorded output below is an example. File counts, paths, and stage numbers can differ. [Check your work](/start-here/run-the-gate) to see the result for your own project.

## Step 1: The prolog of a concept

::::steps
1. **Edit `topics/what-is-audacity.dita`**
   This one carries every element the stage uses.

   ```diff title="topics/what-is-audacity.dita"
   --- a/topics/what-is-audacity.dita
   +++ b/topics/what-is-audacity.dita
   @@ -4,6 +4,29 @@
    <concept id="what-is-audacity">
      <title>What is <keyword keyref="product-name"/>?</title>
      <shortdesc><keyword keyref="product-name"/> is a free, open-source audio editor and recorder for Windows, macOS and Linux.</shortdesc>
   +  <prolog>
   +    <author type="creator">The Audacity tutorial team</author>
   +    <copyright>
   +      <copyryear year="2026"/>
   +      <copyrholder>DogsBay Ltd.</copyrholder>
   +    </copyright>
   +    <critdates>
   +      <created date="2026-01-10"/>
   +    </critdates>
   +    <metadata>
   +      <audience type="user" experiencelevel="novice"/>
   +      <category>concept</category>
   +      <keywords>
   +        <keyword>Audacity</keyword>
   +        <keyword>audio editor</keyword>
   +        <keyword>open source</keyword>
   +        <indexterm>Audacity<indexterm>overview</indexterm></indexterm>
   +        <indexterm>audio editor<index-see>Audacity</index-see></indexterm>
   +      </keywords>
   +    </metadata>
   +    <resourceid appname="audacity-help" id="intro"/>
   +    <data name="reading-time" value="2 min"/>
   +  </prolog>
      <conbody>
        <p><keyword keyref="product-name"/> records live audio, imports and exports the common audio formats, and edits sound with cut, copy, paste and a library of effects.
        It is free to download and use, and it runs on Windows, macOS and Linux.</p>
   ```

2. **Read the order**
   The children of `<prolog>` come in a fixed order: `<author>`,
   `<source>`, `<publisher>`, `<copyright>`, `<critdates>`,
   `<permissions>`, `<metadata>`, `<resourceid>`, `<data>`. Skip any, but
   do not reorder them. `<resourceid>` comes after `<metadata>`. If you
   put it inside `<metadata>`, the file does not validate. `<data>` is
   allowed in both places: at the end of `<metadata>`, or after
   `<resourceid>` as here.

3. **Read the elements**

   - `<author type="creator">` names who wrote the topic; `type` may also
     be `contributor`.
   - `<copyright>` holds a `<copyryear year="…"/>` and a `<copyrholder>`.
     Only this topic carries one; the map's copyright covers the guide.
   - `<critdates>` holds `<created date="…"/>` and, in the topics that
     have been revised, `<revised modified="…"/>`. Dates are
     `YYYY-MM-DD`.
   - `<metadata>` groups the classification: `<audience type="user"
     experiencelevel="novice"/>`, a `<category>`, and `<keywords>`.
   - `<keywords>` holds `<keyword>`s, which DITA-OT's `html5` transform
     writes into a `<meta name="keywords">` tag, and `<indexterm>`s, which
     the HTML pages do not show: a PDF does, and the book in stage 19
     builds its index from them.
   - `<resourceid appname="…" id="…"/>` is an identifier for another
     system, here a help system that will link to this topic. `<data
     name="…" value="…"/>` is a named property with no defined meaning;
     a processor or a house rule can use it.

4. **See what reaches the page**
   Save the file. Choose **Project** > **Build Deliverables...**, and click
   **Build All**. Open `out/full/topics/what-is-audacity.html` and look at
   its `<head>`. Before the prolog, it had only a
   `<meta name="description">` from the short description and a default
   copyright with no owner. Now it also carries
   `<meta name="keywords" content="Audacity, audio editor, open source">`
   and `<meta name="rights" content="© 2026 DogsBay Ltd.">`, which replaces
   the default.

   That is all that reaches the head in the `html5` transform. The author,
   the dates, the audience, the category and the index terms stay in the
   source, for other outputs, for searches across the project, and for the
   policy in step 5.
::::

## Step 2: The prologs of the other topics

::::steps
1. **Edit `topics/what-is-digital-audio.dita`**
   A revision date, and three index-term forms.

   ```diff title="topics/what-is-digital-audio.dita"
   --- a/topics/what-is-digital-audio.dita
   +++ b/topics/what-is-digital-audio.dita
   @@ -4,6 +4,31 @@
    <concept id="what-is-digital-audio">
      <title>What is digital audio?</title>
      <shortdesc>Digital audio is sound stored as numbers: samples taken thousands of times a second at a chosen precision.</shortdesc>
   +  <prolog>
   +    <author type="creator">The Audacity tutorial team</author>
   +    <critdates>
   +      <created date="2026-01-10"/>
   +      <revised modified="2026-03-02"/>
   +    </critdates>
   +    <metadata>
   +      <audience type="user" experiencelevel="novice"/>
   +      <category>concept</category>
   +      <keywords>
   +        <keyword>digital audio</keyword>
   +        <keyword>sample rate</keyword>
   +        <keyword>bit depth</keyword>
   +        <keyword>waveform</keyword>
   +        <keyword>decibel</keyword>
   +        <indexterm>digital audio</indexterm>
   +        <indexterm>sample rate</indexterm>
   +        <indexterm>bit depth</indexterm>
   +        <indexterm>waveform</indexterm>
   +        <indexterm>decibel<index-see-also>amplitude</index-see-also></indexterm>
   +        <indexterm>amplitude</indexterm>
   +        <indexterm><index-sort-as>clipping</index-sort-as>Clipping (distortion)</indexterm>
   +      </keywords>
   +    </metadata>
   +  </prolog>
      <conbody>
        <p>Digital audio is sound that has been converted into a numerical representation.
        A few key concepts help you make better recordings and edits.
   ```

2. **Edit `topics/installing-audacity.dita`**
   A nested index term.

   ```diff title="topics/installing-audacity.dita"
   --- a/topics/installing-audacity.dita
   +++ b/topics/installing-audacity.dita
   @@ -4,6 +4,25 @@
    <task id="installing-audacity">
      <title>Installing <keyword keyref="product-name"/></title>
      <shortdesc>Download <keyword keyref="product-name"/> from the official website, or install it with your package manager, and launch it once to finish setup.</shortdesc>
   +  <prolog>
   +    <author type="creator">The Audacity tutorial team</author>
   +    <critdates>
   +      <created date="2026-01-12"/>
   +    </critdates>
   +    <metadata>
   +      <audience type="user" experiencelevel="novice"/>
   +      <category>installation</category>
   +      <keywords>
   +        <keyword>install</keyword>
   +        <keyword>download</keyword>
   +        <keyword>Windows</keyword>
   +        <keyword>macOS</keyword>
   +        <keyword>Linux</keyword>
   +        <indexterm>installing<indexterm>Windows</indexterm><indexterm>macOS</indexterm><indexterm>Linux</indexterm></indexterm>
   +        <indexterm>download</indexterm>
   +      </keywords>
   +    </metadata>
   +  </prolog>
      <taskbody>
        <prereq>
          <p>You need about 250 MB of free disk space and an account that can install software.</p>
   ```

3. **Edit `topics/supported-audio-formats.dita`**
   A revision date and an index redirect.

   ```diff title="topics/supported-audio-formats.dita"
   --- a/topics/supported-audio-formats.dita
   +++ b/topics/supported-audio-formats.dita
   @@ -4,6 +4,26 @@
    <reference id="supported-audio-formats">
      <title>Supported audio formats</title>
      <shortdesc>The audio formats <keyword keyref="product-name"/> imports and exports, and when to use each one.</shortdesc>
   +  <prolog>
   +    <author type="creator">The Audacity tutorial team</author>
   +    <critdates>
   +      <created date="2026-01-22"/>
   +      <revised modified="2026-02-14"/>
   +    </critdates>
   +    <metadata>
   +      <audience type="user" experiencelevel="novice"/>
   +      <category>reference</category>
   +      <keywords>
   +        <keyword>audio formats</keyword>
   +        <keyword>lossless</keyword>
   +        <keyword>lossy</keyword>
   +        <keyword>MP3</keyword>
   +        <keyword>FLAC</keyword>
   +        <indexterm>audio formats<indexterm>lossless</indexterm><indexterm>lossy</indexterm></indexterm>
   +        <indexterm>codecs<index-see>audio formats</index-see></indexterm>
   +      </keywords>
   +    </metadata>
   +  </prolog>
      <refbody>
        <section>
          <p><keyword keyref="product-name"/> imports and exports audio in several formats.
   @@ -46,7 +66,9 @@
              </row>
              <row>
                <entry>MP3</entry>
   -            <entry><term keyref="gl-compression">Compressed</term></entry>
   +            <entry>
   +              <term keyref="gl-compression">Compressed</term>
   +            </entry>
                <entry>Lossy</entry>
                <entry>Widely supported, small files.
                Needs the <i>LAME</i> encoder, bundled since version 2.3.2.
   ```

4. **Read the index terms**

   - `<indexterm>` is an entry in the index. An `<indexterm>` inside
     another is a sub-entry: `installing` with `Windows`, `macOS` and
     `Linux` under it.
   - `<index-see>` inside an entry redirects it: "codecs, *see* audio
     formats". The entry with `<index-see>` has no page of its own.
   - `<index-see-also>` adds a cross-reference under an entry that does
     have pages: "decibel … *see also* amplitude".
   - `<index-sort-as>` gives the text to sort by when it differs from the
     text shown: `Clipping (distortion)` sorts as `clipping`.
   - The HTML pages do not show index entries, but you can list them. In
     the **Terminal** panel, run `dogsbay-xml index-audit .`. Each line is
     an index entry with the number of topics that carry it, and `>`
     separates an entry from its sub-entry, for example
     `installing > Windows`. A PDF index is built from the same terms.
   - `project-health` checks the index. Change the redirect to
     `audio format` and it reports, under *Index redirects to nothing*,
     `"codecs" see "audio format"`.

5. **Edit the remaining five tasks**
   Same shape: author, `<created>`, audience, category, keywords and index
   terms. The noise task is the one topic for an intermediate audience.

   :::details{title="topics/exporting-audio.dita"}
   ```diff title="topics/exporting-audio.dita"
   --- a/topics/exporting-audio.dita
   +++ b/topics/exporting-audio.dita
   @@ -4,6 +4,25 @@
    <task id="exporting-audio">
      <title>Exporting audio</title>
      <shortdesc>Export a finished project as a WAV, MP3, OGG or FLAC file that other programs and devices can play.</shortdesc>
   +  <prolog>
   +    <author type="creator">The Audacity tutorial team</author>
   +    <critdates>
   +      <created date="2026-01-22"/>
   +    </critdates>
   +    <metadata>
   +      <audience type="user" experiencelevel="novice"/>
   +      <category>exporting</category>
   +      <keywords>
   +        <keyword>export</keyword>
   +        <keyword>MP3</keyword>
   +        <keyword>WAV</keyword>
   +        <keyword>FLAC</keyword>
   +        <keyword>OGG</keyword>
   +        <indexterm>exporting</indexterm>
   +        <indexterm>audio formats<indexterm>MP3</indexterm><indexterm>WAV</indexterm></indexterm>
   +      </keywords>
   +    </metadata>
   +  </prolog>
      <taskbody>
        <prereq>
          <p>Finish editing and save the project.
   ```
   :::

   :::details{title="topics/preparing-to-record.dita"}
   ```diff title="topics/preparing-to-record.dita"
   --- a/topics/preparing-to-record.dita
   +++ b/topics/preparing-to-record.dita
   @@ -4,6 +4,24 @@
    <task id="preparing-to-record">
      <title>Preparing to record</title>
      <shortdesc>Checks to make in any order before you press Record: input level, sample rate and disk space.</shortdesc>
   +  <prolog>
   +    <author type="creator">The Audacity tutorial team</author>
   +    <critdates>
   +      <created date="2026-01-15"/>
   +    </critdates>
   +    <metadata>
   +      <audience type="user" experiencelevel="novice"/>
   +      <category>recording</category>
   +      <keywords>
   +        <keyword>recording</keyword>
   +        <keyword>levels</keyword>
   +        <keyword>sample rate</keyword>
   +        <keyword>disk space</keyword>
   +        <indexterm>recording<indexterm>preparing</indexterm><indexterm>levels</indexterm></indexterm>
   +        <indexterm>clipping</indexterm>
   +      </keywords>
   +    </metadata>
   +  </prolog>
      <taskbody>
        <context>
          <p>None of these checks depends on another, so do them in whatever order suits you.</p>
   ```
   :::

   :::details{title="topics/recording-your-first-track.dita"}
   ```diff title="topics/recording-your-first-track.dita"
   --- a/topics/recording-your-first-track.dita
   +++ b/topics/recording-your-first-track.dita
   @@ -4,6 +4,24 @@
    <task id="recording-your-first-track">
      <title>Recording your first track</title>
      <shortdesc>Make your first recording in <keyword keyref="product-name"/>: pick a microphone, record, stop and play it back.</shortdesc>
   +  <prolog>
   +    <author type="creator">The Audacity tutorial team</author>
   +    <critdates>
   +      <created date="2026-01-15"/>
   +    </critdates>
   +    <metadata>
   +      <audience type="user" experiencelevel="novice"/>
   +      <category>recording</category>
   +      <keywords>
   +        <keyword>recording</keyword>
   +        <keyword>microphone</keyword>
   +        <keyword>Device Toolbar</keyword>
   +        <indexterm>recording<indexterm>first track</indexterm></indexterm>
   +        <indexterm>microphone</indexterm>
   +        <indexterm>Device Toolbar</indexterm>
   +      </keywords>
   +    </metadata>
   +  </prolog>
      <taskbody>
        <context>
          <p>This task walks you through making your first audio recording.
   ```
   :::

   :::details{title="topics/removing-background-noise.dita"}
   ```diff title="topics/removing-background-noise.dita"
   --- a/topics/removing-background-noise.dita
   +++ b/topics/removing-background-noise.dita
   @@ -4,6 +4,24 @@
    <task id="removing-background-noise">
      <title>Removing background noise</title>
      <shortdesc>Take a noise profile from a quiet section, then apply Noise Reduction to the whole track to remove hum, hiss and fan noise.</shortdesc>
   +  <prolog>
   +    <author type="creator">The Audacity tutorial team</author>
   +    <critdates>
   +      <created date="2026-01-20"/>
   +    </critdates>
   +    <metadata>
   +      <audience type="user" experiencelevel="intermediate"/>
   +      <category>editing</category>
   +      <keywords>
   +        <keyword>noise reduction</keyword>
   +        <keyword>noise profile</keyword>
   +        <keyword>effects</keyword>
   +        <indexterm>noise reduction</indexterm>
   +        <indexterm>effects<indexterm>Noise Reduction</indexterm></indexterm>
   +        <indexterm>hum<index-see>noise reduction</index-see></indexterm>
   +      </keywords>
   +    </metadata>
   +  </prolog>
      <taskbody>
        <context>
          <p>Background noise such as hum from electronics, hiss from a microphone or fan noise degrades a recording.
   ```
   :::

   :::details{title="topics/trimming-audio.dita"}
   ```diff title="topics/trimming-audio.dita"
   --- a/topics/trimming-audio.dita
   +++ b/topics/trimming-audio.dita
   @@ -4,6 +4,25 @@
    <task id="trimming-audio">
      <title>Trimming audio</title>
      <shortdesc>Remove silence, false starts and mistakes by selecting a region of the waveform and deleting it.</shortdesc>
   +  <prolog>
   +    <author type="creator">The Audacity tutorial team</author>
   +    <critdates>
   +      <created date="2026-01-18"/>
   +    </critdates>
   +    <metadata>
   +      <audience type="user" experiencelevel="novice"/>
   +      <category>editing</category>
   +      <keywords>
   +        <keyword>trimming</keyword>
   +        <keyword>editing</keyword>
   +        <keyword>delete</keyword>
   +        <keyword>selection</keyword>
   +        <indexterm>trimming</indexterm>
   +        <indexterm>editing<indexterm>trimming</indexterm></indexterm>
   +        <indexterm>selection</indexterm>
   +      </keywords>
   +    </metadata>
   +  </prolog>
      <taskbody>
        <context>
          <p>Trimming removes unwanted audio from the start, end or middle of a track.
   ```
   :::
::::

## Step 3: The glossary and the shared topics

::::steps
1. **Edit `topics/glossary/audio-units.dita`**
   The group gets a prolog of its own, and so does each entry inside it.

   ```diff title="topics/glossary/audio-units.dita"
   --- a/topics/glossary/audio-units.dita
   +++ b/topics/glossary/audio-units.dita
   @@ -3,9 +3,28 @@
    
    <glossgroup id="audio-units">
      <title>Units</title>
   +  <prolog>
   +    <author type="creator">The Audacity tutorial team</author>
   +    <metadata>
   +      <keywords>
   +        <keyword>units</keyword>
   +        <keyword>glossary</keyword>
   +      </keywords>
   +    </metadata>
   +  </prolog>
      <glossentry id="gl-decibel">
        <glossterm>decibel</glossterm>
        <glossdef>A logarithmic unit for the ratio between two levels. In audio editing, 0 dB is the loudest level a file can hold, and quieter sounds are negative.</glossdef>
   +    <prolog>
   +      <author type="creator">The Audacity tutorial team</author>
   +      <metadata>
   +        <keywords>
   +          <keyword>decibel</keyword>
   +          <keyword>glossary</keyword>
   +          <indexterm>decibel</indexterm>
   +        </keywords>
   +      </metadata>
   +    </prolog>
        <glossBody>
          <glossSurfaceForm>decibel (dB)</glossSurfaceForm>
          <glossAlt>
   @@ -16,6 +35,16 @@
      <glossentry id="gl-hertz">
        <glossterm>hertz</glossterm>
        <glossdef>Cycles per second. Sample rates are given in hertz: 44,100 Hz means 44,100 samples every second.</glossdef>
   +    <prolog>
   +      <author type="creator">The Audacity tutorial team</author>
   +      <metadata>
   +        <keywords>
   +          <keyword>hertz</keyword>
   +          <keyword>glossary</keyword>
   +          <indexterm>hertz</indexterm>
   +        </keywords>
   +      </metadata>
   +    </prolog>
        <glossBody>
          <glossSurfaceForm>hertz (Hz)</glossSurfaceForm>
          <glossAlt>
   ```

2. **Read why the group has one**
   A `<glossgroup>` is a topic, and the policy in step 5 says every topic
   needs a keyword. Give the entries prologs and leave the group without,
   and `project-health` reports
   `topics/glossary/audio-units.dita — [error] missing required <keyword>`.
   In a `<glossentry>` the `<prolog>` follows `<glossdef>`, and in a
   `<glossgroup>` it follows `<title>`, before the first entry.

3. **Edit the six entry files**
   Author and keywords, with the term as an index entry. No `<created>`:
   the policy requires a date on tasks only.

   :::details{title="topics/glossary/g-bit-depth.dita, and the other five alike"}
   ```diff title="topics/glossary/g-bit-depth.dita"
   --- a/topics/glossary/g-bit-depth.dita
   +++ b/topics/glossary/g-bit-depth.dita
   @@ -4,4 +4,14 @@
    <glossentry id="gl-bit-depth">
      <glossterm>Bit depth</glossterm>
      <glossdef>The number of bits used to store each sample, which sets how precisely amplitude is recorded. 16-bit audio has 65,536 possible values; 24-bit has over 16 million.</glossdef>
   +  <prolog>
   +    <author type="creator">The Audacity tutorial team</author>
   +    <metadata>
   +      <keywords>
   +        <keyword>Bit depth</keyword>
   +        <keyword>glossary</keyword>
   +        <indexterm>Bit depth</indexterm>
   +      </keywords>
   +    </metadata>
   +  </prolog>
    </glossentry>
   ```

   ```diff title="topics/glossary/g-clipping.dita"
   --- a/topics/glossary/g-clipping.dita
   +++ b/topics/glossary/g-clipping.dita
   @@ -4,4 +4,14 @@
    <glossentry id="gl-clipping">
      <glossterm>Clipping</glossterm>
      <glossdef>Distortion that occurs when a signal exceeds the maximum amplitude the system can represent. Clipped peaks are cut flat and cannot be repaired.</glossdef>
   +  <prolog>
   +    <author type="creator">The Audacity tutorial team</author>
   +    <metadata>
   +      <keywords>
   +        <keyword>Clipping</keyword>
   +        <keyword>glossary</keyword>
   +        <indexterm>Clipping</indexterm>
   +      </keywords>
   +    </metadata>
   +  </prolog>
    </glossentry>
   ```

   ```diff title="topics/glossary/g-compression.dita"
   --- a/topics/glossary/g-compression.dita
   +++ b/topics/glossary/g-compression.dita
   @@ -4,4 +4,14 @@
    <glossentry id="gl-compression">
      <glossterm>Compression</glossterm>
      <glossdef>Reducing the dynamic range of audio so that quiet parts are louder and loud parts quieter. Not to be confused with file compression such as MP3.</glossdef>
   +  <prolog>
   +    <author type="creator">The Audacity tutorial team</author>
   +    <metadata>
   +      <keywords>
   +        <keyword>Compression</keyword>
   +        <keyword>glossary</keyword>
   +        <indexterm>Compression</indexterm>
   +      </keywords>
   +    </metadata>
   +  </prolog>
    </glossentry>
   ```

   ```diff title="topics/glossary/g-normalization.dita"
   --- a/topics/glossary/g-normalization.dita
   +++ b/topics/glossary/g-normalization.dita
   @@ -4,4 +4,14 @@
    <glossentry id="gl-normalization">
      <glossterm>Normalization</glossterm>
      <glossdef>Raising or lowering the level of a recording so that its loudest peak sits at a chosen target, giving consistent loudness across clips.</glossdef>
   +  <prolog>
   +    <author type="creator">The Audacity tutorial team</author>
   +    <metadata>
   +      <keywords>
   +        <keyword>Normalization</keyword>
   +        <keyword>glossary</keyword>
   +        <indexterm>Normalization</indexterm>
   +      </keywords>
   +    </metadata>
   +  </prolog>
    </glossentry>
   ```

   ```diff title="topics/glossary/g-sample-rate.dita"
   --- a/topics/glossary/g-sample-rate.dita
   +++ b/topics/glossary/g-sample-rate.dita
   @@ -4,4 +4,14 @@
    <glossentry id="gl-sample-rate">
      <glossterm>Sample rate</glossterm>
      <glossdef>The number of audio samples captured per second, measured in hertz. Higher sample rates capture more detail. CD audio uses 44,100 Hz; video production typically uses 48,000 Hz.</glossdef>
   +  <prolog>
   +    <author type="creator">The Audacity tutorial team</author>
   +    <metadata>
   +      <keywords>
   +        <keyword>Sample rate</keyword>
   +        <keyword>glossary</keyword>
   +        <indexterm>Sample rate</indexterm>
   +      </keywords>
   +    </metadata>
   +  </prolog>
    </glossentry>
   ```

   ```diff title="topics/glossary/g-waveform.dita"
   --- a/topics/glossary/g-waveform.dita
   +++ b/topics/glossary/g-waveform.dita
   @@ -4,4 +4,14 @@
    <glossentry id="gl-waveform">
      <glossterm>Waveform</glossterm>
      <glossdef>The shape of a sound wave over time, drawn as amplitude against time. Audacity shows each track as a waveform.</glossdef>
   +  <prolog>
   +    <author type="creator">The Audacity tutorial team</author>
   +    <metadata>
   +      <keywords>
   +        <keyword>Waveform</keyword>
   +        <keyword>glossary</keyword>
   +        <indexterm>Waveform</indexterm>
   +      </keywords>
   +    </metadata>
   +  </prolog>
    </glossentry>
   ```
   :::

4. **Edit the two shared topics**
   Shared topics are topics too. Their audience is `author`, because a
   writer is the only person who reads them, and their category is
   `warehouse`.

   ```diff title="shared/common-notes.dita"
   --- a/shared/common-notes.dita
   +++ b/shared/common-notes.dita
   @@ -4,6 +4,21 @@
    <topic id="common-notes">
      <title>Common notes</title>
      <shortdesc>Reusable notes, tips and lists that other topics pull in by conref. This topic is a warehouse and is not published on its own.</shortdesc>
   +  <prolog>
   +    <author type="creator">The Audacity tutorial team</author>
   +    <critdates>
   +      <created date="2026-01-15"/>
   +    </critdates>
   +    <metadata>
   +      <audience type="author"/>
   +      <category>warehouse</category>
   +      <keywords>
   +        <keyword>reuse</keyword>
   +        <keyword>conref</keyword>
   +        <keyword>notes</keyword>
   +      </keywords>
   +    </metadata>
   +  </prolog>
      <body>
        <note id="backup-warning" type="warning">This effect changes the audio data.
        Save the project first; you can undo with <uicontrol>Ctrl+Z</uicontrol> (Windows and Linux) or <uicontrol>Cmd+Z</uicontrol> (macOS).</note>
   ```

   :::details{title="shared/common-steps.dita"}
   ```diff title="shared/common-steps.dita"
   --- a/shared/common-steps.dita
   +++ b/shared/common-steps.dita
   @@ -4,6 +4,21 @@
    <task id="common-steps">
      <title>Common steps</title>
      <shortdesc>Reusable steps that other tasks pull in by conref. This task is a warehouse and is not published on its own.</shortdesc>
   +  <prolog>
   +    <author type="creator">The Audacity tutorial team</author>
   +    <critdates>
   +      <created date="2026-01-15"/>
   +    </critdates>
   +    <metadata>
   +      <audience type="author"/>
   +      <category>warehouse</category>
   +      <keywords>
   +        <keyword>reuse</keyword>
   +        <keyword>conref</keyword>
   +        <keyword>steps</keyword>
   +      </keywords>
   +    </metadata>
   +  </prolog>
      <taskbody>
        <steps>
          <step id="select-region">
   ```
   :::
::::

## Step 4: Metadata for the publication

::::steps
1. **Edit `audacity-guide.ditamap`**

   ```diff title="audacity-guide.ditamap"
   --- a/audacity-guide.ditamap
   +++ b/audacity-guide.ditamap
   @@ -5,6 +5,20 @@
      <title>Audacity User Guide</title>
      <topicmeta>
        <shortdesc>Record, edit and export audio with Audacity, from your first track to a finished file.</shortdesc>
   +    <author type="creator">The Audacity tutorial team</author>
   +    <publisher>DogsBay Ltd.</publisher>
   +    <copyright>
   +      <copyryear year="2026"/>
   +      <copyrholder>DogsBay Ltd.</copyrholder>
   +    </copyright>
   +    <critdates>
   +      <created date="2026-01-10"/>
   +    </critdates>
   +    <audience type="user"/>
   +    <keywords>
   +      <keyword>Audacity</keyword>
   +      <keyword>audio editing</keyword>
   +    </keywords>
      </topicmeta>
    
      <mapref href="keydefs-product.ditamap"/>
   ```

2. **Read the map metadata**
   The map's `<topicmeta>` takes the same elements as a prolog, in the
   same order, without the `<metadata>` wrapper: `<author>`,
   `<publisher>`, `<copyright>`, `<critdates>`, `<audience>` and
   `<keywords>` sit directly inside it. Map metadata applies to the
   publication and, for the `html5` transform, cascades into every page: the
   `<meta name="rights">` tag on each page comes from this copyright.

3. **See the publication metadata**
   Save the map, choose **Project** > **Build Deliverables...**, and click
   **Build All**. The start page, `out/full/index.html`, carries the map's
   keywords in `<meta name="keywords">` and a `<meta name="rights">` tag
   from the map's copyright. A topic page such as
   `out/full/topics/what-is-digital-audio.html` also has the rights tag,
   although its topic has no copyright of its own. Its keywords stay the
   topic's own.
::::

## Step 5: The metadata policy and the check

::::steps
1. **Set the metadata policy**
   The policy is kept with the project, in `.dogsbay/config.xml`, and
   makes fields required or recommended. Choose **Project** >
   **Metadata** > **Edit Policy...**. The dialog shows a table of rules,
   one rule per row. Add three rules:

   | Topic type (blank = any) | Field | Presence | Allowed values (space-separated) | Pattern |
   |---|---|---|---|---|
   | `task` | `created` | `required` | (empty) | `\d{4}-\d\d-\d\d` |
   | (empty) | `author` | `recommended` | (empty) | (empty) |
   | (empty) | `keyword` | `required` | (empty) | (empty) |

   For each rule, click **Add rule**. A new row starts with the field
   `author` and the presence `required`. Choose the **Field** and the
   **Presence** from their lists, and double-click a text cell to type in
   it. When the table shows the three rules, click **OK**. A message
   confirms that the rules were saved to `.dogsbay/config.xml`; click
   **OK**.

2. **Read the rules**

   - Each rule names a field, a presence and, optionally, a topic type and
     a pattern. A missing `required` field is an error and fails the check;
     a missing `recommended` field is a warning, shown beside a file's
     errors.
   - `created` is required on every task, and must match `\d{4}-\d\d-\d\d`.
     Concepts, references and glossary entries may leave it out.
   - `author` is recommended everywhere. `keyword` is required everywhere,
     which is the rule that reaches the glossary group.
   - `project-health` applies the policy as one of its checks, so the
     health stage of **Check Project** and `dogsbay-xml check` enforces it.
     The editor applies the same policy. DITA-OT and other DITA tools do
     not read this policy.

3. **Format and check your work**

   Format the topics: choose **XML** > **Format** in each changed topic
   and save it, or choose **Project** > **Project Tools** >
   **Format Project** to format every file at once. From the command line,
   run:

   ```bash
   dogsbay-xml format -i topics/*.dita topics/glossary/*.dita shared/*.dita
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
     unused key: start-here  [/home/you/my-audacity-guide/audacity-guide.ditamap:26] — nothing references it
     unused key: digital-audio  [/home/you/my-audacity-guide/audacity-guide.ditamap:33] — nothing references it
     unused key: gl-normalization  [/home/you/my-audacity-guide/keydefs-glossary.ditamap:10] — nothing references it
   build    full                 ok  /home/you/my-audacity-guide/out/full
   output   clean (full)
   Ready: the project is healthy, every deliverable built, and the output of full holds together. 3 unused keys above: worth knowing, and not treated as failures.
   ```

   The unused keys are defined for later stages: stage 14 refers to
   `gl-normalization`, and stage 18 uses `start-here` and `digital-audio`.

   The check also builds the guide into `out/full/`. The `html5` pages
   carry the keywords and rights tags from step 1 and step 4. The PDF
   transform uses the index terms to generate an index.
::::

Test the metadata policy. In *Trimming audio*, put the cursor in
`<critdates>`, choose **XML** > **Select Element**, delete the element,
save, and check your work. The check stops at health and names the file. Example
output:

```
health   NOT CLEAN
  /home/you/my-audacity-guide/topics/trimming-audio.dita:7  created — missing required <created @date>
  (run project-health for the full report)
  unused key: start-here  [/home/you/my-audacity-guide/audacity-guide.ditamap:26] — nothing references it
  unused key: digital-audio  [/home/you/my-audacity-guide/audacity-guide.ditamap:33] — nothing references it
  unused key: gl-normalization  [/home/you/my-audacity-guide/keydefs-glossary.ditamap:10] — nothing references it
Not ready: the project itself has faults. The build and the built output were not checked.
```

The **Project Validation** panel, or `dogsbay-xml project-health .`, gives
the full report:

```
Root map: audacity-guide.ditamap (project config); house rules: none (none configured)
Metadata policy (1 of 21 files):
  /home/you/my-audacity-guide/topics/trimming-audio.dita — [error] missing required <created @date>
Unused keys (3):
  start-here  [/home/you/my-audacity-guide/audacity-guide.ditamap:26]
  digital-audio  [/home/you/my-audacity-guide/audacity-guide.ditamap:33]
  gl-normalization  [/home/you/my-audacity-guide/keydefs-glossary.ditamap:10]

Summary
  Metadata policy               1  in 1 of 21 files (1 error(s), 0 warning(s))
    missing required <created @date>                                  1
  Unused keys                   3
```

The file is valid; `<critdates>` is optional to the DTD. The DogsBay XML
policy, not the DITA DTD, makes it required for tasks in this project.

Undo the policy test with **Edit** > **Undo**, save, and check again. Then test the element order. Move `<resourceid>` and `<data>` in *What is
Audacity?* to before `<metadata>`, and check your work. The file is no
longer valid, and the check names it with the line, column, and message.
Example output:

```
health   NOT CLEAN
  invalid: /home/you/my-audacity-guide/topics/what-is-audacity.dita
    29:12  The content of element type "prolog" does not match its content model.
  (run project-health for the full report)
  unused key: start-here  [/home/you/my-audacity-guide/audacity-guide.ditamap:26] — nothing references it
  unused key: digital-audio  [/home/you/my-audacity-guide/audacity-guide.ditamap:33] — nothing references it
  unused key: gl-normalization  [/home/you/my-audacity-guide/keydefs-glossary.ditamap:10] — nothing references it
Not ready: the project itself has faults. The build and the built output were not checked.
```

The **Project Validation** panel, or `dogsbay-xml project-health .`, gives
the full report. It begins:

```
Invalid files (1 of 21):
  /home/you/my-audacity-guide/topics/what-is-audacity.dita:
    29:12  error: The content of element type "prolog" does not match its content model.
```

Undo the change and check again. Confirm that the check reports `Ready`
before you continue.

## What you learned

- `<prolog>` after `<shortdesc>`, with `<author>`, `<copyright>`,
  `<critdates>` (`<created>`, `<revised>`), `<metadata>` (`<audience>`,
  `<category>`, `<keywords>`), then `<resourceid>` and `<data>`, in that
  order.
- `<indexterm>`, nested for sub-entries; `<index-see>`, `<index-see-also>`
  and `<index-sort-as>` inside an entry.
- A `<glossgroup>` and a shared topic are topics and carry prologs like any
  other.
- Map `<topicmeta>` takes the same elements for the publication.
- A metadata policy, set in **Project** > **Metadata** > **Edit Policy...**:
  rules by topic type, field, presence and pattern, enforced by
  `project-health` and so by the check.
- End of Part 2: a map with structure and a reltable, a key space, a
  shared topic, a glossary and metadata, published as one HTML5 deliverable.

## Next lesson

**Checkpoint:** `tutorial/13-metadata-and-index`. If you use Git, commit
your work.

Continue with [Stage 14: conditional text](/part-3-conditions/stage-14-conditional-text).

For the core course, use the [learning path](/start-here/learning-path) to skip optional modules.

[Compare this stage with its predecessor](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/12-glossary...tutorial/13-metadata-and-index).
