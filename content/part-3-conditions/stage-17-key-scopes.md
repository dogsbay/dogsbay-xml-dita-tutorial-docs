---
title: "Stage 17: Key scopes"
description: Give each guide its own start-here key, publish all three guides in one collection without key collisions, and link across deliverables with a peer map.
type: tutorial
---

# Stage 17: Key scopes

Combine three guides in a collection while retaining a separate
`start-here` key for each guide. A `@keyscope` on each `<mapref>`
qualifies the keys as `scope.key`, so the definitions can coexist.

Add the collection map and its deliverable. Also add a peer map reference
to the podcaster guide to expose the beginner guide's keys for links
between separately published guides.

**Optional module.** See [Choose a learning path](/start-here/learning-path)
for the starting checkpoint and the next core lesson.

**Time:** about 30 minutes.
**You need:** stage 16 complete.


Recorded output below is an example. File counts, paths, and stage numbers can differ. [Check your work](/start-here/run-the-gate) to see the result for your own project.

## Step 1: A key per guide

::::steps
1. **Edit `beginner-guide.ditamap`**

   ```diff title="beginner-guide.ditamap"
   --- a/beginner-guide.ditamap
   +++ b/beginner-guide.ditamap
   @@ -9,6 +9,8 @@
      <!-- Abbreviated forms link into this glossary group; toc="no" publishes it without a TOC entry. -->
      <topicref href="topics/glossary/audio-units.dita" toc="no"/>
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
     deliverable. No topic in this stage uses one.
   - DITA-OT 4.3.5 does not resolve peer-map keys such as
     `beginner.install`. A link by such a key renders as an empty link, and
     the build prints no warning. The editor's key list and `dogsbay-xml
     keys` still show these keys. Do not use them in a topic unless you set
     up cross-deliverable publishing that maps them to published URLs.
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
   common-notes  →  shared/common-notes.dita   [/home/you/my-audacity-guide/podcaster-guide.ditamap:9]  file: /home/you/my-audacity-guide/shared/common-notes.dita
   start-here  →  topics/podcast-production-workflow.dita   [/home/you/my-audacity-guide/podcaster-guide.ditamap:11]  file: /home/you/my-audacity-guide/topics/podcast-production-workflow.dita
   …
   beginner.common-notes  →  shared/common-notes.dita   [/home/you/my-audacity-guide/beginner-guide.ditamap:11]  file: /home/you/my-audacity-guide/shared/common-notes.dita
   beginner.start-here  →  topics/recording-your-first-track.dita   [/home/you/my-audacity-guide/beginner-guide.ditamap:13]  file: /home/you/my-audacity-guide/topics/recording-your-first-track.dita
   beginner.install  →  topics/installing-audacity.dita   [/home/you/my-audacity-guide/beginner-guide.ditamap:18]  file: /home/you/my-audacity-guide/topics/installing-audacity.dita
   beginner.formats  →  topics/supported-audio-formats.dita   [/home/you/my-audacity-guide/beginner-guide.ditamap:26]  file: /home/you/my-audacity-guide/topics/supported-audio-formats.dita
   …
   47 key(s).
   ```
::::

## Step 2: The collection

::::steps
1. **Create `audacity-collection.ditamap`**
   Right-click `my-audacity-guide` in the **Explorer**, choose
   **New File**, enter `audacity-collection.ditamap`, and choose the
   **Map** template. Replace the template's content with this map. The
   comments are part of the lesson.

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
     <mapref href="keydefs-glossary.ditamap"/>
     <keydef keys="common-notes" href="shared/common-notes.dita"/>
     <keydef keys="formats" href="topics/supported-audio-formats.dita"/>
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

   - The root scope defines the product keys, `common-notes`, the glossary
     keys and `formats`. Some topics are processed outside any guide's
     scope: the recording task through the conref push, and topics that
     other root-scope topics link to. Their key references resolve in the
     root scope. Without the glossary keys there, an
     `<abbreviated-form keyref="gl-decibel"/>` in such a copy renders as
     nothing at all.
   - `keyscope="userguide"` on a `<mapref>` puts the whole full guide in
     a scope: every key it defines is `userguide.<key>` from the
     collection's point of view, and inside the guide the bare names go
     on working. The three `start-here`s no longer collide because they
     are `userguide.start-here`, `beginner.start-here` and
     `podcaster.start-here`.
   - A bare key is not visible in the parent scope. `start-here` is
     undefined at the root of the collection; only the three scoped names
     exist. The last comment in the map says what happens if you forget.
   - Keys defined in the root scope are visible in every child scope. If
     an ancestor scope and a child scope define the same key, the
     definition in the ancestor scope takes precedence (DITA 1.3). The
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
   start-here  →  topics/what-is-audacity.dita  (scope: userguide)   [/home/you/my-audacity-guide/audacity-guide.ditamap:27]  file: /home/you/my-audacity-guide/topics/what-is-audacity.dita
   start-here  →  topics/recording-your-first-track.dita  (scope: beginner)   [/home/you/my-audacity-guide/beginner-guide.ditamap:13]  file: /home/you/my-audacity-guide/topics/recording-your-first-track.dita
   start-here  →  topics/podcast-production-workflow.dita  (scope: podcaster)   [/home/you/my-audacity-guide/podcaster-guide.ditamap:11]  file: /home/you/my-audacity-guide/topics/podcast-production-workflow.dita
   ```

   The same key resolves to a different target in each scope. Without
   `--scope`:

   ```
   Error: Key 'start-here' is not defined in audacity-collection.ditamap
   ```
::::

## Step 3: The deliverable and the check

::::steps
1. **Add the deliverable**
   Choose **Project** > **Project Tools** > **Manage Deliverables...** and
   click **Add...**. Enter these values:

   | Field | Value |
   |---|---|
   | **Name** | `collection` |
   | **Input map** | `audacity-collection.ditamap` |
   | **DITAVAL (optional)** | (leave empty) |
   | **Transtype** | `html5` |
   | **Output (optional)** | `out/collection` |
   | **Publication parameters** | `nav-toc` = `full` |

   The `nav-toc` parameter with the value `full` puts the navigation of
   the whole collection on each page, as it does for `full`. Click
   **Save...**, click **OK** to write the deliverable, and click
   **Close**.

2. **Check your work**
   In the editor, choose **Project** > **Check Project** and read the
   result in the **Project Validation** panel. From the command line, run:

   ```bash
   dogsbay-xml check .
   ```

   Example output:

   ```
   health   clean, with warnings
     unused key: start-here  [/home/you/my-audacity-guide/audacity-guide.ditamap:27] — nothing references it
     unused key: digital-audio  [/home/you/my-audacity-guide/audacity-guide.ditamap:34] — nothing references it
     unused key: podcast-workflow  [/home/you/my-audacity-guide/audacity-guide.ditamap:50] — nothing references it
   build    full                 ok  /home/you/my-audacity-guide/out/full
     5 note(s) — run with --verbose to see them
   build    beginner-mac         ok  /home/you/my-audacity-guide/out/beginner-mac
   build    beginner-windows     ok  /home/you/my-audacity-guide/out/beginner-windows
   build    podcaster-linux      ok  /home/you/my-audacity-guide/out/podcaster-linux
   build    review               ok  /home/you/my-audacity-guide/out/review
     4 note(s) — run with --verbose to see them
   build    install-variants     ok  /home/you/my-audacity-guide/out/install-variants
     6 note(s) — run with --verbose to see them
   build    collection           ok  /home/you/my-audacity-guide/out/collection
     16 note(s) — run with --verbose to see them
   output   clean (full, beginner-mac, beginner-windows, podcaster-linux, review, install-variants, collection)
   Ready: the project is healthy, every deliverable built, and the output of full, beginner-mac, beginner-windows, podcaster-linux, review, install-variants, collection holds together. 3 unused keys above: worth knowing, and not treated as failures.
   ```

   The note lines count DITA-OT notes from each build. To print them,
   run `dogsbay-xml check -v .`, or read them in the
   **Project Validation** panel. The notes of the `collection`
   deliverable include `DOTJ047I` notes for glossary keys, such as
   `gl-decibel`, that DITA-OT did not find in the root scope. The output
   check finds no broken links in the collection.

3. **Read the output**
   `out/collection/index.html` has the three
   guides one after another, and `topics/` has a page per use of each
   topic: `what-is-audacity.html` for the full guide, then
   `what-is-audacity-1.html` for the beginner guide and
   `what-is-audacity-2.html` for the podcaster guide. A topic used in
   several scopes is a page in each, so that each copy's links resolve in
   its own scope. A topic with no key references, such as *Trimming
   audio* or a glossary entry, is one page that the three chapters share.
::::

Test an unqualified key at the root. Add `<topicref keyref="start-here"/>`
to `audacity-collection.ditamap`, above the first `<topichead>`, and check
your work. The check reports `Ready`, and the `collection` build reports
one more note than before. DITA-OT drops the topicref and records it as an
informational note. To see it, check the `collection` deliverable with the
`-v` option:

```bash
dogsbay-xml check --deliverable=collection -v .
```

Among the notes, the output includes this line. In the editor, the
**Project Validation** panel shows the same note after **Check Project**.

```
  INFO  [DOTJ047I] file:/home/you/my-audacity-guide/audacity-collection.ditamap:16  Unable to find key definition for key reference 'start-here' in root scope. Using the @href attribute as fallback if it exists.
```

There is no `@href`, so the entry is missing from the table of contents and
nothing else. The build succeeds and no link breaks, so the output check
has nothing to report. Health does not see it either: it checks the
default root map, the full guide. The key resolver, pointed at the
collection, reports an error:

```bash
dogsbay-xml keys audacity-collection.ditamap --resolve start-here
```

```
Error: Key 'start-here' is not defined in audacity-collection.ditamap
```

A key reference from the root has to name the scope,
`keyref="userguide.start-here"`, or the map has to define the bare key in
the root. When you add a root map, check its key space by hand.

After each error exercise, undo the deliberate change and check again.
Confirm that the check reports `Ready` before you continue.

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
- A topic whose key references resolve differently across scopes can
  produce multiple output pages.

## Next lesson

**Checkpoint:** `tutorial/17-key-scopes`. If you use Git, commit your work.

Continue with [Stage 18: chunking and output](/part-3-conditions/stage-18-chunking-and-output).

For the core course, use the [learning path](/start-here/learning-path) to skip optional modules.

[Compare this stage with its predecessor](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/16-branch-filtering...tutorial/17-key-scopes).
