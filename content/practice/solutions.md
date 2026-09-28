---
title: Suggested exercise solutions
description: Compare your decisions and observed output with the expected results for the five DITA practice exercises.
type: reference
---

# Suggested exercise solutions

Use these checks after attempting the [practice exercises](/practice/exercises).
Several designs can meet the requirements. Explain how yours serves the
reader and verify the published result.

## Topics

A suitable task has a saved recording as a prerequisite, actions to listen
to the track, inspect unwanted audio, and confirm the selection, followed
by a result that states the recording is ready for export. Link to the
existing formats reference for format comparisons. Keep only the context
needed to complete the checking procedure in the task.

A task with one long `cmd` containing several unrelated actions needs to
be divided into steps. A list of audio-format definitions belongs in the
reference topic.

## Maps and reuse

The practice map needs the product key-definition map, a `common-notes` key,
and the shared steps as a resource-only reference. The selected topics also
use keys such as `formats`; either include the referenced topics or define
the required targets. Inspect the source references to find the dependencies.

Define the recording key on its `topicref`. A link such as
`<xref keyref="record"/>` then uses that definition. A reused step should
have the same purpose in both procedures and remain understandable in each
location. Use the existing save step when its action and result match.

The expected output contains resolved product names, working links, and a
complete save instruction. A successful build with empty link text does not
meet the exercise requirements.

## Conditions

Use `<note audience="podcaster">` around the note. The beginner filters
exclude `podcaster`; the Linux podcaster filter includes it. The subject
scheme identifies allowed values, while the DITAVAL selects or flags content
for a particular publication. A spelling error can pass the DTD because the
DTD does not contain this project's controlled vocabulary.

The scheme's `readers` and `os` keys identify containers. They are not
allowed audience or platform values in this enumeration. A tool that lists
them as selectable values needs correction.

## Published output

Report observed behavior for each format. A table can have correct source
headers but inadequate header associations in the output. An image can
have an alternative that is too vague to explain the information it
conveys. A source link can resolve to a file while missing its intended
section in the generated HTML. These findings require different fixes.

The report should include enough information for another person to repeat
the check. Do not mark an output accessible solely because it validates.

## House rules

For the concept used here, the DTD permits omission of `shortdesc`. The
project's Schematron requires it. DTD validation and house-style validation
therefore answer different questions. After restoring it, both checks
should pass and the final diff should contain no deliberate-error change.
