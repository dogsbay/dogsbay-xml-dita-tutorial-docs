# Building the DogsBay DITA tutorial site

The plan for `dogsbay-xml-dita-tutorial-docs`: the reader-facing walkthrough
of the step-by-step DITA tutorial whose stage branches live in
`dogsbay-xml-dita-tutorial`. Written 2026-09-21.

## Scope

In: one page per stage of the ladder in
`dogsbay-xml-dita-tutorial/plans/step-by-step-tutorial.md`, the pages a
reader needs before stage 00 (how the branches work, installing the tools,
running the gate), and a reference section (element-to-stage index, the gate,
branch cheatsheet).

Now: complete. The start-here pages, Part 1 (stages 00 to 06), Part 2
(stages 07 to 12), Part 3 (stages 13 to 17), Part 4 (stages 18 to 22) and
Part 5 (stages 23 to 25) are written, one page per branch, and the home
page, element index and branches reference link every stage. Remaining work
is maintenance: when a stage branch is rewritten, refresh its page's blocks
and re-run its gate output.

Out: documenting the `main` branch's deliberately broken demo (that is the
`dogsbay-xml-docs` tutorial), and anything about the DogsBay XML editor that
is not needed to follow a stage.

## Readers

- **A writer new to DITA** who knows what a topic is and wants to learn the
  language by building something real, in order, with a check at every step.
  Served by the stage pages: every file shown in full, every element explained
  in a sentence or two, the gate run at the end.
- **A DogsBay XML user** who wants to see the project features (validation,
  health, later keys, reuse, filters and deliverables) exercised on a small
  project. Served by the gate and reference pages.
- **An AI agent** pointed at a stage branch and told "do the next stage".
  Served by the same pages: the file paths, the branch names and the compare
  links are exact, and the `.md` mirror of every page is on.

## Structure

`content/nav.yml`:

1. Start here: what you will build (the home page), how the tutorial works,
   set up your tools, run the gate.
2. Parts 1 to 4: stages 00 to 22, one page each.
3. Part 5: stages 23 to 25, one page each.
4. Reference: element index, the gate, branches.

Each stage page has the same skeleton as
`dogsbay-xml-docs/content/getting-started/tutorial-agentic.md`: an H1, a
two-paragraph introduction, **Time** and **You need**, `## Step n:` headings
each with a `:::steps` block, the file to write in a fenced `xml` block titled
with its path, the command to run in a `bash` block, what the gate prints,
`## What you learned`, and `## Where to go next` as `:::cards` linking to the
next stage and to the GitHub compare URL for the stage.

## Style

As `dogsbay-xml-docs`: IBM Style Guide, Diátaxis `type:` per page (stage
pages are `tutorial`, set-up and gate pages are `how-to`, the reference pages
are `reference`, the "how it works" page is `explanation`), second person,
sentence-case headings, no "simply/just/easy". No screenshots; stage 06's
images are generated illustrations and the page says so.

Frontmatter is `title`, `description` and `type` only.

DITA markup and anything in braces stays inside inline code or code fences:
the build evaluates Jinja-style syntax outside them.

## Truth

Every file shown on a stage page is the file on that stage branch, pasted from
`git show tutorial/NN-slug:path`, not written by hand. New files are shown
whole; edited files are shown as the exact changed region or whole. The gate
output on each page is from a real run of `scripts/check-stage.sh` on an
extracted copy of the branch. Every stage has a branch now, so nothing is
listed from the plan alone.

When a stage branch changes, its page changes with it. The planned
`scripts/check-excerpts.sh` in the tutorial repo will verify that every fenced
block titled with a path matches that file on the page's stage branch.
