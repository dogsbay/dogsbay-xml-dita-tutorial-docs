---
title: Verify excerpts and output links
description: Check lesson excerpts against stage branches and detect missing pages, fragment targets, and assets across built HTML deliverables.
type: how-to
---

# Verify excerpts and output links

The tutorial repository's `main` branch supplies checks for documentation
drift and generated links. Run them alongside the stage gate. They require
Python 3.9 or later and Git; they use only the Python standard library.

Keep a clone of `main` for these scripts if you are working in a stage
worktree. Historical stage branches do not include the new scripts.

## Check quoted source

From the tutorial repository on `main`, with the documentation repository
beside it, run:

```bash
bash scripts/check-excerpts.sh ../dogsbay-xml-dita-tutorial-docs
```

In a fresh clone where stage refs are remote-tracking branches, use:

```bash
bash scripts/check-excerpts.sh ../dogsbay-xml-dita-tutorial-docs --ref-prefix origin/tutorial/
```

The check compares every titled file listing on a stage page with that
stage's file. For a diff, it checks both sides of each hunk against the
preceding and current stages, including line counts and offsets. It also
checks the gate-script listing on the gate reference page against stage 26.
Missing refs, missing files, malformed hunks, and changed text fail the check.

The check covers copied stage files. Untitled examples, recorded terminal
output, and independent exercise snippets need separate verification.
Run this check after changing a stage branch or its quoted documentation.

## Check all generated HTML deliverables

First run the stage gate with builds enabled. Note the directory printed by
its build step. Replace the example below with that directory's `out/` path:

```bash
python3 scripts/check-output-links.py /tmp/check-stage-12345/out
```

If you built directly with `dita --project=project.json`, pass the project's
`out/` directory. Pass the common parent of all deliverables so that links
between them can resolve.

The script checks local `href` and `src` targets, HTML fragment IDs, named
anchors, and duplicate IDs. It reports missing pages and assets, paths that
escape the supplied build directory, and unsupported HTML `base` elements.
An empty directory fails. A passing run exits with code `0`; detected
problems exit with code `1`.

External URLs, PDF fragments, JavaScript navigation, CSS URLs, `srcset`, and
accessibility behavior are outside this check. Inspect them separately.
Root-relative URLs are resolved from the directory you supply; use a
browser-based crawler for output whose server routing changes that meaning.

## Check the reference stages

From `main`, `scripts/check-all-stages.sh` now checks output links after each
successful stage build. For example:

```bash
bash scripts/check-all-stages.sh 07 14 25
```

This runner uses local `tutorial/*` branches. Create those tracking branches
or use your maintainer clone before running it. `SKIP_BUILD=1` also skips
output-link checks. A reference stage can pass its historical gate and fail
the new output check; investigate the reported output rather than suppressing
the finding.

The final reference build currently has 21 reported failures across 178 HTML
pages. See [Known output issues](/reference/known-output-issues) for the
affected deliverables and investigation points.

For checker regression tests, run `python3 -B scripts/test_checks.py`.

## Review the result

For a missing fragment, compare the source reference with the generated
target's ID. For a missing variant page, inspect the branch filter's naming
settings. Fix a source example on its stage branch, update subsequent
branches as needed, then regenerate the quoted documentation and rerun the
checks. Avoid editing generated HTML to conceal a source or processor issue.
