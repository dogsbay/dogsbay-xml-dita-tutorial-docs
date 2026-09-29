---
title: What you will build
description: Learn DITA 1.3 one feature at a time by building an Audacity user guide, stage by stage, with a check at every step.
type: explanation
---

# What you will build

Build a DITA 1.3 documentation set using an Audacity user guide as the
example. Start with a project folder and a concept topic. Add maps, keys,
reusable content, conditional publishing, a PDF book, and house-style checks
across 27 stages.

Choose the [core course or optional modules](/start-here/learning-path).
Start with [Plan your topics](/start-here/plan-your-topics), then publish a
one-topic HTML guide in stages 02 and 03. Each module includes
[practice exercises](/practice/exercises), and the core course ends with an
[interview-workflow capstone](/practice/capstone).

Every stage is a Git branch in the
[tutorial repository](https://github.com/dogsbay/dogsbay-xml-dita-tutorial).
Compare adjacent branches to see each lesson's changes. Check your work with
**Project** > **Check Project** in the DogsBay XML editor, or
`dogsbay-xml check` on the command line, and inspect the output.

The sample guide is teaching material adapted from the Audacity Manual.
Use the current Audacity documentation for product instructions.

## Stages

The tutorial has five parts. Each stage has a branch and a lesson page.
The maintainer's completed version is tagged `tutorial/final`.

| Stage | Branch | Builds | Introduces |
|---|---|---|---|
| [00](/part-1-topics/stage-00-setup) | `tutorial/00-setup` | An empty project the tools recognize | DOCTYPEs and the DITA-OT catalog, checking your work |
| [01](/part-1-topics/stage-01-concept) | `tutorial/01-concept` | *What is Audacity?* | `<concept>`, `<title>`, `<shortdesc>`, `<conbody>`, `<p>`, `<ul>`, `<section>` |
| [02](/part-1-topics/stage-02-first-map) | `tutorial/02-first-map` | A one-topic guide map | `<map>`, `<topicref>`, relative paths, project root |
| [03](/part-1-topics/stage-03-first-build) | `tutorial/03-first-build` | The first HTML guide | **Manage Deliverables**, DITA-OT, output inspection |
| [04](/part-1-topics/stage-04-task-and-reference) | `tutorial/04-task-and-reference` | *Recording your first track*, *Supported audio formats* | `<task>`, `<steps>`, `<cmd>`, `<reference>`, CALS `<table>`; the three information types |
| [05](/part-1-topics/stage-05-inline-and-block) | `tutorial/05-inline-and-block` | *What is digital audio?*, *Trimming audio* | `<uicontrol>`, `<menucascade>`, `<term>`, `<filepath>`, `<note>`, `<dl>`, `<ol>`, `<sl>`, `<fn>`, `<simpletable>`, `<codeblock>` |
| [06](/part-1-topics/stage-06-rich-tasks) | `tutorial/06-rich-tasks` | *Installing Audacity*, *Exporting audio*, *Removing background noise*, *Preparing to record* | `<prereq>`, `<postreq>`, `<substeps>`, `<choices>`, `<choicetable>`, `<stepxmp>`, `<stepresult>`, `<steps-unordered>`, `<example>` |
| [07](/part-1-topics/stage-07-links) | `tutorial/07-links` | Topics that point at each other | `<xref>`, `<related-links>`, `<link>`, `<linklist>`, `<linkpool>`, `<linktext>`, `<desc>` |
| [08](/part-1-topics/stage-08-figures) | `tutorial/08-figures` | Illustrations, an inline SVG, an image map and an equation | `<fig>`, `<image>`, `<alt>`, `<imagemap>`, `<svg-container>`, `<equation-block>`, `<mathml>` |
| [09](/part-2-maps/stage-09-map-structure) | `tutorial/09-map-structure` | Hierarchy, sequences and generated links | nested `<topicref>`, `@collection-type`, `@linking`, `@locktitle`, `<navtitle>`, `<topicgroup>`, `<reltable>` |
| [10](/part-2-maps/stage-10-keys) | `tutorial/10-keys` | Product variables and indirect links | `<keydef>`, `<keyword keyref>`, `@keyref`, `@keys`, `<mapref>`, `<linktext>` |
| [11](/part-2-maps/stage-11-reuse) | `tutorial/11-reuse` | Shared steps and notes | `@conref`, `@conrefend`, `@conkeyref`, conref push with `@conaction`, `@processing-role="resource-only"` |
| [12](/part-2-maps/stage-12-glossary) | `tutorial/12-glossary` | Glossary entries and abbreviations | `<glossentry>`, `<glossterm>`, `<glossdef>`, `<glossAlt>`, `<abbreviated-form>`, `<term keyref>`, `<glossgroup>` |
| [13](/part-2-maps/stage-13-metadata-and-index) | `tutorial/13-metadata-and-index` | Prolog metadata and a real index | `<prolog>`, `<metadata>`, `<author>`, `<critdates>`, `<audience>`, `<indexterm>`, `<index-see>`; the metadata policy |
| [14](/part-3-conditions/stage-14-conditional-text) | `tutorial/14-conditional-text` | Beginner and podcaster guides, per platform, and a review build | `@platform`, `@audience`, `@rev`, `<ph>` alternatives; DITAVAL `<prop action>`, `<revprop>`, `<startflag>`, `<style-conflict>`; one DITAVAL per deliverable |
| [15](/part-3-conditions/stage-15-subject-scheme) | `tutorial/15-subject-scheme` | A controlled vocabulary for the conditions | `<subjectScheme>`, `<subjectdef>`, `<enumerationdef>`, `<attributedef>`, `<hasNarrower>`, `<mapref type="subjectScheme">`; `validate-conditions` |
| [16](/part-3-conditions/stage-16-branch-filtering) | `tutorial/16-branch-filtering` | Three install variants from one topic | `<ditavalref>`, `<ditavalmeta>`, `<dvrResourcePrefix>`, `<dvrResourceSuffix>`, `<dvrKeyscopePrefix>` |
| [17](/part-3-conditions/stage-17-key-scopes) | `tutorial/17-key-scopes` | One collection of three guides | `@keyscope`, scoped keys, `<mapref scope="peer">` |
| [18](/part-3-conditions/stage-18-chunking-and-output) | `tutorial/18-chunking-and-output` | Control over pages and file names | `@chunk`, `@copy-to`, nested topics, `@print`, `@toc`, `@search`, `@outputclass`, `<topicset>`, `<topicsetref>` |
| [19](/part-4-books/stage-19-bookmap) | `tutorial/19-bookmap` | The PDF book | `<bookmap>`, `<booktitle>`, `<bookmeta>`, `<frontmatter>`, `<booklists>`, `<part>`, `<chapter>`, `<appendix>`, `<backmatter>`; the `pdf` transtype |
| [20](/part-4-books/stage-20-troubleshooting) | `tutorial/20-troubleshooting` | *The recording is silent* | `<troubleshooting>`, `<condition>`, `<troubleSolution>`, `<cause>`, `<remedy>`, `<steps-informal>`, `<note type="trouble">` |
| [21](/part-4-books/stage-21-hazards-and-safety) | `tutorial/21-hazards-and-safety` | Hearing-safety statements | `<hazardstatement>`, `<messagepanel>`, `<typeofhazard>`, `<consequence>`, `<howtoavoid>`, `<hazardsymbol>`; a conref'd hazard |
| [22](/part-4-books/stage-22-software-domains) | `tutorial/22-software-domains` | Scripting Audacity from the command line | `<coderef>`, `<screen>`, `<userinput>`, `<systemoutput>`, `<cmdname>`, `<parmname>`, `<option>`, `<parml>`, `<msgblock>`, `<syntaxdiagram>`, `<properties>` |
| [23](/part-4-books/stage-23-learning) | `tutorial/23-learning` | A "check your understanding" page | `<learningAssessment>`, `<lcObjectives>`, `<lcDuration>`, `<lcInteraction>`, `<lcTrueFalse2>`, `<lcSingleSelect2>`, `<lcMultipleSelect2>`, `<lcSequencing2>`; the check and the L&T DTDs |
| [24](/part-5-governance/stage-24-drafts-and-localization) | `tutorial/24-drafts-and-localization` | Ready for review and translation | `@xml:lang`, `@translate`, `@dir`, `<sort-as>`, `<draft-comment>`, `<required-cleanup>`, `@status`, `<revised>`, `<change-historylist>` |
| [25](/part-5-governance/stage-25-house-rules) | `tutorial/25-house-rules` | Rules as code | Schematron `house-style.sch`, **House Rules (Schematron)**, `AGENTS.md`, the house-style skill; house rules in the check |
| [26](/part-5-governance/stage-26-final) | `tutorial/26-final` | The complete guide, tagged `tutorial/final` | Nothing new: every deliverable builds, the project is clean |

## How to use it

:::steps
1. **Read [How the tutorial works](/start-here/how-the-tutorial-works)**
   Choose whether to author the files or inspect completed stages.

2. **[Set up your tools](/start-here/set-up)**
   DITA-OT 4.3.5, the DogsBay XML command line, and a clone of the repository.

3. **Follow your chosen learning path**
   Each page lists prerequisites, provides source examples, and ends with
   checks. Write the files in your authoring repository or inspect the
   corresponding reference branch. Use the checkpoint instructions when
   skipping optional modules; stage diffs assume the preceding stage's files.

4. **Compare when you are stuck**
   Every page links to the GitHub compare view for its stage, which shows
   exactly what changed.
:::

**Time:** 10 to 40 minutes per stage, plus practice exercises.
**You need:** a text editor, Git, and the tools on the set-up page. The
DogsBay XML editor is recommended but not required.

Follow the core course for topic authoring, maps, reuse, variants, and
validation. Choose optional modules when you need their publishing features.
Use the checkpoints in the learning path when skipping stages. Allow
additional time for setup, troubleshooting, and the capstone.

## Where to go next

:::cards
- **[Choose a learning path](/start-here/learning-path)** {icon="book-open"}
  The core course, optional modules, and starting checkpoints.

- **[How the tutorial works](/start-here/how-the-tutorial-works)** {icon="git-branch"}
  Branches, diffs, the orphan root and the "You are on" line.

- **[Set up your tools](/start-here/set-up)** {icon="wrench"}
  Install the DogsBay XML editor and its command line.

- **[Stage 00: Set up the project](/part-1-topics/stage-00-setup)** {icon="play"}
  Start building.
:::
