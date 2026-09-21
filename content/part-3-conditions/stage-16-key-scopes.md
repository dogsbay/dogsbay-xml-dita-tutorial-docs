---
title: "Stage 16: Key scopes"
description: Give each guide its own start-here key, publish all three guides in one collection without key collisions, and link across deliverables with a peer map.
type: tutorial
---

# Stage 16: Key scopes

Three guides now share one key space, and it is about to get crowded. Each
guide wants a `start-here` key for its own landing page: *What is
Audacity?* in the full guide, *Recording your first track* for beginners,
the workflow topic for podcasters. Put the three maps together in one
publication and, in a flat key space, the first definition wins and the
other two guides point at the wrong page. A *key scope* fixes this: a
`@keyscope` on a `<mapref>` puts everything under it in a named scope, and
from outside, the keys are reachable as `scope.key`.

This stage adds the two `start-here` keys, a collection map that pulls the
three guides in under three scopes, and a `scope="peer"` mapref in the
podcaster guide that makes the beginner guide's keys visible without
publishing it again.

**Time:** about 30 minutes.
**You need:** stage 15 complete.

## Step 1: A key per guide

::::steps
1. **Edit `beginner-guide.ditamap`**

   ```diff title="beginner-guide.ditamap"
   --- a/beginner-guide.ditamap
   +++ b/beginner-guide.ditamap
   @@ -7,6 +7,8 @@
      <mapref href="keydefs-product.ditamap"/>
      <mapref href="keydefs-glossary.ditamap"/>
      <keydef keys="common-notes" href="shared/common-notes.dita"/>
   +  <!-- This guide's own landing page, under the same bare key every guide uses. -->
   +  <keydef keys="start-here" href="topics/recording-your-first-track.dita"/>
      <topicref href="shared/common-steps.dita" processing-role="resource-only"/>
    
      <topichead navtitle="Introduction">
   ```

2. **Edit `podcaster-guide.ditamap`**
   A `start-here` of its own, the workflow topic keyed as in the full
   guide, and a peer reference to the beginner guide.

   ```diff title="podcaster-guide.ditamap"
   --- a/podcaster-guide.ditamap
   +++ b/podcaster-guide.ditamap
   @@ -7,6 +7,11 @@
      <mapref href="keydefs-product.ditamap"/>
      <mapref href="keydefs-glossary.ditamap"/>
      <keydef keys="common-notes" href="shared/common-notes.dita"/>
   +  <!-- This guide's own landing page, under the same bare key every guide uses. -->
   +  <keydef keys="start-here" href="topics/podcast-production-workflow.dita"/>
   +  <!-- Cross-deliverable linking: keys of the beginner guide are visible here as
   +       beginner.<key>, but resolve to that separately published deliverable. -->
   +  <mapref href="beginner-guide.ditamap" scope="peer" keyscope="beginner"/>
      <topicref href="shared/common-steps.dita" processing-role="resource-only"/>
    
      <topichead navtitle="Getting started">
   @@ -22,7 +27,7 @@
        <topicref href="topics/exporting-audio.dita"/>
      </topichead>
      <topichead navtitle="Podcast production">
   -    <topicref href="topics/podcast-production-workflow.dita"/>
   +    <topicref href="topics/podcast-production-workflow.dita" keys="podcast-workflow"/>
      </topichead>
      <topichead navtitle="Reference">
        <topicref href="topics/supported-audio-formats.dita" keys="formats"/>
   ```

3. **Read the peer mapref**
   - `scope="peer"` on a `<mapref>` says the referenced map is published
     separately; its topics are not added to this publication.
   - `keyscope="beginner"` gives that map's keys a scope name. Inside the
     podcaster guide they are `beginner.start-here`, `beginner.install`,
     `beginner.formats`. A link by such a key is a link to the other
     deliverable, and making it resolve to a published URL is up to the
     publishing setup; no topic in this stage uses one yet.
   - A peer mapref without a `@keyscope` would pour the beginner guide's
     bare keys into this guide, and its `start-here` would collide with
     the one defined here.

4. **List the keys of the podcaster guide**

   ```bash
   dogsbay-xml keys podcaster-guide.ditamap
   ```

   The guide's own keys come first, then the peer's, each with the scope
   prefix:

   ```
   start-here  →  topics/podcast-production-workflow.dita   [/home/you/audacity-guide/podcaster-guide.ditamap:11]  file: /home/you/audacity-guide/topics/podcast-production-workflow.dita
   …
   beginner.start-here  →  topics/recording-your-first-track.dita   [/home/you/audacity-guide/beginner-guide.ditamap:11]  file: /home/you/audacity-guide/topics/recording-your-first-track.dita
   beginner.install  →  topics/installing-audacity.dita   [/home/you/audacity-guide/beginner-guide.ditamap:16]  file: /home/you/audacity-guide/topics/installing-audacity.dita
   beginner.formats  →  topics/supported-audio-formats.dita   [/home/you/audacity-guide/beginner-guide.ditamap:24]  file: /home/you/audacity-guide/topics/supported-audio-formats.dita
   ```
::::

## Step 2: The collection

::::steps
1. **Create `audacity-collection.ditamap`**
   The comments are part of the lesson.

   ```xml title="audacity-collection.ditamap"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE map PUBLIC "-//OASIS//DTD DITA Map//EN" "map.dtd">

   <map>
     <title>Audacity documentation collection</title>
     <mapref href="subject-scheme.ditamap" type="subjectScheme"/>
     <!-- Root-scope keys. The conref push in shared/common-steps.dita reaches the
          recording task directly, outside any guide's scope, and that copy of the
          topic resolves its keys here. Keys used by such content must exist in the
          root scope too. -->
     <mapref href="keydefs-product.ditamap"/>
     <keydef keys="common-notes" href="shared/common-notes.dita"/>
     <!-- Each guide defines its own "start-here" key. Combined flat they would
          collide (first definition wins). A key scope on each mapref keeps them
          apart: userguide.start-here, beginner.start-here, podcaster.start-here. -->
     <topichead navtitle="Audacity User Guide">
       <mapref href="audacity-guide.ditamap" keyscope="userguide"/>
     </topichead>
     <topichead navtitle="Beginner's Guide">
       <mapref href="beginner-guide.ditamap" keyscope="beginner"/>
     </topichead>
     <topichead navtitle="Podcaster's Guide">
       <mapref href="podcaster-guide.ditamap" keyscope="podcaster"/>
     </topichead>
     <!-- A topicref keyref="start-here" here would resolve to nothing: the bare
          key is not defined in the root scope, only the scoped
          userguide.start-here, beginner.start-here and podcaster.start-here are.
          DITA-OT drops such a topicref from the output silently (DOTJ047I only
          with -v), and topics pulled in from the root scope would lose the keys
          their own guide defines, so the collection only aggregates the guides. -->
   </map>
   ```

2. **Read the scopes**
   - `keyscope="userguide"` on a `<mapref>` puts the whole full guide in
     a scope: every key it defines is `userguide.<key>` from the
     collection's point of view, and inside the guide the bare names go
     on working. The three `start-here`s no longer collide because they
     are `userguide.start-here`, `beginner.start-here` and
     `podcaster.start-here`.
   - A bare key is not visible in the parent scope. `start-here` is
     undefined at the root of the collection; only the three scoped names
     exist. The last comment in the map says what happens if you forget.
   - Keys defined in the root scope are visible in every child scope. The
     collection defines the product keys and `common-notes` at its root
     as well as inside each guide, for a reason the first comment gives:
     the conref push in `shared/common-steps.dita` reaches the recording
     task directly, outside any guide's scope, and that copy of the topic
     resolves its keys in the root scope. Content that a push or a
     resource-only reference reaches from the root needs its keys in the
     root.
   - A `<topichead>` around each mapref gives the collection three
     chapters, one per guide.

3. **Resolve the same key in three scopes**

   ```bash
   dogsbay-xml keys audacity-collection.ditamap --resolve start-here --scope userguide
   dogsbay-xml keys audacity-collection.ditamap --resolve start-here --scope beginner
   dogsbay-xml keys audacity-collection.ditamap --resolve start-here --scope podcaster
   ```

   ```
   start-here  →  topics/what-is-audacity.dita  (scope: userguide)   [/home/you/audacity-guide/audacity-guide.ditamap:27]  file: /home/you/audacity-guide/topics/what-is-audacity.dita
   start-here  →  topics/recording-your-first-track.dita  (scope: beginner)   [/home/you/audacity-guide/beginner-guide.ditamap:11]  file: /home/you/audacity-guide/topics/recording-your-first-track.dita
   start-here  →  topics/podcast-production-workflow.dita  (scope: podcaster)   [/home/you/audacity-guide/podcaster-guide.ditamap:11]  file: /home/you/audacity-guide/topics/podcast-production-workflow.dita
   ```

   One key, three answers, depending on where you stand. Without
   `--scope`:

   ```
   Error: Key 'start-here' is not defined in audacity-collection.ditamap
   ```
::::

## Step 3: The deliverable, the README and the gate

::::steps
1. **Edit `project.json`**

   ```diff title="project.json"
   --- a/project.json
   +++ b/project.json
   @@ -103,6 +103,23 @@
          "publication": {
            "transtype": "html5"
          }
   +    },
   +    {
   +      "name": "collection",
   +      "context": {
   +        "id": "collection",
   +        "input": "audacity-collection.ditamap"
   +      },
   +      "output": "out/collection",
   +      "publication": {
   +        "transtype": "html5",
   +        "params": [
   +          {
   +            "name": "nav-toc",
   +            "value": "full"
   +          }
   +        ]
   +      }
        }
      ]
    }
   ```

2. **Change the "You are on" line and the layout**

   ```diff title="README.md"
   --- a/README.md
   +++ b/README.md
   @@ -5,7 +5,7 @@ branch is the previous branch plus one lesson, from a single concept topic to a
    complete, publish-ready **Audacity User Guide**. The diff between two branches
    is the lesson.
    
   -You are on **stage 15 — branch filtering**: three platform variants of one topic from a single map.
   +You are on **stage 16 — key scopes**: three guides in one collection without key collisions, and a peer map.
    
    ## Stages
    
   @@ -39,6 +39,7 @@ override the defaults). A stage is done when it prints `STAGE OK`.
    audacity-guide.ditamap  the main guide (start here)
    installation-variants.ditamap  branch filtering: one install topic, three platform variants
    beginner-guide.ditamap, podcaster-guide.ditamap  audience-specific guides that reuse the same topics
   +audacity-collection.ditamap  all three guides in one publication, each in its own key scope
    filters/              DITAVAL files: what each deliverable includes, excludes or flags
    keydefs-product.ditamap key definitions: product name, version, download URL, project extension
    keydefs-glossary.ditamap key definitions for the glossary entries
   ```

3. **Check**

   ```bash
   scripts/check-stage.sh
   ```

   ```
   == validate-project  /home/you/audacity-guide ==
   34 file(s): 34 valid, 0 invalid.

   == project-health  /home/you/audacity-guide ==
   Root map: audacity-guide.ditamap (project config); house rules: none (none configured)
   Project is healthy: valid, no broken references, keys, orphans, or broken element ids.

   == validate-conditions  /home/you/audacity-guide ==
   34 file(s): 34 pass, 0 with violations.

   == build  project.json -> /tmp/check-stage-425102 ==
   all deliverables built

   STAGE OK
   ```

4. **Read the output**
   `/tmp/check-stage-425102/out/collection/index.html` has the three
   guides one after another, and `topics/` has a page per use of each
   topic: `what-is-audacity.html` for the full guide, then
   `what-is-audacity-1.html` for the beginner guide and
   `what-is-audacity-2.html` for the podcaster guide. A topic used in
   several scopes is a page in each, so that each copy's links resolve in
   its own scope. A topic with no key references, such as *Trimming
   audio* or a glossary entry, is one page that the three chapters share.
::::

Now use the bare key from the root. Add `<topicref keyref="start-here"/>`
to `audacity-collection.ditamap`, above the first `<topichead>`, and run the
gate. It passes. DITA-OT drops the topicref, and only a verbose build
(`dita … -v`) says so:

```
file:/home/you/audacity-guide/audacity-collection.ditamap:16:34: [DOTJ047I] Unable to find key definition for key reference 'start-here' in root scope. Using the @href attribute as fallback if it exists.
```

There is no `@href`, so the entry is missing from the table of contents and
nothing else. `project-health` does not see it either: it checks the
default root map, the full guide. The tool that does object is the key
resolver, pointed at the collection:

```bash
dogsbay-xml keys audacity-collection.ditamap --resolve start-here
```

```
Error: Key 'start-here' is not defined in audacity-collection.ditamap
```

A key reference from the root has to name the scope,
`keyref="userguide.start-here"`, or the map has to define the bare key in
the root. When you add a root map, check its key space by hand.

## What you learned

- `@keyscope` on a `<mapref>` (or any `<topicref>`) names a scope; inside
  it bare keys work, outside it they are `scope.key`.
- Root-scope keys are visible in every scope; a child scope's keys are not
  visible in the parent. Content that a conref push reaches from the root
  needs its keys in the root.
- `<mapref scope="peer" keyscope="…">` makes another deliverable's keys
  visible without publishing its topics.
- `dogsbay-xml keys <map> --resolve <key> --scope <scope>` resolves a key
  in a scope; `dogsbay-xml keys <map>` lists every scoped name.
- A topic used in several scopes is published once per scope.

## Where to go next

:::cards
- **[Stage 17: Chunking and output](/part-3-conditions/stage-17-chunking-and-output)** {icon="arrow-right"}
  Which topics become which pages: `@chunk`, `@copy-to`, print-only
  topics, and a topicset shared between maps.

- **[Compare 15 to 16 on GitHub](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/15-branch-filtering...tutorial/16-key-scopes)** {icon="github"}
  Exactly what this stage added.
:::
