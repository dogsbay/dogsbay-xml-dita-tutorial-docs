---
title: "Stage 25: Final"
description: No new DITA. The README lists every stage and every deliverable, the branch is tagged tutorial/final, and the full gate builds all eight deliverables clean.
type: tutorial
---

# Stage 25: Final

The last stage adds no DITA. Twenty-five branches have built, from one
concept topic, a guide of 26 topic files and nine maps that publishes as
three html5 guides in five variants, a collection, a set of install
variants and a PDF book, checked by a gate that validates every file,
resolves every link and key, enforces a metadata policy, a subject scheme
and the house rules, and builds every deliverable. This stage writes that
down: the README on the branch gets the full stage table and the list of
deliverables, and the commit is tagged `tutorial/final`, a name that will
keep pointing at the finished tree however the chain is rebased.

Read it as a checklist. If your own project has come this far, it has
everything on this page; if it has not, the table says which stage to go
back to.

**Time:** about 15 minutes.
**You need:** stage 24 complete.

## Step 1: The README

::::steps
1. **Write the stage table and the deliverables**

   ```diff title="README.md"
   --- a/README.md
   +++ b/README.md
   @@ -5,7 +5,7 @@ branch is the previous branch plus one lesson, from a single concept topic to a
    complete, publish-ready **Audacity User Guide**. The diff between two branches
    is the lesson.
    
   -You are on **stage 24 — house rules**: Schematron rules, an agent guide and a skill, and the gate that enforces them.
   +You are on **stage 25 — final**: the complete guide, also tagged `tutorial/final`. Every deliverable builds and the gate is clean.
    
    ## Stages
    
   @@ -18,10 +18,29 @@ You are on **stage 24 — house rules**: Schematron rules, an agent guide and a
    | `tutorial/04-rich-tasks` | prereqs, substeps, choices, examples |
    | `tutorial/05-links` | cross-references and related links |
    | `tutorial/06-figures` | images, SVG and an equation |
   -| … | maps, keys, reuse, glossary, metadata, conditions, subject schemes, branch filtering, key scopes, chunking, bookmap, troubleshooting, hazards, software domains, learning, drafts and localization, house rules |
   +| `tutorial/07-first-map` | the first map and build |
   +| `tutorial/08-map-structure` | hierarchy, sequences, a relationship table |
   +| `tutorial/09-keys` | product variables and indirect links |
   +| `tutorial/10-reuse` | conref, conkeyref, ranges and push |
   +| `tutorial/11-glossary` | glossary entries and abbreviations |
   +| `tutorial/12-metadata-and-index` | prolog metadata, index terms, a metadata policy |
   +| `tutorial/13-conditional-text` | conditions, DITAVAL filters and flags, audience maps |
   +| `tutorial/14-subject-scheme` | controlled values |
   +| `tutorial/15-branch-filtering` | one topic, three platform variants |
   +| `tutorial/16-key-scopes` | three guides in one collection |
   +| `tutorial/17-chunking-and-output` | pages, file names, print and search control |
   +| `tutorial/18-bookmap` | a book and a PDF |
   +| `tutorial/19-troubleshooting` | a troubleshooting topic |
   +| `tutorial/20-hazards-and-safety` | hazard statements |
   +| `tutorial/21-software-domains` | commands, syntax, messages, code samples |
   +| `tutorial/22-learning` | a learning assessment |
   +| `tutorial/23-drafts-and-localization` | review markup, language, translation |
   +| `tutorial/24-house-rules` | Schematron rules, an agent guide and skill |
    | `tutorial/25-final` | the complete guide (also tagged `tutorial/final`) |
    
   -The walkthrough for readers is the docs site (see the `main` branch README).
   +Deliverables in `project.json`: `full`, `beginner-mac`, `beginner-windows`,
   +`podcaster-linux`, `review`, `install-variants`, `collection` (html5) and
   +`book-pdf` (PDF).
    
    ## The gate
    
   ```

   The whole file, as it is on the branch:

   ````md title="README.md"
   # DogsBay DITA tutorial — step by step

   A DITA 1.3 project built up one feature at a time. Each `tutorial/NN-slug`
   branch is the previous branch plus one lesson, from a single concept topic to a
   complete, publish-ready **Audacity User Guide**. The diff between two branches
   is the lesson.

   You are on **stage 25 — final**: the complete guide, also tagged `tutorial/final`. Every deliverable builds and the gate is clean.

   ## Stages

   | Branch | Adds |
   |--------|------|
   | `tutorial/00-setup` | project settings, the gate script |
   | `tutorial/01-concept` | one concept topic |
   | `tutorial/02-task-and-reference` | a task and a reference topic |
   | `tutorial/03-inline-and-block` | inline semantics, notes, lists, code |
   | `tutorial/04-rich-tasks` | prereqs, substeps, choices, examples |
   | `tutorial/05-links` | cross-references and related links |
   | `tutorial/06-figures` | images, SVG and an equation |
   | `tutorial/07-first-map` | the first map and build |
   | `tutorial/08-map-structure` | hierarchy, sequences, a relationship table |
   | `tutorial/09-keys` | product variables and indirect links |
   | `tutorial/10-reuse` | conref, conkeyref, ranges and push |
   | `tutorial/11-glossary` | glossary entries and abbreviations |
   | `tutorial/12-metadata-and-index` | prolog metadata, index terms, a metadata policy |
   | `tutorial/13-conditional-text` | conditions, DITAVAL filters and flags, audience maps |
   | `tutorial/14-subject-scheme` | controlled values |
   | `tutorial/15-branch-filtering` | one topic, three platform variants |
   | `tutorial/16-key-scopes` | three guides in one collection |
   | `tutorial/17-chunking-and-output` | pages, file names, print and search control |
   | `tutorial/18-bookmap` | a book and a PDF |
   | `tutorial/19-troubleshooting` | a troubleshooting topic |
   | `tutorial/20-hazards-and-safety` | hazard statements |
   | `tutorial/21-software-domains` | commands, syntax, messages, code samples |
   | `tutorial/22-learning` | a learning assessment |
   | `tutorial/23-drafts-and-localization` | review markup, language, translation |
   | `tutorial/24-house-rules` | Schematron rules, an agent guide and skill |
   | `tutorial/25-final` | the complete guide (also tagged `tutorial/final`) |

   Deliverables in `project.json`: `full`, `beginner-mac`, `beginner-windows`,
   `podcaster-linux`, `review`, `install-variants`, `collection` (html5) and
   `book-pdf` (PDF).

   ## The gate

   ```bash
   scripts/check-stage.sh            # validate every file, check project health, build every deliverable
   SKIP_BUILD=1 scripts/check-stage.sh
   ```

   It needs the `dogsbay-xml` CLI and DITA-OT 4.3.5 (`DOGSBAY_XML`, `DITA_HOME`
   override the defaults). A stage is done when it prints `STAGE OK`.

   ## Layout

   ```
   audacity-guide.ditamap  the main guide (start here)
   installation-variants.ditamap  branch filtering: one install topic, three platform variants
   beginner-guide.ditamap, podcaster-guide.ditamap  audience-specific guides that reuse the same topics
   audacity-book.ditamap  the same guide as a book (PDF): parts, chapters, appendices, glossary, index
   audacity-collection.ditamap  all three guides in one publication, each in its own key scope
   filters/              DITAVAL files: what each deliverable includes, excludes or flags
   keydefs-product.ditamap key definitions: product name, version, download URL, project extension
   keydefs-glossary.ditamap key definitions for the glossary entries
   topics/glossary/      glossary entries and a glossary group
   project.json          DITA-OT project file: the deliverables this guide ships
   house-style.sch       house rules as Schematron; run by project-health
   AGENTS.md             project context and house style for AI agents (and people)
   .xagent/skills/       a project-specific agent skill
   subject-scheme.ditamap  the controlled values for platform and audience
   .dogsbay/config.xml   shared editor project settings (project type, framework, default map and deliverable, format style)
   scripts/              the gate
   topics/               topics
   learning/             learning and training: an assessment
   samples/              code samples pulled into topics by coderef
   shared/               warehouses: content pulled in by conref (common-steps, common-notes)
   images/               illustrations referenced by <image>
   ```

   ## Licence and attribution

   CC BY 4.0, see [LICENSE](LICENSE). Topic text is adapted from the
   [Audacity Manual](https://manual.audacityteam.org/) (CC BY 3.0); see
   [NOTICE](NOTICE) for the credit the licence requires.
   ````

2. **Read it**
   The "You are on" line names the stage, as on every branch since stage
   00. The table that Part 1 started with six rows and an ellipsis now
   has all 26, one per branch, with what each adds in the words the
   branch's page uses. The deliverables line is the contents of
   `project.json` in one sentence. The rest of the file, the gate, the
   layout and the licence, has grown one line at a time since stage 00
   and is unchanged here.
::::

## Step 2: The tag

::::steps
1. **Tag the branch**
   On the branch, once the gate is clean:

   ```bash
   git tag tutorial/final
   git tag --points-at tutorial/25-final
   ```

   ```
   tutorial/final
   ```

2. **Read it**
   A branch moves: stage 25 is rebased every time a middle stage is
   edited, as [Branches](/reference/branches) describes, and its commit
   gets a new hash. A tag is a name for one commit. `tutorial/final` is
   moved by hand, to the new tip of the chain, after the ladder has been
   re-checked, so a script or a reader who wants the finished project
   can fetch that name and get a tree that passed the gate:

   ```bash
   git fetch --tags
   git show tutorial/final:README.md | head -9
   ```
::::

## Step 3: Everything it ships

::::steps
1. **Read `project.json`**
   Eight deliverables, each with a map, an optional DITAVAL and a
   transtype:

   | Deliverable | Map | Filter | Output | Since |
   |---|---|---|---|---|
   | `full` | `audacity-guide.ditamap` | none | html5, full navigation | [07](/part-2-maps/stage-07-first-map) |
   | `beginner-mac` | `beginner-guide.ditamap` | `filters/mac-beginner.ditaval` | html5, partial navigation | [13](/part-3-conditions/stage-13-conditional-text) |
   | `beginner-windows` | `beginner-guide.ditamap` | `filters/windows-beginner.ditaval` | html5, partial navigation | [13](/part-3-conditions/stage-13-conditional-text) |
   | `podcaster-linux` | `podcaster-guide.ditamap` | `filters/linux-podcaster.ditaval` | html5 | [13](/part-3-conditions/stage-13-conditional-text) |
   | `review` | `audacity-guide.ditamap` | `filters/review.ditaval` | html5, everything flagged | [13](/part-3-conditions/stage-13-conditional-text) |
   | `install-variants` | `installation-variants.ditamap` | in the map, by `<ditavalref>` | html5 | [15](/part-3-conditions/stage-15-branch-filtering) |
   | `collection` | `audacity-collection.ditamap` | none | html5, full navigation | [16](/part-3-conditions/stage-16-key-scopes) |
   | `book-pdf` | `audacity-book.ditamap` | none | PDF | [18](/part-4-books/stage-18-bookmap) |

2. **Run the full gate**

   ```bash
   scripts/check-stage.sh
   ```

   ```
   == validate-project  /home/you/audacity-guide ==
   /home/you/audacity-guide/learning/check-your-understanding.dita:
     -1:-1  error: Validation failed: /home/you/audacity-guide/learning/learningAssessment.dtd (No such file or directory)
   42 file(s): 41 valid, 1 invalid.

   == validate learning/ against the DITA-OT catalog ==
   VALID

   == project-health  /home/you/audacity-guide ==
   Root map: audacity-guide.ditamap (project config); house rules: house-style.sch (project config)
   Project is healthy: valid, no broken references, keys, orphans, or broken element ids.

   == house rules  /home/you/audacity-guide ==
   Root map: audacity-guide.ditamap (project config); house rules: house-style.sch (project config)
   Project is healthy: valid, no broken references, keys, orphans, or broken element ids.

   == validate-conditions  /home/you/audacity-guide  (scheme: subject-scheme.ditamap) ==
   42 file(s): 42 pass, 0 with violations.

   == build  project.json -> /tmp/check-stage-<pid> ==
   all deliverables built

   STAGE OK
   ```

   Every step, every deliverable. The build's output folder has one
   directory per deliverable:

   ```
   beginner-mac
   beginner-windows
   book-pdf
   collection
   full
   install-variants
   podcaster-linux
   review
   ```

   The run takes under a minute on the full guide, most of it the PDF.
   That is the cost of knowing that the branch you tag is the branch that
   builds.
::::

## What to build next

The guide is complete for what it set out to teach, and it is a starting
point. Each of these is one more branch, in the same shape as the 26
before it:

- **A second language.** Copy the topics to `topics/de/`, translate them,
  set `xml:lang="de"` on each root, and publish a `full-de` deliverable
  from a map that references them. The `translate="no"` marks from stage
  23 say what stays in English.
- **A constraint module.** The house rule against `<b>` runs after the
  fact; a DTD constraint removes `<b>` from the grammar, so that the
  editor never offers it. Constraints and specialisation are the next
  layer of DITA above what this tutorial covers.
- **A DITA-OT plugin.** The PDF from stage 18 uses the default
  stylesheets. A small plugin puts the hazard symbol on the cover, the
  product name in the running header and the house's fonts on the page.
- **A learning plan.** Stage 22 wrote one assessment; a
  `<learningPlan>`, a `<learningContent>` and a `<learningMap>` turn the
  guide into a course.
- **Continuous checking.** `scripts/check-stage.sh` runs anywhere
  `dogsbay-xml` and DITA-OT are installed. A workflow that runs it on
  every pull request is twenty lines, and it makes the gate a rule of the
  repository rather than a habit of its authors.

## What you learned

- Nothing new in DITA. The README as the map of the project, the tag as
  the fixed name for a moving branch, `project.json` as the list of what
  ships.
- The full gate on the full guide: validation, health, house rules,
  controlled values, and every deliverable built.
- Where to go from here: languages, constraints, a plugin, a course,
  continuous checking.

## Where to go next

:::cards
- **[The element index](/reference/element-index)** {icon="book-open"}
  Every element and attribute in the tutorial, with the stage that
  introduced it.

- **[Branches](/reference/branches)** {icon="git-branch"}
  Every stage branch, the `tutorial/final` tag, and how to compare any
  two stages.

- **[Compare 24 to 25 on GitHub](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/24-house-rules...tutorial/25-final)** {icon="github"}
  Exactly what this stage added.
:::
