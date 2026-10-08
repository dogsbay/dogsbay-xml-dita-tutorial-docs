---
title: "Stage 23: Learning"
description: Create a learning assessment with four question types, and check it with the Learning and Training DTDs that the editor includes.
type: tutorial
---

# Stage 23: Learning

Create a `<learningAssessment>` with true-or-false, single-select,
multiple-select, and sequencing questions. Add objectives, a duration,
and feedback for each answer.

The assessment uses a DOCTYPE from the DITA Learning and Training
package. The editor includes those DTDs, so the assessment validates and
checks like any other topic.

**Optional module.** See [Choose a learning path](/start-here/learning-path)
for the starting checkpoint and the next core lesson.

**Time:** about 30 minutes.
**You need:** stage 22 complete.


Recorded output below is an example. File counts, paths, and stage numbers can differ. [Check your work](/start-here/run-the-gate) to see the result for your own project.

## Step 1: The assessment

::::steps
1. **Create `learning/check-your-understanding.dita`**
   A new folder, `learning/`, keeps the learning content apart from the
   topics. In the **Explorer**, right-click `my-audacity-guide`, choose
   **New Folder**, and enter `learning`. Then right-click `learning`,
   choose **New File**, and enter `check-your-understanding.dita`. There
   is no template for an assessment, so choose **Blank XML Document**.
   After the XML declaration, add the rest of this listing:

   ```xml title="learning/check-your-understanding.dita"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE learningAssessment PUBLIC "-//OASIS//DTD DITA Learning Assessment//EN" "learningAssessment.dtd">
   
   <learningAssessment id="check-your-understanding">
     <title>Check your understanding</title>
     <shortdesc>Four questions on recording, editing and exporting, with feedback on every answer.</shortdesc>
     <prolog>
       <author type="creator">The Audacity tutorial team</author>
       <metadata>
         <audience type="user" experiencelevel="novice"/>
         <category>learning</category>
         <keywords>
           <keyword>quiz</keyword>
           <keyword>assessment</keyword>
         </keywords>
       </metadata>
     </prolog>
     <learningAssessmentbody>
       <lcIntro>
         <p>Answer these after working through the recording and editing chapters.
         Each answer explains why it is right or wrong.</p>
       </lcIntro>
       <lcObjectives>
         <lcObjectivesStem>After this guide you can:</lcObjectivesStem>
         <lcObjectivesGroup>
           <lcObjective>Make a recording with sensible levels.</lcObjective>
           <lcObjective>Remove noise before shaping the sound.</lcObjective>
           <lcObjective>Choose an export format for the purpose.</lcObjective>
         </lcObjectivesGroup>
       </lcObjectives>
       <lcDuration>
         <lcTime value="PT5M">About 5 minutes</lcTime>
       </lcDuration>
       <lcInteraction>
         <lcTrueFalse2 id="q1">
           <lcQuestion2>Clipping can be repaired afterwards with the Normalize effect.</lcQuestion2>
           <lcAnswerOptionGroup2>
             <lcAnswerOption2>
               <lcAnswerContent2>True</lcAnswerContent2>
               <lcFeedback2>Normalize only changes the level; the flattened peaks stay flat.</lcFeedback2>
             </lcAnswerOption2>
             <lcAnswerOption2>
               <lcAnswerContent2>False</lcAnswerContent2>
               <lcCorrectResponse2/>
               <lcFeedback2>Correct. Clipped samples are lost at recording time, so leave headroom instead.</lcFeedback2>
             </lcAnswerOption2>
           </lcAnswerOptionGroup2>
         </lcTrueFalse2>
         <lcSingleSelect2 id="q2">
           <lcQuestion2>What is a good peak level to aim for while recording speech?</lcQuestion2>
           <lcAnswerOptionGroup2>
             <lcAnswerOption2>
               <lcAnswerContent2>0 dB, as loud as possible</lcAnswerContent2>
               <lcFeedback2>At 0 dB any louder syllable clips.</lcFeedback2>
             </lcAnswerOption2>
             <lcAnswerOption2>
               <lcAnswerContent2>About -12 dB</lcAnswerContent2>
               <lcCorrectResponse2/>
               <lcFeedback2>Correct. That leaves headroom for louder moments and for the effects chain.</lcFeedback2>
             </lcAnswerOption2>
             <lcAnswerOption2>
               <lcAnswerContent2>About -40 dB</lcAnswerContent2>
               <lcFeedback2>Too quiet: raising it later also raises the noise floor.</lcFeedback2>
             </lcAnswerOption2>
           </lcAnswerOptionGroup2>
         </lcSingleSelect2>
         <lcMultipleSelect2 id="q3">
           <lcQuestion2>Which formats keep every sample of the recording? Choose all that apply.</lcQuestion2>
           <lcAnswerOptionGroup2>
             <lcAnswerOption2>
               <lcAnswerContent2>WAV</lcAnswerContent2>
               <lcCorrectResponse2/>
             </lcAnswerOption2>
             <lcAnswerOption2>
               <lcAnswerContent2>MP3</lcAnswerContent2>
               <lcFeedback2>MP3 is lossy: it discards detail to shrink the file.</lcFeedback2>
             </lcAnswerOption2>
             <lcAnswerOption2>
               <lcAnswerContent2>FLAC</lcAnswerContent2>
               <lcCorrectResponse2/>
             </lcAnswerOption2>
             <lcAnswerOption2>
               <lcAnswerContent2>OGG Vorbis</lcAnswerContent2>
               <lcFeedback2>OGG Vorbis is lossy, like MP3.</lcFeedback2>
             </lcAnswerOption2>
           </lcAnswerOptionGroup2>
         </lcMultipleSelect2>
         <lcSequencing2 id="q4">
           <lcQuestion2>Put the post-production steps for a podcast in order.</lcQuestion2>
           <lcSequenceOptionGroup2>
             <lcSequenceOption2>
               <lcAnswerContent2>Noise reduction</lcAnswerContent2>
               <lcSequence2 value="1"/>
             </lcSequenceOption2>
             <lcSequenceOption2>
               <lcAnswerContent2>Compression</lcAnswerContent2>
               <lcSequence2 value="2"/>
             </lcSequenceOption2>
             <lcSequenceOption2>
               <lcAnswerContent2>Normalization</lcAnswerContent2>
               <lcSequence2 value="3"/>
             </lcSequenceOption2>
             <lcSequenceOption2>
               <lcAnswerContent2>Export</lcAnswerContent2>
               <lcSequence2 value="4"/>
             </lcSequenceOption2>
           </lcSequenceOptionGroup2>
         </lcSequencing2>
       </lcInteraction>
       <lcSummary>
         <p>Leave headroom, clean before you shape, and keep a lossless master.</p>
       </lcSummary>
     </learningAssessmentbody>
   </learningAssessment>
   ```

2. **Read the frame**

   - The DOCTYPE is `-//OASIS//DTD DITA Learning Assessment//EN` with
     `learningAssessment.dtd`; the body is `<learningAssessmentbody>`.
     The `<prolog>` is the usual one, so the metadata policy applies.
   - `<lcIntro>` opens the assessment. `<lcObjectives>` lists what the
     reader should now be able to do: an `<lcObjectivesStem>` and an
     `<lcObjectivesGroup>` of `<lcObjective>`s.
   - `<lcDuration>` holds an `<lcTime>` whose `value` is an ISO 8601
     duration, `PT5M`, five minutes, with the readable text as content.
     `<lcDuration>` allows only a `<title>` and an `<lcTime>`, and `value`
     is required; the demo at the end writes a paragraph there instead.
   - `<lcSummary>` closes it.

3. **Read the interactions**
   `<lcInteraction>` holds the questions. DITA 1.3 has two generations of
   interaction elements; the ones ending in `2` are the DITA 1.3
   versions, with a content model built on `<div>` rather than `<fig>`,
   and they are the ones to use in new content.
   - `<lcTrueFalse2>`, `<lcSingleSelect2>` and `<lcMultipleSelect2>`
     share a shape: an `<lcQuestion2>`, then an `<lcAnswerOptionGroup2>`
     of `<lcAnswerOption2>`s, each with `<lcAnswerContent2>`, an empty
     `<lcCorrectResponse2/>` on the right ones, and `<lcFeedback2>`. A
     single-select has one correct option, a multiple-select as many as
     apply, a true/false exactly two options.
   - `<lcSequencing2>` asks for an order: an `<lcSequenceOptionGroup2>` of
     `<lcSequenceOption2>`s, each with its `<lcAnswerContent2>` and an
     `<lcSequence2 value="n"/>` giving its place in the right order.
   - Every interaction has an `id`, so a learning map or an LMS export
     can address it. The HTML5 transform renders each question as a list
     of its options with their feedback. It drops `<lcCorrectResponse2/>`
     and `<lcSequence2>` from the page, but the page still shows the
     answers to a careful reader. The feedback on each correct option
     starts with "Correct.", only the wrong options in the multiple-select
     question have feedback, and the sequencing options are listed in the
     answer order. For a real quiz, publish the interactions to a learning
     platform, or write the feedback and the option order so that they do
     not reveal the answer.
   - The HTML5 output renders `<lcDuration>` as an empty section. The
     `<lcTime>` value is not shown.
::::

## Step 2: The maps

::::steps
1. **Edit `audacity-guide.ditamap`**

   ```diff title="audacity-guide.ditamap"
   --- a/audacity-guide.ditamap
   +++ b/audacity-guide.ditamap
   @@ -53,6 +53,10 @@
        </topicset>
      </topichead>
    
   +  <topichead navtitle="Check your understanding">
   +    <topicref href="learning/check-your-understanding.dita"/>
   +  </topichead>
   +
      <topichead navtitle="Troubleshooting" id="troubleshooting">
        <topicref href="topics/recording-is-silent.dita" keys="silent"/>
      </topichead>
   ```

2. **Edit `beginner-guide.ditamap`**

   ```diff title="beginner-guide.ditamap"
   --- a/beginner-guide.ditamap
   +++ b/beginner-guide.ditamap
   @@ -22,6 +22,7 @@
        <topicref href="topics/trimming-audio.dita"/>
        <topicref href="topics/exporting-audio.dita"/>
      </topichead>
   +  <topicref href="learning/check-your-understanding.dita"/>
      <topichead navtitle="Troubleshooting">
        <topicref href="topics/recording-is-silent.dita" keys="silent"/>
      </topichead>
   ```

   An assessment is a topic like any other to a map. In the full guide it
   is a chapter of its own, *Check your understanding*, after the chapters
   it tests and before *Troubleshooting*. In the beginner guide it is a
   plain `<topicref>` before *Troubleshooting*. A map publishes only the
   topics it references. The assessment is for novices, as its prolog
   says, so the podcaster guide and the book do not reference it.
::::

## Step 3: Validate the assessment

::::steps
1. **Validate the file**
   In the assessment, choose **XML** > **Validate**. The **Errors** panel
   reports **Valid Document**. To validate the one file from the command
   line, run:

   ```bash
   dogsbay-xml validate learning/check-your-understanding.dita
   ```

   ```
   VALID
   ```

   The DOCTYPE's public identifier,
   `-//OASIS//DTD DITA Learning Assessment//EN`, resolves through the
   editor's built-in catalog, which includes the DITA 1.3 Learning and
   Training DTDs. You do not need a catalog option or a separate DITA-OT
   installation.
::::

## Step 4: Check your work

::::steps
1. **Format and check your work**

   Save all the files. Format each file that you changed: in the editor,
   choose **XML** > **Format** and save the file, or choose **Project** >
   **Project Tools** > **Format Project** to format every file at once.
   From the command line, format them all:

   ```bash
   dogsbay-xml format -i learning/*.dita *.ditamap
   ```

   Then build. In the editor, choose **Project** > **Build Deliverables...**
   and click **Build All** to build every deliverable. To check the
   project and the built output as well, choose **Project** >
   **Check Project** instead and read the result in the
   **Project Validation** panel. From the command line, run:

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
   Ready: the project is healthy, every deliverable built, and the output of full, beginner-mac, beginner-windows, podcaster-linux, review, install-variants, collection holds together.
   ```

   Each HTML deliverable whose map includes the assessment builds it as a
   page, for example `out/full/learning/check-your-understanding.html`.

2. **Take the assessment**
   You added the assessment to two maps. To see how many deliverables
   publish it, list the `learning` folders in the output in the
   **Terminal** panel:

   ```bash
   ls -1d out/*/learning
   ```

   There are five, from two maps: `full` and `review` both build the full
   guide, `beginner-mac` and `beginner-windows` both build the beginner
   guide, and `collection` pulls in both maps. The podcaster guide, the
   installation variants, and the book have no `learning` folder.

   Open `out/beginner-mac/learning/check-your-understanding.html` and
   choose **View** > **Preview in Tab**. The introduction and the
   objectives come first, then the questions. Each option is listed with
   its feedback, if it has any, so the page shows all the feedback at
   once and gives the answers away, as described in step 1. The duration
   section is empty.
::::

Test the duration content model. Replace the `<lcTime>` line with
`<p>About 5 minutes</p>` so that `<lcDuration>` holds a paragraph, save,
and choose **XML** > **Validate**. The **Errors** panel reports that the
content of `lcDuration` must match `(title?,lcTime?)`. Project health
fails the same way, so **Project** > **Check Project** stops at health and
nothing is built. Example output:

```
health   NOT CLEAN
  invalid: /home/you/my-audacity-guide/learning/check-your-understanding.dita
    33:18  The content of element type "lcDuration" does not match its content model.
  (run project-health for the full report)
Not ready: the project itself has faults. The build and the built output were not checked.
```

`dogsbay-xml validate learning/check-your-understanding.dita` gives the
full message:

```
INVALID
  33:18 [error] The content of element type "lcDuration" must match "(title?,lcTime?)".
```

`<lcDuration>` takes an optional title and an `<lcTime>`, and `<lcTime>`
takes a `value`: the `value` supplies a machine-readable duration for a
learning platform.

Undo the change, save, and validate again. Confirm that the **Errors**
panel reports **Valid Document** before you continue.

## What you learned

- `<learningAssessment>`, `<learningAssessmentbody>`, `<lcIntro>`,
  `<lcObjectives>` with `<lcObjectivesStem>`, `<lcObjectivesGroup>` and
  `<lcObjective>`, `<lcDuration>` with `<lcTime value>`, `<lcSummary>`.
- `<lcInteraction>` with the DITA 1.3 interactions `<lcTrueFalse2>`,
  `<lcSingleSelect2>`, `<lcMultipleSelect2>` and `<lcSequencing2>`, and
  their `<lcQuestion2>`, `<lcAnswerOptionGroup2>`, `<lcAnswerOption2>`,
  `<lcAnswerContent2>`, `<lcCorrectResponse2>`, `<lcFeedback2>`,
  `<lcSequenceOptionGroup2>`, `<lcSequenceOption2>`, `<lcSequence2>`.
- The Learning and Training DTDs are in the editor's catalog, so
  `learning/` validates and checks like the rest of the project.
- End of Part 4: a book, a troubleshooting topic, hazard statements, the
  software domains and an assessment, on the same guide.

## Next lesson

**Checkpoint:** `tutorial/23-learning`. If you use Git, commit your work.

Continue with [Stage 24: drafts and localization](/part-5-governance/stage-24-drafts-and-localization).

For the core course, use the [learning path](/start-here/learning-path) to skip optional modules.

[Compare this stage with its predecessor](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/22-software-domains...tutorial/23-learning).
