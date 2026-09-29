---
title: "Stage 26: Final"
description: Verify every deliverable and generated link, complete the capstone, and tag your finished guide.
type: tutorial
---

# Stage 26: Final

Complete the [capstone](/practice/capstone), check the source, and inspect
every deliverable before tagging your work.

**You need:** stage 25 complete. **Time:** about 15 minutes, plus corrections.

Recorded output below is an example. File counts, paths, and stage numbers can differ. [Check your work](/start-here/run-the-gate) to see the result for your own project.

## Review the project

**Project** > **Project Tools** > **Manage Deliverables...** lists eight
deliverables: `full`, `beginner-mac`, `beginner-windows`,
`podcaster-linux`, `review`, `install-variants`, `collection`, and
`book-pdf`. The chunking examples have separate maps and are built manually.

The README records the complete branch sequence:

```md title="README.md"
# DITA tutorial

Build a guide through 27 stages. Maps begin at stage 02; HTML publication begins at stage 03.

You are on stage 26: final.

## Stages

| Branch | Lesson |
|---|---|
| `tutorial/00-setup` | setup |
| `tutorial/01-concept` | concept |
| `tutorial/02-first-map` | first map |
| `tutorial/03-first-build` | first build |
| `tutorial/04-task-and-reference` | task and reference |
| `tutorial/05-inline-and-block` | inline and block |
| `tutorial/06-rich-tasks` | rich tasks |
| `tutorial/07-links` | links |
| `tutorial/08-figures` | figures |
| `tutorial/09-map-structure` | map structure |
| `tutorial/10-keys` | keys |
| `tutorial/11-reuse` | reuse |
| `tutorial/12-glossary` | glossary |
| `tutorial/13-metadata-and-index` | metadata and index |
| `tutorial/14-conditional-text` | conditional text |
| `tutorial/15-subject-scheme` | subject scheme |
| `tutorial/16-branch-filtering` | branch filtering |
| `tutorial/17-key-scopes` | key scopes |
| `tutorial/18-chunking-and-output` | chunking and output |
| `tutorial/19-bookmap` | bookmap |
| `tutorial/20-troubleshooting` | troubleshooting |
| `tutorial/21-hazards-and-safety` | hazards and safety |
| `tutorial/22-software-domains` | software domains |
| `tutorial/23-learning` | learning |
| `tutorial/24-drafts-and-localization` | drafts and localization |
| `tutorial/25-house-rules` | house rules |
| `tutorial/26-final` | final |

## Verification

Run `scripts/check-stage.sh` from the project root. From stage 03, the gate builds the deliverables in `project.json`.
Run `python3 scripts/check-output-links.py <output-directory>` after publishing HTML. Source health and generated output checks report separate results.

The chunking experiment under `examples/chunking/` is available from stage 18. Build its maps separately; they are not release deliverables.

Topic text is adapted from the Audacity Manual. See LICENSE and NOTICE for licensing and attribution.
```

## Check and publish

Check the finished project. In the editor, choose **Project** >
**Check Project** and read the result in the **Project Validation** panel.
From the command line, run from the project root:

```bash
dogsbay-xml check .
```

The check validates the source, runs project health with the metadata
policy and the house rules, builds all eight deliverables into `out/`,
and checks every link, image, and fragment in the HTML output. Example
output for the finished guide:

```
health   clean
build    full                 ok  /home/you/audacity-guide/out/full
build    beginner-mac         ok  /home/you/audacity-guide/out/beginner-mac
build    beginner-windows     ok  /home/you/audacity-guide/out/beginner-windows
build    podcaster-linux      ok  /home/you/audacity-guide/out/podcaster-linux
build    review               ok  /home/you/audacity-guide/out/review
build    install-variants     ok  /home/you/audacity-guide/out/install-variants
build    collection           ok  /home/you/audacity-guide/out/collection
build    book-pdf             ok  /home/you/audacity-guide/out/book-pdf
  PDF rendering reported 9 warnings (2 The following feature isn't implemented by Apache FOP, yet: table-layout=… (on fo:table) (…, 2 The contents of fo:inline line n exceed the available area in the inline-progression direc…, 2 The contents of fo:block line n exceed the available area in the inline-progression direct…, and 3 other kinds)
output   wrote a file, no pages to check links in book-pdf
output   clean (full, beginner-mac, beginner-windows, podcaster-linux, review, install-variants, collection)
Ready: the project is healthy, every deliverable built, and the output of full, beginner-mac, beginner-windows, podcaster-linux, review, install-variants, collection holds together.
```

The README's *Verification* section names `scripts/check-stage.sh` and
`scripts/check-output-links.py`, the scripts that maintainers use to
verify the stage branches. You do not need them. See
[Check your work](/start-here/run-the-gate).

The output check reads HTML pages only. For the PDF, it confirms that the
build wrote `out/book-pdf/audacity-book.pdf`. Open the PDF and inspect its
navigation, layout, and accessibility yourself.

The final reference build checks clean. See
[Known output issues](/reference/known-output-issues) for the defects that
earlier builds had and how the stage branches fixed them.

## Tag your result

Commit your lesson changes and confirm that `git status` is clean. After
completing the checks, create a tag for your own result:

```bash
git tag my-tutorial-final
git show --stat my-tutorial-final
```

The supplied reference is `tutorial/26-final`, also tagged `tutorial/final`.
It includes every optional lesson, and it checks as `Ready`.

[Compare this stage with its predecessor](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/25-house-rules...tutorial/26-final).
