# Plan: make the lessons consistent with the editor and the watch-along videos

Written 2026-09-28. Source: the dry run of all 27 stages on 2026-09-27, recorded in the shared doc "DITA tutorial dry run: issues and improvements", and the stage 01 recordings made in the real editor for the promo work (`dogsbay-promo/plans/xml-promo.md`).

## Goal

A reader following a lesson in DogsBay XML, or watching its video, sees the same steps, dialogs and results that the page describes. Readers check their work with the editor and the `dogsbay-xml` CLI, not with the tutorial's own scripts.

## Phases

The style guide keeps file listings identical to the stage branches, and several lesson changes depend on editor features. So the work runs in three phases.

| Phase | What | Depends on |
|---|---|---|
| 1 | Wrong claims, start-here fixes, stage 01 from the minimal template, editor steps that already work, and lesson gaps that need no source change | Nothing. This branch. |
| 2 | Replace the gate script with a **Check Project** command; re-record every check output; templates for maps and other file kinds; set up with the Project Manager and branch switcher; stage 18 rewrite | The editor changes below |
| 3 | Source fixes: "Ctrl+ACmd+A", the shared select-region step, `notice` in the house rule, `xml:lang` on `effect-presets.dita`, formatting of `examples/chunking` | The tutorial branches first (`scripts/rebase-stages.sh`), then this repo's listings |

## Editor changes needed

"Blocks" means the lessons cannot describe the editor way until the change exists.

| Change | Blocks | Why |
|---|---|---|
| A **Check Project** command (validate, health, build, check built output), in the editor and as `dogsbay-xml check` | Phase 2 | Replaces `scripts/check-stage.sh` in every lesson |
| An output link check after a build: missing pages, fragments and images | Phase 2 | Replaces `scripts/check-output-links.py` |
| Templates for map, keydef map, bookmap, DITAVAL, subject scheme, generic topic, glossentry, glossgroup, troubleshooting and Schematron; the list filtered by file extension; readable names | Phase 2 | Stages 02 onwards create these files. Today a `.ditamap` is offered topic templates, with a concept pre-selected. |
| Blank XML Document without the `<dogsbay>` placeholder root and leading empty line | Phase 2 | The only fallback for kinds with no template |
| The Default Root Map field saves what it shows, or shows only what is saved | Phase 2 (stage 02) | It displays `audacity-guide.ditamap` while `.dogsbay/config.xml` has no `default-root-map` |
| `project-health` reports orphan topics as `health` does | Phase 2 | A check that misses "forgot to add the topic to the map" |
| The formatter re-indents continuation lines under `text-continuation="block"`, and stops double-escaping `&amp;` | No, but needed before recording | A reader's formatted file differs from the branch |
| Built-in assets refreshed on upgrade, and kept under `--settings` | No, but needed before recording | Stale templates; the home path shows in Validate messages |
| Subject-scheme containers rejected as values; scoped key checking; `project-health --include` wording | No | Lessons 15, 17, 23 and 25 carry workarounds until then |
| Minimal templates: `@id` from the file name, and a `<shortdesc/>` | No | Would shorten stage 01. The phase 1 text follows today's template and changes when this lands. |
| Confirm the status-bar branch switcher works with `origin/tutorial/NN-slug` branches | Phase 2 (setup) | Readers move between stages without git commands |

## Phase 1 checklist

- [x] Stage 01: start from **New File** and the `dita-concept-minimal` template, as in the video
- [x] Stage 02: explain `<map>` and `<topicref>`; exercise uses the gate, since `validate` does not check hrefs
- [x] Stage 05: note before `<cmd>` is allowed
- [x] Stage 06: `prereq`/`postreq` labels need `args.gen.task.lbl`
- [x] Stage 07: map hrefs were checked from stage 02; `<desc>` is a tooltip; point to the output link check for fragments
- [x] Stage 09: which links the map regenerates; relcell linking
- [x] Stage 11: recorded push diagnostic
- [x] Stage 12: `<abbreviated-form>` uses the singular surface form
- [x] Stage 13: only `<resourceid>` fails inside `<metadata>`
- [x] Stage 17: peer keys do not resolve in DITA-OT; ancestor-scope precedence
- [x] Stage 19: three appendixes
- [x] Stage 20: `list-branches` claim; xref from branch copies
- [x] Stage 22: Windows pipe claim; `<screen>` domain; sample link is a known failure
- [x] Stage 23: what the output gives away; empty `lcDuration`
- [x] Stage 24: `<created>` arrives in stage 13
- [x] Stage 25: `<simpletable>` arrives in stage 05
- [x] Start here: "You are on" line commands and example
- [x] Solutions: wording on the `<note audience>` and containers

## Tests

- `dogsbay site check --source --strict` (clean before this work).
- `bash scripts/check-excerpts.sh ../dogsbay-xml-dita-tutorial-docs` from the tutorial repository: every listing still matches its branch.
- `dogsbay site build --strict`, the Astro build, and `dogsbay site check --dist` before the branch is merged.
