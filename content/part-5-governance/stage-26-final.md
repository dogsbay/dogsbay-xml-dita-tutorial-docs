---
title: "Stage 26: Final"
description: Verify every deliverable and generated link, complete the capstone, and mark your finished guide.
type: tutorial
---

# Stage 26: Final

Complete the [capstone](/practice/capstone), check the source, and inspect
every deliverable before you mark your finished guide.

**You need:** stage 25 complete. **Time:** about 15 minutes, plus corrections.

Recorded output below is an example. File counts, paths, and stage numbers can differ. [Check your work](/start-here/run-the-gate) to see the result for your own project.

## Review the project

**Project** > **Project Tools** > **Manage Deliverables...** lists eight
deliverables: `full`, `beginner-mac`, `beginner-windows`,
`podcaster-linux`, `review`, `install-variants`, `collection`, and
`book-pdf`. The chunking examples have separate maps and are built manually.

The last stage adds no files. Your project now holds everything that the
lessons create: the topics, glossary entries, shared content, the learning
assessment, the sample script, nine maps, the filters, the house rules, and
the project settings.

## Check and publish

Check the finished project. In the editor, choose **Project** >
**Check Project** and read the result in the **Project Validation** panel.
From the command line, run from `my-audacity-guide`:

```bash
dogsbay-xml check .
```

The check validates the source, runs project health with the metadata
policy and the house rules, builds all eight deliverables into `out/`,
and checks every link, image, and fragment in the HTML output. Example
output for the finished guide:

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

`Ready` also tells you that the lessons' exercises are undone. A short
description removed for the house rules would fail health, and two
branches with the same prefix would fail the output check.

The output check reads HTML pages only. For the PDF, it confirms that the
build wrote `out/book-pdf/audacity-book.pdf`. No check reads a page the
way a reader does, so inspect the output yourself:

1. **One page reads right.** Since stage 18, the digital audio topic is
   part of the full guide's `out/full/topics/what-is-audacity.html` page.
   Open that page and find the link to `recording-your-first-track.html`,
   the image `waveform.png`, which the build copied to the output, and the
   table header from the `<sthead>`. Then open
   `topics/what-is-digital-audio.dita` and choose **View** >
   **Preview in Tab**. Read the title and the short description, and
   follow the link once. Check that the figure's picture matches its
   title, and that the bit depth table's headers say what each column
   holds.
2. **A filtered page holds only its own content.** Open
   `out/beginner-windows/topics/installing-audacity.html` and choose
   **View** > **Preview in Tab**. The install step shows one row, the
   Windows installer. A row for every platform would mean that a
   deliverable lost its filter, and no check would report it.
3. **The book is complete.** In the **Terminal** panel, print the PDF's
   first page:

   ```bash
   pdftotext -l 1 out/book-pdf/audacity-book.pdf -
   ```

   Example output:

   ```
   Audacity User Guide
   Record, edit and export audio with Audacity
   ```

   The cover carries the book's title and subtitle from the bookmap.
   Then open `audacity-book.pdf` in your PDF viewer. Check the contents,
   the opening page of a chapter, a table, and the index, and follow one
   bookmark and one index entry. Inspect its navigation, layout, and
   accessibility.

The final reference build checks clean. See
[Known output issues](/reference/known-output-issues) for the defects that
earlier builds had and how the stage branches fixed them.

## Mark your result

If you use Git, commit your work in the **Git** panel. Then mark the
finished guide with a tag:

1. In the **Git** panel, open the **…** menu and choose **Tag** >
   **Create Tag...**.
2. In **Tag name**, type `my-tutorial-final`. In **Message**, type a short
   note, such as `Finished the DITA tutorial`.
3. Click **OK**.

A tag marks one commit and does not move when you commit again, so it keeps
this version of your project while you continue to change the guide.
**Tag** > **Tags...** lists your tags.

**Checkpoint:** `tutorial/26-final`, also tagged `tutorial/final` in the
[sample project](/start-here/set-up#the-sample-project). It includes every
optional lesson, and it checks as `Ready`. Compare your files with it to
find any differences that remain.

[Compare this stage with its predecessor](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/25-house-rules...tutorial/26-final).
