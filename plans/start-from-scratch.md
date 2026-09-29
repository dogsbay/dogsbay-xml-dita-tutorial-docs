# Plan: start the tutorial from an empty folder

Written 2026-09-29. Follows the editor-consistency work (`plans/editor-consistency.md`) and the narrated videos (`dogsbay-promo/plans/voiceover.md`).

## Goal

A beginner starts with an empty folder called `my-audacity-guide` and builds the guide in DogsBay XML, one stage at a time. They need no clone, no Git and no command line. Project settings arrive when a stage first needs them. The sample project on GitHub, with one branch per stage, is a reference: readers compare their files with it, copy files they cannot type, or jump to a checkpoint.

## Decisions

| Question | Decision | Why |
|---|---|---|
| Starting point | An empty folder, `my-audacity-guide`, opened in the editor | How people really start; the name does not clash with a clone of the sample project |
| Stage 01 | A `topics` folder and one file in it; no project settings | A first topic should not need configuration. The folder hierarchy is taught, not the tooling |
| Project settings | The editor writes the base config (type, framework, format style) when the first topic is created; the rest is set in the editor when first needed: root map (02), deliverables (03), metadata policy (13), house rules (25) | Each setting is explained when it has a purpose |
| Formatting | The project declares its format style, including `newline="lf"`, in `.dogsbay/config.xml`, which the editor writes when the first topic is created | An undeclared project deliberately keeps each file's line endings; a declared style is what a real project carries, and the lessons can explain why the listings look as they do |
| Git | Recommended, not required: one repository, and a commit or branch per stage | Lets readers compare and roll back; a beginner can skip it |
| The sample project | Mentioned in set-up as a resource: compare, copy, or start from a checkpoint | Keeps the branches useful without making them the starting point |
| Bookkeeping | No README "You are on" line, no `.gitkeep`, no LICENSE or NOTICE, no `scripts/` in the reader's project | They keep a clone identical to a branch and teach nothing about DITA |
| Paths in recorded output | `/home/you/my-audacity-guide` | Matches the reader's folder |

## What the editor does today (survey of dogsbay-xml `d3564ad`)

- **Opening a folder:** **File** > **Open Project Folder...** accepts an empty folder (the chooser can create one). An empty folder is not detected as DITA and nothing is written to it. **File** > **New Project** registers a name and folder in the user's settings and writes nothing to the folder either; its type defaults to None.
- **Format style:** the built-in default equals the tutorial's style, attribute for attribute. With no `<format-style>`, the project does not declare its line endings, so the formatter leaves existing CRLF files alone.
- **Root map and house rules:** **Project** > **Manage Projects...**, settings panel: "Default Root Map" and "House Rules (Schematron)", written into `.dogsbay/config.xml` in place.
- **Metadata policy:** **Project** > **Metadata** > **Edit Policy...**, a table of rules, saved to `config.xml`.
- **Deliverables:** **Project** > **Project Tools** > **Manage Deliverables...** creates `project.json` if there is none. The deliverable dialog has input map, one DITAVAL, transtype, output, and a publication parameters table (for `nav-toc`, `args.draft`). One DITAVAL is enough: no deliverable on `tutorial/26-final` has more than one.
- **New files:** Explorer **New File** and **New Folder**. `.ditaval` offers a **DITAVAL** template and `.sch` a **Schematron Rules** template. Non-XML files (`project.json`, `.py`) are created empty.
- **Bulk edits:** metadata (**Project** > **Metadata** > **Normalize...**), profiling values (**Refactor** > **Rename Profiling Value...**), **Format Project**. There is no bulk "set an attribute on many files".
- **Git:** the Git panel commits, branches and checks out remote branches (the status-bar switcher creates tracking branches). There is no **git init**.

## Editor changes needed

| Change | Needed by | Why |
|---|---|---|
| Creating the first `.dita` or `.ditamap` file in a folder that is not yet a project makes it a DITA project: `ProjectAutoConfigurer` runs on that first file (not on opening an empty folder, which is evidence of nothing), without requiring a root map, and writes `.dogsbay/config.xml` with the project type, the bundled framework, and the format style including `newline="lf"` | Stage 01 | Validate and **Check Project** need a DITA project, and the listings need a declared style. The editor keeps undeclared projects' line endings on purpose (`HeadlessProcessingCommands.java:329`), so the style is declared rather than assumed |
| Confirm **Check Project** in that new project, with no root map, reports the orphan and "no deliverables yet" as today | Stage 01 | The first check happens before the root map exists |
| **Initialize Repository** in the Git panel for a folder with no repository, like the sample project's `Git.init()` but without an automatic first commit, offering a `.gitignore` with `.dogsbay/temp/` | Set-up (optional Git) | The reader's first commit should be their own. Stage 03 adds `out/` to `.gitignore` when builds start |
| A bulk "set attribute" over a set of files: `edit-structure`'s set-attribute applied across a FileSet (map, folder or glob), in the **Refactor** menu and the CLI, with a dry run and `--only-if-absent`. It splices text at element boundaries, so each file keeps its formatting and line endings | Stage 24 | About 35 files get `xml:lang`; a topic that already says `de` keeps it |
| Optional: "Copy from sample project" for a file or folder at a stage (images, samples) | Stages 08, 21, 22 | Readers cannot type PNGs; today they would download from GitHub |

## Changes to the lessons

### Start here

- **Set up your tools:** install the editor; create and open `my-audacity-guide`; optional Git (initialize, then commit or branch per stage). Replace "Clone the repository" with "The sample project": what it is, where it is on GitHub, and how to compare, copy or start from a checkpoint (**Open Sample Project** on the welcome screen, or clone and use the branch switcher).
- **How the tutorial works:** stages build one project; each lesson names the checkpoint branch that matches its end.
- **Check your work:** paths become `my-audacity-guide`; before stage 02 there is no root map, before stage 03 no deliverables.

### Stages

| Stage | Change |
|---|---|
| 00 | Becomes "Create your project folder": open an empty `my-audacity-guide` in the editor, create `topics` with **New Folder**, optionally initialize Git. No config, no README, no LICENSE, no scripts |
| 01 | New File in `topics`, as now; the editor now writes `.dogsbay/config.xml`, and the lesson says what is in it and why (format style, line endings). Drop the README and `.gitkeep` steps. Check output re-recorded |
| 02 | Set the root map in **Manage Projects** (settings panel), and show what it writes to `.dogsbay/config.xml` |
| 03 | Add `out/` to `.gitignore` (if using Git). Create the `full` deliverable in **Manage Deliverables** (map, html5, output `out/full`, parameter `nav-toc` = `full`) instead of typing `project.json`; show the resulting file |
| 08, 21 | Images: copy from the sample project (link to the files on GitHub, or **Open Sample Project**) |
| 13 | Metadata policy in **Project** > **Metadata** > **Edit Policy...** |
| 14, 16 | DITAVAL files from the **DITAVAL** template; deliverables with filters in **Manage Deliverables** |
| 22 | `samples/export-mp3.py`: copy from the sample project, or create an empty file and paste |
| 24 | `xml:lang` with the new bulk action instead of editing each file |
| 25 | `house-style.sch` from the **Schematron Rules** template; house rules set in **Manage Projects**. `AGENTS.md` and the agent skill become optional, copied from the sample project |
| All | Remove "Update the README" and `.gitkeep` steps; move the "You are on" convention to a maintainers' note. Paths in recorded output become `my-audacity-guide`. Each lesson ends with its checkpoint branch and an optional "commit your work" |

## The stage branches

The branches stay the reference, and the excerpt checker keeps guarding every quoted listing. They must match what a reader following the lessons produces:

- `.dogsbay/config.xml` on each branch is rebuilt from what the editor writes: none at stage 00, the base config (type, framework, format style) from stage 01, then only the elements the lessons set.
- `project.json` from stage 03 is compared with what **Manage Deliverables** writes (key order, `context.id`, parameters). Either the editor writes the branch's shape or the branches take the editor's.
- README, LICENSE, NOTICE and `scripts/` stay on the branches for maintainers and GitHub visitors. The lessons no longer quote them, and "reader's project equals the branch" is checked for the files the lessons create, not the whole tree.

## Videos

The narrated videos (`dogsbay-promo/xml/voice`) record in `/tmp/my-audacity-guide`. Stage 00 starts from an empty folder; each later stage starts from the previous checkpoint. The README and `.gitkeep` cues are already gone from the stage 01 script.

## Verification before rewriting

1. In the editor as it is: open an empty folder, add `topics/what-is-audacity.dita` from the Concept template, then **Validate** and **Check Project**. Record what works and what needs the first editor change.
2. **Manage Deliverables**: create `full` as in stage 03 and diff the result with the branch's `project.json`. Repeat for a filtered deliverable from stage 14.
3. **Manage Projects**: set the root map and house rules; diff `config.xml` with the branches.
4. **Edit Policy**: enter stage 13's three rules; diff.

## Phases

| Phase | What | Depends on |
|---|---|---|
| 1 | Verification above | Nothing (editor requests agreed with the xml team, 2026-09-29) |
| 2 | Start-here pages and stages 00 to 03 rewritten; branches' `config.xml` and `project.json` aligned from stage 00 | Editor: first topic makes a DITA project and writes the config |
| 3 | Stages 04 to 26: bookkeeping removed, settings through the editor, assets from the sample project, paths re-recorded | Editor: bulk attribute action (stage 24), Git init |
| 4 | Videos re-recorded from an empty folder | Phases 2 and 3 |

## Tests

- `dogsbay site check --source --strict` and the excerpt check, as now.
- A from-scratch run: follow stages 00 to 26 in a new empty folder in the editor (scripted, like the videos), and after each stage diff the files the lesson created with the checkpoint branch.
- `dogsbay-xml check` Ready (or clean before stage 03) after every stage of that run.
