---
title: Element index
description: Every DITA element and attribute in the tutorial, with the stage that introduces it, from stage 00 to stage 25.
type: reference
---

# Element index

The stage that introduces each element or attribute. Look an element up here,
then read the stage page for what it is for and where it may appear. All 26
stages are written, so every row links to its page.

## Part 1: Topics

| Element or attribute | Stage |
|---|---|
| DOCTYPE and the DITA-OT catalog | [00](/part-1-topics/stage-00-setup) |
| `.dogsbay/config.xml` (project type, framework, format style) | [00](/part-1-topics/stage-00-setup) |
| `<concept>`, `<conbody>` | [01](/part-1-topics/stage-01-concept) |
| `@id` on a topic | [01](/part-1-topics/stage-01-concept) |
| `<title>`, `<shortdesc>` | [01](/part-1-topics/stage-01-concept) |
| `<p>`, `<ul>`, `<li>` | [01](/part-1-topics/stage-01-concept) |
| `<section>` | [01](/part-1-topics/stage-01-concept) |
| `<task>`, `<taskbody>` | [02](/part-1-topics/stage-02-task-and-reference) |
| `<context>`, `<steps>`, `<step>`, `<cmd>`, `<info>`, `<result>` | [02](/part-1-topics/stage-02-task-and-reference) |
| `<reference>`, `<refbody>` | [02](/part-1-topics/stage-02-task-and-reference) |
| `<table>`, `<tgroup>`, `<colspec>`, `<thead>`, `<tbody>`, `<row>`, `<entry>` | [02](/part-1-topics/stage-02-task-and-reference) |
| `<uicontrol>`, `<menucascade>`, `<shortcut>`, `<wintitle>`, `<filepath>` | [03](/part-1-topics/stage-03-inline-and-block) |
| `<term>`, `<ph>`, `<cmdname>`, `<b>`, `<i>` | [03](/part-1-topics/stage-03-inline-and-block) |
| `<note>` and `@type` (`tip`, `important`, `warning`, `caution`) | [03](/part-1-topics/stage-03-inline-and-block) |
| `<dl>`, `<dlentry>`, `<dt>`, `<dd>` | [03](/part-1-topics/stage-03-inline-and-block) |
| `<ol>`, `<sl>`, `<sli>` | [03](/part-1-topics/stage-03-inline-and-block) |
| `<lq>`, `<fn>` | [03](/part-1-topics/stage-03-inline-and-block) |
| `<simpletable>`, `<sthead>`, `<strow>`, `<stentry>` | [03](/part-1-topics/stage-03-inline-and-block) |
| `<codeblock>`, `<pre>` | [03](/part-1-topics/stage-03-inline-and-block) |
| `<prereq>`, `<postreq>` | [04](/part-1-topics/stage-04-rich-tasks) |
| `<substeps>`, `<substep>` | [04](/part-1-topics/stage-04-rich-tasks) |
| `<choices>`, `<choice>` | [04](/part-1-topics/stage-04-rich-tasks) |
| `<choicetable>`, `<chhead>`, `<choptionhd>`, `<chdeschd>`, `<chrow>`, `<choption>`, `<chdesc>` | [04](/part-1-topics/stage-04-rich-tasks) |
| `<stepxmp>`, `<stepresult>`, `<steptroubleshooting>` | [04](/part-1-topics/stage-04-rich-tasks) |
| `<steps-unordered>`, `<tutorialinfo>` | [04](/part-1-topics/stage-04-rich-tasks) |
| `<example>`, `<tasktroubleshooting>` | [04](/part-1-topics/stage-04-rich-tasks) |
| `@importance="optional"` | [04](/part-1-topics/stage-04-rich-tasks) |
| `<codeph>` | [04](/part-1-topics/stage-04-rich-tasks) |
| `<xref>` (local, `file#topic/element`, `@scope="external"`, `@format`) | [05](/part-1-topics/stage-05-links) |
| `@id` on a `<section>` as a link target | [05](/part-1-topics/stage-05-links) |
| `<related-links>`, `<link>`, `<linktext>`, `<desc>` | [05](/part-1-topics/stage-05-links) |
| `<linklist>`, `<linkpool>` | [05](/part-1-topics/stage-05-links) |
| `<fig>`, `<image>` (`@href`, `@scale`, `@placement`), `<alt>` | [06](/part-1-topics/stage-06-figures) |
| `<svg-container>` and the `svg:` prefix | [06](/part-1-topics/stage-06-figures) |
| `<equation-block>`, `<equation-inline>`, `<mathml>` | [06](/part-1-topics/stage-06-figures) |
| `<imagemap>`, `<area>`, `<shape>`, `<coords>` | [06](/part-1-topics/stage-06-figures) |

## Part 2: Maps and publishing

| Element or attribute | Stage |
|---|---|
| `<map>` and its DOCTYPE, `<title>` of a map | [07](/part-2-maps/stage-07-first-map) |
| `<topicref>` with `@href` | [07](/part-2-maps/stage-07-first-map) |
| `<topichead>` with `@navtitle` | [07](/part-2-maps/stage-07-first-map) |
| `<topicmeta>` with `<shortdesc>` | [07](/part-2-maps/stage-07-first-map) |
| `project.json`: deliverables, `transtype` `html5`, `params` (`nav-toc`) | [07](/part-2-maps/stage-07-first-map) |
| `dita --project=project.json`, `dita -i … -f html5 -o …` | [07](/part-2-maps/stage-07-first-map) |
| `.dogsbay/config.xml`: `<default-root-map>`, `<default-deliverable>` | [07](/part-2-maps/stage-07-first-map) |
| nested `<topicref>` (parent and child) | [08](/part-2-maps/stage-08-map-structure) |
| `@collection-type` (`family`, `sequence`) | [08](/part-2-maps/stage-08-map-structure) |
| `<topicgroup>` | [08](/part-2-maps/stage-08-map-structure) |
| `@linking="targetonly"` | [08](/part-2-maps/stage-08-map-structure) |
| `@id` on a `<topichead>` | [08](/part-2-maps/stage-08-map-structure) |
| `@locktitle="yes"` with `<navtitle>` in `<topicmeta>` | [08](/part-2-maps/stage-08-map-structure) |
| `<reltable>`, `<relheader>`, `<relcolspec type>`, `<relrow>`, `<relcell>` | [08](/part-2-maps/stage-08-map-structure) |
| `<keydef>` with `@keys`; `<keywords>`, `<keyword>` in `<topicmeta>` | [09](/part-2-maps/stage-09-keys) |
| `<keydef href scope="external" format="html">` with `<linktext>` | [09](/part-2-maps/stage-09-keys) |
| `<mapref>` | [09](/part-2-maps/stage-09-keys) |
| `@keys` on a `<topicref>`; `<keydef keys href>` for a topic | [09](/part-2-maps/stage-09-keys) |
| `<keyword keyref>` | [09](/part-2-maps/stage-09-keys) |
| `<xref keyref>`, `<link keyref>`, `key/element-id` | [09](/part-2-maps/stage-09-keys) |
| `dogsbay-xml keys <rootmap>` | [09](/part-2-maps/stage-09-keys) |
| warehouse topics: `<topic>`, `<body>`; `@id` on reusable elements | [10](/part-2-maps/stage-10-reuse) |
| `@conref="file#topic/id"` on `<step>`, `<note>`, `<li>` | [10](/part-2-maps/stage-10-reuse) |
| `@conkeyref="key/id"` | [10](/part-2-maps/stage-10-reuse) |
| `@conrefend` (a conref range) | [10](/part-2-maps/stage-10-reuse) |
| conref push: `@conaction` (`pushbefore`, `mark`; also `pushafter`, `pushreplace`) | [10](/part-2-maps/stage-10-reuse) |
| `@processing-role="resource-only"` | [10](/part-2-maps/stage-10-reuse) |
| the `<cmd/>` placeholder in a conref'd `<step>` | [10](/part-2-maps/stage-10-reuse) |
| `<glossentry>` and its DOCTYPE, `<glossterm>`, `<glossdef>` | [11](/part-2-maps/stage-11-glossary) |
| `<glossgroup>` and its DOCTYPE | [11](/part-2-maps/stage-11-glossary) |
| `<glossBody>`, `<glossSurfaceForm>`, `<glossAlt>`, `<glossAbbreviation>`, `<glossSynonym>` | [11](/part-2-maps/stage-11-glossary) |
| `<keydef>` to an element inside a group, `file#id` | [11](/part-2-maps/stage-11-glossary) |
| `<term keyref>`, `<abbreviated-form keyref>` | [11](/part-2-maps/stage-11-glossary) |
| `<prolog>` and the order of its children | [12](/part-2-maps/stage-12-metadata-and-index) |
| `<author type>`, `<copyright>`, `<copyryear>`, `<copyrholder>` | [12](/part-2-maps/stage-12-metadata-and-index) |
| `<critdates>`, `<created date>`, `<revised modified>` | [12](/part-2-maps/stage-12-metadata-and-index) |
| `<metadata>`, `<audience type experiencelevel>`, `<category>`, `<keywords>`, `<keyword>` | [12](/part-2-maps/stage-12-metadata-and-index) |
| `<indexterm>` (nested), `<index-see>`, `<index-see-also>`, `<index-sort-as>` | [12](/part-2-maps/stage-12-metadata-and-index) |
| `<resourceid>`, `<data>` | [12](/part-2-maps/stage-12-metadata-and-index) |
| map `<topicmeta>`: `<author>`, `<publisher>`, `<copyright>`, `<critdates>`, `<audience>`, `<keywords>` | [12](/part-2-maps/stage-12-metadata-and-index) |
| `.dogsbay/config.xml`: `<metadata-policy>`, `<rule field presence topic-type pattern>` | [12](/part-2-maps/stage-12-metadata-and-index) |

## Part 3: Conditions and variants

| Element or attribute | Stage |
|---|---|
| `@platform`, `@audience` on `<p>`, `<section>`, `<note>`, `<chrow>`, `<ph>`; multiple values; the other profiling attributes `@product`, `@otherprops`, `@props` | [13](/part-3-conditions/stage-13-conditional-text) |
| two `<ph platform>` alternatives for text that differs | [13](/part-3-conditions/stage-13-conditional-text) |
| `@rev` on a `<cmd>` | [13](/part-3-conditions/stage-13-conditional-text) |
| `@audience` on a `<topichead>` (filtering a map branch) | [13](/part-3-conditions/stage-13-conditional-text) |
| DITAVAL: `<val>`, `<prop att val action>` (`include`, `exclude`, `flag`), `@backcolor`, `@color` | [13](/part-3-conditions/stage-13-conditional-text) |
| DITAVAL: `<revprop val action changebar>`, `<startflag>`, `<endflag>`, `<alt-text>`, `<style-conflict>` | [13](/part-3-conditions/stage-13-conditional-text) |
| `project.json`: `profiles.ditavals`, one deliverable per DITAVAL | [13](/part-3-conditions/stage-13-conditional-text) |
| `<subjectScheme>` and its DOCTYPE; nested `<subjectdef keys>` with `<navtitle>` | [14](/part-3-conditions/stage-14-subject-scheme) |
| `<enumerationdef>`, `<attributedef name>`, `<subjectdef keyref>` | [14](/part-3-conditions/stage-14-subject-scheme) |
| `<hasNarrower>` | [14](/part-3-conditions/stage-14-subject-scheme) |
| `<mapref type="subjectScheme">` | [14](/part-3-conditions/stage-14-subject-scheme) |
| `dogsbay-xml list-subjects`, `dogsbay-xml validate-conditions -m <rootmap>`; DITA-OT `DOTJ049W` | [14](/part-3-conditions/stage-14-subject-scheme) |
| `<ditavalref href>` under a `<topicref>` (one filters, several copy) | [15](/part-3-conditions/stage-15-branch-filtering) |
| `<ditavalmeta>`, `<dvrResourcePrefix>`, `<dvrResourceSuffix>`, `<dvrKeyscopePrefix>` | [15](/part-3-conditions/stage-15-branch-filtering) |
| `dogsbay-xml list-branches <map>` | [15](/part-3-conditions/stage-15-branch-filtering) |
| `@keyscope` on a `<mapref>`; `scope.key` names; root-scope keys | [16](/part-3-conditions/stage-16-key-scopes) |
| `<mapref scope="peer" keyscope="…">` | [16](/part-3-conditions/stage-16-key-scopes) |
| `dogsbay-xml keys <map> --resolve <key> --scope <scope>` | [16](/part-3-conditions/stage-16-key-scopes) |
| `@chunk="to-content"` on a parent `<topicref>` | [17](/part-3-conditions/stage-17-chunking-and-output) |
| a `<reference>` nested in a `<reference>`; `@chunk="by-topic"`; `<dita>` (the composite root, and why not) | [17](/part-3-conditions/stage-17-chunking-and-output) |
| `@copy-to` | [17](/part-3-conditions/stage-17-chunking-and-output) |
| `@print="printonly"`, `@toc="no"`, `@search="no"`; a generic `<topic>` as front matter | [17](/part-3-conditions/stage-17-chunking-and-output) |
| `<topicset id navtitle>`, `<topicsetref href="map#id">` | [17](/part-3-conditions/stage-17-chunking-and-output) |
| `@outputclass` on a `<table>` | [17](/part-3-conditions/stage-17-chunking-and-output) |
| `dogsbay-xml health` (unused keys on their own) | [17](/part-3-conditions/stage-17-chunking-and-output) |

## Part 4: Books and specialised topics

| Element or attribute | Stage |
|---|---|
| `<bookmap>` and its DOCTYPE; `<booktitle>`, `<mainbooktitle>`, `<booktitlealt>` | [18](/part-4-books/stage-18-bookmap) |
| `<bookmeta>`: `<publisherinformation>`, `<organization>`, `<bookid>`, `<bookpartno>`, `<edition>`, `<bookrights>`, `<copyrfirst>`, `<year>`, `<bookowner>` | [18](/part-4-books/stage-18-bookmap) |
| `<frontmatter>` (keydefs, maprefs and resource-only refs inside it), `<booklists>`, `<toc>`, `<figurelist>`, `<tablelist>`, `<notices>`, `<preface>` | [18](/part-4-books/stage-18-bookmap) |
| `<part>`, `<chapter>`, `<appendix>`, `<backmatter>`, `<glossarylist>`, `<indexlist>` | [18](/part-4-books/stage-18-bookmap) |
| `project.json`: a deliverable with the `pdf` transtype | [18](/part-4-books/stage-18-bookmap) |
| `<troubleshooting>` and its DOCTYPE, `<troublebody>`, `<condition>`, `<troubleSolution>`, `<cause>`, `<remedy>` | [19](/part-4-books/stage-19-troubleshooting) |
| `<steps-informal>`; a profiling attribute on a `<troubleSolution>` | [19](/part-4-books/stage-19-troubleshooting) |
| `<note type="trouble">` | [19](/part-4-books/stage-19-troubleshooting) |
| `<hazardstatement type>`, `<messagepanel>`, `<typeofhazard>`, `<consequence>`, `<howtoavoid>`, `<hazardsymbol>` with `<alt>` | [20](/part-4-books/stage-20-hazards-and-safety) |
| the placeholder `<messagepanel>` in a conref'd `<hazardstatement>` | [20](/part-4-books/stage-20-hazards-and-safety) |
| `<syntaxdiagram>`, `<groupseq>`, `<kwd>`, `<delim>`, `<var>`, `<repsep>`, `<synph>` | [21](/part-4-books/stage-21-software-domains) |
| `<refsyn>`; `<parml>`, `<plentry>`, `<pt>`, `<pd>` | [21](/part-4-books/stage-21-software-domains) |
| `<cmdname>`, `<parmname>`, `<varname>`, `<option>`, `<apiname>` | [21](/part-4-books/stage-21-software-domains) |
| `<msgph>`, `<msgblock>`, `<msgnum>` | [21](/part-4-books/stage-21-software-domains) |
| `<properties>`, `<prophead>`, `<proptypehd>`, `<propvaluehd>`, `<propdeschd>`, `<property>`, `<proptype>`, `<propvalue>`, `<propdesc>` | [21](/part-4-books/stage-21-software-domains) |
| `<codeblock outputclass="language-…">` with `<coderef href>` | [21](/part-4-books/stage-21-software-domains) |
| `<screen>`, `<userinput>`, `<systemoutput>` | [21](/part-4-books/stage-21-software-domains) |
| `<learningAssessment>` and its DOCTYPE, `<learningAssessmentbody>`, `<lcIntro>`, `<lcSummary>` | [22](/part-4-books/stage-22-learning) |
| `<lcObjectives>`, `<lcObjectivesStem>`, `<lcObjectivesGroup>`, `<lcObjective>`; `<lcDuration>`, `<lcTime value>` | [22](/part-4-books/stage-22-learning) |
| `<lcInteraction>`; `<lcTrueFalse2>`, `<lcSingleSelect2>`, `<lcMultipleSelect2>`, `<lcQuestion2>`, `<lcAnswerOptionGroup2>`, `<lcAnswerOption2>`, `<lcAnswerContent2>`, `<lcCorrectResponse2>`, `<lcFeedback2>` | [22](/part-4-books/stage-22-learning) |
| `<lcSequencing2>`, `<lcSequenceOptionGroup2>`, `<lcSequenceOption2>`, `<lcSequence2 value>` | [22](/part-4-books/stage-22-learning) |
| `dogsbay-xml validate --catalog <catalog>`; `project-health --include=…` | [22](/part-4-books/stage-22-learning) |

## Part 5: Governance and the finish

| Element or attribute | Stage |
|---|---|
| `@xml:lang` on every root and on a `<ph>`; `@dir="rtl"` | [23](/part-5-governance/stage-23-drafts-and-localization) |
| `@translate="no"` on a `<keyword>`, `<parml>`, `<msgblock>`, `<codeblock>`, `<screen>` | [23](/part-5-governance/stage-23-drafts-and-localization) |
| `<sort-as value>` in a `<glossterm>`; `<index-see>` for an abbreviation | [23](/part-5-governance/stage-23-drafts-and-localization) |
| `<draft-comment author time disposition>`, `<required-cleanup remap>`; DITA-OT `args.draft` | [23](/part-5-governance/stage-23-drafts-and-localization) |
| `@status` on a topic and on a `<row>`; `<revised modified>`; `<change-historylist>`, `<change-item>`, `<change-person>`, `<change-completed>`, `<change-summary>` | [23](/part-5-governance/stage-23-drafts-and-localization) |
| `project-health`'s *Authoring leftovers* warnings | [23](/part-5-governance/stage-23-drafts-and-localization) |
| Schematron `house-style.sch`: `<schema>`, `<pattern>`, `<rule context>`, `<assert test>`, `<report test>` | [24](/part-5-governance/stage-24-house-rules) |
| `<default-schematron>` in `.dogsbay/config.xml`; `project-health --include=schematron`; the gate's *house rules* step | [24](/part-5-governance/stage-24-house-rules) |
| `AGENTS.md`; a skill's `SKILL.md` with `name` and `description` frontmatter | [24](/part-5-governance/stage-24-house-rules) |
| `<cite>` for the title of a work; `<simpletable>` in place of a `<required-cleanup remap="table">` | [24](/part-5-governance/stage-24-house-rules) |
| Nothing new: the README's stage table and deliverables, `project.json` in full, the `tutorial/final` tag | [25](/part-5-governance/stage-25-final) |
