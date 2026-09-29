---
title: Element index
description: Every DITA element and attribute in the tutorial, with the stage that introduces it, from stage 00 to stage 26.
type: reference
---

# Element index

The stage that introduces each element or attribute. Look an element up here,
then read the stage page for what it is for and where it may appear. All 27
stages are written, so every row links to its page.

## Part 1: Topics

| Element or attribute | Stage |
|---|---|
| DOCTYPE and the DITA-OT catalog | [00](/part-1-topics/stage-00-setup) |
| Project settings (project type, framework, format style), kept by the editor | [00](/part-1-topics/stage-00-setup) |
| `<concept>`, `<conbody>` | [01](/part-1-topics/stage-01-concept) |
| `@id` on a topic | [01](/part-1-topics/stage-01-concept) |
| `<title>`, `<shortdesc>` | [01](/part-1-topics/stage-01-concept) |
| `<p>`, `<ul>`, `<li>` | [01](/part-1-topics/stage-01-concept) |
| `<section>` | [01](/part-1-topics/stage-01-concept) |
| `<task>`, `<taskbody>` | [04](/part-1-topics/stage-04-task-and-reference) |
| `<context>`, `<steps>`, `<step>`, `<cmd>`, `<info>`, `<result>` | [04](/part-1-topics/stage-04-task-and-reference) |
| `<reference>`, `<refbody>` | [04](/part-1-topics/stage-04-task-and-reference) |
| `<table>`, `<tgroup>`, `<colspec>`, `<thead>`, `<tbody>`, `<row>`, `<entry>` | [04](/part-1-topics/stage-04-task-and-reference) |
| `<uicontrol>`, `<menucascade>`, `<shortcut>`, `<wintitle>`, `<filepath>` | [05](/part-1-topics/stage-05-inline-and-block) |
| `<term>`, `<ph>`, `<cmdname>`, `<b>`, `<i>` | [05](/part-1-topics/stage-05-inline-and-block) |
| `<note>` and `@type` (`tip`, `important`, `warning`, `caution`) | [05](/part-1-topics/stage-05-inline-and-block) |
| `<dl>`, `<dlentry>`, `<dt>`, `<dd>` | [05](/part-1-topics/stage-05-inline-and-block) |
| `<ol>`, `<sl>`, `<sli>` | [05](/part-1-topics/stage-05-inline-and-block) |
| `<lq>`, `<fn>` | [05](/part-1-topics/stage-05-inline-and-block) |
| `<simpletable>`, `<sthead>`, `<strow>`, `<stentry>` | [05](/part-1-topics/stage-05-inline-and-block) |
| `<codeblock>`, `<pre>` | [05](/part-1-topics/stage-05-inline-and-block) |
| `<prereq>`, `<postreq>` | [06](/part-1-topics/stage-06-rich-tasks) |
| `<substeps>`, `<substep>` | [06](/part-1-topics/stage-06-rich-tasks) |
| `<choices>`, `<choice>` | [06](/part-1-topics/stage-06-rich-tasks) |
| `<choicetable>`, `<chhead>`, `<choptionhd>`, `<chdeschd>`, `<chrow>`, `<choption>`, `<chdesc>` | [06](/part-1-topics/stage-06-rich-tasks) |
| `<stepxmp>`, `<stepresult>`, `<steptroubleshooting>` | [06](/part-1-topics/stage-06-rich-tasks) |
| `<steps-unordered>`, `<tutorialinfo>` | [06](/part-1-topics/stage-06-rich-tasks) |
| `<example>`, `<tasktroubleshooting>` | [06](/part-1-topics/stage-06-rich-tasks) |
| `@importance="optional"` | [06](/part-1-topics/stage-06-rich-tasks) |
| `<codeph>` | [06](/part-1-topics/stage-06-rich-tasks) |
| `<xref>` (local, `file#topic/element`, `@scope="external"`, `@format`) | [07](/part-1-topics/stage-07-links) |
| `@id` on a `<section>` as a link target | [07](/part-1-topics/stage-07-links) |
| `<related-links>`, `<link>`, `<linktext>`, `<desc>` | [07](/part-1-topics/stage-07-links) |
| `<linklist>`, `<linkpool>` | [07](/part-1-topics/stage-07-links) |
| `<fig>`, `<image>` (`@href`, `@scale`, `@placement`), `<alt>` | [08](/part-1-topics/stage-08-figures) |
| `<svg-container>` and the `svg:` prefix | [08](/part-1-topics/stage-08-figures) |
| `<equation-block>`, `<equation-inline>`, `<mathml>` | [08](/part-1-topics/stage-08-figures) |
| `<imagemap>`, `<area>`, `<shape>`, `<coords>` | [08](/part-1-topics/stage-08-figures) |

## Part 2: Maps and publishing

| Element or attribute | Stage |
|---|---|
| `<map>` and its DOCTYPE, `<title>` of a map | [02](/part-1-topics/stage-02-first-map) |
| `<topicref>` with `@href` | [02](/part-1-topics/stage-02-first-map) |
| `<topichead>` with `@navtitle` | [09](/part-2-maps/stage-09-map-structure) |
| `<topicmeta>` with `<shortdesc>` | [09](/part-2-maps/stage-09-map-structure) |
| **Manage Deliverables**: a deliverable, **Transtype** `html5`, **Publication parameters** (`nav-toc`) | [03](/part-1-topics/stage-03-first-build) |
| **Manage Projects**: **Default Root Map** | [02](/part-1-topics/stage-02-first-map) |
| The active deliverable (**Set active**, or the status bar) | [03](/part-1-topics/stage-03-first-build) |
| nested `<topicref>` (parent and child) | [09](/part-2-maps/stage-09-map-structure) |
| `@collection-type` (`family`, `sequence`) | [09](/part-2-maps/stage-09-map-structure) |
| `<topicgroup>` | [09](/part-2-maps/stage-09-map-structure) |
| `@linking="targetonly"` | [09](/part-2-maps/stage-09-map-structure) |
| `@id` on a `<topichead>` | [09](/part-2-maps/stage-09-map-structure) |
| `@locktitle="yes"` with `<navtitle>` in `<topicmeta>` | [09](/part-2-maps/stage-09-map-structure) |
| `<reltable>`, `<relheader>`, `<relcolspec type>`, `<relrow>`, `<relcell>` | [09](/part-2-maps/stage-09-map-structure) |
| `<keydef>` with `@keys`; `<keywords>`, `<keyword>` in `<topicmeta>` | [10](/part-2-maps/stage-10-keys) |
| `<keydef href scope="external" format="html">` with `<linktext>` | [10](/part-2-maps/stage-10-keys) |
| `<mapref>` | [10](/part-2-maps/stage-10-keys) |
| `@keys` on a `<topicref>`; `<keydef keys href>` for a topic | [10](/part-2-maps/stage-10-keys) |
| `<keyword keyref>` | [10](/part-2-maps/stage-10-keys) |
| `<xref keyref>`, `<link keyref>`, `key/element-id` | [10](/part-2-maps/stage-10-keys) |
| `dogsbay-xml keys <rootmap>` | [10](/part-2-maps/stage-10-keys) |
| shared topic topics: `<topic>`, `<body>`; `@id` on reusable elements | [11](/part-2-maps/stage-11-reuse) |
| `@conref="file#topic/id"` on `<step>`, `<note>`, `<li>` | [11](/part-2-maps/stage-11-reuse) |
| `@conkeyref="key/id"` | [11](/part-2-maps/stage-11-reuse) |
| `@conrefend` (a conref range) | [11](/part-2-maps/stage-11-reuse) |
| conref push: `@conaction` (`pushbefore`, `mark`; also `pushafter`, `pushreplace`) | [11](/part-2-maps/stage-11-reuse) |
| `@processing-role="resource-only"` | [11](/part-2-maps/stage-11-reuse) |
| the `<cmd/>` placeholder in a conref'd `<step>` | [11](/part-2-maps/stage-11-reuse) |
| `<glossentry>` and its DOCTYPE, `<glossterm>`, `<glossdef>` | [12](/part-2-maps/stage-12-glossary) |
| `<glossgroup>` and its DOCTYPE | [12](/part-2-maps/stage-12-glossary) |
| `<glossBody>`, `<glossSurfaceForm>`, `<glossAlt>`, `<glossAbbreviation>`, `<glossSynonym>` | [12](/part-2-maps/stage-12-glossary) |
| `<keydef>` to an element inside a group, `file#id` | [12](/part-2-maps/stage-12-glossary) |
| `<term keyref>`, `<abbreviated-form keyref>` | [12](/part-2-maps/stage-12-glossary) |
| `<prolog>` and the order of its children | [13](/part-2-maps/stage-13-metadata-and-index) |
| `<author type>`, `<copyright>`, `<copyryear>`, `<copyrholder>` | [13](/part-2-maps/stage-13-metadata-and-index) |
| `<critdates>`, `<created date>`, `<revised modified>` | [13](/part-2-maps/stage-13-metadata-and-index) |
| `<metadata>`, `<audience type experiencelevel>`, `<category>`, `<keywords>`, `<keyword>` | [13](/part-2-maps/stage-13-metadata-and-index) |
| `<indexterm>` (nested), `<index-see>`, `<index-see-also>`, `<index-sort-as>` | [13](/part-2-maps/stage-13-metadata-and-index) |
| `<resourceid>`, `<data>` | [13](/part-2-maps/stage-13-metadata-and-index) |
| map `<topicmeta>`: `<author>`, `<publisher>`, `<copyright>`, `<critdates>`, `<audience>`, `<keywords>` | [13](/part-2-maps/stage-13-metadata-and-index) |
| **Metadata** > **Edit Policy**: rules by topic type, field, presence, and pattern | [13](/part-2-maps/stage-13-metadata-and-index) |

## Part 3: Conditions and variants

| Element or attribute | Stage |
|---|---|
| `@platform`, `@audience` on `<p>`, `<section>`, `<note>`, `<chrow>`, `<ph>`; multiple values; the other profiling attributes `@product`, `@otherprops`, `@props` | [14](/part-3-conditions/stage-14-conditional-text) |
| two `<ph platform>` alternatives for text that differs | [14](/part-3-conditions/stage-14-conditional-text) |
| `@rev` on a `<cmd>` | [14](/part-3-conditions/stage-14-conditional-text) |
| `@audience` on a `<topichead>` (filtering a map branch) | [14](/part-3-conditions/stage-14-conditional-text) |
| DITAVAL: `<val>`, `<prop att val action>` (`include`, `exclude`, `flag`), `@backcolor`, `@color` | [14](/part-3-conditions/stage-14-conditional-text) |
| DITAVAL: `<revprop val action changebar>`, `<startflag>`, `<endflag>`, `<alt-text>`, `<style-conflict>` | [14](/part-3-conditions/stage-14-conditional-text) |
| **Manage Deliverables**: the **DITAVAL** field, one deliverable per DITAVAL | [14](/part-3-conditions/stage-14-conditional-text) |
| `<subjectScheme>` and its DOCTYPE; nested `<subjectdef keys>` with `<navtitle>` | [15](/part-3-conditions/stage-15-subject-scheme) |
| `<enumerationdef>`, `<attributedef name>`, `<subjectdef keyref>` | [15](/part-3-conditions/stage-15-subject-scheme) |
| `<hasNarrower>` | [15](/part-3-conditions/stage-15-subject-scheme) |
| `<mapref type="subjectScheme">` | [15](/part-3-conditions/stage-15-subject-scheme) |
| `dogsbay-xml list-subjects`, `dogsbay-xml validate-conditions .`, **Check Project**; DITA-OT `DOTJ049W` | [15](/part-3-conditions/stage-15-subject-scheme) |
| `<ditavalref href>` under a `<topicref>` (one filters, several copy) | [16](/part-3-conditions/stage-16-branch-filtering) |
| `<ditavalmeta>`, `<dvrResourcePrefix>`, `<dvrResourceSuffix>`, `<dvrKeyscopePrefix>` | [16](/part-3-conditions/stage-16-branch-filtering) |
| `dogsbay-xml list-branches <map>` | [16](/part-3-conditions/stage-16-branch-filtering) |
| `@keyscope` on a `<mapref>`; `scope.key` names; root-scope keys | [17](/part-3-conditions/stage-17-key-scopes) |
| `<mapref scope="peer" keyscope="…">` | [17](/part-3-conditions/stage-17-key-scopes) |
| `dogsbay-xml keys <map> --resolve <key> --scope <scope>` | [17](/part-3-conditions/stage-17-key-scopes) |
| `@chunk="to-content"` on a parent `<topicref>` | [18](/part-3-conditions/stage-18-chunking-and-output) |
| a `<reference>` nested in a `<reference>`; `@chunk="by-topic"`; `<dita>` (the composite root, and why not) | [18](/part-3-conditions/stage-18-chunking-and-output) |
| `@copy-to` | [18](/part-3-conditions/stage-18-chunking-and-output) |
| `@print="printonly"`, `@toc="no"`, `@search="no"`; a generic `<topic>` as front matter | [18](/part-3-conditions/stage-18-chunking-and-output) |
| `<topicset id navtitle>`, `<topicsetref href="map#id">` | [18](/part-3-conditions/stage-18-chunking-and-output) |
| `@outputclass` on a `<table>` | [18](/part-3-conditions/stage-18-chunking-and-output) |
| `dogsbay-xml health` (unused keys on their own) | [18](/part-3-conditions/stage-18-chunking-and-output) |

## Part 4: Books and specialized topics

| Element or attribute | Stage |
|---|---|
| `<bookmap>` and its DOCTYPE; `<booktitle>`, `<mainbooktitle>`, `<booktitlealt>` | [19](/part-4-books/stage-19-bookmap) |
| `<bookmeta>`: `<publisherinformation>`, `<organization>`, `<bookid>`, `<bookpartno>`, `<edition>`, `<bookrights>`, `<copyrfirst>`, `<year>`, `<bookowner>` | [19](/part-4-books/stage-19-bookmap) |
| `<frontmatter>` (keydefs, maprefs and resource-only refs inside it), `<booklists>`, `<toc>`, `<figurelist>`, `<tablelist>`, `<notices>`, `<preface>` | [19](/part-4-books/stage-19-bookmap) |
| `<part>`, `<chapter>`, `<appendix>`, `<backmatter>`, `<glossarylist>`, `<indexlist>` | [19](/part-4-books/stage-19-bookmap) |
| **Manage Deliverables**: a deliverable with the `pdf` transtype | [19](/part-4-books/stage-19-bookmap) |
| `<troubleshooting>` and its DOCTYPE, `<troublebody>`, `<condition>`, `<troubleSolution>`, `<cause>`, `<remedy>` | [20](/part-4-books/stage-20-troubleshooting) |
| `<steps-informal>`; a profiling attribute on a `<troubleSolution>` | [20](/part-4-books/stage-20-troubleshooting) |
| `<note type="trouble">` | [20](/part-4-books/stage-20-troubleshooting) |
| `<hazardstatement type>`, `<messagepanel>`, `<typeofhazard>`, `<consequence>`, `<howtoavoid>`, `<hazardsymbol>` with `<alt>` | [21](/part-4-books/stage-21-hazards-and-safety) |
| the placeholder `<messagepanel>` in a conref'd `<hazardstatement>` | [21](/part-4-books/stage-21-hazards-and-safety) |
| `<syntaxdiagram>`, `<groupseq>`, `<kwd>`, `<delim>`, `<var>`, `<repsep>`, `<synph>` | [22](/part-4-books/stage-22-software-domains) |
| `<refsyn>`; `<parml>`, `<plentry>`, `<pt>`, `<pd>` | [22](/part-4-books/stage-22-software-domains) |
| `<cmdname>`, `<parmname>`, `<varname>`, `<option>`, `<apiname>` | [22](/part-4-books/stage-22-software-domains) |
| `<msgph>`, `<msgblock>`, `<msgnum>` | [22](/part-4-books/stage-22-software-domains) |
| `<properties>`, `<prophead>`, `<proptypehd>`, `<propvaluehd>`, `<propdeschd>`, `<property>`, `<proptype>`, `<propvalue>`, `<propdesc>` | [22](/part-4-books/stage-22-software-domains) |
| `<codeblock outputclass="language-…">` with `<coderef href>` | [22](/part-4-books/stage-22-software-domains) |
| `<screen>`, `<userinput>`, `<systemoutput>` | [22](/part-4-books/stage-22-software-domains) |
| `<learningAssessment>` and its DOCTYPE, `<learningAssessmentbody>`, `<lcIntro>`, `<lcSummary>` | [23](/part-4-books/stage-23-learning) |
| `<lcObjectives>`, `<lcObjectivesStem>`, `<lcObjectivesGroup>`, `<lcObjective>`; `<lcDuration>`, `<lcTime value>` | [23](/part-4-books/stage-23-learning) |
| `<lcInteraction>`; `<lcTrueFalse2>`, `<lcSingleSelect2>`, `<lcMultipleSelect2>`, `<lcQuestion2>`, `<lcAnswerOptionGroup2>`, `<lcAnswerOption2>`, `<lcAnswerContent2>`, `<lcCorrectResponse2>`, `<lcFeedback2>` | [23](/part-4-books/stage-23-learning) |
| `<lcSequencing2>`, `<lcSequenceOptionGroup2>`, `<lcSequenceOption2>`, `<lcSequence2 value>` | [23](/part-4-books/stage-23-learning) |
| `dogsbay-xml validate` on a learning topic, with no catalog option | [23](/part-4-books/stage-23-learning) |

## Part 5: Governance and the finish

| Element or attribute | Stage |
|---|---|
| `@xml:lang` on every root and on a `<ph>`; `@dir="rtl"` | [24](/part-5-governance/stage-24-drafts-and-localization) |
| `@translate="no"` on a `<keyword>`, `<parml>`, `<msgblock>`, `<codeblock>`, `<screen>` | [24](/part-5-governance/stage-24-drafts-and-localization) |
| `<sort-as value>` in a `<glossterm>`; `<index-see>` for an abbreviation | [24](/part-5-governance/stage-24-drafts-and-localization) |
| `<draft-comment author time disposition>`, `<required-cleanup remap>`; DITA-OT `args.draft` | [24](/part-5-governance/stage-24-drafts-and-localization) |
| `@status` on a topic and on a `<row>`; `<revised modified>`; `<change-historylist>`, `<change-item>`, `<change-person>`, `<change-completed>`, `<change-summary>` | [24](/part-5-governance/stage-24-drafts-and-localization) |
| `project-health`'s *Authoring leftovers* warnings | [24](/part-5-governance/stage-24-drafts-and-localization) |
| Schematron `house-style.sch`: `<schema>`, `<pattern>`, `<rule context>`, `<assert test>`, `<report test>` | [25](/part-5-governance/stage-25-house-rules) |
| **Manage Projects**: **House Rules (Schematron)**; `project-health --include=schematron`; the house rules in **Check Project** | [25](/part-5-governance/stage-25-house-rules) |
| `AGENTS.md`; a skill's `SKILL.md` with `name` and `description` front matter | [25](/part-5-governance/stage-25-house-rules) |
| `<cite>` for the title of a work; `<simpletable>` in place of a `<required-cleanup remap="table">` | [25](/part-5-governance/stage-25-house-rules) |
| Nothing new: the README's stage table and the eight deliverables, the `tutorial/final` tag | [26](/part-5-governance/stage-26-final) |
