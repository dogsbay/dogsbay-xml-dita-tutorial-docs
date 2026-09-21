---
title: "Stage 01: A concept topic"
description: Write What is Audacity? as a DITA concept, learn the parts every topic has, and validate one file with the gate.
type: tutorial
---

# Stage 01: A concept topic

In this stage you write the first topic of the guide, *What is Audacity?*, as a
DITA `<concept>`. A concept explains what something is or how it works; it is
the information type for background, and it is the right home for a product
introduction.

You also meet the parts that every DITA topic has, whatever its type: an `@id`,
a `<title>`, a `<shortdesc>` and a body. Everything you learn here carries
over to tasks and references in the next stage.

**Time:** about 15 minutes.
**You need:** stage 00 complete, with the gate passing.

## Step 1: Write the topic

::::steps
1. **Create `topics/what-is-audacity.dita`**
   The file name matches the topic `@id`. That is a convention, not a rule,
   but it keeps links readable when the map arrives.

   ```xml title="topics/what-is-audacity.dita"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE concept PUBLIC "-//OASIS//DTD DITA Concept//EN" "concept.dtd">

   <concept id="what-is-audacity">
     <title>What is Audacity?</title>
     <shortdesc>Audacity is a free, open-source audio editor and recorder for Windows, macOS and Linux.</shortdesc>
     <conbody>
       <p>Audacity records live audio, imports and exports the common audio formats, and edits sound with cut, copy, paste and a library of effects.
       It is free to download and use, and it runs on Windows, macOS and Linux.</p>
       <p>With Audacity you can:</p>
       <ul>
         <li>Record live audio through a microphone or mixer.</li>
         <li>Import and export audio in many formats, including WAV, MP3, OGG and FLAC.</li>
         <li>Edit audio with cut, copy, paste and delete.</li>
         <li>Apply effects such as noise reduction, equalization and compression.</li>
       </ul>
       <section>
         <title>Who uses it</title>
         <p>Podcasters record and clean up their episodes.
         Archivists digitize vinyl records and cassettes.
         Video makers edit sound effects and voice-overs.
         Whatever the project, Audacity provides the tools at no cost.</p>
       </section>
     </conbody>
   </concept>
   ```

2. **Read it element by element**
   - The XML declaration and the DOCTYPE come first. `-//OASIS//DTD DITA
     Concept//EN` is the public identifier the catalog resolves; `concept.dtd`
     is the system identifier, kept relative so the file does not depend on
     where DITA-OT is installed.
   - `<concept id="…">` is the topic. Every topic has an `@id`, and a link to
     a topic points at that id. Use lower-case words joined by hyphens.
   - `<title>` is required and is the first child. It becomes the page
     heading, the entry in the table of contents and, by default, the link
     text of any link to this topic.
   - `<shortdesc>` is one sentence that says what the topic is about. It is
     shown under the title, under links to the topic, and in search results.
     Write it for every topic; later stages make it a house rule.
   - `<conbody>` is the body of a concept. Each information type has its own
     body element with its own content rules; a concept body is free-form
     text, lists and sections.
   - `<p>` is a paragraph. `<ul>` with `<li>` is an unordered list.
   - `<section>` groups part of the body under its own `<title>`. Sections do
     not nest, and a section cannot contain another section; when you want a
     second level, you want a second topic.
::::

## Step 2: Update the README

::::steps
1. **Change the "You are on" line**
   Every stage moves it. The rest of the README is unchanged.

   ````diff title="README.md"
   --- a/README.md
   +++ b/README.md
   @@ -5,8 +5,7 @@ branch is the previous branch plus one lesson, from a single concept topic to a
    complete, publish-ready **Audacity User Guide**. The diff between two branches
    is the lesson.
    
   -You are on **stage 00 — setup**: an empty project the tools recognise, and the
   -gate every later stage must pass.
   +You are on **stage 01 — concept**: one concept topic, and the gate proves it validates.
    
    ## Stages
    
   @@ -39,7 +38,7 @@ override the defaults). A stage is done when it prints `STAGE OK`.
    ```
    .dogsbay/config.xml   shared editor project settings (project type, framework, format style)
    scripts/              the gate
   -topics/               topics (empty at this stage)
   +topics/               topics
    ```
    
    ## Licence and attribution
   ````

2. **Remove `topics/.gitkeep`**
   The folder is no longer empty.
::::

## Step 3: Format and run the gate

::::steps
1. **Format the topic**
   Apply the project's format style before every check, so your diff stays
   about the feature. This is how every stage branch was written.

   ```bash
   dogsbay-xml format -i topics/*.dita
   ```

2. **Run the gate**

   ```bash
   scripts/check-stage.sh
   ```

   ```
   == validate-project  /home/you/audacity-guide ==
   1 file(s): 1 valid, 0 invalid.

   == project-health  /home/you/audacity-guide ==
   Root map: none (none configured); house rules: none (none configured)
   Project is healthy: valid, no broken references, keys, orphans, or broken element ids.

   == build  skipped (no project.json yet) ==

   STAGE OK
   ```
::::

To see a validation error, put a `<section>` inside another `<section>` and
run the gate again. `validate-project` names the file, the line and the
element whose content model was broken. Undo the change before you go on.

## What you learned

- A concept is the information type for "what is it": `<concept>`,
  `<conbody>`, sections.
- Every topic has an `@id`, a `<title>` and a `<shortdesc>`; the short
  description is not optional in practice.
- The DOCTYPE names a public identifier, and the catalog finds the DTD.
- `<section>` is one level deep; deeper structure means another topic.
- Format, then gate, every time.

## Where to go next

:::cards
- **[Stage 02: A task and a reference](/part-1-topics/stage-02-task-and-reference)** {icon="arrow-right"}
  The other two information types, side by side with the concept.

- **[Compare 00 to 01 on GitHub](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/00-setup...tutorial/01-concept)** {icon="github"}
  Exactly what this stage added.
:::
