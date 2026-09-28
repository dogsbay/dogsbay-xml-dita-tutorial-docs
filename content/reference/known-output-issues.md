---
title: Known output issues
description: The final reference guide checks clean. Review the output defects of earlier builds, how the stages fixed them, and what the check does not cover.
type: reference
---

# Known output issues

The final reference branch, `tutorial/26-final`, checks clean. A
`dogsbay-xml check .` of the branch builds all eight deliverables with the
DITA-OT that the editor includes, and the output check finds no broken
links, images, or fragments in the seven HTML deliverables. The
maintainers' output checker, `scripts/check-output-links.py`, reports the
same result: 180 HTML pages checked, 0 failures.

## Fixed defects

Earlier builds of the final branch had 12 failing local references in the
HTML output: seven glossary links and five sample links. The stage
branches were corrected, and every later branch carries the fixes. The
table also lists two related defects that the link check did not count
as failures.

| Defect | Fix |
|---|---|
| Seven links to `topics/glossary/audio-units.html` had no target page in the beginner guides and the installation variants. | Stages 14 and 16 add `<topicref href="topics/glossary/audio-units.dita" toc="no"/>` to `beginner-guide.ditamap` and `installation-variants.ditamap`, because a key definition that points at a fragment does not make DITA-OT publish the topic. |
| Five links from the scripting reference to `samples/export-mp3.py` had no target, because the build does not copy `samples/` into the output. | Stage 22 names the script with `<filepath>` and links to the task that lists it, *Exporting from a script*. |
| The Windows and Linux copies of the recording task in the installation variants linked to the macOS copy of *The recording is silent*. | Stage 20 gives the trouble note `keyref="silent"` with an `href` fallback and defines the key in each branch, so each copy links to its own platform's copy. |
| Eight chunk links and a stylesheet reference were broken when the effects and presets topics were chunked into one page. | The effects and presets topics are separate files. The `chunk="by-topic"` example stays in `examples/chunking/`, which no deliverable publishes. |

## What the output check does not cover

- **The PDF.** The output check reads links in HTML pages. For the
  `book-pdf` deliverable it confirms that the build wrote
  `out/book-pdf/audacity-book.pdf`. Open the PDF to inspect its
  navigation, layout, and accessibility. The check summarizes Apache FOP
  layout warnings, such as content that is wider than its column, in one
  line. The warnings do not fail the check.
- **Keys that only the root map defines.** Project health resolves keys
  against the root map, `audacity-guide.ditamap`. If a topic uses a key
  that another publishing map does not define, the cross-reference in
  that map's PDF is empty, and the check still reports `Ready`. Stage 21
  shows the case and its fix.
- **External URLs.** The check does not follow links to other sites.

## Completing a lesson

[Check your work](/start-here/run-the-gate) at the end of every lesson.
If the check reports a broken link in the output, fix its source before
you continue. A defect that you introduce in one stage stays in every
later stage until you fix it.
