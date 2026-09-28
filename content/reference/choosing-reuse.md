---
title: Choose a reuse method
description: Choose between topic reuse, keys, conref, conkeyref, and separate content by considering meaning, context, and maintenance.
type: reference
---

# Choose a reuse method

Reuse content when its meaning and maintenance are shared across uses.
Similar wording alone is insufficient: two instructions can look alike
while requiring different actions or changing independently.

| Requirement | Method | Example |
|---|---|---|
| Publish the same explanation in several guides | Reference the topic from each map | An audio-formats reference in the full and beginner guides |
| Substitute a product value or indirect link target | Key definition and `keyref` | A product name or download link |
| Repeat a stable element or group of elements | `conref` | A save step used in several procedures |
| Select reusable content through each publication's key space | `conkeyref` | A shared note whose source can differ by guide |
| Include a small audience or platform variation | Profiling attributes and a DITAVAL | A keyboard shortcut that differs by platform |
| Express a different reader goal or substantially different procedure | Separate topic | Recording from a microphone and importing an existing file |

Start with topic reuse and simple pull references. Use conref ranges or
push only when they solve a specific maintenance requirement. A pushed step
is less visible in the receiving source, so inspect the resolved output.

## Review a proposed reusable element

- Include enough context for each use. Avoid references such as "the option
  above" or step numbers that change between topics.
- Keep an instruction and its necessary explanation together. Reusing
  fragments of sentences can create grammar and translation problems.
- Check that the receiving location accepts the element and that required
  placeholder children are present.
- Give reusable targets stable IDs. Check references before renaming them.
- Review every affected deliverable after changing shared content.

For example, a save step belongs in `shared/` if every task uses the same
action and result. If one task must export a distribution copy, describe
that action separately. It has a different purpose and output.

See [Stage 10](/part-2-maps/stage-10-keys) for keys and
[Stage 11](/part-2-maps/stage-11-reuse) for reusable elements.
