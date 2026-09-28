---
title: "Stage 01: A concept topic"
description: Write What is Audacity? as a DITA concept, learn the parts every topic has, and validate one file with the gate.
type: tutorial
---

# Stage 01: A concept topic

Write the guide's first topic, *What is Audacity?*, as a DITA `<concept>`.
A concept provides background information about a subject or explains how
something works.

Add a topic identifier, title, short description, and body. The tutorial
requires a short description for each concept, task, and reference topic;
the DTD makes some of these elements optional.

**Time:** about 15 minutes.
**You need:** stage 00 complete, with the gate passing.


Recorded diagnostic examples below come from earlier runs. File counts, paths, and stage numbers can differ. Run the gate for your current checkout.

## Step 1: Write the topic

::::steps
1. **Create `topics/what-is-audacity.dita` from a template**
   In the **Explorer**, right-click the `topics` folder and choose
   **New File**. Enter `what-is-audacity.dita` and press Enter. The
   **New XML Document** dialog lists the templates for XML files. Choose
   **dita-concept-minimal** and click **OK**.

   Use the topic `@id` as the file name to keep links readable. This is a
   project convention.

   The editor opens the new file with the template's content:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE concept PUBLIC "-//OASIS//DTD DITA Concept//EN" "concept.dtd">
   <concept id="concept_id">
       <title>Concept Title</title>
       <conbody>

       </conbody>
   </concept>
   ```

   The template supplies the XML declaration, the DOCTYPE, and the parts
   every concept needs. The editor recognizes the file as a DITA concept
   from its name, so validation and formatting work from the start.

   If you use another editor, create the file and type the finished topic
   shown in step 3.

2. **Replace the placeholders**
   Double-click `concept_id` and type `what-is-audacity`. Click inside the
   title, choose **XML** > **Select Element Content** (Ctrl+Shift+E), and
   type `What is Audacity?`.

3. **Complete the topic**
   Add the short description after the title, then the paragraphs, list,
   and section inside `<conbody>`. When you type the `>` of a start tag,
   the editor inserts the matching end tag after the cursor. Type the
   content, then move the cursor past the end tag to continue. Leave the
   indentation to the formatter in "Step 3: Format and run the gate".

   The finished topic:

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

4. **Read it element by element**

   - The XML declaration and the DOCTYPE come first. `-//OASIS//DTD DITA
     Concept//EN` is the public identifier the catalog resolves; `concept.dtd`
     is the system identifier, kept relative so the file does not depend on
     where DITA-OT is installed.
   - `<concept id="…">` is the topic. Every topic has an `@id`, and a link to
     a topic points at that id. Use lowercase words joined by hyphens.
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
   @@ -2,7 +2,7 @@
    
    Build a guide through 27 stages. Maps begin at stage 02; HTML publication begins at stage 03.
    
   -You are on stage 00: setup.
   +You are on stage 01: concept.
    
    ## Stages
    
   ````

2. **Remove `topics/.gitkeep`**
   The folder is no longer empty.
::::

## Step 3: Format and run the gate

::::steps
1. **Format the topic**
   Apply the project's format style before every check, so your diff stays
   about the feature. This is how every stage branch was written.

   In the editor, choose **XML** > **Format**, then save the file. To check
   the topic against its DTD, choose **XML** > **Validate**. The **Errors**
   panel reports `Valid Document`.

   From the command line, format every topic:

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
element whose content model was broken. For example:

```
24:15  error: The content of element type "section" does not match its content model.
```

In the editor, **XML** > **Validate** reports the same error in the
**Errors** panel. Undo the change before you go on.

After each error exercise, undo the deliberate change and rerun the gate.
Confirm that it prints `STAGE OK` before continuing.

## What you learned

- A concept is the information type for "what is it": `<concept>`,
  `<conbody>`, sections.
- Concepts, tasks, and references in this tutorial have an `@id`, a
  `<title>`, and a `<shortdesc>`. Stage 25 makes the short description a
  house rule.
- The DOCTYPE names a public identifier, and the catalog finds the DTD.
- `<section>` is one level deep; deeper structure means another topic.
- Format, then gate, every time.

## Next lesson

Continue with [Stage 02: first map](/part-1-topics/stage-02-first-map).

For the core course, use the [learning path](/start-here/learning-path) to skip optional modules.

[Compare this stage with its predecessor](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/00-setup...tutorial/01-concept).
