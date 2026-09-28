---
title: "Stage 25: House rules"
description: Write the house style as Schematron, name it in the project config so the gate enforces it, explain it to agents, and fix the ten violations it finds.
type: tutorial
---

# Stage 25: House rules

Add Schematron rules for the project's house style: require short
descriptions, use semantic markup for UI labels, reference product names
through keys, and resolve review markup before release.

Configure `house-style.sch` in `.dogsbay/config.xml` and document the
rules in `AGENTS.md` and the project skill. Resolve the ten reported
violations, including converting the cleanup content to a table and
recording the review decision in the change history.

**Time:** about 40 minutes.
**You need:** stage 24 complete.


Recorded diagnostic examples below come from earlier runs. File counts, paths, and stage numbers can differ. Run the gate for your current checkout.

## Step 1: The rules

::::steps
1. **Create `house-style.sch`**

   ```xml title="house-style.sch"
   <?xml version="1.0" encoding="UTF-8"?>
   <!--
     Audacity guide house style as machine-checkable rules.
     Run across the project with:  dogsbay-xml project-health . (house rules from .dogsbay/config.xml)
     (or ask the agent: "run our house-style rules and fix the violations").
   
     Design notes:
      - The product-name / extension tests match direct text() only, so a literal
        inside a descendant <filepath>/<codeblock> (e.g.
        <filepath>C:\Program Files\Audacity</filepath>) does NOT trip the enclosing
        <p> — the house rule deliberately leaves such literals alone.
      - Coverage spans the places hardcoded names actually hide: prose <p>, list
        items <li>, table cells <entry>, definitions <dd>, shortdescs, and step
        commands <cmd>. One pattern per element keeps each rule readable and its
        report message specific.
   -->
   <schema xmlns="http://purl.oclc.org/dsdl/schematron">
     <title>Audacity User Guide house style</title>
   
     <!-- every topic needs a shortdesc (one pattern per topic type) -->
     <pattern id="shortdesc-topic"><rule context="topic"><assert test="shortdesc">Every topic needs a shortdesc (a one- or two-sentence description).</assert></rule></pattern>
     <pattern id="shortdesc-concept"><rule context="concept"><assert test="shortdesc">Every topic needs a shortdesc (a one- or two-sentence description).</assert></rule></pattern>
     <pattern id="shortdesc-task"><rule context="task"><assert test="shortdesc">Every topic needs a shortdesc (a one- or two-sentence description).</assert></rule></pattern>
     <pattern id="shortdesc-reference"><rule context="reference"><assert test="shortdesc">Every topic needs a shortdesc (a one- or two-sentence description).</assert></rule></pattern>
   
     <pattern id="ui-labels-use-uicontrol">
       <rule context="b">
         <report test="true()">Use uicontrol for UI labels (and drop decorative bold); do not use b.</report>
       </rule>
     </pattern>
   
     <!-- no hardcoded product name; direct text() only (filepath/codeblock literals exempt) -->
     <pattern id="pn-p"><rule context="p"><report test="text()[contains(., 'Audacity')]">Do not hardcode the product name "Audacity" in prose; use a keyword with keyref="product-name".</report></rule></pattern>
     <pattern id="pn-li"><rule context="li"><report test="text()[contains(., 'Audacity')]">Do not hardcode the product name "Audacity" in a list item; use keyref="product-name".</report></rule></pattern>
     <pattern id="pn-entry"><rule context="entry"><report test="text()[contains(., 'Audacity')]">Do not hardcode the product name "Audacity" in a table cell; use keyref="product-name".</report></rule></pattern>
     <pattern id="pn-dd"><rule context="dd"><report test="text()[contains(., 'Audacity')]">Do not hardcode the product name "Audacity" in a definition; use keyref="product-name".</report></rule></pattern>
     <pattern id="pn-shortdesc"><rule context="shortdesc"><report test="text()[contains(., 'Audacity')]">Do not hardcode the product name "Audacity" in a shortdesc; use keyref="product-name".</report></rule></pattern>
     <pattern id="pn-cmd"><rule context="cmd"><report test="text()[contains(., 'Audacity')]">Do not hardcode the product name "Audacity" in a step command; use keyref="product-name".</report></rule></pattern>
   
     <!-- no hardcoded project extension; use keyref="project-extension" -->
     <pattern id="ext-p"><rule context="p"><report test="text()[contains(., '.aup3')]">Do not hardcode ".aup3"; use keyref="project-extension".</report></rule></pattern>
     <pattern id="ext-li"><rule context="li"><report test="text()[contains(., '.aup3')]">Do not hardcode ".aup3"; use keyref="project-extension".</report></rule></pattern>
     <pattern id="ext-entry"><rule context="entry"><report test="text()[contains(., '.aup3')]">Do not hardcode ".aup3"; use keyref="project-extension".</report></rule></pattern>
     <pattern id="ext-dd"><rule context="dd"><report test="text()[contains(., '.aup3')]">Do not hardcode ".aup3"; use keyref="project-extension".</report></rule></pattern>
   
     <!-- leftover authoring markers must not ship -->
     <pattern id="todo-p"><rule context="p"><report test="contains(., 'TODO') or contains(., 'FIXME') or contains(., 'TBD')">Remove authoring markers (TODO/FIXME/TBD) before publishing.</report></rule></pattern>
     <pattern id="todo-li"><rule context="li"><report test="contains(., 'TODO') or contains(., 'FIXME') or contains(., 'TBD')">Remove authoring markers (TODO/FIXME/TBD) before publishing.</report></rule></pattern>
     <pattern id="todo-cmd"><rule context="cmd"><report test="contains(., 'TODO') or contains(., 'FIXME') or contains(., 'TBD')">Remove authoring markers (TODO/FIXME/TBD) before publishing.</report></rule></pattern>
     <pattern id="todo-draftcomment"><rule context="draft-comment"><report test="true()">Remove draft-comment before publishing.</report></rule></pattern>
     <pattern id="todo-reqcleanup"><rule context="required-cleanup"><report test="true()">Remove required-cleanup before publishing.</report></rule></pattern>
   
     <!-- a step command must say something (conref'd steps exempt) -->
     <pattern id="cmd-not-empty"><rule context="cmd"><assert test="normalize-space(.) != '' or @conref or @conkeyref or parent::step/@conref or parent::step/@conkeyref">A cmd must have text, or pull content via conref.</assert></rule></pattern>
   
     <pattern id="note-type-allowed">
       <rule context="note[@type]">
         <assert test="@type = 'note' or @type = 'tip' or @type = 'important' or @type = 'caution' or @type = 'warning' or @type = 'attention' or @type = 'danger' or @type = 'fastpath' or @type = 'remember' or @type = 'restriction' or @type = 'trouble' or @type = 'other'">note type is not an allowed DITA value.</assert>
       </rule>
     </pattern>
   
     <pattern id="step-has-cmd">
       <rule context="step">
         <assert test="cmd">Every step must contain a cmd.</assert>
       </rule>
     </pattern>
   </schema>
   ```

2. **Read it**

   - A Schematron `<schema>` is a list of `<pattern>`s. Each `<pattern>`
     holds `<rule>`s; a `<rule context="…">` names, in XPath, the
     elements it applies to; inside it `<assert test="…">` fires when
     the test is false and `<report test="…">` fires when it is true.
     The element content supplies the diagnostic message. These are the
     Schematron constructs used in this lesson.
   - The four `shortdesc-*` patterns are one rule each because a rule's
     context is one XPath: `topic`, `concept`, `task`, `reference`. The
     `<troubleshooting>` and `<glossentry>` topics are not listed, so a
     glossary entry, which has no `<shortdesc>` by design, passes.
   - `ui-labels-use-uicontrol` fires on every `<b>`: `test="true()"`
     with a `<report>` is how to forbid an element outright.
   - The product-name and extension rules test `text()` only, the direct
     text of a `<p>`, `<li>`, `<entry>`, `<dd>`, `<shortdesc>` or
     `<cmd>`. A literal inside a child element does not count, so
     `<filepath>C:\Program Files\Audacity</filepath>` in a step is left
     alone, and so is *The Audacity Manual* once it is inside `<cite>`.
     One pattern per element keeps each message specific.
   - The `todo-*` patterns catch authoring markers in text and, with
     `test="true()"`, any `<draft-comment>` or `<required-cleanup>`:
     the two warnings from stage 24 are now violations.
   - `cmd-not-empty` allows an empty `<cmd/>` only when the `<cmd>` or
     its `<step>` carries `conref` or `conkeyref`, which is the
     placeholder pattern from stage 11. `note-type-allowed` lists the
     `type` values the DTD accepts, except `notice`. DITA 1.3 allows
     `notice`, but this rule reports it as a violation. `step-has-cmd` is
     a DTD rule restated so that its message is the house's.
   - The comment at the top says how to run the rules and why they are
     shaped as they are. A rules file is read by the next writer more
     often than it is edited; say what it is for.
::::

## Step 2: Run them on stage 24

::::steps
1. **Edit `.dogsbay/config.xml`**

   ```diff title=".dogsbay/config.xml"
   --- a/.dogsbay/config.xml
   +++ b/.dogsbay/config.xml
   @@ -6,6 +6,7 @@
      <framework>DITA-OT 4.3.5</framework>
      <format-style indent="spaces" size="2" max-line-width="0" preserve-mixed="true" newline="lf" final-newline="true" preserve-text-breaks="true" preserve-blank-lines="true" text-continuation="block" trim-whitespace="true"/>
      <default-deliverable file="project.json" name="full"/>
   +  <default-schematron>house-style.sch</default-schematron>
      <metadata-policy>
        <rule topic-type="task" field="created" presence="required" pattern="\d{4}-\d\d-\d\d"/>
        <rule field="author" presence="recommended"/>
   ```

   `<default-schematron>` names the rules file relative to the project
   root. From now on `project-health` runs it with everything else, the
   editor uses the same setting, and the `house rules:` part of the
   health report's first line names it instead of saying *none*.

2. **Run the rules before you change anything else**
   With the rules file and the config line in place, and the files still
   as they were at the end of stage 24:

   ```bash
   dogsbay-xml project-health --include=schematron .
   ```

   ```
   Root map: audacity-guide.ditamap (project config); house rules: house-style.sch (project config)
   House rules (4 of 42 files):
     /home/you/audacity-guide/audacity-guide.ditamap:7 — Do not hardcode the product name "Audacity" in a shortdesc; use keyref="product-name".
     /home/you/audacity-guide/topics/about-this-guide.dita:19 — Do not hardcode the product name "Audacity" in prose; use a keyword with keyref="product-name".
     /home/you/audacity-guide/topics/about-this-guide.dita:25 — Do not hardcode the product name "Audacity" in prose; use a keyword with keyref="product-name".
     /home/you/audacity-guide/topics/exporting-audio.dita:78 — Remove draft-comment before publishing.
     /home/you/audacity-guide/topics/podcast-production-workflow.dita:48 — Use uicontrol for UI labels (and drop decorative bold); do not use b.
     /home/you/audacity-guide/topics/podcast-production-workflow.dita:49 — Use uicontrol for UI labels (and drop decorative bold); do not use b.
     /home/you/audacity-guide/topics/podcast-production-workflow.dita:50 — Use uicontrol for UI labels (and drop decorative bold); do not use b.
     /home/you/audacity-guide/topics/podcast-production-workflow.dita:51 — Use uicontrol for UI labels (and drop decorative bold); do not use b.
     /home/you/audacity-guide/topics/podcast-production-workflow.dita:52 — Use uicontrol for UI labels (and drop decorative bold); do not use b.
     /home/you/audacity-guide/topics/podcast-production-workflow.dita:54 — Remove required-cleanup before publishing.

   Summary
     House rules                  10  in 4 of 42 files
       Use uicontrol for UI labels (and drop decorative bold); do no…    5
       Do not hardcode the product name "Audacity" in prose; use a k…    2
       Do not hardcode the product name "Audacity" in a shortdesc; u…    1
       Remove draft-comment before publishing.                           1
       Remove required-cleanup before publishing.                        1
   ```

   Ten violations in four files, every one a house-style decision the
   README had stated and nobody had checked. The main map's `<shortdesc>`
   spells out the product name. *About this guide* names it twice in
   prose, once as part of a title and once as part of an organization.
   The podcast workflow has five decorative `<b>`s and the
   `<required-cleanup>` from stage 24; *Exporting audio* has the
   `<draft-comment>`. `--include=schematron` runs only the rules, which is
   the fast loop while you fix them.
::::

## Step 3: Resolve the violations

::::steps
1. **Edit `audacity-guide.ditamap`**

   ```diff title="audacity-guide.ditamap"
   --- a/audacity-guide.ditamap
   +++ b/audacity-guide.ditamap
   @@ -4,7 +4,7 @@
    <map xml:lang="en-GB">
      <title>Audacity User Guide</title>
      <topicmeta>
   -    <shortdesc>Record, edit and export audio with Audacity, from your first track to a finished file.</shortdesc>
   +    <shortdesc>Record, edit and export audio with <keyword keyref="product-name"/>, from your first track to a finished file.</shortdesc>
        <author type="creator">The Audacity tutorial team</author>
        <publisher>DogsBay Ltd.</publisher>
        <copyright>
   ```

   A `<shortdesc>` in `<topicmeta>` takes the same inline content as one
   in a topic, so the key from stage 10 works here too.

2. **Edit `topics/about-this-guide.dita`**

   ```diff title="topics/about-this-guide.dita"
   --- a/topics/about-this-guide.dita
   +++ b/topics/about-this-guide.dita
   @@ -17,12 +17,12 @@
      </prolog>
      <body>
        <p>This guide is published under the Creative Commons Attribution 4.0 licence.
   -    Its topics are adapted from the Audacity Manual, copyright the Audacity Team and the Manual's authors, under the Creative Commons Attribution 3.0 licence.</p>
   +    Its topics are adapted from <cite>The Audacity Manual</cite>, copyright the <keyword keyref="product-name"/> Team and the Manual's authors, under the Creative Commons Attribution 3.0 licence.</p>
        <p>New to <keyword keyref="product-name"/>? Start with <xref keyref="start-here"/>.
        New to audio? Read <xref keyref="digital-audio"/> first.
        Here for podcasting? Go straight to <xref keyref="podcast-workflow"/>.
        Recording nothing but silence? See <xref keyref="silent"/>.</p>
        <p><keyword keyref="product-name"/> is a registered trademark of Dominic Mazzoni.
   -    The Audacity Team does not endorse this guide and is not affiliated with it.</p>
   +    The <keyword keyref="product-name"/> Team does not endorse this guide and is not affiliated with it.</p>
      </body>
    </topic>
   ```

   Two different fixes for two different uses of the word. *The Audacity
   Manual* is the title of a work, so it goes in `<cite>`, where the rule
   does not look, and it should not be keyed: if the product were renamed
   the manual's title would not change. *The Audacity Team* is the
   product's name used as a name, so it takes the key.

3. **Edit `topics/podcast-production-workflow.dita`**

   ```diff title="topics/podcast-production-workflow.dita"
   --- a/topics/podcast-production-workflow.dita
   +++ b/topics/podcast-production-workflow.dita
   @@ -45,13 +45,30 @@
          <title>Post-production</title>
          <p>Process the audio in this order (see <xref keyref="effects"/> for each effect's parameters):</p>
          <ol>
   -        <li><b>Noise reduction</b>: remove background noise using the noise profile from the silence.</li>
   -        <li><b>Editing</b>: trim mistakes, long pauses and false starts.</li>
   -        <li><b><term keyref="gl-normalization">Normalization</term></b>: bring the peak level to -1.0 dB.</li>
   -        <li><b><term keyref="gl-compression">Compression</term></b>: even out volume differences (threshold -18 dB, ratio 3:1).</li>
   -        <li><b>Export</b>: MP3 at 128 kbps mono for distribution, by hand or from a script (<xref keyref="scripting"/>).</li>
   +        <li>Noise reduction: remove background noise using the noise profile from the silence.</li>
   +        <li>Editing: trim mistakes, long pauses and false starts.</li>
   +        <li><term keyref="gl-normalization">Normalization</term>: bring the peak level to -1.0 dB.</li>
   +        <li><term keyref="gl-compression">Compression</term>: even out volume differences (threshold -18 dB, ratio 3:1).</li>
   +        <li>Export: MP3 at 128 kbps mono for distribution, by hand or from a script (<xref keyref="scripting"/>).</li>
          </ol>
   -      <required-cleanup remap="table">Target loudness per platform: Spotify -14 LUFS, Apple -16 LUFS, YouTube -14 LUFS. Turn into a table once the list is confirmed.</required-cleanup>
   +      <simpletable>
   +        <sthead>
   +          <stentry>Platform</stentry>
   +          <stentry>Target loudness</stentry>
   +        </sthead>
   +        <strow>
   +          <stentry>Spotify</stentry>
   +          <stentry>-14 LUFS</stentry>
   +        </strow>
   +        <strow>
   +          <stentry>Apple Podcasts</stentry>
   +          <stentry>-16 LUFS</stentry>
   +        </strow>
   +        <strow>
   +          <stentry>YouTube</stentry>
   +          <stentry>-14 LUFS</stentry>
   +        </strow>
   +      </simpletable>
        </section>
        <section audience="beginner">
          <title>Getting started with podcasting</title>
   ```

   Remove the five `<b>` wrappers used for decorative emphasis. Keep the
   `<term>` markup that identifies defined terms. The `<required-cleanup>`
   asked to become a table, so it becomes the `<simpletable>` from stage
   05, with a header row and one row per platform. Move the content into that table and remove the cleanup wrapper.

4. **Edit `topics/exporting-audio.dita`**

   ```diff title="topics/exporting-audio.dita"
   --- a/topics/exporting-audio.dita
   +++ b/topics/exporting-audio.dita
   @@ -27,7 +27,7 @@
          <change-item>
            <change-person>The Audacity tutorial team</change-person>
            <change-completed>2026-03-05</change-completed>
   -        <change-summary>Marked the metadata-tags step as new in 3.4.</change-summary>
   +        <change-summary>Marked the metadata-tags step as new in 3.4; confirmed it is still a separate dialog.</change-summary>
          </change-item>
        </change-historylist>
      </prolog>
   @@ -75,7 +75,6 @@
          </step>
        </steps>
        <result>
   -      <draft-comment author="reviewer" time="2026-03-05" disposition="open">Check the 3.4 export dialog: is the metadata tags step still a separate dialog?</draft-comment>
          <p>The exported file is in the folder you chose.
          The project itself is unchanged.</p>
        </result>
   ```

   The reviewer's question has been answered, so the comment goes and the
   answer is recorded where the next reviewer will look for it, in the
   `<change-summary>` of the `<change-historylist>` from stage 24.
::::

## Step 4: Tell the agents

::::steps
1. **Create `AGENTS.md`**

   ```md title="AGENTS.md"
   # Project context — Audacity User Guide (DITA)
   
   This is the documentation set for **Audacity**, authored in DITA 1.3 and built
   with DogsBay XML. It is also the finished state of a step-by-step DITA tutorial:
   every `tutorial/NN-*` branch adds one feature, and this file describes the
   conventions the finished project keeps.
   
   ## Layout
   
   - `audacity-guide.ditamap` — the main guide (start here). `beginner-guide`,
     `podcaster-guide`, `audacity-book` (PDF), `audacity-collection` (all three
     guides, key-scoped) and `installation-variants` (branch filtering) reuse the
     same topics.
   - `topics/` — concepts, tasks, references, a troubleshooting topic;
     `topics/glossary/` — glossary entries; `learning/` — a learning assessment;
     `shared/` — warehouses pulled in by conref; `samples/` — code pulled in by
     coderef; `images/` — illustrations.
   - `keydefs-product.ditamap`, `keydefs-glossary.ditamap` — key definitions.
   - `subject-scheme.ditamap` — the controlled values for `@platform`
     (`windows`, `mac`, `linux`) and `@audience` (`beginner`, `podcaster`).
   - `filters/` — DITAVAL files, one per deliverable plus `review.ditaval` (flags).
   - `project.json` — the DITA-OT project file: every deliverable this guide ships.
   - `.dogsbay/config.xml` — default root map and deliverable, the metadata
     policy, the house rules (`house-style.sch`) and the format style.
   - `scripts/check-stage.sh` — the gate: validation, project health,
     controlled values, house rules and a build of every deliverable.
   
   ## House style
   
   - **Never hardcode the product name, version, download URL or project
     extension.** Use the keys `product-name`, `product-version`, `download-url`,
     `project-extension`: `<keyword keyref="product-name"/>`. Literals inside
     `<filepath>`, `<codeph>`, `<codeblock>` and `<cite>` are fine.
   - **UI labels** use `<uicontrol>`, never `<b>`; menu paths use
     `<menucascade>`; a mnemonic is `<shortcut>` inside `<uicontrol>`.
   - **Reuse, don't repeat.** Shared steps, notes and hazards live in `shared/`
     and are pulled in with `conref` or `conkeyref`.
   - **Conditions** use only the values in `subject-scheme.ditamap`;
     `validate-conditions` fails on anything else.
   - **Every topic needs a `<shortdesc>`** right after the title, under about 155
     characters, leading with what the topic is about.
   - **Every topic needs keywords** in its prolog; tasks need a `<created>` date;
     an `<author>` is recommended. The metadata policy in `.dogsbay/config.xml`
     enforces this.
   - **Review markup does not ship.** Resolve `<draft-comment>` and
     `<required-cleanup>` before a release; the house rules report them.
   - **Titles** are sentence case. **Language** is `xml:lang="en-GB"` on every
     root element; product names and code carry `translate="no"`.
   - **Format** every file with `dogsbay-xml format -i` before committing. Do not
     put character entities such as `&amp;` in text; write "and".
   
   ## Checks
   
   Run `scripts/check-stage.sh` before every commit; it must print `STAGE OK`.
   For one dimension: `dogsbay-xml validate-project .`, `project-health .`,
   `validate-conditions -S subject-scheme.ditamap .`,
   `project-health --include=schematron .`, or `dita --project=project.json`.
   ```

2. **Create `.xagent/skills/audacity-house-style/SKILL.md`**

   ```md title=".xagent/skills/audacity-house-style/SKILL.md"
   ---
   name: audacity-house-style
   description: Apply the Audacity guide's house style to DITA topics — use product keys instead of hardcoded names, semantic UI elements instead of bold, conref shared content, and sentence-case titles. Use when asked to apply house style, clean up a topic, or make content consistent.
   ---
   
   # Audacity guide house style
   
   When asked to apply house style (or to "clean up" / "make consistent") to one or
   more DITA topics, apply these rules and re-validate afterwards:
   
   1. **Product references → keys.** Replace any literal "Audacity", the version
      number, the download URL, or the project extension (`.aup3`) with the matching
      key:
      - `Audacity` → `<keyword keyref="product-name"/>`
      - version (e.g. `3.4`) → `<keyword keyref="product-version"/>`
      - download URL → `<keyword keyref="download-url"/>`
      - `.aup3` → `<keyword keyref="project-extension"/>`
      Keys are defined in `keydefs-product.ditamap`. Use `list_keys` to confirm.
      Do **not** replace literals inside `<codeblock>` or `<filepath>` examples
      (e.g. a shell command or `C:\Program Files\Audacity`).
   
   2. **UI labels → `<uicontrol>`.** Replace `<b>Record</b>`-style highlighting of
      buttons, menu items, and field names with `<uicontrol>Record</uicontrol>`.
      Multi-level menu paths use
      `<menucascade><uicontrol>…</uicontrol>…</menucascade>`.
   
   3. **Deduplicate via conref.** If a step or note is copied verbatim from
      `shared/common-steps.dita` or `shared/common-notes.dita`, replace the copy with
      a `conref` to the shared element instead of repeating it.
   
   4. **Titles** are sentence case.
   
   5. After editing, **validate** each changed topic and fix any errors before
      reporting done.
   
   Make the minimal edits needed; preserve meaning and surrounding markup.
   ```

3. **Read them**

   - `AGENTS.md` is the file an AI coding agent reads before it touches a
     project; most agent tools look for it by that name. It is also the
     shortest description of the project for a person. The layout section
     says where things are; the house style section says the same rules
     as `house-style.sch`, plus the ones a rule cannot check, such as
     titles in sentence case and the formatter's `&amp;` limitation; the
     checks section says how to run the gate and each of its parts.
   - The skill is a recipe for one task. Its front matter `name` and
     `description` are what an agent matches a request against, so the
     description names the task in the words a person would use, *apply
     house style*, *clean up a topic*, *make consistent*. The body is the
     steps, in order, ending with *validate before reporting done*.
   - The same rules now exist three times, for three readers: the
     machine in `house-style.sch`, which fails the gate; the person or
     agent reading the project in `AGENTS.md`; the agent given a task in
     the skill. Keep them in step: a rule added to the Schematron is added
     to the prose the same day.
::::

## Step 5: README and the gate

::::steps
1. **Change the "You are on" line and the layout**

   ```diff title="README.md"
   --- a/README.md
   +++ b/README.md
   @@ -2,7 +2,7 @@
    
    Build a guide through 27 stages. Maps begin at stage 02; HTML publication begins at stage 03.
    
   -You are on stage 24: drafts and localization.
   +You are on stage 25: house rules.
    
    ## Stages
    
   ```

2. **Format and check**

   ```bash
   dogsbay-xml format -i topics/*.dita *.ditamap
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
   Root map: audacity-guide.ditamap (project config); house rules: house-style.sch (project config)
   Project is healthy: valid, no broken references, keys, orphans, or broken element ids.

   == house rules  /home/you/audacity-guide ==
   Root map: audacity-guide.ditamap (project config); house rules: house-style.sch (project config)
   Project is healthy: valid, no broken references, keys, orphans, or broken element ids.

   == validate-conditions  /home/you/audacity-guide  (scheme: subject-scheme.ditamap) ==
   42 file(s): 42 pass, 0 with violations.

   == build  project.json -> /tmp/check-stage-<pid> ==
   all deliverables built

   STAGE OK
   ```

   Two things have changed since stage 24. The health report's first
   line names the rules file, and a new step, *house rules*, runs
   `project-health --include=schematron` on its own and prints the same
   summary when the rules pass. The gate's health
   step has run with an explicit `--include` list since stage 23, to keep
   the `learning/` folder out of the editor's grammar pass, and
   `schematron` is not in that list because `--include` needs a rules
   file to exist; so the script looks for `<default-schematron>` in the
   config and, when it finds one, runs the rules as their own step. The
   step fails the gate on any violation, so the two stage 24 warnings are
   now something a branch cannot carry.
::::

Test an invalid value. Put one `<b>` back in
`topics/podcast-production-workflow.dita`, around *Noise reduction* in the
post-production list, and run the gate with `SKIP_BUILD=1`:

```
== house rules  /home/you/audacity-guide ==
Root map: audacity-guide.ditamap (project config); house rules: house-style.sch (project config)
House rules (1 of 42 files):
  /home/you/audacity-guide/topics/podcast-production-workflow.dita:48 — Use uicontrol for UI labels (and drop decorative bold); do not use b.

Summary
  House rules                   1  in 1 of 42 files
    Use uicontrol for UI labels (and drop decorative bold); do no…    1
FAIL: house-rule violations
```

The message is the one from the `ui-labels-use-uicontrol` pattern, with
the file and line, and the gate fails. Nothing else changed: the file is
valid DITA, the links resolve, the metadata is complete. The house rule is
the only thing that knows this project does not use `<b>`, which is what
the rule is for.

After each error exercise, undo the deliberate change and rerun the gate.
Confirm that it prints `STAGE OK` before continuing.

## What you learned

- Schematron: `<schema>`, `<pattern>`, `<rule context>`, `<assert test>`
  and `<report test>`, with the message as content.
- Testing `text()` so that literals inside `<filepath>` and `<cite>` are
  exempt; `test="true()"` to forbid an element.
- `<default-schematron>` in `.dogsbay/config.xml`;
  `project-health --include=schematron`; the gate's *house rules* step.
- Resolving review markup is content work: a `<required-cleanup>` becomes
  its table, an answered `<draft-comment>` becomes a `<change-summary>`.
- The same rules for three readers: the Schematron, `AGENTS.md` and a
  skill.

## Next lesson

Continue with [Stage 26: final](/part-5-governance/stage-26-final).

For the core course, use the [learning path](/start-here/learning-path) to skip optional modules.

[Compare this stage with its predecessor](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/24-drafts-and-localization...tutorial/25-house-rules).
