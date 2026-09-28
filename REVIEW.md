# Tutorial documentation review

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
