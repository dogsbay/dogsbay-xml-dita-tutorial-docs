---
title: "Stage 26: Final"
description: Verify every deliverable and generated link, complete the capstone, and tag your finished guide.
type: tutorial
---

# Stage 26: Final

Complete the [capstone](/practice/capstone), check the source, and inspect
every deliverable before tagging your work.

**You need:** stage 25 complete. **Time:** about 15 minutes, plus corrections.

## Review the project

`project.json` defines eight deliverables: full, beginner-mac,
beginner-windows, podcaster-linux, review, install-variants, collection,
and book-pdf. The chunking examples have separate maps and are built manually.

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

## Validate and publish

Run these commands from the project root:

```bash
scripts/check-stage.sh
dita --project=project.json --output=out
python3 scripts/check-output-links.py out
```

The gate checks source validation, project health, controlled values, house
rules, and publication. The output checker tests local HTML links and assets.
Inspect PDF navigation, accessibility, and layout separately.

Review [known output issues](/reference/known-output-issues). A successful
source gate does not override a failed output check. Resolve release defects
before describing your own guide as ready for publication.

## Tag your result

Commit your lesson changes and confirm that `git status` is clean. After
completing the checks, create a tag for your own result:

```bash
git tag my-tutorial-final
git show --stat my-tutorial-final
```

The supplied reference is `tutorial/26-final`, also tagged `tutorial/final`.
It includes every optional lesson. The tag identifies a checkpoint, not a
claim that every known publishing issue has been resolved.

[Compare this stage with its predecessor](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/25-house-rules...tutorial/26-final).
