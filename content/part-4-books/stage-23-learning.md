---
title: "Stage 23: Learning"
description: Create a learning assessment with four question types and configure the gate to validate it with the DITA-OT catalog.
type: tutorial
---

# Stage 23: Learning

Create a `<learningAssessment>` with true-or-false, single-select,
multiple-select, and sequencing questions. Add objectives, a duration,
and feedback for each answer.

Update the gate to validate `learning/` with the DITA-OT catalog. The
DogsBay XML catalog used for these examples lacks the Learning and Training
DTDs. The separate catalog check allows the gate to validate the assessment.

**Optional module.** See [Choose a learning path](/start-here/learning-path)
for the starting checkpoint and the next core lesson.

**Time:** about 30 minutes.
**You need:** stage 22 complete.


Recorded diagnostic examples below come from earlier runs. File counts, paths, and stage numbers can differ. Run the gate for your current checkout.

## Step 1: The assessment

::::steps
1. **Create `learning/check-your-understanding.dita`**
   A new folder, `learning/`, because the gate treats it differently.

   ```xml title="learning/check-your-understanding.dita"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE learningAssessment PUBLIC "-//OASIS//DTD DITA Learning Assessment//EN" "learningAssessment.dtd">
   
   <learningAssessment id="check-your-understanding">
     <title>Check your understanding</title>
     <shortdesc>Four questions on recording, editing and exporting, with feedback on every answer.</shortdesc>
     <prolog>
       <author type="creator">The Audacity tutorial team</author>
       <metadata>
         <audience type="user" experiencelevel="novice"/>
         <category>learning</category>
         <keywords>
           <keyword>quiz</keyword>
           <keyword>assessment</keyword>
         </keywords>
       </metadata>
     </prolog>
     <learningAssessmentbody>
       <lcIntro>
         <p>Answer these after working through the recording and editing chapters.
         Each answer explains why it is right or wrong.</p>
       </lcIntro>
       <lcObjectives>
         <lcObjectivesStem>After this guide you can:</lcObjectivesStem>
         <lcObjectivesGroup>
           <lcObjective>Make a recording with sensible levels.</lcObjective>
           <lcObjective>Remove noise before shaping the sound.</lcObjective>
           <lcObjective>Choose an export format for the purpose.</lcObjective>
         </lcObjectivesGroup>
       </lcObjectives>
       <lcDuration>
         <lcTime value="PT5M">About 5 minutes</lcTime>
       </lcDuration>
       <lcInteraction>
         <lcTrueFalse2 id="q1">
           <lcQuestion2>Clipping can be repaired afterwards with the Normalize effect.</lcQuestion2>
           <lcAnswerOptionGroup2>
             <lcAnswerOption2>
               <lcAnswerContent2>True</lcAnswerContent2>
               <lcFeedback2>Normalize only changes the level; the flattened peaks stay flat.</lcFeedback2>
             </lcAnswerOption2>
             <lcAnswerOption2>
               <lcAnswerContent2>False</lcAnswerContent2>
               <lcCorrectResponse2/>
               <lcFeedback2>Correct. Clipped samples are lost at recording time, so leave headroom instead.</lcFeedback2>
             </lcAnswerOption2>
           </lcAnswerOptionGroup2>
         </lcTrueFalse2>
         <lcSingleSelect2 id="q2">
           <lcQuestion2>What is a good peak level to aim for while recording speech?</lcQuestion2>
           <lcAnswerOptionGroup2>
             <lcAnswerOption2>
               <lcAnswerContent2>0 dB, as loud as possible</lcAnswerContent2>
               <lcFeedback2>At 0 dB any louder syllable clips.</lcFeedback2>
             </lcAnswerOption2>
             <lcAnswerOption2>
               <lcAnswerContent2>About -12 dB</lcAnswerContent2>
               <lcCorrectResponse2/>
               <lcFeedback2>Correct. That leaves headroom for louder moments and for the effects chain.</lcFeedback2>
             </lcAnswerOption2>
             <lcAnswerOption2>
               <lcAnswerContent2>About -40 dB</lcAnswerContent2>
               <lcFeedback2>Too quiet: raising it later also raises the noise floor.</lcFeedback2>
             </lcAnswerOption2>
           </lcAnswerOptionGroup2>
         </lcSingleSelect2>
         <lcMultipleSelect2 id="q3">
           <lcQuestion2>Which formats keep every sample of the recording? Choose all that apply.</lcQuestion2>
           <lcAnswerOptionGroup2>
             <lcAnswerOption2>
               <lcAnswerContent2>WAV</lcAnswerContent2>
               <lcCorrectResponse2/>
             </lcAnswerOption2>
             <lcAnswerOption2>
               <lcAnswerContent2>MP3</lcAnswerContent2>
               <lcFeedback2>MP3 is lossy: it discards detail to shrink the file.</lcFeedback2>
             </lcAnswerOption2>
             <lcAnswerOption2>
               <lcAnswerContent2>FLAC</lcAnswerContent2>
               <lcCorrectResponse2/>
             </lcAnswerOption2>
             <lcAnswerOption2>
               <lcAnswerContent2>OGG Vorbis</lcAnswerContent2>
               <lcFeedback2>OGG Vorbis is lossy, like MP3.</lcFeedback2>
             </lcAnswerOption2>
           </lcAnswerOptionGroup2>
         </lcMultipleSelect2>
         <lcSequencing2 id="q4">
           <lcQuestion2>Put the post-production steps for a podcast in order.</lcQuestion2>
           <lcSequenceOptionGroup2>
             <lcSequenceOption2>
               <lcAnswerContent2>Noise reduction</lcAnswerContent2>
               <lcSequence2 value="1"/>
             </lcSequenceOption2>
             <lcSequenceOption2>
               <lcAnswerContent2>Compression</lcAnswerContent2>
               <lcSequence2 value="2"/>
             </lcSequenceOption2>
             <lcSequenceOption2>
               <lcAnswerContent2>Normalization</lcAnswerContent2>
               <lcSequence2 value="3"/>
             </lcSequenceOption2>
             <lcSequenceOption2>
               <lcAnswerContent2>Export</lcAnswerContent2>
               <lcSequence2 value="4"/>
             </lcSequenceOption2>
           </lcSequenceOptionGroup2>
         </lcSequencing2>
       </lcInteraction>
       <lcSummary>
         <p>Leave headroom, clean before you shape, and keep a lossless master.</p>
       </lcSummary>
     </learningAssessmentbody>
   </learningAssessment>
   ```

2. **Read the frame**

   - The DOCTYPE is `-//OASIS//DTD DITA Learning Assessment//EN` with
     `learningAssessment.dtd`; the body is `<learningAssessmentbody>`.
     The `<prolog>` is the usual one, so the metadata policy applies.
   - `<lcIntro>` opens the assessment. `<lcObjectives>` lists what the
     reader should now be able to do: an `<lcObjectivesStem>` and an
     `<lcObjectivesGroup>` of `<lcObjective>`s.
   - `<lcDuration>` holds an `<lcTime>` whose `value` is an ISO 8601
     duration, `PT5M`, five minutes, with the readable text as content.
     `<lcDuration>` allows only a `<title>` and an `<lcTime>`, and `value`
     is required; the demo at the end writes a paragraph there instead.
   - `<lcSummary>` closes it.

3. **Read the interactions**
   `<lcInteraction>` holds the questions. DITA 1.3 has two generations of
   interaction elements; the ones ending in `2` are the DITA 1.3
   versions, with a content model built on `<div>` rather than `<fig>`,
   and they are the ones to use in new content.
   - `<lcTrueFalse2>`, `<lcSingleSelect2>` and `<lcMultipleSelect2>`
     share a shape: an `<lcQuestion2>`, then an `<lcAnswerOptionGroup2>`
     of `<lcAnswerOption2>`s, each with `<lcAnswerContent2>`, an empty
     `<lcCorrectResponse2/>` on the right ones, and `<lcFeedback2>`. A
     single-select has one correct option, a multiple-select as many as
     apply, a true/false exactly two options.
   - `<lcSequencing2>` asks for an order: an `<lcSequenceOptionGroup2>` of
     `<lcSequenceOption2>`s, each with its `<lcAnswerContent2>` and an
     `<lcSequence2 value="n"/>` giving its place in the right order.
   - Every interaction has an `id`, so a learning map or an LMS export
     can address it. The HTML5 transform renders each question as a list
     of its options with their feedback and drops `<lcCorrectResponse2/>`
     from the page: which answer is right is data for a learning
     platform, not something a reader page gives away.
::::

## Step 2: The maps

::::steps
1. **Edit `audacity-guide.ditamap`**

   ```diff title="audacity-guide.ditamap"
   --- a/audacity-guide.ditamap
   +++ b/audacity-guide.ditamap
   @@ -51,6 +51,10 @@
          <topicref href="topics/removing-background-noise.dita" linking="targetonly"/>
          <topicref href="topics/effect-order.dita"/>
        </topicset>
   +  </topichead>
   +
   +  <topichead navtitle="Check your understanding">
   +    <topicref href="learning/check-your-understanding.dita"/>
      </topichead>
    
      <topichead navtitle="Troubleshooting" id="troubleshooting">
   ```

2. **Edit `beginner-guide.ditamap`**

   ```diff title="beginner-guide.ditamap"
   --- a/beginner-guide.ditamap
   +++ b/beginner-guide.ditamap
   @@ -20,6 +20,7 @@
        <topicref href="topics/trimming-audio.dita"/>
        <topicref href="topics/exporting-audio.dita"/>
      </topichead>
   +  <topicref href="learning/check-your-understanding.dita"/>
      <topichead navtitle="Troubleshooting">
        <topicref href="topics/recording-is-silent.dita"/>
      </topichead>
   ```

   A plain `<topicref>` in both: an assessment is a topic like any other
   to a map. The podcaster guide and the book do not carry it.
::::

## Step 3: What the tools say

::::steps
1. **Run the old gate**
   With the file in place and the gate as it was on stage 22,
   `validate-project` fails:

   ```
   == validate-project  /home/you/audacity-guide ==
   /home/you/audacity-guide/learning/check-your-understanding.dita:
     -1:-1  error: Validation failed: /home/you/audacity-guide/learning/learningAssessment.dtd (No such file or directory)
   42 file(s): 41 valid, 1 invalid.
   FAIL: validation errors
   ```

   The DOCTYPE's system identifier, `learningAssessment.dtd`, is resolved
   through the editor's built-in catalog, and that catalog does not have
   it: what the editor ships covered every DOCTYPE up to stage 22, and
   not the Learning and Training package. `project-health` runs the same
   validation pass and fails for the same reason.

2. **Validate against the DITA-OT catalog**
   DITA-OT ships every OASIS DTD, Learning and Training included, and its
   `catalog-dita.xml` maps the public identifiers to them. The command
   line takes a catalog:

   ```bash
   dogsbay-xml validate --catalog "$DITA_HOME/catalog-dita.xml" learning/check-your-understanding.dita
   ```

   ```
   VALID
   ```

   The file is valid DITA. What is missing is a grammar in one catalog,
   and the editor needs the Learning and Training
   DTDs in its built-in catalog. The gate uses the DITA-OT catalog for them.
::::

## Step 4: Update the gate

::::steps
1. **Edit `scripts/check-stage.sh`**

   ```diff title="scripts/check-stage.sh"
   --- a/scripts/check-stage.sh
   +++ b/scripts/check-stage.sh
   @@ -3,6 +3,7 @@
    #
    # Runs, from the project root:
    #   1. dogsbay-xml validate-project   every topic and map validates against its DOCTYPE
   +#      (learning/*.dita against the DITA-OT catalog: the editor's catalog has no L&T DTDs)
    #   2. dogsbay-xml project-health     links, keys, element ids, conref pushes, index,
    #                                     metadata policy and house rules are clean
    #   2b. dogsbay-xml validate-conditions (once a subject scheme is referenced) every
   @@ -16,7 +17,7 @@
    #
    # Tooling is found from, in order: $DOGSBAY_XML / $DITA_HOME, the PATH, then the
    # developer defaults below.
   -set -u
   +set -u -o pipefail
    
    ROOT="$(cd "${1:-$(dirname "$0")/..}" && pwd)"
    DOGSBAY_XML="${DOGSBAY_XML:-$(command -v dogsbay-xml || echo "$HOME/github/dogsbay-xml/bin/dogsbay-xml")}"
   @@ -33,13 +34,38 @@
    [ -x "$DOGSBAY_XML" ] || { echo "dogsbay-xml CLI not found (set DOGSBAY_XML)"; exit 2; }
    
    say "validate-project  $ROOT"
   -"$DOGSBAY_XML" validate-project "$ROOT" || fail "validation errors"
   +vp="$(mktemp)"
   +if ! "$DOGSBAY_XML" validate-project "$ROOT" | tee "$vp"; then
   +  # The editor's built-in catalog does not ship the Learning and Training DTDs.
   +  # Files under learning/ are validated against DITA-OT's catalog instead; any
   +  # other invalid file is a real failure.
   +  others=$(grep -E '^/.*\.dita(map)?:$' "$vp" | grep -v "^$ROOT/learning/" || true)
   +  if [ -n "$others" ]; then
   +    fail "validation errors"
   +  else
   +    say "validate learning/ against the DITA-OT catalog"
   +    for f in "$ROOT"/learning/*.dita; do
   +      "$DOGSBAY_XML" validate --catalog "$DITA_HOME/catalog-dita.xml" "$f" || fail "validation errors in $f"
   +    done
   +  fi
   +fi
   +rm -f "$vp"
    
    say "project-health  $ROOT"
   +health=("$DOGSBAY_XML" project-health)
   +# validate-project above already covered grammar validation; project-health's own
   +# validation pass would trip over learning/ (no L&T DTDs in the editor catalog).
   +# Schematron is left out of the explicit list because --include requires rules;
   +# once .dogsbay/config.xml names a <default-schematron>, run it separately below.
   +[ -d "$ROOT/learning" ] && health+=(--include=reuse,elementIds,metadata,proposals,conrefPush,index,format,markers)
    if [ "$STRICT" = "1" ]; then
   -  "$DOGSBAY_XML" project-health "$ROOT" || fail "project-health found issues"
   +  "${health[@]}" "$ROOT" || fail "project-health found issues"
    else
   -  "$DOGSBAY_XML" project-health --severity=error "$ROOT" || fail "project-health found errors"
   +  "${health[@]}" --severity=error "$ROOT" || fail "project-health found errors"
   +fi
   +if [ -d "$ROOT/learning" ] && grep -q '<default-schematron>' "$ROOT/.dogsbay/config.xml" 2>/dev/null; then
   +  say "house rules  $ROOT"
   +  "$DOGSBAY_XML" project-health --include=schematron "$ROOT" || fail "house-rule violations"
    fi
    
    scheme=$(grep -l '<subjectScheme' "$ROOT"/*.ditamap 2>/dev/null | head -1 || true)
   ```

2. **Read the changes**

   - `set -u -o pipefail`. Step 1 now pipes `validate-project` through
     `tee` to keep its output, and without `pipefail` the pipeline's
     exit status comes from `tee`. A successful `tee` could therefore hide
     a validation failure. The tutorial's own driver script,
     `check-all-stages.sh`, needed the same fix.
   - When `validate-project` fails, the gate reads the files it named. If
     any is outside `learning/`, that is a real failure. If they are all
     under `learning/`, each is validated with
     `dogsbay-xml validate --catalog "$DITA_HOME/catalog-dita.xml"`, and
     only a failure there fails the stage.
   - `project-health` has a `--include` switch that names the checks to
     run. When a `learning/` folder exists, the gate lists every check
     except the grammar pass, which step 1 has already done, and except
     `schematron`: naming it in `--include` is an error unless house
     rules are configured. When stage 25 configures a
     `<default-schematron>`, the gate runs the rules as a separate step,
     and the `if` for that is already in place.
   - `validate-conditions` and the build are unchanged. DITA-OT builds
     the assessment as it builds any topic.

3. **Change the "You are on" line and the layout**

   ```diff title="README.md"
   --- a/README.md
   +++ b/README.md
   @@ -2,7 +2,7 @@
    
    Build a guide through 27 stages. Maps begin at stage 02; HTML publication begins at stage 03.
    
   -You are on stage 22: software domains.
   +You are on stage 23: learning.
    
    ## Stages
    
   ```

4. **Format and check**

   ```bash
   dogsbay-xml format -i topics/*.dita *.ditamap
   scripts/check-stage.sh
   ```

   ```
   == validate-project  /home/you/audacity-guide ==
   /home/you/audacity-guide/learning/check-your-understanding.dita:
     -1:-1  error: Validation failed: /home/you/audacity-guide/learning/learningAssessment.dtd (No such file or directory)
   42 file(s): 41 valid, 1 invalid.

   == validate learning/ against the DITA-OT catalog ==
   VALID

   == project-health  /home/you/audacity-guide ==
   Root map: audacity-guide.ditamap (project config); house rules: none (none configured)
   Project is healthy: valid, no broken references, keys, orphans, or broken element ids.

   == validate-conditions  /home/you/audacity-guide  (scheme: subject-scheme.ditamap) ==
   42 file(s): 42 pass, 0 with violations.

   == build  project.json -> /tmp/check-stage-<pid> ==
   all deliverables built

   STAGE OK
   ```

   Read it from the top: `validate-project` still reports the file as
   invalid, the gate sees that only `learning/` is affected and validates
   it against the DITA-OT catalog, `project-health` runs with the
   explicit list and reports the project healthy, and every deliverable
   builds. `STAGE OK` means what it always meant.
::::

Test the duration content model. Replace the `<lcTime>` line with
`<p>About 5 minutes</p>` so that `<lcDuration>` holds a paragraph, and run
the gate with `SKIP_BUILD=1`:

```
== validate-project  /home/you/audacity-guide ==
/home/you/audacity-guide/learning/check-your-understanding.dita:
  -1:-1  error: Validation failed: /home/you/audacity-guide/learning/learningAssessment.dtd (No such file or directory)
42 file(s): 41 valid, 1 invalid.

== validate learning/ against the DITA-OT catalog ==
INVALID
  33:18 [error] The content of element type "lcDuration" must match "(title?,lcTime?)".
FAIL: validation errors in /home/you/audacity-guide/learning/check-your-understanding.dita
```

The error comes from the DITA-OT catalog pass, since the editor's catalog
never gets as far as the content model. `<lcDuration>` takes an `<lcTime>`,
and `<lcTime>` takes a `value`: the `value` supplies a machine-readable duration for a learning platform.

After each error exercise, undo the deliberate change and rerun the gate.
Confirm that it prints `STAGE OK` before continuing.

## What you learned

- `<learningAssessment>`, `<learningAssessmentbody>`, `<lcIntro>`,
  `<lcObjectives>` with `<lcObjectivesStem>`, `<lcObjectivesGroup>` and
  `<lcObjective>`, `<lcDuration>` with `<lcTime value>`, `<lcSummary>`.
- `<lcInteraction>` with the DITA 1.3 interactions `<lcTrueFalse2>`,
  `<lcSingleSelect2>`, `<lcMultipleSelect2>` and `<lcSequencing2>`, and
  their `<lcQuestion2>`, `<lcAnswerOptionGroup2>`, `<lcAnswerOption2>`,
  `<lcAnswerContent2>`, `<lcCorrectResponse2>`, `<lcFeedback2>`,
  `<lcSequenceOptionGroup2>`, `<lcSequenceOption2>`, `<lcSequence2>`.
- A grammar can be valid and still unknown to a catalog;
  `dogsbay-xml validate --catalog` chooses the catalog.
- The gate: `pipefail`, `project-health --include`, and house rules as a
  separate step once they exist.
- End of Part 4: a book, a troubleshooting topic, hazard statements, the
  software domains and an assessment, on the same guide.

## Next lesson

Continue with [Stage 24: drafts and localization](/part-5-governance/stage-24-drafts-and-localization).

For the core course, use the [learning path](/start-here/learning-path) to skip optional modules.

[Compare this stage with its predecessor](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/22-software-domains...tutorial/23-learning).
