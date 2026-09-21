---
title: "Stage 23: Drafts and localization"
description: Mark the guide for review and translation with a language on every root, translate flags, a right-to-left phrase, sort keys, review markup and a change history.
type: tutorial
---

# Stage 23: Drafts and localization

A guide that is finished for its authors is not finished for a reviewer or
a translator. Both need things the text does not say: which language a
topic is in, which words must not be translated, which way a phrase runs,
how a term sorts when its letters would put it in the wrong place, what a
reviewer still has to check, and what changed since the last edition. DITA
carries all of that in the XML, as attributes and a few small elements, so
that it survives every build and every hand-off.

This stage touches every file once, for `xml:lang`, and a handful of them
for the rest: `translate="no"` on the product keys and the code, a Hebrew
phrase with `dir="rtl"`, a `<sort-as>` on a glossary term and an
`<index-see>` for its abbreviation, a `<draft-comment>` and a
`<required-cleanup>` for the reviewer, `status` and `rev` on new content,
and a `<change-historylist>` in a prolog. The gate reports the review
markup as warnings; stage 24 turns them into failures.

**Time:** about 30 minutes.
**You need:** stage 22 complete.

## Step 1: Language and translation

::::steps
1. **Add `xml:lang="en-GB"` to every root element**

   ```diff title="topics/what-is-audacity.dita"
   --- a/topics/what-is-audacity.dita
   +++ b/topics/what-is-audacity.dita
   @@ -1,7 +1,7 @@
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE concept PUBLIC "-//OASIS//DTD DITA Concept//EN" "concept.dtd">
    
   -<concept id="what-is-audacity">
   +<concept id="what-is-audacity" xml:lang="en-GB">
      <title>What is <keyword keyref="product-name"/>?</title>
      <shortdesc><keyword keyref="product-name"/> is a free, open-source audio editor and recorder for Windows, macOS and Linux.</shortdesc>
      <prolog>
   ```

   The same one-line change goes on the root element of all 35 `.dita`
   and `.ditamap` files on the branch: every topic, every glossary entry,
   the warehouses in `shared/`, the assessment in `learning/`, and all
   nine maps. The `compare` link at the end shows them all.

2. **Read it**
   - `xml:lang` is XML's own language attribute, in the `xml` namespace,
     so it needs no declaration. Its value is a BCP 47 language tag:
     `en-GB` for British English, `de` for German, `pt-BR` for Brazilian
     Portuguese. An element without one inherits its parent's, so one
     attribute on the root covers the file.
   - DITA-OT reads it. The map's language chooses the generated text in
     the output, the *CAUTION* of a hazard statement, the *Table 1* of a
     table title, the headings of the index, and the sort order of the
     index and glossary in the PDF. Each html5 page carries the topic's
     language as `<html lang="en-gb">`, which is what a screen reader
     uses to choose a voice.
   - A translation tool reads it too: the source language of every file
     it is given, and the target language once it writes the translated
     copy.

3. **Mark what must not be translated**

   ```diff title="keydefs-product.ditamap"
   --- a/keydefs-product.ditamap
   +++ b/keydefs-product.ditamap
   @@ -1,13 +1,13 @@
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE map PUBLIC "-//OASIS//DTD DITA Map//EN" "map.dtd">
    
   -<map>
   +<map xml:lang="en-GB">
      <title>Product key definitions</title>
    
      <keydef keys="product-name">
        <topicmeta>
          <keywords>
   -        <keyword>Audacity</keyword>
   +        <keyword translate="no">Audacity</keyword>
          </keywords>
        </topicmeta>
      </keydef>
   @@ -26,7 +26,7 @@
      <keydef keys="project-extension">
        <topicmeta>
          <keywords>
   -        <keyword>.aup3</keyword>
   +        <keyword translate="no">.aup3</keyword>
          </keywords>
        </topicmeta>
      </keydef>
   ```

   `translate="no"` on a `<keyword>` says its text is not to be
   translated. The product name and the project extension are marked
   once, where their values live; every `<keyword keyref="product-name"/>`
   in the topics pulls the marked text in. `translate` takes `yes` or
   `no` and inherits, so it can go on a block to cover everything inside.

4. **Mark the code and the commands**

   ```diff title="topics/scripting-reference.dita"
   --- a/topics/scripting-reference.dita
   +++ b/topics/scripting-reference.dita
   @@ -1,7 +1,7 @@
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE reference PUBLIC "-//OASIS//DTD DITA Reference//EN" "reference.dtd">
    
   -<reference id="scripting-reference">
   +<reference id="scripting-reference" xml:lang="en-GB">
      <title>Scripting commands</title>
      <shortdesc>The scripting commands most useful for batch work, their parameters and the responses they return.</shortdesc>
      <prolog>
   @@ -42,7 +42,7 @@
        </section>
        <refsyn>
          <title>Commands</title>
   -      <parml>
   +      <parml translate="no">
            <plentry>
              <pt>
                <cmdname>SelectAll:</cmdname>
   @@ -68,7 +68,7 @@
          <title>Responses</title>
          <p>Every reply ends with a status line.
          Success looks like <msgph>BatchCommand finished: OK</msgph>; a failure names the command:</p>
   -      <msgblock>Export2: Filename=episode.mp3 NumChannels=1
   +      <msgblock translate="no">Export2: Filename=episode.mp3 NumChannels=1
    BatchCommand finished: Failed!</msgblock>
          <p>The <apiname>send</apiname> function in the sample script (<xref href="../samples/export-mp3.py" scope="external" format="py">export-mp3.py</xref>) returns the whole reply; check it for <msgnum>Failed!</msgnum> before sending the next command.</p>
        </section>
   ```

   ```diff title="topics/exporting-from-a-script.dita"
   --- a/topics/exporting-from-a-script.dita
   +++ b/topics/exporting-from-a-script.dita
   @@ -1,7 +1,7 @@
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE task PUBLIC "-//OASIS//DTD DITA Task//EN" "task.dtd">
    
   -<task id="exporting-from-a-script">
   +<task id="exporting-from-a-script" xml:lang="en-GB">
      <title>Exporting from a script</title>
      <shortdesc>Enable mod-script-pipe, then normalize and export the open project from a Python script.</shortdesc>
      <prolog>
   @@ -31,13 +31,13 @@
          <step>
            <cmd>Save the sample script as <filepath>export-mp3.py</filepath>.</cmd>
            <info>
   -          <codeblock outputclass="language-python"><coderef href="../samples/export-mp3.py"/></codeblock>
   +          <codeblock outputclass="language-python" translate="no"><coderef href="../samples/export-mp3.py"/></codeblock>
            </info>
          </step>
          <step>
            <cmd>Run it, naming the output file.</cmd>
            <info>
   -          <screen><userinput>python3 export-mp3.py episode.mp3</userinput>
   +          <screen translate="no"><userinput>python3 export-mp3.py episode.mp3</userinput>
    <systemoutput>BatchCommand finished: OK
    BatchCommand finished: OK
    BatchCommand finished: OK</systemoutput></screen>
   ```

   The parameter list, the message block, the code block and the screen
   from stage 21 are all things a translator must leave alone. The DITA
   specification says that many of the code elements, `<codeblock>`,
   `<screen>`, `<cmdname>`, `<parmname>`, `<varname>` among them, are
   `translate="no"` by default, but a translation tool sees only what is
   in the file, so the branch says it on the blocks. Marking the whole
   `<parml>` keeps the `<pd>` descriptions in English as well as the
   `<pt>` terms; mark each `<pt>` instead if the descriptions are to be
   translated.
::::

## Step 2: Direction and sort order

::::steps
1. **Edit `topics/what-is-digital-audio.dita`**

   ```diff title="topics/what-is-digital-audio.dita"
   --- a/topics/what-is-digital-audio.dita
   +++ b/topics/what-is-digital-audio.dita
   @@ -1,7 +1,7 @@
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE concept PUBLIC "-//OASIS//DTD DITA Concept//EN" "concept.dtd">
    
   -<concept id="what-is-digital-audio">
   +<concept id="what-is-digital-audio" xml:lang="en-GB">
      <title>What is digital audio?</title>
      <shortdesc>Digital audio is sound stored as numbers: samples taken thousands of times a second at a chosen precision.</shortdesc>
      <prolog>
   @@ -26,6 +26,7 @@
            <indexterm>decibel<index-see-also>amplitude</index-see-also></indexterm>
            <indexterm>amplitude</indexterm>
            <indexterm><index-sort-as>clipping</index-sort-as>Clipping (distortion)</indexterm>
   +        <indexterm>dB<index-see>decibel</index-see></indexterm>
          </keywords>
        </metadata>
      </prolog>
   @@ -133,6 +134,7 @@
            </dlentry>
          </dl>
          <lq reftitle="The Audacity Manual">Audio is measured in decibels because the ear hears ratios, not differences.</lq>
   +      <p>The same idea in Hebrew, for a right-to-left example: <ph xml:lang="he" dir="rtl">האוזן שומעת יחסים</ph> (the ear hears ratios).</p>
        </section>
      </conbody>
      <related-links>
   ```

2. **Read it**
   - `xml:lang="he"` on a `<ph>` overrides the root's `en-GB` for that
     phrase alone. `dir="rtl"` says the phrase runs right to left; the
     values are `ltr`, `rtl`, `lro` and `rlo`, the last two overriding
     the characters' own direction. Hebrew and Arabic letters carry their
     direction, so the attribute matters most where a phrase mixes them
     with digits and punctuation, and it is what a stylesheet keys on.
   - `<index-see>`, from stage 12, adds an index entry that redirects:
     *dB, see decibel*. The abbreviation gets its own line in the index
     and the reader is sent to the entry that has the page numbers.

3. **Edit `topics/glossary/audio-units.dita`**

   ```diff title="topics/glossary/audio-units.dita"
   --- a/topics/glossary/audio-units.dita
   +++ b/topics/glossary/audio-units.dita
   @@ -1,7 +1,7 @@
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE glossgroup PUBLIC "-//OASIS//DTD DITA Glossary Group//EN" "glossgroup.dtd">
    
   -<glossgroup id="audio-units">
   +<glossgroup id="audio-units" xml:lang="en-GB">
      <title>Units</title>
      <prolog>
        <author type="creator">The Audacity tutorial team</author>
   @@ -13,7 +13,7 @@
        </metadata>
      </prolog>
      <glossentry id="gl-decibel">
   -    <glossterm>decibel</glossterm>
   +    <glossterm><sort-as value="decibel"/>dB (decibel)</glossterm>
        <glossdef>A logarithmic unit for the ratio between two levels. In audio editing, 0 dB is the loudest level a file can hold, and quieter sounds are negative.</glossdef>
        <prolog>
          <author type="creator">The Audacity tutorial team</author>
   ```

   The displayed term is now *dB (decibel)*, and `<sort-as value="decibel"/>`
   tells a sorted glossary to place it as if it read *decibel*. `<sort-as>`
   is a DITA 1.3 element allowed in titles and glossary terms. It is
   for the cases a plain sort gets wrong: a term that starts with a symbol,
   a digit or an abbreviation, or a language where the sort key is not the
   displayed characters, as with Japanese kanji sorted by their reading.
   `<index-sort-as>` on the *Clipping (distortion)* entry, three lines up
   in the same prolog, is the index term's equivalent.
::::

## Step 3: Review markup and a change history

::::steps
1. **Edit `topics/exporting-audio.dita`**

   ```diff title="topics/exporting-audio.dita"
   --- a/topics/exporting-audio.dita
   +++ b/topics/exporting-audio.dita
   @@ -1,13 +1,14 @@
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE task PUBLIC "-//OASIS//DTD DITA Task//EN" "task.dtd">
    
   -<task id="exporting-audio">
   +<task id="exporting-audio" xml:lang="en-GB" status="changed">
      <title>Exporting audio</title>
      <shortdesc>Export a finished project as a WAV, MP3, OGG or FLAC file that other programs and devices can play.</shortdesc>
      <prolog>
        <author type="creator">The Audacity tutorial team</author>
        <critdates>
          <created date="2026-01-22"/>
   +      <revised modified="2026-03-05"/>
        </critdates>
        <metadata>
          <audience type="user" experiencelevel="novice"/>
   @@ -22,6 +23,13 @@
            <indexterm>audio formats<indexterm>MP3</indexterm><indexterm>WAV</indexterm></indexterm>
          </keywords>
        </metadata>
   +    <change-historylist>
   +      <change-item>
   +        <change-person>The Audacity tutorial team</change-person>
   +        <change-completed>2026-03-05</change-completed>
   +        <change-summary>Marked the metadata-tags step as new in 3.4.</change-summary>
   +      </change-item>
   +    </change-historylist>
      </prolog>
      <taskbody>
        <prereq>
   @@ -67,6 +75,7 @@
          </step>
        </steps>
        <result>
   +      <draft-comment author="reviewer" time="2026-03-05" disposition="open">Check the 3.4 export dialog: is the metadata tags step still a separate dialog?</draft-comment>
          <p>The exported file is in the folder you chose.
          The project itself is unchanged.</p>
        </result>
   ```

2. **Read it**
   - `status="changed"` on the root says the topic changed since the
     last release. The values are `new`, `changed`, `deleted` and
     `unchanged`, and the attribute is allowed on most elements, so a
     single row or paragraph can carry it. Nothing renders it by default;
     a DITAVAL or a report can select on it.
   - `<revised modified="2026-03-05"/>` joins the `<created>` from stage
     12 in `<critdates>`. The content model is `created?` then `revised*`,
     in that order, one `<revised>` per revision.
   - `<change-historylist>` is DITA 1.3's structured change log, in the
     `<prolog>` after `<metadata>`. Each `<change-item>` holds who,
     `<change-person>`, when, `<change-completed>`, and what,
     `<change-summary>`. The other children are `<change-organization>`,
     `<change-revisionid>`, `<change-request-reference>` and
     `<change-started>`.
   - `<draft-comment>` is a note to the reviewer, in the flow where it
     applies. `author` and `time` say who and when; `disposition` tracks
     it, `open`, `accepted`, `rejected`, `deferred`, `duplicate`,
     `reopened`, `unassigned` or `completed`. DITA-OT drops draft
     comments from the output unless you ask for them, which step 4
     shows.

3. **Edit `topics/podcast-production-workflow.dita`**

   ```diff title="topics/podcast-production-workflow.dita"
   --- a/topics/podcast-production-workflow.dita
   +++ b/topics/podcast-production-workflow.dita
   @@ -1,7 +1,7 @@
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE concept PUBLIC "-//OASIS//DTD DITA Concept//EN" "concept.dtd">
    
   -<concept id="podcast-production-workflow">
   +<concept id="podcast-production-workflow" xml:lang="en-GB">
      <title>Podcast production workflow</title>
      <shortdesc>A typical order of work for a spoken-word podcast: prepare, record, clean up, level and export.</shortdesc>
      <prolog>
   @@ -51,6 +51,7 @@
            <li><b><term keyref="gl-compression">Compression</term></b>: even out volume differences (threshold -18 dB, ratio 3:1).</li>
            <li><b>Export</b>: MP3 at 128 kbps mono for distribution, by hand or from a script (<xref keyref="scripting"/>).</li>
          </ol>
   +      <required-cleanup remap="table">Target loudness per platform: Spotify -14 LUFS, Apple -16 LUFS, YouTube -14 LUFS. Turn into a table once the list is confirmed.</required-cleanup>
        </section>
        <section audience="beginner">
          <title>Getting started with podcasting</title>
   ```

   `<required-cleanup>` marks content that is not in its final form. It
   is allowed almost anywhere and holds almost anything, which is the
   point: it is where content goes that does not yet fit the grammar of
   its place. `remap="table"` records what it should become. Like a draft
   comment, it is dropped from the output by default.

4. **Edit `topics/effects-reference.dita`**

   ```diff title="topics/effects-reference.dita"
   --- a/topics/effects-reference.dita
   +++ b/topics/effects-reference.dita
   @@ -3,7 +3,7 @@
    
    <!-- Nested topics: two topics in one file. The map decides whether they publish
         as one page (the default) or one page each (chunk="by-topic"). -->
   -<reference id="effects-reference">
   +<reference id="effects-reference" xml:lang="en-GB">
      <title>Effects reference</title>
      <shortdesc>The built-in effects, what each one does, and the parameters that matter.</shortdesc>
      <prolog>
   @@ -19,6 +19,7 @@
            <keyword>Amplify</keyword>
            <keyword>Compressor</keyword>
            <keyword>Normalize</keyword>
   +        <keyword>Loudness Normalization</keyword>
            <indexterm>effects<indexterm>reference</indexterm></indexterm>
          </keywords>
        </metadata>
   @@ -70,6 +71,11 @@
                <entry>Sets the peak amplitude to a target level and optionally removes DC offset.</entry>
                <entry>Peak amplitude (dB), typically -1.0; <uicontrol>Remove DC offset</uicontrol>.</entry>
              </row>
   +          <row rev="3.4" status="new">
   +            <entry>Loudness Normalization</entry>
   +            <entry>Sets the perceived loudness of the selection to a target, for podcast platforms that specify one.</entry>
   +            <entry>Target (LUFS), typically -16 for podcasts; <uicontrol>Normalize</uicontrol> to perceived loudness or RMS.</entry>
   +          </row>
              <row>
                <entry>Fade In, Fade Out</entry>
                <entry>Ramps the volume of the selection up from silence, or down to it.</entry>
   ```

   A new effect for the next release, as a table row. `rev="3.4"` is the
   revision mark from stage 13, which `filters/review.ditaval` flags;
   `status="new"` says the same thing to a tool that reads status rather
   than revisions. The new effect's name also joins the `<keywords>` in
   the prolog, which the metadata policy from stage 12 asks for.
::::

## Step 4: README and the gate

::::steps
1. **Change the "You are on" line**

   ```diff title="README.md"
   --- a/README.md
   +++ b/README.md
   @@ -5,7 +5,7 @@ branch is the previous branch plus one lesson, from a single concept topic to a
    complete, publish-ready **Audacity User Guide**. The diff between two branches
    is the lesson.
    
   -You are on **stage 22 — learning**: a learning assessment with true/false, single, multiple and sequencing questions.
   +You are on **stage 23 — drafts and localization**: review markup, change history, language and direction, translate flags and sort keys.
    
    ## Stages
    
   ```

2. **Format and check**

   ```bash
   dogsbay-xml format -i topics/*.dita topics/glossary/*.dita shared/*.dita *.ditamap
   scripts/check-stage.sh
   ```

   ```
   == validate-project  /home/you/audacity-guide ==
   /home/you/audacity-guide/learning/check-your-understanding.dita:
     -1:-1  error: Validation failed: /home/you/audacity-guide/learning/learningAssessment.dtd (No such file or directory)
   42 file(s): 41 valid, 1 invalid.

   == validate learning/ against the DITA-OT catalog ==
   VALID

   == project-health  /home/you/audacity-guide ==
   Root map: audacity-guide.ditamap (project config); house rules: none (none configured)
   Authoring leftovers (2; warnings):
     /home/you/audacity-guide/topics/exporting-audio.dita:78  draft-comment: Check the 3.4 export dialog: is the metadata tags step still a separate dialog?
     /home/you/audacity-guide/topics/podcast-production-workflow.dita:54  required-cleanup: Target loudness per platform: Spotify -14 LUFS, Apple -16 LUFS, YouTube -14 LUFS. Turn into a table…
   Project is healthy: valid, no broken references, keys, orphans, or broken element ids.

   == validate-conditions  /home/you/audacity-guide  (scheme: subject-scheme.ditamap) ==
   42 file(s): 42 pass, 0 with violations.

   == build  project.json -> /tmp/check-stage-<pid> ==
   all deliverables built

   STAGE OK
   ```

   `project-health` now has something to say. *Authoring leftovers* is
   its name for review markup, and it counts the draft comment and the
   required cleanup, two, with the file and line of each. They are
   warnings: the project is still healthy, the gate still prints
   `STAGE OK`, and the markup goes into the repository as the record of
   what is unfinished. Stage 24 adds a house rule that makes the same two
   findings failures, so that they cannot reach a release.

3. **Read the review build**
   The `review` deliverable from stage 13 flags revision `3.4`, so in
   `out/review/topics/effects-reference.html` the new row carries the
   flag's colour:

   ```
   <tr style="color:#b00020;" class="row">
   <td class="entry" headers="effects-reference__entry__1">Loudness Normalization</td>
   <td class="entry" headers="effects-reference__entry__2">Sets the perceived loudness of the selection to a target, for podcast platforms that specify one.</td>
   <td class="entry" headers="effects-reference__entry__3">Target (LUFS), typically -16 for podcasts; <span class="ph uicontrol">Normalize</span> to perceived loudness or RMS.</td>
   </tr>
   ```

   The `[new in 3.4]` start-flag text is not on the row: DITA-OT puts
   flag text only where text can go, and a table row has no place for
   it outside its cells. On the step that stage 13 marked, in
   `out/review/topics/exporting-audio.html`, both the colour and the
   text appear:

   ```
   <span style="color:#b00020;" class="ph cmd">[new in 3.4]Fill in the <span class="keyword wintitle">Edit Metadata Tags</span> dialog and click <span class="ph uicontrol">OK</span>.</span>
   ```

   The draft comment is in neither build, nor is the required cleanup:
   DITA-OT drops `<draft-comment>` and `<required-cleanup>` unless the
   build asks for them with `args.draft=yes`. A search for the comment's
   text across every html5 deliverable the gate built,
   `grep -rl 'metadata tags step' /tmp/check-stage-<pid>`, prints nothing.
   To see them, build the review map once with the parameter:

   ```bash
   dita -i audacity-guide.ditamap -f html5 --filter=filters/review.ditaval -o /tmp/review-draft --args.draft=yes
   ```

   ```
   <div class="draft-comment" style="background-color: #99FF99; border: 1pt black solid;"><strong>Draft comment: </strong>reviewer open 2026-03-05<br>Check the 3.4 export dialog: is the metadata tags step still a separate dialog?</div>
   <div class="required-cleanup" style="background-color: #FFFF99; color:#CC3333; border: 1pt black solid;"><strong>Required cleanup: </strong>[table] Target loudness per platform: Spotify -14 LUFS, Apple -16 LUFS, YouTube -14 LUFS. Turn into a table once the list is confirmed.</div>
   ```

   The comment and the cleanup are rendered in place, boxed and coloured,
   with the author, disposition and date. So the review markup has two
   audiences: a reviewer who builds with `args.draft=yes` and sees it
   where it applies, and a release that never does.
::::

Now make the mistake. Change the new row in `topics/effects-reference.dita`
to `status="draft"`, a value that reads well and does not exist, and run the
gate with `SKIP_BUILD=1`:

```
== validate-project  /home/you/audacity-guide ==
/home/you/audacity-guide/learning/check-your-understanding.dita:
  -1:-1  error: Validation failed: /home/you/audacity-guide/learning/learningAssessment.dtd (No such file or directory)
/home/you/audacity-guide/topics/effects-reference.dita:
  74:41  error: Attribute "status" with value "draft" must have a value from the list "changed deleted new unchanged -dita-use-conref-target ".
42 file(s): 40 valid, 2 invalid.
FAIL: validation errors
```

`status` is an enumerated attribute in the DTD, unlike `rev`, which takes
any string. The four values, plus `-dita-use-conref-target`, which every
enumerated attribute accepts for conref, are the whole vocabulary; a workflow state such
as *draft* or *in review* belongs in a `<draft-comment disposition>` or in
`<change-historylist>`, not in `status`.

One caution about the formatter. `dogsbay-xml format -i` escapes `&` in
text on every pass, so an `&amp;` that is in a file becomes `&amp;amp;` the
next time the file is formatted, and worse the time after. Until that is
fixed in the editor, do not put character entities in text: write *and*,
and let an element whose only content would be a space, such as the
`<repsep>` of stage 21, stay empty.

## What you learned

- `xml:lang` on every root; `translate="no"` on the product keys and the
  code; `xml:lang` and `dir="rtl"` on a phrase.
- `<sort-as>` in a glossary term, `<index-see>` for an abbreviation.
- `<draft-comment author time disposition>` and
  `<required-cleanup remap>`: review markup that DITA-OT drops unless
  `args.draft=yes`.
- `status` on a topic or a row, `<revised>` beside `<created>`, and
  `<change-historylist>` with `<change-item>`.
- `project-health` reports review markup as *Authoring leftovers*,
  warnings until a house rule says otherwise.

## Where to go next

:::cards
- **[Stage 24: House rules](/part-5-governance/stage-24-house-rules)** {icon="arrow-right"}
  The house style as Schematron, enforced by the gate and explained to
  agents.

- **[Compare 22 to 23 on GitHub](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/22-learning...tutorial/23-drafts-and-localization)** {icon="github"}
  Exactly what this stage added, all 36 files.
:::
