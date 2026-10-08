---
title: "Stage 19: Bookmap"
description: Publish the guide as a PDF book with a bookmap, book metadata, front matter, parts, chapters, appendixes, a glossary list and an index.
type: tutorial
---

# Stage 19: Bookmap

Publish the guide as a PDF book. A `<bookmap>` identifies book divisions
such as the preface, chapters, appendixes, and back matter. The PDF
transform uses that structure to place and number the content.

Create `audacity-book.ditamap` from the existing topics and add a
`book-pdf` deliverable. Include the print-only license topic from stage 18.

**Optional module.** See [Choose a learning path](/start-here/learning-path)
for the starting checkpoint and the next core lesson.

**Time:** about 30 minutes.
**You need:** stage 18 complete. The editor builds the PDF with the DITA-OT
and Apache FOP that it includes, so you do not need to install anything
else.


Recorded output below is an example. File counts, paths, and stage numbers can differ. [Check your work](/start-here/run-the-gate) to see the result for your own project.

## Step 1: The bookmap

::::steps
1. **Create `audacity-book.ditamap`**
   Right-click `my-audacity-guide` in the **Explorer**, choose
   **New File**, enter `audacity-book.ditamap`, and choose the
   **Bookmap** template. Replace the template's content with this
   bookmap:

   ```xml title="audacity-book.ditamap"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE bookmap PUBLIC "-//OASIS//DTD DITA BookMap//EN" "bookmap.dtd">
   
   <bookmap>
     <booktitle>
       <mainbooktitle>Audacity User Guide</mainbooktitle>
       <booktitlealt>Record, edit and export audio with Audacity</booktitlealt>
     </booktitle>
     <bookmeta>
       <author type="creator">The Audacity tutorial team</author>
       <publisherinformation>
         <organization>DogsBay Ltd.</organization>
       </publisherinformation>
       <critdates>
         <created date="2026-01-10"/>
         <revised modified="2026-03-02"/>
       </critdates>
       <bookid>
         <bookpartno>AUD-UG-34</bookpartno>
         <edition>3.4</edition>
       </bookid>
       <bookrights>
         <copyrfirst>
           <year>2026</year>
         </copyrfirst>
         <bookowner>
           <organization>DogsBay Ltd.</organization>
         </bookowner>
       </bookrights>
     </bookmeta>
   
     <frontmatter>
       <!-- A bookmap's content model is strict: keys and resource-only refs live
            inside frontmatter, before the book lists. -->
       <mapref href="subject-scheme.ditamap" type="subjectScheme"/>
       <mapref href="keydefs-product.ditamap"/>
       <mapref href="keydefs-glossary.ditamap"/>
       <keydef keys="common-notes" href="shared/common-notes.dita"/>
       <keydef keys="start-here" href="topics/what-is-audacity.dita"/>
       <topicref href="shared/common-steps.dita" processing-role="resource-only"/>
       <booklists>
         <toc/>
         <figurelist/>
         <tablelist/>
       </booklists>
       <notices href="topics/about-this-guide.dita"/>
       <preface href="topics/what-is-audacity.dita"/>
     </frontmatter>
   
     <part navtitle="Getting started">
       <chapter href="topics/what-is-digital-audio.dita" keys="digital-audio"/>
       <chapter href="topics/installing-audacity.dita" keys="install"/>
     </part>
     <part navtitle="Recording and editing">
       <chapter href="topics/preparing-to-record.dita">
         <topicref href="topics/recording-your-first-track.dita"/>
       </chapter>
       <chapter href="topics/trimming-audio.dita"/>
       <chapter href="topics/removing-background-noise.dita">
         <topicref href="topics/effect-order.dita"/>
       </chapter>
       <chapter href="topics/exporting-audio.dita"/>
     </part>
     <part navtitle="Podcasting">
       <chapter href="topics/podcast-production-workflow.dita" keys="podcast-workflow"/>
     </part>
   
     <appendix href="topics/supported-audio-formats.dita" keys="formats"/>
     <appendix href="topics/effects-reference.dita" keys="effects"/>
     <appendix href="topics/effect-presets.dita"/>
   
     <backmatter>
       <booklists>
         <glossarylist>
           <topicref href="topics/glossary/audio-units.dita"/>
           <topicref href="topics/glossary/g-bit-depth.dita"/>
           <topicref href="topics/glossary/g-clipping.dita"/>
           <topicref href="topics/glossary/g-compression.dita"/>
           <topicref href="topics/glossary/g-normalization.dita"/>
           <topicref href="topics/glossary/g-sample-rate.dita"/>
           <topicref href="topics/glossary/g-waveform.dita"/>
         </glossarylist>
         <indexlist/>
       </booklists>
     </backmatter>
   </bookmap>
   ```

2. **Read the title and the metadata**

   - The DOCTYPE is `-//OASIS//DTD DITA BookMap//EN` with `bookmap.dtd`.
     A bookmap is a map, so everything from Part 2 still applies inside it.
   - `<booktitle>` replaces a map's `<title>`: `<mainbooktitle>` is the
     title, `<booktitlealt>` a subtitle. Both print on the cover.
   - `<bookmeta>` replaces `<topicmeta>` at the map level and takes the
     prolog elements from stage 13, `<author>` and `<critdates>`, plus
     book-only ones: `<publisherinformation>` with an `<organization>`,
     `<bookid>` with a `<bookpartno>` and an `<edition>`, and
     `<bookrights>` with the first copyright year in `<copyrfirst>` and
     the `<bookowner>`. The order is fixed by the DTD, and it is the order
     shown.

3. **Read the front matter**
   `<frontmatter>` holds everything before chapter 1. Two things are in
   it that you might expect at the top of the map:
   - The maprefs to the subject scheme and the two key definition maps,
     the `common-notes` and `start-here` keydefs, and the resource-only
     reference to `common-steps`. A bookmap's content model is strict:
     at the top level it allows only `<booktitle>`, `<bookmeta>`,
     `<frontmatter>`, chapters, parts, appendixes, `<backmatter>` and
     reltables, in that order. Key definitions and resource-only
     references go inside `<frontmatter>`, and the demo at the end shows
     what happens when they do not.
   - `<booklists>` names the generated lists: `<toc/>`, `<figurelist/>`
     and `<tablelist/>` are empty elements, and the PDF transform fills
     them from the book. `<notices>` points at the license topic from
     stage 18, and `<preface>` at *What is Audacity?*. Both are
     topicrefs with a role.

4. **Read the body and the back matter**

   - `<part navtitle="…">` groups chapters and prints as *Part I*, *Part
     II*; each `<chapter href>` is a topic and its nested `<topicref>`s
     are its sections. The keys the topicrefs carried in the full guide,
     `digital-audio`, `install`, `podcast-workflow`, `formats` and
     `effects`, are carried here too, so the `<xref keyref>`s and the
     license topic's `<xref keyref="start-here"/>` resolve in the book.
   - `<appendix href>` after the parts: the three reference topics become
     *Appendix A*, *Appendix B* and *Appendix C*.
   - `<backmatter>` holds a second `<booklists>`: a `<glossarylist>`
     with the seven glossary topics inside it, so they print as one
     glossary, and an empty `<indexlist/>`, which asks the transform for
     an index built from the `<indexterm>` elements in the prolog
     metadata of stage 13. The index is the last entry in the contents
     list in step 3.
::::

## Step 2: The deliverable

::::steps
1. **Add the deliverable**
   Choose **Project** > **Project Tools** > **Manage Deliverables...** and
   click **Add...**. Enter these values:

   | Field | Value |
   |---|---|
   | **Name** | `book-pdf` |
   | **Input map** | `audacity-book.ditamap` |
   | **DITAVAL (optional)** | (leave empty) |
   | **Transtype** | `pdf` |
   | **Output (optional)** | `out/book-pdf` |
   | **Publication parameters** | (none) |

   Change **Transtype** from `html5` to `pdf`. Click **Save...**, click
   **OK** to write the deliverable, and click **Close**.

2. **Read it**
   The eighth deliverable, `book-pdf`, is the bookmap with the `pdf`
   transtype. DITA-OT's built-in PDF transform, `org.dita.pdf2`,
   renders the book with Apache FOP. The editor includes both, and
   **Project** > **Build Deliverables...** builds the PDF with the other
   seven deliverables into `out/book-pdf/`.

   Before you build, predict: the seven chapters sit in three parts. Do
   the chapter numbers start again at 1 in each part, or run on through
   the book?
::::

## Step 3: Check your work

::::steps
1. **Format and build**

   Format the changed files: choose **XML** > **Format** in each changed
   file and save it, or choose **Project** > **Project Tools** >
   **Format Project** to format every file at once. From the command line,
   run:

   ```bash
   dogsbay-xml format -i topics/*.dita *.ditamap
   ```

   Then choose **Project** > **Build Deliverables...** and click
   **Build All**. It builds eight deliverables now, the book among them.
   **Project** > **Check Project** also builds every deliverable, and
   checks the project and the built pages as well. Read its result in the
   **Project Validation** panel. From the command line, the same full
   check is:

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
   build    review               ok  /home/you/my-audacity-guide/out/review
     4 note(s) — run with --verbose to see them
   build    install-variants     ok  /home/you/my-audacity-guide/out/install-variants
     5 note(s) — run with --verbose to see them
   build    collection           ok  /home/you/my-audacity-guide/out/collection
     5 note(s) — run with --verbose to see them
   build    book-pdf             ok  /home/you/my-audacity-guide/out/book-pdf
     WARN  /home/you/my-audacity-guide/audacity-book.ditamap  PDF rendering reported 11 warnings (7 The following feature isn't implemented by Apache FOP, yet: table-layout=… (on fo:table) (…, 1 The contents of fo:external-graphic line n exceed the available area in the inline-progres…, 1 The contents of fo:instream-foreign-object line n exceed the available area in the inline-…, and 2 other kinds)
     5 note(s) — run with --verbose to see them
   output   wrote a file, no pages to check links in book-pdf
   output   clean (full, beginner-mac, beginner-windows, podcaster-linux, review, install-variants, collection)
   Ready: the project is healthy, every deliverable built, and the output of full, beginner-mac, beginner-windows, podcaster-linux, review, install-variants, collection holds together.
   ```

   The check builds eight deliverables. The `book-pdf` line reports the
   PDF, and the `WARN` line under it summarizes the layout warnings from
   Apache FOP, such as content that is wider than its column. The warnings
   do not fail the check. The output check reads links in HTML pages only,
   so for the PDF it confirms that the build wrote a file.

2. **Read the book**
   The editor writes the PDF to `out/book-pdf/audacity-book.pdf` in your
   project. In the **Explorer**, open `out`, then `book-pdf`: the build
   wrote one file, the whole book. Open it in your PDF viewer.

   - Page 1 is the cover, with the main title and the subtitle from
     `<booktitle>`.
   - The contents follow. The front matter pages are numbered in roman
     numerals.
   - Chapter numbers run on through the book, so *Part II* opens with
     *Chapter 3*. Parts get roman numerals, and appendixes get letters.
   - Each chapter opens on a new page with its number and title. A nested
     topic continues the chapter as a section.
   - The index at the end lists the index terms from stage 13 under their
     letters. Most entries have a page number. The terms that come from
     the glossary entries, such as *Bit depth* and *hertz*, are listed
     without one. Follow an entry with a page number to its page.

   `pdftotext` shows the contents page without a viewer:

   ```bash
   pdftotext out/book-pdf/audacity-book.pdf - | sed -n '/^Contents/,/^Index/p'
   ```

   ```
   Contents
   List of Figures..................................................................................................................................v
   List of Tables................................................................................................................................. vii

   About this guide.......................................................................................................ix
   Preface: What is Audacity?.................................................................................... xi
   Part I: Getting started............................................................................................ 13
   Chapter 1: What is digital audio?................................................................ 15
   Chapter 2: Installing Audacity......................................................................19
   Part II: Recording and editing.............................................................................. 21
   Chapter 3: Preparing to record.................................................................... 23
   Recording your first track...................................................................................................................... 24

   Chapter 4: Trimming audio...........................................................................25
   Chapter 5: Removing background noise......................................................27
   Why the order of effects matters........................................................................................................... 28

   Chapter 6: Exporting audio...........................................................................29
   Part III: Podcasting.................................................................................................31
   Chapter 7: Podcast production workflow.................................................... 33
   Appendix A: Supported audio formats.................................................................35
   Appendix B: Effects reference............................................................................... 37

   Appendix C: Presets................................................................................................39
   Glossary.......................................................................................................................................... 41
   Index................................................................................................................................................43
   ```

   The PDF transform generates the numbered parts, chapters, and
   appendixes, the lists of figures and tables, the glossary, and the index
   from the bookmap. The license topic appears as the notices page.
::::

Test the placement of key definitions. Move the two
`<keydef>` lines out of `<frontmatter>` to directly after `</bookmeta>`,
save, and choose **Project** > **Check Project**. The check stops at
health. Example output:

```
health   NOT CLEAN
  invalid: /home/you/my-audacity-guide/audacity-book.ditamap
    86:11  The content of element type "bookmap" does not match its content model.
  (run project-health for the full report)
Not ready: the project itself has faults. The build and the built output were not checked.
```

`<bookmap>` does not allow a `<keydef>` at its top level. The error is
reported at the closing `</bookmap>` tag, line 86, because that is where
the parser stops matching the content model. The model is a title, then
`<bookmeta>`, `<frontmatter>`, chapters or parts, appendixes,
`<backmatter>`, and relationship tables, in that order. Because health
failed, nothing was built. Keys, scheme maprefs and resource-only
references live inside `<frontmatter>` in a bookmap.

Undo the change with **Edit** > **Undo**, and save. Choose **XML** >
**Validate**: the **Errors** panel reports **Valid Document**, so the
bookmap matches its content model again. To rerun the full check, choose
**Project** > **Check Project** and confirm that it reports `Ready`.

## What you learned

- `<bookmap>` and its DOCTYPE; `<booktitle>` with `<mainbooktitle>` and
  `<booktitlealt>`.
- `<bookmeta>`: `<publisherinformation>`, `<bookid>` with `<bookpartno>`
  and `<edition>`, `<bookrights>` with `<copyrfirst>` and `<bookowner>`.
- `<frontmatter>` holds the keydefs, maprefs and resource-only refs, the
  `<booklists>` (`<toc>`, `<figurelist>`, `<tablelist>`), `<notices>` and
  `<preface>`; `<backmatter>` holds `<glossarylist>` and `<indexlist>`.
- `<part>`, `<chapter>`, `<appendix>`, and how the PDF numbers them.
- A deliverable with the `pdf` transtype, added in **Manage Deliverables**.
- The bookmap content model defines the permitted metadata, book divisions,
  and relationship tables at the top level.

## Next lesson

**Checkpoint:** `tutorial/19-bookmap`. If you use Git, commit your work.

Continue with [Stage 20: troubleshooting](/part-4-books/stage-20-troubleshooting).

For the core course, use the [learning path](/start-here/learning-path) to skip optional modules.

[Compare this stage with its predecessor](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/18-chunking-and-output...tutorial/19-bookmap).
