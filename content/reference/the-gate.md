---
title: The gate
description: Reference for scripts/check-stage.sh, the check every stage passes; its steps, switches, environment variables, exit codes, and why not xmllint.
type: reference
---

# The gate

`scripts/check-stage.sh` is the check every tutorial stage branch passes
before it is committed. It is also a one-shot health check for any DITA
project folder. [Run the gate](/start-here/run-the-gate) walks through using
it; this page is the reference.

## Steps

| Step | Command | Proves |
|---|---|---|
| 1 | `dogsbay-xml validate-project <root>` | Every `.dita` and `.ditamap` validates against its DOCTYPE. From [stage 22](/part-4-books/stage-22-learning), files under `learning/` that fail here are validated again with `dogsbay-xml validate --catalog $DITA_HOME/catalog-dita.xml`, because the editor's built-in catalog has no Learning and Training DTDs |
| 2 | `dogsbay-xml project-health <root>` | No broken links, undefined or unused keys, broken element ids, orphan conref pushes or dangling index redirects; no metadata-policy or house-rule violations. House-style formatting and authoring leftovers are warnings. When a `learning/` folder exists the check runs with an explicit `--include` list that leaves out its own grammar pass, and house rules run as a separate step once `.dogsbay/config.xml` names a `<default-schematron>` |
| 2b | `dogsbay-xml validate-conditions -S <scheme> <root>` | Every `platform` and `audience` value is one the subject scheme allows. Runs only when a subject scheme map exists (from [stage 14](/part-3-conditions/stage-14-subject-scheme)); without `-S` or `-m` the command has no scheme and passes vacuously |
| 3 | `dita --project=project.json --output=<tmp>` | Every deliverable in `project.json` builds. The log is scanned for `Error:` lines and DITA-OT `E` and `F` codes, because the build does not always exit non-zero |

Step 2 uses the root map and Schematron configured in `.dogsbay/config.xml`.
Step 1 pipes through `tee`, so the script sets `pipefail`; without it a
failing validation counted as passed.
Step 2b is skipped when no map in the project root has a
`type="subjectScheme"` mapref. Step 3 is skipped when the project has no
`project.json`.

All steps run even after one fails, so one run shows everything. The last
line is `STAGE OK` or `STAGE FAILED`.

## Usage

```bash
scripts/check-stage.sh                 # this project
scripts/check-stage.sh /path/to/stage  # another project root
SKIP_BUILD=1 scripts/check-stage.sh    # skip DITA-OT; a fast loop while authoring
STRICT=0 scripts/check-stage.sh        # project-health at --severity=error only
```

With `STRICT=1` (the default) `project-health` runs at its default severity,
so unused keys, orphans and recommended metadata are reported next to any
error. On their own, those warnings do not fail the run: a project with only
warnings reports healthy and the gate prints `STAGE OK`. The stage branches are gated with the default, after
`dogsbay-xml format -i topics/*.dita`, so formatting warnings never appear on
a branch.

## Environment

| Variable | Default | Purpose |
|---|---|---|
| `DOGSBAY_XML` | `dogsbay-xml` on `PATH`, else `~/github/dogsbay-xml/bin/dogsbay-xml` | The DogsBay XML command line |
| `DITA_HOME` | `~/Downloads/dita-ot-4.3.5` | The DITA-OT installation |
| `DITA` | `dita` on `PATH`, else `$DITA_HOME/bin/dita` | The `dita` executable directly |
| `OUT` | `$TMPDIR/check-stage-<pid>` | Where step 3 writes the build; the log is `$OUT.log` |
| `STRICT` | `1` | `0` runs `project-health --severity=error` |
| `SKIP_BUILD` | `0` | `1` skips step 3 |

## Exit codes

| Code | Meaning |
|---|---|
| `0` | Stage OK |
| `1` | A step failed |
| `2` | A tool was not found: `dogsbay-xml`, or `dita` when a build was needed |

## Output

A passing run on a Part 1 stage, where there is no map yet:

```
== validate-project  /home/you/audacity-guide ==
9 file(s): 9 valid, 0 invalid.

== project-health  /home/you/audacity-guide ==
Root map: none (none configured); house rules: none (none configured)
Project is healthy: valid, no broken references, keys, orphans, or broken element ids.

== build  skipped (no project.json yet) ==

STAGE OK
```

A validation failure names the file, line and column:

```
== validate-project  /home/you/audacity-guide ==
/home/you/audacity-guide/topics/trimming-audio.dita:
  22:170  error: The content of element type "cmd" does not match its content model.
5 file(s): 4 valid, 1 invalid.
FAIL: validation errors
```

A build failure prints the first forty distinct error lines from the DITA-OT
log and the path of the full log.

## Why not xmllint

libxml2 refuses the DITA 1.3 DTDs with "Maximum entity amplification factor
exceeded", even with `--huge`. DTD validation goes through the DogsBay XML
command line, which uses Xerces with the DITA-OT catalog, the same resolution
the editor and DITA-OT use.

## Where it runs

The gate is verified on every stage branch. On the tutorial repository's
`main` branch, which is the deliberately broken demo project, it fails with
two invalid files and the findings listed in that branch's `ISSUES.md`; that
is the expected result there.

The full ladder is checked with `scripts/check-all-stages.sh` on `main`,
which checks out each `tutorial/*` branch into a temporary worktree and runs
the gate. On the full guide, validation takes about 2 seconds, health about
2.5 seconds and the build of four deliverables about 11 seconds; the PDF
book from [stage 18](/part-4-books/stage-18-bookmap) adds about 26 seconds to the build.

## The script

The gate as it is on `tutorial/22-learning`, the newest branch. Stages 00
to 13 carry it without step 2b, stages 14 to 21 without the `learning/`
special case: the editor's built-in catalog has no Learning and Training
DTDs, so from stage 22 the gate validates `learning/*.dita` against the
DITA-OT catalog and runs the health check without its own grammar pass.

```bash title="scripts/check-stage.sh"
#!/usr/bin/env bash
# check-stage.sh — the gate every tutorial stage branch must pass.
#
# Runs, from the project root:
#   1. dogsbay-xml validate-project   every topic and map validates against its DOCTYPE
#      (learning/*.dita against the DITA-OT catalog: the editor's catalog has no L&T DTDs)
#   2. dogsbay-xml project-health     links, keys, element ids, conref pushes, index,
#                                     metadata policy and house rules are clean
#   2b. dogsbay-xml validate-conditions (once a subject scheme is referenced) every
#                                     platform/audience value is a controlled value
#   3. dita --project=project.json    every deliverable actually publishes
#
# Exit code is non-zero when any gate fails. Usage:
#   scripts/check-stage.sh [project-root]           (default: the repo root)
#   STRICT=0 scripts/check-stage.sh                 (project-health at severity=error only)
#   SKIP_BUILD=1 scripts/check-stage.sh             (skip the DITA-OT build)
#
# Tooling is found from, in order: $DOGSBAY_XML / $DITA_HOME, the PATH, then the
# developer defaults below.
set -u -o pipefail

ROOT="$(cd "${1:-$(dirname "$0")/..}" && pwd)"
DOGSBAY_XML="${DOGSBAY_XML:-$(command -v dogsbay-xml || echo "$HOME/github/dogsbay-xml/bin/dogsbay-xml")}"
DITA_HOME="${DITA_HOME:-$HOME/Downloads/dita-ot-4.3.5}"
DITA="${DITA:-$(command -v dita || echo "$DITA_HOME/bin/dita")}"
STRICT="${STRICT:-1}"
SKIP_BUILD="${SKIP_BUILD:-0}"
OUT="${OUT:-${TMPDIR:-/tmp}/check-stage-$$}"

status=0
say() { printf '\n== %s ==\n' "$*"; }
fail() { status=1; printf 'FAIL: %s\n' "$*"; }

[ -x "$DOGSBAY_XML" ] || { echo "dogsbay-xml CLI not found (set DOGSBAY_XML)"; exit 2; }

say "validate-project  $ROOT"
vp="$(mktemp)"
if ! "$DOGSBAY_XML" validate-project "$ROOT" | tee "$vp"; then
  # The editor's built-in catalog does not ship the Learning and Training DTDs.
  # Files under learning/ are validated against DITA-OT's catalog instead; any
  # other invalid file is a real failure.
  others=$(grep -E '^/.*\.dita(map)?:$' "$vp" | grep -v "^$ROOT/learning/" || true)
  if [ -n "$others" ]; then
    fail "validation errors"
  else
    say "validate learning/ against the DITA-OT catalog"
    for f in "$ROOT"/learning/*.dita; do
      "$DOGSBAY_XML" validate --catalog "$DITA_HOME/catalog-dita.xml" "$f" || fail "validation errors in $f"
    done
  fi
fi
rm -f "$vp"

say "project-health  $ROOT"
health=("$DOGSBAY_XML" project-health)
# validate-project above already covered grammar validation; project-health's own
# validation pass would trip over learning/ (no L&T DTDs in the editor catalog).
# Schematron is left out of the explicit list because --include requires rules;
# once .dogsbay/config.xml names a <default-schematron>, run it separately below.
[ -d "$ROOT/learning" ] && health+=(--include=reuse,elementIds,metadata,proposals,conrefPush,index,format,markers)
if [ "$STRICT" = "1" ]; then
  "${health[@]}" "$ROOT" || fail "project-health found issues"
else
  "${health[@]}" --severity=error "$ROOT" || fail "project-health found errors"
fi
if [ -d "$ROOT/learning" ] && grep -q '<default-schematron>' "$ROOT/.dogsbay/config.xml" 2>/dev/null; then
  say "house rules  $ROOT"
  "$DOGSBAY_XML" project-health --include=schematron "$ROOT" || fail "house-rule violations"
fi

scheme=$(grep -l '<subjectScheme' "$ROOT"/*.ditamap 2>/dev/null | head -1 || true)
if [ -n "$scheme" ]; then
  # Without -S (or -m) the command discovers no scheme and passes vacuously.
  say "validate-conditions  $ROOT  (scheme: ${scheme#$ROOT/})"
  "$DOGSBAY_XML" validate-conditions -S "$scheme" "$ROOT" || fail "profiling values outside the subject scheme"
fi

if [ "$SKIP_BUILD" = "1" ]; then
  say "build  skipped (SKIP_BUILD=1)"
elif [ -f "$ROOT/project.json" ]; then
  [ -x "$DITA" ] || { echo "dita command not found (set DITA_HOME or DITA)"; exit 2; }
  say "build  project.json -> $OUT"
  log="$OUT.log"; mkdir -p "$OUT"
  # DITA-OT does not always exit non-zero on content errors, so also grep the log.
  if ! (cd "$ROOT" && "$DITA" --project=project.json --output="$OUT" >"$log" 2>&1) \
     || grep -qE '^Error:|\[(DOT[A-Z]+[0-9]+[EF])\]' "$log"; then
    grep -E '^Error:|\[(DOT[A-Z]+[0-9]+[EF])\]' "$log" | sort -u | head -40
    fail "DITA-OT build reported errors (full log: $log)"
  else
    echo "all deliverables built"
  fi
else
  say "build  skipped (no project.json yet)"
fi

echo
[ "$status" = 0 ] && echo "STAGE OK" || echo "STAGE FAILED"
exit "$status"
```
