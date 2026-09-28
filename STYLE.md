# Documentation style

Use the [IBM Style guidance](https://www.ibm.com/docs/en/technical-content?topic=standards-style)
for the tutorial documentation. Apply the conventions below to original
prose in `content/` and maintainer documentation. Preserve the spelling,
punctuation, and values in quoted source files and diagnostic output.

## Language

- Use US English and sentence case for headings. Preserve product names,
  interface labels, XML names, and file names exactly.
- Start lessons with the task and expected result. State prerequisites
  before the procedure.
- Address the reader as "you" when needed. Begin instructions with an
  action verb and describe the expected result separately.
- Use short, complete sentences. Include articles and avoid unexplained
  abbreviations, metaphors, and idioms.
- State the relevant fact directly. Avoid rhetorical contrasts such as
  "It is not X; it is Y," promotional claims, and filler such as "simply,"
  "obviously," "seamlessly," or "it is important to note."
- Do not use em dashes or en dashes as sentence punctuation. Use periods,
  commas, colons, or parentheses. Write ranges with "to."
- Use parallel wording in lists. Separate Markdown lists from surrounding
  paragraphs with a blank line.

The restrictions on rhetorical contrasts and dash punctuation are local
editorial preferences requested for this tutorial. They are additional to
the IBM guidance.

## Technical examples

- Format commands, paths, XML names, and attribute values as code. Use bold
  for interface labels and italic for publication or topic titles.
- Keep complete file listings and diffs identical to their stage branches.
  Do not apply prose substitutions inside them. Correct the branch first
  if a source example needs to change.
- Edit shell commands that readers run when needed for correctness. Check
  them against a fresh clone or the documented authoring workflow.
- State the working directory, required variables, and output location.
  Quote paths expanded from shell variables.
- Use `origin/tutorial/NN-slug` for fetched reference branches. Distinguish
  the author's files, local branches, remote-tracking branches, and tags.
- Identify recorded output as an example. Process IDs, generated names,
  line numbers, and timings can differ between runs.
- End deliberate-error exercises with instructions to undo the change and
  rerun the gate.
- Explain the scope and limitations of checks. Reserve claims about a
  completed build or test for results that were actually verified.

## Review and validation

Run `dogsbay site check --source --strict` from the documentation repository.
Run `dogsbay site build --strict` to regenerate the site, then run the Astro
build from `astro/`. Check generated output with `dogsbay site check --dist`.
Review the diff for accidental changes to source excerpts and diagnostics.

From the tutorial repository on `main`, run
`bash scripts/check-excerpts.sh ../dogsbay-xml-dita-tutorial-docs` to check
quoted stage files. Run `python3 scripts/check-output-links.py <build-root>`
on the common output directory after DITA builds. Document any known failures;
a successful historical stage gate does not override an output-check failure.

Quoted examples can contain British spelling or dash punctuation because
the tutorial branches use them. A search match inside an exact quotation
does not justify changing the quotation independently of its source.
