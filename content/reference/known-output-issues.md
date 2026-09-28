---
title: Known output issues
description: Review the broken links and assets found in the final reference guide by the additional HTML output checker.
type: reference
---

# Known output issues

The final reference branch passes its historical gate, but the additional
output-link check finds defects. A DITA-OT 4.3.5 build of `tutorial/26-final`
produced 177 HTML pages with 12 failing local references. All eight
deliverables built. These are baseline findings in the supplied guide.

| Finding | Affected output | Investigation |
|---|---|---|
| Links to `topics/glossary/audio-units.html` have no target file | Beginner Mac, beginner Windows, and installation variants | Review glossary topic inclusion in each root map; a key definition alone does not guarantee a published page. |
| The scripting reference links to `samples/export-mp3.py`, which is absent from the output | Full guide, review guide, podcaster guide, and collection | Configure the sample as a published resource and check both the download link and code listing. |

Generated filenames and process-specific paths can differ between builds.
Use the [output-link checker](/reference/verification) to obtain the paths
for your run. These findings cover local HTML references; they do not assess
external URLs or PDF accessibility.

Separating the effects and presets topics removed the eight broken chunk
links and the missing stylesheet reference from the production builds.
The remaining failures comprise seven glossary links and five sample links.

The original chunking finding is why the tutorial keeps its `chunk="by-topic"` example
in an isolated demonstration map. Production topics should use separate source
files and explicit topicrefs until the exact DITA-OT version used for a release
has been tested. The demonstration is still useful for learning the feature,
but a valid DITA structure is not proof that every processor generates valid
links.

## Completing a lesson

Use the historical gate to compare your source with the reference stage.
Use the output check to identify publishing defects as a separate result.
Record existing findings and any new findings introduced by your changes.
A failed output check remains a failure even when the historical gate passes.

## Preparing a release

Resolve the output defects before describing the generated guide as ready
for publication. Source corrections belong on the affected stage branches,
with subsequent branches and quoted examples updated together. The tutorial's
deliberately broken `main` project is a separate exercise and should retain
its planted errors.
