---
title: Review accessibility
description: Review link text, image alternatives, table headers, keyboard navigation, and reading order in the published tutorial output.
type: how-to
---

# Review accessibility

Review both the DITA source and each published format. Valid XML does not
establish that the output is accessible.

## Review the source

1. Check that titles and link text identify their destination. Replace
   labels such as "here" with the subject or action readers can expect.
2. Give informative images useful text alternatives. Describe the information
   readers need, including details of a waveform or diagram that support
   the surrounding instructions. Add a nearby text explanation when a short
   alternative cannot convey the full meaning.
3. Supply text links for actions available through an image map. Readers
   should be able to reach the same destinations without selecting an area
   in the image.
4. Identify table headers with the appropriate table elements. For a
   simple table, use `sthead` for column headers and consider `keycol` for
   row headers. Avoid using tables only to position text.
5. Check that instructions and review flags remain understandable without
   color. Include a text label where color indicates a state.
6. Declare the correct language and mark language changes in the text.

## Review the output

Open the built HTML and use the keyboard to follow the navigation, topic
links, and image-map alternatives. Confirm that focus is visible and that
link destinations are correct. Check the heading order and enlarge the text
to look for clipped or overlapping content.

Read representative pages with a screen reader. Check whether image
alternatives, table headers, and link names provide enough context. The
rendered output can expose problems that are not apparent in the source.

For PDF, inspect reading order, bookmarks, text alternatives, and table
structure with a PDF accessibility tool. Record problems in the publishing
transform as well as source problems. An HTML check does not validate a PDF.

## Record the result

Record the deliverable, page, interaction, expected behavior, and observed
behavior for each issue. Recheck the affected output after a fix. This
review is a practical tutorial exercise, not a claim of conformance to an
accessibility standard.
