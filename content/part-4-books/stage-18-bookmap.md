---
title: "Stage 18: Bookmap"
description: Publish the guide as a PDF book with a bookmap, book metadata, front matter, parts, chapters, appendices, a glossary list and an index.
type: tutorial
---

# Stage 18: Bookmap

A map makes a website. A book needs more: a title page, a copyright notice,
a table of contents, parts and chapters, appendices, a glossary and an
index, in that order. DITA's `<bookmap>` is a map specialised for exactly
that. Every topicref in it says what kind of book division it is:
`<preface>`, `<chapter>`, `<appendix>`, and the PDF transform numbers and
places them accordingly.

This stage adds `audacity-book.ditamap`, a bookmap over the same topics as
the full guide, and a `book-pdf` deliverable that publishes it with DITA-OT's
`pdf` transtype. No topic changes; the licence topic that has been print-only
since stage 17 finally appears. Part 4 begins here.

**Time:** about 30 minutes.
**You need:** stage 17 complete. The PDF build needs a Java runtime, which
DITA-OT already needed.

## Step 1: The bookmap

::::steps
1. **Create `audacity-book.ditamap`**

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
     prolog elements from stage 12, `<author>` and `<critdates>`, plus
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
     `<frontmatter>`, chapters, parts, appendices, `<backmatter>` and
     reltables, in that order. Key definitions and resource-only
     references go inside `<frontmatter>`, and the demo at the end shows
     what happens when they do not.
   - `<booklists>` names the generated lists: `<toc/>`, `<figurelist/>`
     and `<tablelist/>` are empty elements, and the PDF transform fills
     them from the book. `<notices>` points at the licence topic from
     stage 17, and `<preface>` at *What is Audacity?*. Both are
     topicrefs with a role.

4. **Read the body and the back matter**
   - `<part navtitle="…">` groups chapters and prints as *Part I*, *Part
     II*; each `<chapter href>` is a topic and its nested `<topicref>`s
     are its sections. The keys the topicrefs carried in the full guide,
     `digital-audio`, `install`, `podcast-workflow`, `formats` and
     `effects`, are carried here too, so the `<xref keyref>`s and the
     licence topic's `<xref keyref="start-here"/>` resolve in the book.
   - `<appendix href>` after the parts: the two reference topics become
     *Appendix A* and *Appendix B*.
   - `<backmatter>` holds a second `<booklists>`: a `<glossarylist>`
     with the seven glossary topics inside it, so they print as one
     glossary, and an empty `<indexlist/>` that the transform fills from
     every `<indexterm>` in the prolog metadata of stage 12.
::::

## Step 2: The deliverable

::::steps
1. **Edit `project.json`**

   ```diff title="project.json"
   --- a/project.json
   +++ b/project.json
   @@ -120,6 +120,17 @@
              }
            ]
          }
   +    },
   +    {
   +      "name": "book-pdf",
   +      "context": {
   +        "id": "book-pdf",
   +        "input": "audacity-book.ditamap"
   +      },
   +      "output": "out/book-pdf",
   +      "publication": {
   +        "transtype": "pdf"
   +      }
        }
      ]
    }
   ```

2. **Read it**
   The eighth deliverable, `book-pdf`, is the bookmap with
   `"transtype": "pdf"`. DITA-OT's built-in PDF transform, `org.dita.pdf2`,
   uses Apache FOP and needs nothing installed beyond DITA-OT itself. The
   gate builds it with the other seven; on its own, the PDF takes about
   26 seconds on this project.
::::

## Step 3: README and the gate

::::steps
1. **Change the "You are on" line and the layout**

   ```diff title="README.md"
   --- a/README.md
   +++ b/README.md
   @@ -5,7 +5,7 @@ branch is the previous branch plus one lesson, from a single concept topic to a
    complete, publish-ready **Audacity User Guide**. The diff between two branches
    is the lesson.
    
   -You are on **stage 17 — chunking and output control**: pages, file names, print-only and search-hidden topics, composite files and topicsets.
   +You are on **stage 18 — bookmap**: a book with front matter, parts, chapters, appendices, a glossary and an index, published as PDF.
    
    ## Stages
    
   @@ -39,6 +39,7 @@ override the defaults). A stage is done when it prints `STAGE OK`.
    audacity-guide.ditamap  the main guide (start here)
    installation-variants.ditamap  branch filtering: one install topic, three platform variants
    beginner-guide.ditamap, podcaster-guide.ditamap  audience-specific guides that reuse the same topics
   +audacity-book.ditamap  the same guide as a book (PDF): parts, chapters, appendices, glossary, index
    audacity-collection.ditamap  all three guides in one publication, each in its own key scope
    filters/              DITAVAL files: what each deliverable includes, excludes or flags
    keydefs-product.ditamap key definitions: product name, version, download URL, project extension
   ```

2. **Format and check**

   ```bash
   dogsbay-xml format -i topics/*.dita *.ditamap
   scripts/check-stage.sh
   ```

   ```
   == validate-project  /home/you/audacity-guide ==
   38 file(s): 38 valid, 0 invalid.

   == project-health  /home/you/audacity-guide ==
   Root map: audacity-guide.ditamap (project config); house rules: none (none configured)
   Project is healthy: valid, no broken references, keys, orphans, or broken element ids.

   == validate-conditions  /home/you/audacity-guide  (scheme: subject-scheme.ditamap) ==
   38 file(s): 38 pass, 0 with violations.

   == build  project.json -> /tmp/check-stage-<pid> ==
   all deliverables built

   STAGE OK
   ```

3. **Read the book**
   The PDF is at `out/book-pdf/audacity-book.pdf` under the build folder
   the gate names (`/tmp/check-stage-<pid>/`). `pdftotext` shows the
   contents page without a viewer:

   ```bash
   pdftotext /tmp/check-stage-<pid>/out/book-pdf/audacity-book.pdf - | sed -n '/^Contents/,/^Index/p'
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
   Chapter 4: Trimming audio.......................................................................... 25
   Chapter 5: Removing background noise......................................................27
   Why the order of effects matters........................................................................................................... 28
   Chapter 6: Exporting audio.......................................................................... 29
   Part III: Podcasting.................................................................................................31
   Chapter 7: Podcast production workflow....................................................33
   Appendix A: Supported audio formats.................................................................35
   Appendix B: Effects reference............................................................................... 37
   Presets..................................................................................................................................................................38

   Glossary.......................................................................................................................................... 39
   Index................................................................................................................................................41
   ```

   The preface, the parts with their roman numerals, the chapters numbered
   within the book, the appendices lettered, the glossary and the index:
   all of it from the bookmap, none of it written by hand. The licence
   topic prints as the notices page.
::::

Now put the key definitions where a map would have them. Move the two
`<keydef>` lines out of `<frontmatter>` to directly after `</bookmeta>`, and
run the gate:

```
== validate-project  /home/you/audacity-guide ==
/home/you/audacity-guide/audacity-book.ditamap:
  85:11  error: The content of element type "bookmap" does not match its content model.
38 file(s): 37 valid, 1 invalid.
FAIL: validation errors

== build  project.json -> /tmp/check-stage-<pid> ==
Error: file:/home/you/audacity-guide/audacity-book.ditamap:85:11: [DOTJ088E] XML parsing error: The content of element type "bookmap" must match "((title|booktitle)?,bookmeta?,frontmatter?,chapter*,part*,(appendices?|appendix*),backmatter?,reltable*)".
FAIL: DITA-OT build reported errors (full log: /tmp/check-stage-<pid>.log)

STAGE FAILED
```

`<bookmap>` does not allow a `<keydef>` at its top level, and neither
validation nor DITA-OT accepts it. The editor reports the content model at
the closing tag, line 85, because that is where the parser gives up on
matching; DITA-OT's `DOTJ088E` spells the model out, and the build stops
before it can write a page. Keys, scheme maprefs and resource-only
references live inside `<frontmatter>` in a bookmap.

## What you learned

- `<bookmap>` and its DOCTYPE; `<booktitle>` with `<mainbooktitle>` and
  `<booktitlealt>`.
- `<bookmeta>`: `<publisherinformation>`, `<bookid>` with `<bookpartno>`
  and `<edition>`, `<bookrights>` with `<copyrfirst>` and `<bookowner>`.
- `<frontmatter>` holds the keydefs, maprefs and resource-only refs, the
  `<booklists>` (`<toc>`, `<figurelist>`, `<tablelist>`), `<notices>` and
  `<preface>`; `<backmatter>` holds `<glossarylist>` and `<indexlist>`.
- `<part>`, `<chapter>`, `<appendix>`, and how the PDF numbers them.
- `project.json`: a deliverable with the `pdf` transtype.
- The bookmap content model: nothing but book divisions at the top level.

## Where to go next

:::cards
- **[Stage 19: Troubleshooting](/part-4-books/stage-19-troubleshooting)** {icon="arrow-right"}
  A troubleshooting topic with a condition, causes and remedies, and a
  trouble note that points at it.

- **[Compare 17 to 18 on GitHub](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/17-chunking-and-output...tutorial/18-bookmap)** {icon="github"}
  Exactly what this stage added.
:::
