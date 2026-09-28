# Tutorial documentation review

## Source fixes and Check Project for every stage, September 28, 2026

The tutorial branches were rebuilt from stage 11 with the phase 3 source
fixes (see `plans/editor-consistency.md`, "Status"). Every stage now checks
clean with the current editor: stages 00 to 02 report healthy with no
deliverables yet, and stages 03 to 26 report Ready, including the PDF.

- Stages 14 to 26 now use **Project** > **Check Project** instead of the
  gate. Every recorded check output and deliberate-error exercise was
  re-recorded with `dogsbay-xml check` on the rebuilt branches.
- Set-up and the start-here pages move between stages with the status-bar
  branch switcher; `git switch` stays as the command-line option.
- Stage 23 validates learning topics without a catalog option; stage 24
  builds the draft with a deliverable parameter instead of the `dita` CLI.
- Known output issues: the final build has 0 broken links across 180
  pages. Two limitations remain and are documented: the split chunking
  example in stage 18 has two broken generated links, and the PDF built in
  the editor has no index.

The docs source audit and the excerpt check (190 listings, 0 failures)
passed.

## New file templates, stages 01 to 13, September 28, 2026

Every step that creates a DITA file now starts from the editor's **New
File** dialog and names the template to choose: Concept, Task, Reference,
Topic, Map, Key Definition Map, Glossary Entry, or Glossary Group. Each
kind's template content is shown once, where it is first used, with the
placeholders to replace. The editor sets the root `@id` from the file name;
stage 12 tells readers to change the glossary ids from `g-` to `gl-`.

In stages 04 to 06, the map update now comes before the check, because
the check reports a topic missing from the map as an orphan.

The docs source audit and the excerpt check (193 listings, 0 failures)
passed. Titled listings are unchanged.

## Check Project, stages 00 to 13, September 28, 2026

Readers of stages 00 to 13 now check their work with **Project** >
**Check Project** in the editor, or `dogsbay-xml check`, instead of
`scripts/check-stage.sh`. The script stays on the branches for maintainers
and is documented as the stage gate.

- Every recorded check output comes from running `dogsbay-xml check` on
  that stage's branch (dogsbay-xml `58cf9a2`). Every deliberate-error
  exercise was re-run to record what the check and `project-health` now
  print.
- Set-up no longer asks for a separate DITA-OT install or `DITA_HOME`;
  the editor and its command line include DITA-OT 4.3.5.
- Stage 00 no longer has readers write the gate script.

Stages 14 to 26 still describe the gate: on the current branches and
editor, a correct project does not yet check as ready there (see
`plans/editor-consistency.md`, "Phase 2 status").

The docs source audit, the excerpt check (193 listings, 0 failures; one
listing fewer because the gate script is no longer quoted), the strict site
build, the Astro production build, and the generated-site audit passed.

## Editor-consistency pass, September 28, 2026

A dry run of all 27 stages found wrong claims, stale recorded output, and
steps the editor now does differently. This pass (phase 1 of
`plans/editor-consistency.md`) changed prose only:

- Stage 01 starts from the `dita-concept-minimal` template, as the
  watch-along video does, and shows the editor's **Format** and
  **Validate** beside the command line.
- Stage 02 explains the map and names the editor path for the default
  root map. Its exercise uses the gate, since `validate` does not check
  hrefs.
- Stages 05, 06, 07, 09, 11, 12, 13, 17, 19, 20, 22, 23, 24, and 25 no
  longer make claims that the dry run disproved by building or
  validating.
- The start-here pages show the real "You are on" line and a command that
  prints it.

All 27 stage gates passed with the current `dogsbay-xml` and DITA-OT
4.3.5. All 194 quoted listings still match their branches. The docs source
audit, the strict site build, the Astro production build, and the
generated-site audit passed. Recorded check output is re-recorded in
phase 2, once the editor has a single project check.

## Redesign verification, September 22, 2026

The implemented course now has 27 stages. Stage 02 creates the permanent
map, and stage 03 adds the HTML deliverable. Every later authoring stage
can be built and inspected. The branches form a new linear history ending
at `tutorial/26-final`, also tagged `tutorial/final`.

All 27 stage gates passed with DITA-OT 4.3.5. All 194 quoted listings and
diffs match the new branches. The docs source audit, generated-site audit,
and Astro production build passed. The stage gates still report formatting
warnings for the isolated chunking demonstration.

Separating effects and presets removed nine output failures. The final
production build now has 177 HTML pages with 12 remaining glossary and
sample-download failures. The combined chunking example passes its output
check; the split example reproduces two broken generated links.

The earlier review below records the findings before the reorganization.

## Original review

Reviewed the 26 stage lessons, introductory pages, and reference pages
against the tutorial's Git workflow, gate script, and deliverable definitions.
The review focused on instructional accuracy and editorial style. It did
not audit the editor implementation or execute all 26 DITA builds. The
deliberate errors on the tutorial repository's `main` branch are part of a
separate exercise.

## Corrections made

| Finding | Reader impact | Correction |
|---|---|---|
| Stage 00 creates an independent repository, but stage 06 assumes tutorial refs exist there. | Image retrieval fails. | Fetch reference branches during setup and retrieve images with `git restore --source=origin/...`. |
| Comparison commands assume local stage branches. | Commands fail in a fresh clone. | Use remote-tracking refs for comparisons and file inspection. |
| The branch reference describes checkout as discarding work. | Readers misunderstand which files they are viewing or retaining. | Explain checkout behavior and use a separate worktree for inspection. |
| The gate writes to a temporary directory while lessons inspect project-local output. | Readers open missing or stale files. | Explain both locations and add explicit local builds before inspection. |
| Stage 22 expands an optional, unquoted `DITA_HOME`. | Catalog validation fails for unset variables or paths with spaces. | Require the installation variable during setup and quote it in commands. |
| Stage 25 creates the existing `tutorial/final` tag. | The command fails in a clone or tags the wrong learner state. | Use a learner-owned tag after committing and checking the work. |
| Descriptions overstate what validation proves. | Readers can miss unresolved links or incorrect variants after a passing gate. | Describe check scope, warnings, output inspection, and gate limitations. |
| Lessons contain rhetorical contrasts, metaphors, and inconsistent spelling. | Instructions take longer to interpret. | Rewrite introductions and explanations using direct language and consistent US English. |

These findings have high confidence and low correction risk. Changes are
limited to documentation; copied branch files and diagnostic output retain
their original content.

## Implemented course improvements

The core course now includes content planning, an HTML preview after stage
01, a reuse decision guide, five independent exercises with suggested
solutions, an interview-workflow capstone, and an accessibility review.
The learning-path page identifies optional modules and the checkpoints
needed when skipping them. Stage numbers and branch ancestry are retained.

The subject-scheme explanation now excludes container subjects from allowed
profiling values. The table guidance includes simple-table headers and
relative widths. Composite-topic and chunking observations are identified
as checker or processor limitations.

The tutorial repository now includes `scripts/check-excerpts.sh` and
`scripts/check-output-links.py`. The excerpt checker passed all 190 titled
listings and diffs. Seven regression tests cover excerpt drift and local
output-link failures. The all-stages runner checks HTML after successful builds.

The early preview was built and its two HTML pages passed the link check.
The final reference guide passed its historical gate and built all eight
deliverables. The output checker found 21 broken references across 178 HTML
pages. See `content/reference/known-output-issues.md`. Those source and
processor problems remain to be resolved across the stage branches.

## Original follow-up recommendations

1. Excerpt verification is implemented. Run it in CI when stage branches or
   documentation change.
2. Independent exercises and suggested solutions are implemented.
3. Generated HTML link checking is implemented and has exposed additional
   baseline defects. Fix the affected stage sources and recheck the outputs.
4. Replace universal tool claims with versioned observations when recording
   future runs. Catalog coverage, formatter behavior, and publishing output
   can change between tool versions.

Update the corresponding stage branches before changing their copied
examples. Keep the documentation's editorial conventions in [STYLE.md](STYLE.md).
