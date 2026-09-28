---
title: "Stage 12: Glossary"
description: Add glossary entries and a glossary group with abbreviations, key them in a keydef map, and bind terms in the text to their definitions.
type: tutorial
---

# Stage 12: Glossary

Add a glossary and link terms in the guide to their definitions. A
`<glossentry>` defines one term and can include abbreviations or synonyms.
A `<glossgroup>` contains several entries in one file.

Define a key for each entry. Use `<term keyref>` to link a term to its
entry and `<abbreviated-form keyref>` to let the processor choose the
expanded or abbreviated form.

**Optional module.** See [Choose a learning path](/start-here/learning-path)
for the starting checkpoint and the next core lesson.

**Time:** about 30 minutes.
**You need:** stage 11 complete.


Recorded diagnostic examples below come from earlier runs. File counts, paths, and stage numbers can differ. Run the gate for your current checkout.

## Step 1: Write the entries

::::steps
1. **Create `topics/glossary/` and the six entry files**
   Each is a `<glossentry>` with the glossary entry DOCTYPE: a
   `<glossterm>` and a `<glossdef>`, nothing else.

   ```xml title="topics/glossary/g-sample-rate.dita"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE glossentry PUBLIC "-//OASIS//DTD DITA Glossary Entry//EN" "glossentry.dtd">
   
   <glossentry id="gl-sample-rate">
     <glossterm>Sample rate</glossterm>
     <glossdef>The number of audio samples captured per second, measured in hertz. Higher sample rates capture more detail. CD audio uses 44,100 Hz; video production typically uses 48,000 Hz.</glossdef>
   </glossentry>
   ```

   ```xml title="topics/glossary/g-bit-depth.dita"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE glossentry PUBLIC "-//OASIS//DTD DITA Glossary Entry//EN" "glossentry.dtd">
   
   <glossentry id="gl-bit-depth">
     <glossterm>Bit depth</glossterm>
     <glossdef>The number of bits used to store each sample, which sets how precisely amplitude is recorded. 16-bit audio has 65,536 possible values; 24-bit has over 16 million.</glossdef>
   </glossentry>
   ```

   ```xml title="topics/glossary/g-clipping.dita"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE glossentry PUBLIC "-//OASIS//DTD DITA Glossary Entry//EN" "glossentry.dtd">
   
   <glossentry id="gl-clipping">
     <glossterm>Clipping</glossterm>
     <glossdef>Distortion that occurs when a signal exceeds the maximum amplitude the system can represent. Clipped peaks are cut flat and cannot be repaired.</glossdef>
   </glossentry>
   ```

   ```xml title="topics/glossary/g-normalization.dita"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE glossentry PUBLIC "-//OASIS//DTD DITA Glossary Entry//EN" "glossentry.dtd">
   
   <glossentry id="gl-normalization">
     <glossterm>Normalization</glossterm>
     <glossdef>Raising or lowering the level of a recording so that its loudest peak sits at a chosen target, giving consistent loudness across clips.</glossdef>
   </glossentry>
   ```

   ```xml title="topics/glossary/g-compression.dita"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE glossentry PUBLIC "-//OASIS//DTD DITA Glossary Entry//EN" "glossentry.dtd">
   
   <glossentry id="gl-compression">
     <glossterm>Compression</glossterm>
     <glossdef>Reducing the dynamic range of audio so that quiet parts are louder and loud parts quieter. Not to be confused with file compression such as MP3.</glossdef>
   </glossentry>
   ```

   ```xml title="topics/glossary/g-waveform.dita"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE glossentry PUBLIC "-//OASIS//DTD DITA Glossary Entry//EN" "glossentry.dtd">
   
   <glossentry id="gl-waveform">
     <glossterm>Waveform</glossterm>
     <glossdef>The shape of a sound wave over time, drawn as amplitude against time. Audacity shows each track as a waveform.</glossdef>
   </glossentry>
   ```

2. **Create `topics/glossary/audio-units.dita`**
   A `<glossgroup>` with two entries whose terms have abbreviations.

   ```xml title="topics/glossary/audio-units.dita"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE glossgroup PUBLIC "-//OASIS//DTD DITA Glossary Group//EN" "glossgroup.dtd">
   
   <glossgroup id="audio-units">
     <title>Units</title>
     <glossentry id="gl-decibel">
       <glossterm>decibel</glossterm>
       <glossdef>A logarithmic unit for the ratio between two levels. In audio editing, 0 dB is the loudest level a file can hold, and quieter sounds are negative.</glossdef>
       <glossBody>
         <glossSurfaceForm>decibel (dB)</glossSurfaceForm>
         <glossAlt>
           <glossAbbreviation>dB</glossAbbreviation>
         </glossAlt>
       </glossBody>
     </glossentry>
     <glossentry id="gl-hertz">
       <glossterm>hertz</glossterm>
       <glossdef>Cycles per second. Sample rates are given in hertz: 44,100 Hz means 44,100 samples every second.</glossdef>
       <glossBody>
         <glossSurfaceForm>hertz (Hz)</glossSurfaceForm>
         <glossAlt>
           <glossAbbreviation>Hz</glossAbbreviation>
         </glossAlt>
         <glossAlt>
           <glossSynonym>cycles per second</glossSynonym>
         </glossAlt>
       </glossBody>
     </glossentry>
   </glossgroup>
   ```

3. **Read the elements**

   - `<glossentry>` is a topic: it has an `@id`, and `<glossterm>` takes
     the place of `<title>`. `<glossdef>` is the definition, a short
     paragraph.
   - `<glossgroup>` is also a topic, with its own DOCTYPE, `@id` and
     `<title>`, containing `<glossentry>` topics. In stage 13 the metadata
     policy treats it as a topic in its own right, so it gets a prolog like
     any other.
   - `<glossBody>` holds everything beyond the definition.
     `<glossSurfaceForm>` is how the term is written the first time a
     reader meets it, "decibel (dB)". Each `<glossAlt>` is one alternative
     form: a `<glossAbbreviation>` or a `<glossSynonym>`.
::::

## Step 2: Key the entries and put them in the guide

::::steps
1. **Create `keydefs-glossary.ditamap`**

   ```xml title="keydefs-glossary.ditamap"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE map PUBLIC "-//OASIS//DTD DITA Map//EN" "map.dtd">
   
   <map>
     <title>Glossary key definitions</title>
   
     <keydef keys="gl-sample-rate" href="topics/glossary/g-sample-rate.dita"/>
     <keydef keys="gl-bit-depth" href="topics/glossary/g-bit-depth.dita"/>
     <keydef keys="gl-clipping" href="topics/glossary/g-clipping.dita"/>
     <keydef keys="gl-normalization" href="topics/glossary/g-normalization.dita"/>
     <keydef keys="gl-compression" href="topics/glossary/g-compression.dita"/>
     <keydef keys="gl-waveform" href="topics/glossary/g-waveform.dita"/>
     <keydef keys="gl-decibel" href="topics/glossary/audio-units.dita#gl-decibel"/>
     <keydef keys="gl-hertz" href="topics/glossary/audio-units.dita#gl-hertz"/>
   </map>
   ```

2. **Edit `audacity-guide.ditamap`**
   The keydef map joins the key space by `<mapref>`, and a *Glossary*
   heading puts the group and the six entries in the table of contents.

   ```diff title="audacity-guide.ditamap"
   --- a/audacity-guide.ditamap
   +++ b/audacity-guide.ditamap
   @@ -8,6 +8,7 @@
      </topicmeta>
    
      <mapref href="keydefs-product.ditamap"/>
   +  <mapref href="keydefs-glossary.ditamap"/>
      <keydef keys="start-here" href="topics/what-is-audacity.dita"/>
      <!-- Warehouses: pulled in by conref and conkeyref, never pages of their own. -->
      <keydef keys="common-notes" href="shared/common-notes.dita"/>
   @@ -37,6 +38,16 @@
          </topicmeta>
        </topicref>
      </topichead>
   +  <topichead navtitle="Glossary">
   +    <topicref href="topics/glossary/audio-units.dita"/>
   +    <topicref href="topics/glossary/g-bit-depth.dita"/>
   +    <topicref href="topics/glossary/g-clipping.dita"/>
   +    <topicref href="topics/glossary/g-compression.dita"/>
   +    <topicref href="topics/glossary/g-normalization.dita"/>
   +    <topicref href="topics/glossary/g-sample-rate.dita"/>
   +    <topicref href="topics/glossary/g-waveform.dita"/>
   +  </topichead>
   +
      <reltable>
        <title>Concept, task and reference links</title>
        <relheader>
   ```

3. **Read the keys**

   - A key per entry, named `gl-…` so a glance at a `@keyref` says it is a
     glossary key.
   - The two entries inside the group are keyed with a fragment,
     `audio-units.dita#gl-decibel`: the key resolves to the `<glossentry>`
     element by its `@id`, not to the group. That is what lets an
     `<abbreviated-form>` find the surface form of the right entry.
   - The entries are ordinary topics in the map. DITA-OT publishes them as
     pages under `out/full/topics/glossary/`, and links from the text land
     on them.
::::

## Step 3: Bind the terms in the text

::::steps
1. **Edit the three topics**

   ```diff title="topics/what-is-digital-audio.dita"
   --- a/topics/what-is-digital-audio.dita
   +++ b/topics/what-is-digital-audio.dita
   @@ -10,7 +10,7 @@
        If you would rather start recording, go straight to <xref href="recording-your-first-track.dita"/> and come back later.</p>
        <section>
          <title>Waveforms</title>
   -      <p>Sound travels through air as a continuous <term>waveform</term> of pressure changes.
   +      <p>Sound travels through air as a continuous <term keyref="gl-waveform">waveform</term> of pressure changes.
          A microphone converts these pressure changes into an electrical signal.
          To store the signal digitally, the computer takes thousands of measurements, called <term>samples</term>, of the signal's amplitude every second.</p>
          <fig id="fig-waveform">
   @@ -34,7 +34,7 @@
        </section>
        <section>
          <title>Sample rate</title>
   -      <p>The <term>sample rate</term> is the number of samples captured per second, measured in hertz (Hz).
   +      <p>The <term keyref="gl-sample-rate">sample rate</term> is the number of samples captured per second, measured in <abbreviated-form keyref="gl-hertz"/>.
          Common sample rates are:</p>
          <sl>
            <sli>44,100 Hz: CD quality, enough for most audio work</sli>
   @@ -45,7 +45,7 @@
        </section>
        <section>
          <title>Bit depth</title>
   -      <p><term>Bit depth</term> determines how precisely each sample's amplitude is recorded.</p>
   +      <p><term keyref="gl-bit-depth">Bit depth</term> determines how precisely each sample's amplitude is recorded.</p>
          <simpletable>
            <sthead>
              <stentry>Bit depth</stentry>
   @@ -75,7 +75,7 @@
            <dlentry>
              <dt>Amplitude</dt>
              <dd>The height of a sound wave, corresponding to how loud the sound is.
   -          Measured in decibels (dB), a ratio against a reference level:
   +          Measured in <abbreviated-form keyref="gl-decibel"/>, a ratio against a reference level:
              <equation-block>
                <mathml>
                  <m:math xmlns:m="http://www.w3.org/1998/Math/MathML" display="block">
   @@ -93,7 +93,9 @@
              A level of <equation-inline><mathml><m:math xmlns:m="http://www.w3.org/1998/Math/MathML"><m:mrow><m:mo>−</m:mo><m:mn>6</m:mn><m:mtext>dB</m:mtext></m:mrow></m:math></mathml><ph>−6 dB</ph></equation-inline> is half the amplitude of the reference.</dd>
            </dlentry>
            <dlentry>
   -          <dt>Clipping</dt>
   +          <dt>
   +            <term keyref="gl-clipping">Clipping</term>
   +          </dt>
              <dd>Distortion that occurs when the signal exceeds the maximum amplitude the system can represent.
              It appears as flat-topped waveforms.</dd>
            </dlentry>
   ```

   ```diff title="topics/preparing-to-record.dita"
   --- a/topics/preparing-to-record.dita
   +++ b/topics/preparing-to-record.dita
   @@ -10,9 +10,9 @@
        </context>
        <steps-unordered>
          <step>
   -        <cmd>Speak at normal volume and watch the recording meter; aim for peaks around -12 dB.</cmd>
   +        <cmd>Speak at normal volume and watch the recording meter; aim for peaks around -12 <abbreviated-form keyref="gl-decibel"/>.</cmd>
            <tutorialinfo>
   -          <p>Leaving headroom avoids clipping, the flat-topped distortion that no effect can repair.</p>
   +          <p>Leaving headroom avoids <term keyref="gl-clipping">clipping</term>, the flat-topped distortion that no effect can repair.</p>
            </tutorialinfo>
          </step>
          <step>
   ```

   ```diff title="topics/supported-audio-formats.dita"
   --- a/topics/supported-audio-formats.dita
   +++ b/topics/supported-audio-formats.dita
   @@ -7,7 +7,7 @@
      <refbody>
        <section>
          <p><keyword keyref="product-name"/> imports and exports audio in several formats.
   -      <term>Lossless</term> formats keep every sample of the source; <term>lossy</term> formats discard detail to make smaller files.
   +      <term>Lossless</term> formats keep every sample at the recorded <term keyref="gl-bit-depth">bit depth</term> of the source; <term>lossy</term> formats discard detail to make smaller files.
          Exported files are named after the project, so <filepath>podcast-episode-1.aup3</filepath> exports as <filepath>podcast-episode-1.mp3</filepath>.</p>
        </section>
        <section id="choosing">
   @@ -46,7 +46,7 @@
              </row>
              <row>
                <entry>MP3</entry>
   -            <entry>Compressed</entry>
   +            <entry><term keyref="gl-compression">Compressed</term></entry>
                <entry>Lossy</entry>
                <entry>Widely supported, small files.
                Needs the <i>LAME</i> encoder, bundled since version 2.3.2.
   ```

2. **Read the two forms**

   - `<term keyref="gl-clipping">clipping</term>` keeps the word as
     written and binds it to the entry. The output makes it a link to the
     definition, with the `<glossdef>` as the link's hover text. The
     content stays, so "Compressed" in a table cell and "clipping" in a
     sentence keep their own capitalization and grammar.
   - `<abbreviated-form keyref="gl-decibel"/>` is empty. The processor
     writes the term for you: the `<glossSurfaceForm>` where the reader
     first meets it, and the `<glossAbbreviation>` after that. In the
     HTML5 output of DITA-OT 4.3.5 each of the two topics that uses it
     shows `decibel (dB)`, linked to the entry, and "hertz (Hz)" likewise.
   - `<abbreviated-form>` inserts the surface form exactly as the glossary
     entry writes it, in the singular. The output reads "-12 decibel (dB)"
     and "Measured in decibel (dB)". For a measurement after a number,
     where the reader expects "-12 dB", keep the plain unit instead.
   - The `<term>` elements without a `@keyref`, such as *Lossless* and
     *lossy*, are unchanged: they mark a term but have no entry to link to.
   - The *Clipping* `<dt>` is laid out on three lines because that is how
     the formatter writes a `<dt>` whose only content is an element; the
     meaning is unchanged.
::::

## Step 4: Update the README and run the gate

::::steps
1. **Change the "You are on" line and the layout**

   ````diff title="README.md"
   --- a/README.md
   +++ b/README.md
   @@ -2,7 +2,7 @@
    
    Build a guide through 27 stages. Maps begin at stage 02; HTML publication begins at stage 03.
    
   -You are on stage 11: reuse.
   +You are on stage 12: glossary.
    
    ## Stages
    
   ````

2. **Format and check**

   ```bash
   dogsbay-xml format -i topics/*.dita topics/glossary/*.dita
   scripts/check-stage.sh
   ```

   ```
   == validate-project  /home/you/audacity-guide ==
   21 file(s): 21 valid, 0 invalid.

   == project-health  /home/you/audacity-guide ==
   Root map: audacity-guide.ditamap (project config); house rules: none (none configured)
   Project is healthy: valid, no broken references, keys, orphans, or broken element ids.

   == build  project.json -> /tmp/check-stage-357688 ==
   all deliverables built

   STAGE OK
   ```

   Twenty-one files: nine topics, two shared topics, seven glossary files and
   three maps. Run `dita --project=project.json`, then open
   `out/full/topics/glossary/audio-units.html` after the
   build: the group is one page with both entries, each showing the term,
   the definition, the surface form and the alternative forms.
::::

A glossary key is a key like any other. Change the `<term keyref>` in
*Preparing to record* to `gl-clippng` and run the gate:

```
== project-health  /home/you/audacity-guide ==
Root map: audacity-guide.ditamap (project config); house rules: none (none configured)
Undefined keys (1):
  /home/you/audacity-guide/topics/preparing-to-record.dita:15  key 'gl-clippng' not defined
Unused keys (3):
  start-here  [/home/you/audacity-guide/audacity-guide.ditamap:12]
  digital-audio  [/home/you/audacity-guide/audacity-guide.ditamap:19]
  gl-normalization  [/home/you/audacity-guide/keydefs-glossary.ditamap:10]

Summary
  Undefined keys                1
  Unused keys                   3
FAIL: project-health found issues
```

`gl-normalization` appears as unused because no topic mentions
normalization yet; the entry is still published from the map. That line is
information and does not fail the stage on its own.

## What you learned

- `<glossentry>` with `<glossterm>` and `<glossdef>` is a topic per term;
  `<glossgroup>` is a topic that holds several.
- `<glossBody>`, `<glossSurfaceForm>`, `<glossAlt>` with
  `<glossAbbreviation>` or `<glossSynonym>`.
- A keydef map for the glossary, with keys to files and to entries inside
  a group by `file#id`.
- `<term keyref>` binds a written term to its entry;
  `<abbreviated-form keyref>` lets the processor write the surface form.

## Next lesson

Continue with [Stage 13: metadata and index](/part-2-maps/stage-13-metadata-and-index).

For the core course, use the [learning path](/start-here/learning-path) to skip optional modules.

[Compare this stage with its predecessor](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/11-reuse...tutorial/12-glossary).
