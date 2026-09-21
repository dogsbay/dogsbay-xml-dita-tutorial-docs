---
title: What you will build
description: Learn DITA 1.3 one feature at a time by building an Audacity user guide, stage by stage, with a check at every step.
type: explanation
---

# What you will build

This tutorial teaches DITA 1.3 by building one documentation set from nothing:
a user guide for the Audacity audio editor. You start with an empty folder and
a single concept topic. Each stage adds one DITA feature, or one tight group of
features, on top of the previous stage, until the last stage is a complete,
publish-ready guide with maps, keys, reuse, conditions, a book and house rules.

Every stage is a git branch in the
[tutorial repository](https://github.com/dogsbay/dogsbay-xml-dita-tutorial).
The difference between two branches is the lesson, and every branch passes the
same check, so you always know when a stage is done.

## The ladder

Twenty-six stages in five parts. Stages marked *coming* are planned but their
branch and page are not written yet.

| Stage | Branch | Builds | Introduces |
|---|---|---|---|
| [00](/part-1-topics/stage-00-setup) | `tutorial/00-setup` | An empty project the tools recognise | DOCTYPEs and the DITA-OT catalog, `.dogsbay/config.xml`, the gate script |
| [01](/part-1-topics/stage-01-concept) | `tutorial/01-concept` | *What is Audacity?* | `<concept>`, `<title>`, `<shortdesc>`, `<conbody>`, `<p>`, `<ul>`, `<section>` |
| [02](/part-1-topics/stage-02-task-and-reference) | `tutorial/02-task-and-reference` | *Recording your first track*, *Supported audio formats* | `<task>`, `<steps>`, `<cmd>`, `<reference>`, CALS `<table>`; the three information types |
| [03](/part-1-topics/stage-03-inline-and-block) | `tutorial/03-inline-and-block` | *What is digital audio?*, *Trimming audio* | `<uicontrol>`, `<menucascade>`, `<term>`, `<filepath>`, `<note>`, `<dl>`, `<ol>`, `<sl>`, `<fn>`, `<simpletable>`, `<codeblock>` |
| [04](/part-1-topics/stage-04-rich-tasks) | `tutorial/04-rich-tasks` | *Installing Audacity*, *Exporting audio*, *Removing background noise*, *Preparing to record* | `<prereq>`, `<postreq>`, `<substeps>`, `<choices>`, `<choicetable>`, `<stepxmp>`, `<stepresult>`, `<steps-unordered>`, `<example>` |
| [05](/part-1-topics/stage-05-links) | `tutorial/05-links` | Topics that point at each other | `<xref>`, `<related-links>`, `<link>`, `<linklist>`, `<linkpool>`, `<linktext>`, `<desc>` |
| [06](/part-1-topics/stage-06-figures) | `tutorial/06-figures` | Illustrations, an inline SVG, an image map and an equation | `<fig>`, `<image>`, `<alt>`, `<imagemap>`, `<svg-container>`, `<equation-block>`, `<mathml>` |
| [07](/part-2-maps/stage-07-first-map) | `tutorial/07-first-map` | `audacity-guide.ditamap` and the first build | `<map>`, `<topicref>`, `<topichead>`, `<topicmeta>`; DITA-OT html5; `project.json`; `default-root-map` |
| [08](/part-2-maps/stage-08-map-structure) | `tutorial/08-map-structure` | Hierarchy, sequences and generated links | nested `<topicref>`, `@collection-type`, `@linking`, `@locktitle`, `<navtitle>`, `<topicgroup>`, `<reltable>` |
| [09](/part-2-maps/stage-09-keys) | `tutorial/09-keys` | Product variables and indirect links | `<keydef>`, `<keyword keyref>`, `@keyref`, `@keys`, `<mapref>`, `<linktext>` |
| [10](/part-2-maps/stage-10-reuse) | `tutorial/10-reuse` | The `shared/` warehouse | `@conref`, `@conrefend`, `@conkeyref`, conref push with `@conaction`, `@processing-role="resource-only"` |
| [11](/part-2-maps/stage-11-glossary) | `tutorial/11-glossary` | Glossary entries and abbreviations | `<glossentry>`, `<glossterm>`, `<glossdef>`, `<glossAlt>`, `<abbreviated-form>`, `<term keyref>`, `<glossgroup>` |
| [12](/part-2-maps/stage-12-metadata-and-index) | `tutorial/12-metadata-and-index` | Prolog metadata and a real index | `<prolog>`, `<metadata>`, `<author>`, `<critdates>`, `<audience>`, `<indexterm>`, `<index-see>`; the metadata policy |
| [13](/part-3-conditions/stage-13-conditional-text) | `tutorial/13-conditional-text` | Beginner and podcaster guides, per platform, and a review build | `@platform`, `@audience`, `@rev`, `<ph>` alternatives; DITAVAL `<prop action>`, `<revprop>`, `<startflag>`, `<style-conflict>`; `profiles.ditavals` |
| [14](/part-3-conditions/stage-14-subject-scheme) | `tutorial/14-subject-scheme` | A controlled vocabulary for the conditions | `<subjectScheme>`, `<subjectdef>`, `<enumerationdef>`, `<attributedef>`, `<hasNarrower>`, `<mapref type="subjectScheme">`; `validate-conditions` |
| [15](/part-3-conditions/stage-15-branch-filtering) | `tutorial/15-branch-filtering` | Three install variants from one topic | `<ditavalref>`, `<ditavalmeta>`, `<dvrResourcePrefix>`, `<dvrResourceSuffix>`, `<dvrKeyscopePrefix>` |
| [16](/part-3-conditions/stage-16-key-scopes) | `tutorial/16-key-scopes` | One collection of three guides | `@keyscope`, scoped keys, `<mapref scope="peer">` |
| [17](/part-3-conditions/stage-17-chunking-and-output) | `tutorial/17-chunking-and-output` | Control over pages and file names | `@chunk`, `@copy-to`, nested topics, `@print`, `@toc`, `@search`, `@outputclass`, `<topicset>`, `<topicsetref>` |
| [18](/part-4-books/stage-18-bookmap) | `tutorial/18-bookmap` | The PDF book | `<bookmap>`, `<booktitle>`, `<bookmeta>`, `<frontmatter>`, `<booklists>`, `<part>`, `<chapter>`, `<appendix>`, `<backmatter>`; the `pdf` transtype |
| [19](/part-4-books/stage-19-troubleshooting) | `tutorial/19-troubleshooting` | *The recording is silent* | `<troubleshooting>`, `<condition>`, `<troubleSolution>`, `<cause>`, `<remedy>`, `<steps-informal>`, `<note type="trouble">` |
| [20](/part-4-books/stage-20-hazards-and-safety) | `tutorial/20-hazards-and-safety` | Hearing-safety statements | `<hazardstatement>`, `<messagepanel>`, `<typeofhazard>`, `<consequence>`, `<howtoavoid>`, `<hazardsymbol>`; a conref'd hazard |
| [21](/part-4-books/stage-21-software-domains) | `tutorial/21-software-domains` | Scripting Audacity from the command line | `<coderef>`, `<screen>`, `<userinput>`, `<systemoutput>`, `<cmdname>`, `<parmname>`, `<option>`, `<parml>`, `<msgblock>`, `<syntaxdiagram>`, `<properties>` |
| [22](/part-4-books/stage-22-learning) | `tutorial/22-learning` | A "check your understanding" page | `<learningAssessment>`, `<lcObjectives>`, `<lcDuration>`, `<lcInteraction>`, `<lcTrueFalse2>`, `<lcSingleSelect2>`, `<lcMultipleSelect2>`, `<lcSequencing2>`; the gate and the L&T DTDs |
| 23 *coming* | `tutorial/23-drafts-and-localization` | Ready for review and translation | `<draft-comment>`, `<required-cleanup>`, `@status`, `@xml:lang`, `@translate`, `<sort-as>` |
| 24 *coming* | `tutorial/24-house-rules` | Rules as code | Schematron `house-style.sch`, `AGENTS.md`, the house-style skill |
| 25 *coming* | `tutorial/25-final` | The complete guide, tagged `tutorial/final` | Nothing new: every deliverable builds, the project is clean |

## How to use it

:::steps
1. **Read [How the tutorial works](/start-here/how-the-tutorial-works)**
   Five minutes on branches, diffs and the gate, so the rest makes sense.

2. **[Set up your tools](/start-here/set-up)**
   DITA-OT 4.3.5, the DogsBay XML command line, and a clone of the repository.

3. **Work through the stages in order**
   Each page tells you what to write, shows the file as it is on the branch,
   and ends with the command that checks your work. Write the files yourself;
   checking out the branch is the answer key, not the lesson.

4. **Compare when you are stuck**
   Every page links to the GitHub compare view for its stage, which shows
   exactly what changed.
:::

**Time:** about 20 minutes per stage.
**You need:** a text editor, git, and the tools on the set-up page. The
DogsBay XML editor is recommended but not required.

## Where to go next

:::cards
- **[How the tutorial works](/start-here/how-the-tutorial-works)** {icon="git-branch"}
  Branches, diffs, the orphan root and the "You are on" line.

- **[Set up your tools](/start-here/set-up)** {icon="wrench"}
  Install DITA-OT and the DogsBay XML command line.

- **[Stage 00: Set up the project](/part-1-topics/stage-00-setup)** {icon="play"}
  Start building.
:::
