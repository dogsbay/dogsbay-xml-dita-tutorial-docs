---
title: "Stage 25: House rules"
description: Write the house style as Schematron, select it in the project settings so that the check enforces it, and fix the ten violations it finds.
type: tutorial
---

# Stage 25: House rules

Add Schematron rules for the project's house style: require short
descriptions, use semantic markup for UI labels, reference product names
through keys, and resolve review markup before release.

Select `house-style.sch` as the project's house rules. Resolve the ten
reported violations, including converting the cleanup content to a table
and recording the review decision in the change history. If you use an AI
agent, add the files that describe the rules to it.

**Time:** about 40 minutes.
**You need:** stage 24 complete.


Recorded output below is an example. File counts, paths, and stage numbers can differ. [Check your work](/start-here/run-the-gate) to see the result for your own project.

## Step 1: The rules

::::steps
1. **Create `house-style.sch`**
   In the **Explorer**, right-click `my-audacity-guide` and choose
   **New File**. Enter `house-style.sch` and press Enter. In the
   **New XML Document** dialog, click **Schematron Rules**. Replace the
   template's content with this listing and save the
   file:

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
         <assert test="@type = 'note' or @type = 'tip' or @type = 'important' or @type = 'notice' or @type = 'caution' or @type = 'warning' or @type = 'attention' or @type = 'danger' or @type = 'fastpath' or @type = 'remember' or @type = 'restriction' or @type = 'trouble' or @type = 'other'">note type is not an allowed DITA value.</assert>
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
     placeholder pattern from stage 11. `note-type-allowed` lists every
     `type` value that the DITA 1.3 DTD accepts, `notice` included.
     `note-type-allowed` and `step-has-cmd` restate DTD rules so that
     their messages are the house's.
   - The comment at the top says how to run the rules and why they are
     shaped as they are. A rules file is read by the next writer more
     often than it is edited; say what it is for.
::::

## Step 2: Run them on stage 24

::::steps
1. **Select the house rules**
   Choose **Project** > **Manage Projects...** and click your project. In
   the **House Rules (Schematron)** list, which shows `(none)`, choose
   `house-style.sch`. Click **Save**.

   A rules file does nothing until the project names it. From now on
   project health, the first step of **Check Project**, runs the rules
   with everything else, the editor applies the same rules, and the
   `house rules:` part of the health report's first line names
   `house-style.sch` instead of saying *none*.

2. **Run the rules before you change anything else**
   With the rules file selected, and the files still
   as they were at the end of stage 24, run only the rules, without
   building: choose **Project** > **Validate Files** > **With Schematron**.
   The result appears in the **Project Validation** panel, each violation
   with its file, its line, and the message from its rule. From the
   command line, run this command and wait for it to finish:

   ```bash
   dogsbay-xml project-health --include=schematron .
   ```

   ```
   Root map: audacity-guide.ditamap (project config); house rules: house-style.sch (project config)
   House rules (4 of 46 files):
     /home/you/my-audacity-guide/audacity-guide.ditamap:7 — Do not hardcode the product name "Audacity" in a shortdesc; use keyref="product-name".
     /home/you/my-audacity-guide/topics/about-this-guide.dita:19 — Do not hardcode the product name "Audacity" in prose; use a keyword with keyref="product-name".
     /home/you/my-audacity-guide/topics/about-this-guide.dita:25 — Do not hardcode the product name "Audacity" in prose; use a keyword with keyref="product-name".
     /home/you/my-audacity-guide/topics/exporting-audio.dita:78 — Remove draft-comment before publishing.
     /home/you/my-audacity-guide/topics/podcast-production-workflow.dita:48 — Use uicontrol for UI labels (and drop decorative bold); do not use b.
     /home/you/my-audacity-guide/topics/podcast-production-workflow.dita:49 — Use uicontrol for UI labels (and drop decorative bold); do not use b.
     /home/you/my-audacity-guide/topics/podcast-production-workflow.dita:50 — Use uicontrol for UI labels (and drop decorative bold); do not use b.
     /home/you/my-audacity-guide/topics/podcast-production-workflow.dita:51 — Use uicontrol for UI labels (and drop decorative bold); do not use b.
     /home/you/my-audacity-guide/topics/podcast-production-workflow.dita:52 — Use uicontrol for UI labels (and drop decorative bold); do not use b.
     /home/you/my-audacity-guide/topics/podcast-production-workflow.dita:54 — Remove required-cleanup before publishing.

   Summary
     House rules                  10  in 4 of 46 files
       Use uicontrol for UI labels (and drop decorative bold); do no…    5
       Do not hardcode the product name "Audacity" in prose; use a k…    2
       Do not hardcode the product name "Audacity" in a shortdesc; u…    1
       Remove draft-comment before publishing.                           1
       Remove required-cleanup before publishing.                        1
   ```

   Ten violations in four files, every one a house-style decision that
   nothing had checked until now. The main map's `<shortdesc>`
   spells out the product name. *About this guide* names it twice in
   prose, once as part of a title and once as part of an organization.
   The podcast workflow has five decorative `<b>`s and the
   `<required-cleanup>` from stage 24; *Exporting audio* has the
   `<draft-comment>`. **Check Project** would stop at health on these and
   build nothing. **Validate Files** > **With Schematron**, like
   `--include=schematron`, runs only the rules, which is the fast loop
   while you fix them.
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

## Step 4: Optional: tell your AI agent

If you use an AI agent to edit the guide, copy two files from the
checkpoint `tutorial/25-house-rules` of the
[sample project](/start-here/set-up#the-sample-project) into the same paths
in `my-audacity-guide`:

- `AGENTS.md`, in the project folder. An AI coding agent reads this file
  before it changes a project; most agent tools look for it by that name.
  It describes the layout of the project, states the house style in
  prose, including the rules that Schematron cannot check, such as titles
  in sentence case, and names the checks to run.
- `.xagent/skills/audacity-house-style/SKILL.md`, a skill: a recipe for one
  task, *apply house style*. The `name` and `description` in its front
  matter are what an agent matches a request against. The body lists the
  steps and ends with *validate before reporting done*.

Some statements in `AGENTS.md` are for the maintainers of the sample
project, for example the `scripts/check-stage.sh` script. In your own
project, check your work with **Check Project** or `dogsbay-xml check .`.

With these files, the same rules exist for three readers: the machine in
`house-style.sch`, which fails the check; the person or agent reading the
project in `AGENTS.md`; and the agent given a task in the skill. Keep them
in step: when you add a rule to the Schematron, add it to the prose the
same day. If you do not use an AI agent, skip this step. The check does not
read these files.

## Step 5: Check your work

::::steps
1. **Format and check your work**

   Save all the files. Format each file that you changed: in the editor,
   choose **XML** > **Format** and save the file, or choose **Project** >
   **Project Tools** > **Format Project** to format every file at once.
   From the command line, format them all:

   ```bash
   dogsbay-xml format -i topics/*.dita *.ditamap
   ```

   Run the rules again with **Project** > **Validate Files** >
   **With Schematron**. Every file passes. This runs only the house
   rules; the full check also validates, builds, and checks the output.

   Then build. Choose **Project** > **Build Deliverables...** and click
   **Build All**. Every deliverable builds. Before a release, choose
   **Project** > **Check Project**, which runs the house rules first, as
   part of health, and then builds and checks the output, and read the
   result in the **Project Validation** panel. From the command line,
   run:

   ```bash
   dogsbay-xml check .
   ```

   Example output:

   ```
   health   clean
   build    full                 ok  /home/you/my-audacity-guide/out/full
     5 note(s) — run with --verbose to see them
   build    beginner-mac         ok  /home/you/my-audacity-guide/out/beginner-mac
   build    beginner-windows     ok  /home/you/my-audacity-guide/out/beginner-windows
   build    podcaster-linux      ok  /home/you/my-audacity-guide/out/podcaster-linux
     1 note(s) — run with --verbose to see them
   build    review               ok  /home/you/my-audacity-guide/out/review
     4 note(s) — run with --verbose to see them
   build    install-variants     ok  /home/you/my-audacity-guide/out/install-variants
     6 note(s) — run with --verbose to see them
   build    collection           ok  /home/you/my-audacity-guide/out/collection
     5 note(s) — run with --verbose to see them
   build    book-pdf             ok  /home/you/my-audacity-guide/out/book-pdf
     WARN  /home/you/my-audacity-guide/audacity-book.ditamap  PDF rendering reported 19 warnings (12 The following feature isn't implemented by Apache FOP, yet: table-layout=… (on fo:table) (…, 2 The contents of fo:inline line n exceed the available area in the inline-progression direc…, 2 The contents of fo:block line n exceed the available area in the inline-progression direct…, and 3 other kinds)
     5 note(s) — run with --verbose to see them
   output   wrote a file, no pages to check links in book-pdf
   output   clean (full, beginner-mac, beginner-windows, podcaster-linux, review, install-variants, collection)
   Ready: the project is healthy, every deliverable built, and every link in the pages of 7 deliverables leads somewhere.
   ```

   Health now runs the house rules with everything else, because the
   project names them. Step 3 resolved the ten violations,
   including the two stage 24 warnings, so health is clean. From now on a
   house-rule violation stops the check at health, so a release cannot
   carry unresolved review markup.

2. **Read the built page**
   Open `out/full/topics/podcast-production-workflow.html`, choose
   **View** > **Preview in Tab**, and scroll down to *Post-production*.
   The steps are plain text, with the terms still linked, and the
   loudness targets are a table. As a required cleanup, they never
   reached a reader.
::::

Test the bold rule. Put one `<b>` back in
`topics/podcast-production-workflow.dita`, around *Noise reduction* in the
post-production list, save, and choose **Project** > **Validate Files** >
**With Schematron**. One file fails, with the bold rule's message.
**Check Project** runs the same rules in health and stops there. Example
output of the check:

```
health   NOT CLEAN
  /home/you/my-audacity-guide/topics/podcast-production-workflow.dita:48 — Use uicontrol for UI labels (and drop decorative bold); do not use b.
  (run project-health for the full report)
Not ready: the project itself has faults. The build and the built output were not checked.
```

The **Project Validation** panel, or
`dogsbay-xml project-health --include=schematron .`, gives the report for
the rules:

```
Root map: audacity-guide.ditamap (project config); house rules: house-style.sch (project config)
House rules (1 of 46 files):
  /home/you/my-audacity-guide/topics/podcast-production-workflow.dita:48 — Use uicontrol for UI labels (and drop decorative bold); do not use b.

Summary
  House rules                   1  in 1 of 46 files
    Use uicontrol for UI labels (and drop decorative bold); do no…    1
```

The message is the one from the `ui-labels-use-uicontrol` pattern, with
the file and line, and the check stops at health. Nothing else changed: the file is
valid DITA, the links resolve, the metadata is complete. The house rule is
the only thing that knows this project does not use `<b>`, which is what
the rule is for.

Replace the `<b>` element with its words, save, and run
**Validate Files** > **With Schematron** again. Confirm that every file
passes before you continue.

## What you learned

- Schematron: `<schema>`, `<pattern>`, `<rule context>`, `<assert test>`
  and `<report test>`, with the message as content.
- Testing `text()` so that literals inside `<filepath>` and `<cite>` are
  exempt; `test="true()"` to forbid an element.
- **House Rules (Schematron)** in **Manage Projects**;
  **Validate Files** > **With Schematron** and
  `project-health --include=schematron` to run only the rules; a
  house-rule violation stops the check at health.
- Resolving review markup is content work: a `<required-cleanup>` becomes
  its table, an answered `<draft-comment>` becomes a `<change-summary>`.
- Optionally, the same rules for three readers: the Schematron,
  `AGENTS.md` and a skill.

## Next lesson

**Checkpoint:** `tutorial/25-house-rules`. If you use Git, commit your work.

Continue with [Stage 26: final](/part-5-governance/stage-26-final).

For the core course, use the [learning path](/start-here/learning-path) to skip optional modules.

[Compare this stage with its predecessor](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/24-drafts-and-localization...tutorial/25-house-rules).
