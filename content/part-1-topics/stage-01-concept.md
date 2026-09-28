---
title: "Stage 01: A concept topic"
description: Write What is Audacity? as a DITA concept, learn the parts every topic has, and check your work.
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
**You need:** stage 00 complete, with the check reporting `health   clean`.


Recorded output below is an example. File counts, paths, and stage numbers can differ. [Check your work](/start-here/run-the-gate) to see the result for your own project.

## Step 1: Write the topic

::::steps
1. **Create `topics/what-is-audacity.dita` from a template**
   In the **Explorer**, right-click the `topics` folder and choose
   **New File**. Enter `what-is-audacity.dita` and press Enter. The
   **New XML Document** dialog lists the templates for `.dita` files.
   Choose **Concept** and click **OK**.

   Use the topic `@id` as the file name to keep links readable. This is a
   project convention. The editor sets the root `@id` from the file name,
   so the id needs no editing.

   The editor opens the new file with the template's content:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE concept PUBLIC "-//OASIS//DTD DITA Concept//EN" "concept.dtd">

   <concept id="what-is-audacity">
     <title>Concept Title</title>
     <shortdesc></shortdesc>
     <conbody>
       <p></p>
     </conbody>
   </concept>
   ```

   The template supplies the XML declaration, the DOCTYPE, and the parts
   every concept needs. The editor recognizes the file as a DITA concept
   from its name, so validation and formatting work from the start.

   If you use another editor, create the file and type the finished topic
   shown in step 3.

2. **Replace the title and add the short description**
   Click inside the title, choose **XML** > **Select Element Content**
   (Ctrl+Shift+E), and type `What is Audacity?`. Then click between
   `<shortdesc>` and `</shortdesc>` and type the short description.

3. **Complete the topic**
   Type the first paragraph in the empty `<p></p>`, then add the other
   paragraph, the list, and the section inside `<conbody>`. When you type
   the `>` of a start tag, the editor inserts the matching end tag after
   the cursor. Type the content, then move the cursor past the end tag to
   continue. Leave the indentation to the formatter in "Step 3: Format and
   check your work".

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

## Step 3: Format and check your work

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

2. **Check your work**
   In the editor, choose **Project** > **Check Project**. The result appears
   in the **Project Validation** panel. From the command line, run:

   ```bash
   dogsbay-xml check .
   ```

   The output looks like this example:

   ```
   health   clean, with warnings
     orphan topic: /home/you/audacity-guide/topics/what-is-audacity.dita — nothing refers to it, so it will not appear in the output
   Project health is clean — no deliverables yet, so nothing here speaks for the output. 1 orphan topic above: worth knowing, and not treated as failures.
   ```

   The orphan warning is expected. No map refers to the topic yet, so it
   would not appear in any output. Stage 02 adds the map. A warning does not
   make the check fail.

   The project declares no deliverables until stage 03, so the check
   reports on the source only. Look for `health   clean`.
::::

To see a validation error, put a `<section>` inside another `<section>` and
check again. For example, add this inside the *Who uses it* section, after
its `<p>`:

```xml
<section>
  <title>Nested</title>
  <p>Inner.</p>
</section>
```

The check names the file, with the line, column, and message of the first
errors, and stops at the health stage. The output looks like this example:

```
health   NOT CLEAN
  invalid: /home/you/audacity-guide/topics/what-is-audacity.dita
    27:15  The content of element type "section" does not match its content model.
  (run project-health for the full report)
  orphan topic: /home/you/audacity-guide/topics/what-is-audacity.dita — nothing refers to it, so it will not appear in the output
Not ready: the project itself has faults. The build and the built output were not checked.
```

The error is reported on the closing tag of the outer `<section>`, where the
parser finds that its content does not fit. For the full report, open the
**Project Validation** panel in the editor, or run
`dogsbay-xml project-health .`. For example:

```
Root map: none (none configured); house rules: none (none configured)
Invalid files (1 of 1):
  /home/you/audacity-guide/topics/what-is-audacity.dita:
    27:15  error: The content of element type "section" does not match its content model.
Orphan topics (1):
  /home/you/audacity-guide/topics/what-is-audacity.dita

Summary
  Invalid files                 1  of 1
  Orphan topics                 1
```

In the editor, **XML** > **Validate** also reports the error in the
**Errors** panel.

Undo the change and check again. Confirm that the check reports
`health   clean` before you continue.

## What you learned

- A concept is the information type for "what is it": `<concept>`,
  `<conbody>`, sections.
- Concepts, tasks, and references in this tutorial have an `@id`, a
  `<title>`, and a `<shortdesc>`. Stage 25 makes the short description a
  house rule.
- The DOCTYPE names a public identifier, and the catalog finds the DTD.
- `<section>` is one level deep; deeper structure means another topic.
- Format, then check your work, every time.

## Next lesson

Continue with [Stage 02: first map](/part-1-topics/stage-02-first-map).

For the core course, use the [learning path](/start-here/learning-path) to skip optional modules.

[Compare this stage with its predecessor](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/00-setup...tutorial/01-concept).
