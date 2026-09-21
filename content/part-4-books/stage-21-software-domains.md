---
title: "Stage 21: Software domains"
description: Document a command-line interface with the software and programming domains, a syntax diagram, a parameter list, messages, and code pulled from a file.
type: tutorial
---

# Stage 21: Software domains

Audacity can be driven from a script through `mod-script-pipe`. Documenting
that means documenting commands, parameters, variables, options, messages
and code, and DITA's software and programming domains have an element for
each so that a command name is never a bare `<codeph>`. This stage uses
them on a small scripting reference and a task that runs a Python sample.

Two new topics and one new folder. `topics/scripting-reference.dita` has a
`<syntaxdiagram>` of the command syntax, a `<parml>` parameter list, a
`<msgblock>` of a failing response and a `<properties>` table of the pipe
locations per platform. `topics/exporting-from-a-script.dita` pulls
`samples/export-mp3.py` into a `<codeblock>` with `<coderef>`, so the code
is never copied into the XML, and shows a terminal session as `<screen>`
with `<userinput>` and `<systemoutput>`.

**Time:** about 35 minutes.
**You need:** stage 20 complete.

## Step 1: The scripting reference

::::steps
1. **Create `topics/scripting-reference.dita`**

   ```xml title="topics/scripting-reference.dita"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE reference PUBLIC "-//OASIS//DTD DITA Reference//EN" "reference.dtd">

   <reference id="scripting-reference">
     <title>Scripting commands</title>
     <shortdesc>The scripting commands most useful for batch work, their parameters and the responses they return.</shortdesc>
     <prolog>
       <author type="creator">The Audacity tutorial team</author>
       <critdates>
         <created date="2026-03-01"/>
       </critdates>
       <metadata>
         <audience type="user" experiencelevel="expert"/>
         <category>reference</category>
         <keywords>
           <keyword>scripting</keyword>
           <keyword>mod-script-pipe</keyword>
           <keyword>automation</keyword>
           <indexterm>scripting<indexterm>commands</indexterm></indexterm>
         </keywords>
       </metadata>
     </prolog>
     <refbody>
       <section>
         <title>Syntax</title>
         <p>A scripting command is a command name, a colon, and zero or more <varname>Name</varname>=<varname>Value</varname> parameters.
         <keyword keyref="product-name"/> replies with the command's output followed by a status line.</p>
         <syntaxdiagram>
           <title>Scripting command</title>
           <groupseq>
             <kwd>CommandName</kwd>
             <delim>:</delim>
             <groupseq importance="optional">
               <repsep/>
               <var>Name</var>
               <delim>=</delim>
               <var>Value</var>
             </groupseq>
           </groupseq>
         </syntaxdiagram>
         <p>A synopsis in one line: <synph><kwd>Export2</kwd><delim>:</delim> <var>Filename</var>=<var>path</var> <var>NumChannels</var>=<var>1|2</var></synph>.</p>
       </section>
       <refsyn>
         <title>Commands</title>
         <parml>
           <plentry>
             <pt>
               <cmdname>SelectAll:</cmdname>
             </pt>
             <pd>Selects every track from start to end. No parameters.</pd>
           </plentry>
           <plentry>
             <pt><cmdname>Normalize:</cmdname> <parmname>PeakLevel</parmname>=<varname>dB</varname></pt>
             <pd>Normalizes the selection so its peak sits at <varname>dB</varname>, typically <codeph>-1</codeph>.</pd>
           </plentry>
           <plentry>
             <pt><cmdname>Export2:</cmdname> <parmname>Filename</parmname>=<varname>path</varname> <parmname>NumChannels</parmname>=<varname>n</varname></pt>
             <pd>Exports the selection to <varname>path</varname>; the extension selects the format.
             <option>NumChannels=1</option> forces mono.</pd>
           </plentry>
           <plentry>
             <pt><cmdname>GetInfo:</cmdname> <parmname>Type</parmname>=<varname>Commands|Tracks|Clips</varname></pt>
             <pd>Returns a JSON description of the requested objects.</pd>
           </plentry>
         </parml>
       </refsyn>
       <section>
         <title>Responses</title>
         <p>Every reply ends with a status line.
         Success looks like <msgph>BatchCommand finished: OK</msgph>; a failure names the command:</p>
         <msgblock>Export2: Filename=episode.mp3 NumChannels=1
   BatchCommand finished: Failed!</msgblock>
         <p>The <apiname>send</apiname> function in the sample script (<xref href="../samples/export-mp3.py" scope="external" format="py">export-mp3.py</xref>) returns the whole reply; check it for <msgnum>Failed!</msgnum> before sending the next command.</p>
       </section>
       <properties>
         <prophead>
           <proptypehd>Platform</proptypehd>
           <propvaluehd>Pipe location</propvaluehd>
           <propdeschd>Notes</propdeschd>
         </prophead>
         <property platform="linux mac">
           <proptype>Linux, macOS</proptype>
           <propvalue>
             <filepath>/tmp/audacity_script_pipe.to.<varname>uid</varname></filepath>
           </propvalue>
           <propdesc>A named pipe per user id; open it for writing.</propdesc>
         </property>
         <property platform="windows">
           <proptype>Windows</proptype>
           <propvalue>
             <filepath>\\.\pipe\ToSrvPipe</filepath>
           </propvalue>
           <propdesc>A Windows named pipe; open it in text mode.</propdesc>
         </property>
       </properties>
     </refbody>
   </reference>
   ```

2. **Read the syntax diagram**
   - `<syntaxdiagram>` is a block that describes syntax as a tree of
     groups. `<groupseq>` is a sequence; its children are `<kwd>` for a
     literal keyword, `<delim>` for a delimiter, `<var>` for something the
     reader supplies. The outer sequence is *CommandName* `:` then an
     optional inner sequence, `importance="optional"`, of *Name* `=`
     *Value*.
   - `<repsep>` in the inner sequence is the separator between
     repetitions of that group. It is empty here because the separator
     is a space, and the project's formatter trims whitespace-only text.
     It comes *first* inside the `<groupseq>` it applies to, before the
     keywords and variables: the content model of a group is a title,
     then an optional `<repsep>`, then the items. The demo at the end
     puts it in the wrong place.
   - `<synph>` is the inline form: a syntax phrase with the same `<kwd>`,
     `<delim>` and `<var>` children, for a one-line synopsis in running
     text.

3. **Read the parameter list**
   - `<refsyn>` is the reference section for syntax; a `<reference>` body
     may hold it beside plain `<section>`s.
   - `<parml>` is a definition list for parameters: each `<plentry>` has
     a `<pt>` (the term) and a `<pd>` (its description). Inside them,
     `<cmdname>` is the command, `<parmname>` a named parameter,
     `<varname>` a value the reader supplies, `<option>` a switch that
     selects a behaviour, `<codeph>` a literal value.
   - Everything a reader types or reads on a screen gets the element
     that says what it is, and a stylesheet can render `<cmdname>` in
     bold and `<varname>` in italic without you deciding it here.

4. **Read the messages**
   `<msgph>` is a message quoted inline; `<msgblock>` is a message as a
   block, whitespace preserved like `<codeblock>`; `<msgnum>` is the
   message's identifier, here the status word `Failed!`. `<apiname>` is
   the name of a function, class or method in an API.

5. **Read the properties table**
   `<properties>` is a reference table with three fixed columns:
   `<proptype>`, `<propvalue>` and `<propdesc>`, with an optional
   `<prophead>` naming them. Each `<property>` row carries a `platform`
   attribute, so the Windows builds from stage 13 show the Windows pipe
   and the Mac and Linux builds the other. `<filepath>` with a `<varname>`
   inside it names the pipe with the user id as a variable.
::::

## Step 2: The task and the sample

::::steps
1. **Create `samples/export-mp3.py`**

   ```text title="samples/export-mp3.py"
   #!/usr/bin/env python3
   """Export the open Audacity project as MP3 through mod-script-pipe."""
   import sys

   TO_PIPE = "/tmp/audacity_script_pipe.to." + str(1000)
   FROM_PIPE = "/tmp/audacity_script_pipe.from." + str(1000)

   def send(command: str) -> str:
       with open(TO_PIPE, "w") as to_pipe:
           to_pipe.write(command + "\n")
       with open(FROM_PIPE) as from_pipe:
           return from_pipe.read()

   if __name__ == "__main__":
       target = sys.argv[1] if len(sys.argv) > 1 else "episode.mp3"
       print(send("SelectAll:"))
       print(send("Normalize: PeakLevel=-1"))
       print(send(f"Export2: Filename={target} NumChannels=1"))
   ```

2. **Create `topics/exporting-from-a-script.dita`**

   ```xml title="topics/exporting-from-a-script.dita"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE task PUBLIC "-//OASIS//DTD DITA Task//EN" "task.dtd">

   <task id="exporting-from-a-script">
     <title>Exporting from a script</title>
     <shortdesc>Enable mod-script-pipe, then normalize and export the open project from a Python script.</shortdesc>
     <prolog>
       <author type="creator">The Audacity tutorial team</author>
       <critdates>
         <created date="2026-03-01"/>
       </critdates>
       <metadata>
         <audience type="user" experiencelevel="expert"/>
         <category>automation</category>
         <keywords>
           <keyword>scripting</keyword>
           <keyword>Python</keyword>
           <keyword>export</keyword>
           <indexterm>scripting<indexterm>from Python</indexterm></indexterm>
         </keywords>
       </metadata>
     </prolog>
     <taskbody>
       <prereq>
         <p>Python 3 is installed, and a project is open in <keyword keyref="product-name"/>.</p>
       </prereq>
       <steps>
         <step>
           <cmd>Choose <menucascade><uicontrol>Edit</uicontrol><uicontrol>Preferences</uicontrol><uicontrol>Modules</uicontrol></menucascade>, set <uicontrol>mod-script-pipe</uicontrol> to <uicontrol>Enabled</uicontrol>, and restart <keyword keyref="product-name"/>.</cmd>
         </step>
         <step>
           <cmd>Save the sample script as <filepath>export-mp3.py</filepath>.</cmd>
           <info>
             <codeblock outputclass="language-python"><coderef href="../samples/export-mp3.py"/></codeblock>
           </info>
         </step>
         <step>
           <cmd>Run it, naming the output file.</cmd>
           <info>
             <screen><userinput>python3 export-mp3.py episode.mp3</userinput>
   <systemoutput>BatchCommand finished: OK
   BatchCommand finished: OK
   BatchCommand finished: OK</systemoutput></screen>
           </info>
           <steptroubleshooting>
             <p>If the script hangs on the first command, the pipe does not exist: <keyword keyref="product-name"/> is not running or the module is not enabled.
             On Linux, <cmdname>ls</cmdname> <filepath>/tmp/audacity_script_pipe.*</filepath> shows whether the pipes are there.</p>
           </steptroubleshooting>
         </step>
       </steps>
       <result>
         <p>The project is normalized to -1 dB and exported as mono MP3.
         Change <parmname>NumChannels</parmname> to <codeph>2</codeph> for stereo.</p>
       </result>
     </taskbody>
     <related-links>
       <link href="scripting-reference.dita"/>
     </related-links>
   </task>
   ```

3. **Read the coderef**
   `<codeblock outputclass="language-python"><coderef href="../samples/export-mp3.py"/></codeblock>`
   is the whole listing. `<coderef>` is an empty element that, at build
   time, is replaced by the text of the file it points at, relative to
   the topic. The script lives once, in `samples/`, where it can be run
   and tested; the topic never goes stale against it. `outputclass` on
   the `<codeblock>` becomes a class on the `<pre>` in the html5 output,
   `language-python`, the convention that syntax highlighters such as
   highlight.js and Prism read. In the gate's build of the full guide the
   listing is in place:

   ```
   <pre class="pre codeblock language-python"><code>#!/usr/bin/env python3
   """Export the open Audacity project as MP3 through mod-script-pipe."""
   import sys

   TO_PIPE = "/tmp/audacity_script_pipe.to." + str(1000)
   FROM_PIPE = "/tmp/audacity_script_pipe.from." + str(1000)

   def send(command: str) -&gt; str:
       with open(TO_PIPE, "w") as to_pipe:
           to_pipe.write(command + "\n")
       with open(FROM_PIPE) as from_pipe:
           return from_pipe.read()

   if __name__ == "__main__":
       target = sys.argv[1] if len(sys.argv) &gt; 1 else "episode.mp3"
       print(send("SelectAll:"))
       print(send("Normalize: PeakLevel=-1"))
       print(send(f"Export2: Filename={target} NumChannels=1"))</code></pre>
   ```

4. **Read the screen**
   `<screen>` is a block for what a terminal shows, whitespace preserved.
   Inside it, `<userinput>` is what the reader typed and `<systemoutput>`
   what came back, so a stylesheet can tell them apart. The
   `<steptroubleshooting>` from stage 04 covers the one way the step
   fails, with `<cmdname>` and `<filepath>` for the check.

5. **Edit the maps**

   ```diff title="audacity-guide.ditamap"
   --- a/audacity-guide.ditamap
   +++ b/audacity-guide.ditamap
   @@ -61,6 +61,10 @@
        <topicref href="topics/podcast-production-workflow.dita" keys="podcast-workflow"/>
      </topichead>
    
   +  <topichead navtitle="Automation">
   +    <topicref href="topics/exporting-from-a-script.dita"/>
   +  </topichead>
   +
      <topichead navtitle="Reference" id="reference">
        <topicref href="topics/supported-audio-formats.dita" keys="formats" locktitle="yes">
          <topicmeta>
   @@ -69,6 +73,7 @@
        </topicref>
        <!-- One file, two topics: by-topic gives each its own page. -->
        <topicref href="topics/effects-reference.dita" keys="effects" chunk="by-topic"/>
   +    <topicref href="topics/scripting-reference.dita" keys="scripting"/>
      </topichead>
      <topichead navtitle="Glossary">
        <topicref href="topics/glossary/audio-units.dita"/>
   ```

   ```diff title="podcaster-guide.ditamap"
   --- a/podcaster-guide.ditamap
   +++ b/podcaster-guide.ditamap
   @@ -35,6 +35,8 @@
      <topichead navtitle="Reference">
        <topicref href="topics/supported-audio-formats.dita" keys="formats"/>
        <topicref href="topics/effects-reference.dita" keys="effects" chunk="by-topic"/>
   +    <topicref href="topics/scripting-reference.dita" keys="scripting"/>
   +    <topicref href="topics/exporting-from-a-script.dita"/>
        <topicref href="topics/glossary/audio-units.dita"/>
        <topicref href="topics/glossary/g-compression.dita"/>
        <topicref href="topics/glossary/g-normalization.dita"/>
   ```

   ```diff title="audacity-book.ditamap"
   --- a/audacity-book.ditamap
   +++ b/audacity-book.ditamap
   @@ -68,6 +68,9 @@
      <appendix href="topics/recording-is-silent.dita" keys="silent"/>
      <appendix href="topics/supported-audio-formats.dita" keys="formats"/>
      <appendix href="topics/effects-reference.dita" keys="effects"/>
   +  <appendix href="topics/scripting-reference.dita">
   +    <topicref href="topics/exporting-from-a-script.dita"/>
   +  </appendix>
    
      <backmatter>
        <booklists>
   ```

   The full guide gets an *Automation* head for the task and the
   reference under *Reference* with the key `scripting`; the podcaster
   guide takes both under its *Reference*; in the book the reference is a
   fourth appendix with the task as its section. The beginner guide does
   not get them.

6. **Edit `topics/podcast-production-workflow.dita`**

   ```diff title="topics/podcast-production-workflow.dita"
   --- a/topics/podcast-production-workflow.dita
   +++ b/topics/podcast-production-workflow.dita
   @@ -49,7 +49,7 @@
            <li><b>Editing</b>: trim mistakes, long pauses and false starts.</li>
            <li><b><term keyref="gl-normalization">Normalization</term></b>: bring the peak level to -1.0 dB.</li>
            <li><b><term keyref="gl-compression">Compression</term></b>: even out volume differences (threshold -18 dB, ratio 3:1).</li>
   -        <li><b>Export</b>: MP3 at 128 kbps mono for distribution.</li>
   +        <li><b>Export</b>: MP3 at 128 kbps mono for distribution, by hand or from a script (<xref keyref="scripting"/>).</li>
          </ol>
        </section>
        <section audience="beginner">
   ```

   The `scripting` key is used the stage it is defined.
::::

## Step 3: README and the gate

::::steps
1. **Change the "You are on" line and the layout**

   ````diff title="README.md"
   --- a/README.md
   +++ b/README.md
   @@ -5,7 +5,7 @@ branch is the previous branch plus one lesson, from a single concept topic to a
    complete, publish-ready **Audacity User Guide**. The diff between two branches
    is the lesson.
    
   -You are on **stage 20 — hazards and safety**: hazard statements with a message panel and a symbol, reused by conkeyref.
   +You are on **stage 21 — software domains**: commands, parameters, messages, a syntax diagram, a properties table and code pulled from a file.
    
    ## Stages
    
   @@ -50,6 +50,7 @@ subject-scheme.ditamap  the controlled values for platform and audience
    .dogsbay/config.xml   shared editor project settings (project type, framework, default map and deliverable, format style)
    scripts/              the gate
    topics/               topics
   +samples/              code samples pulled into topics by coderef
    shared/               warehouses: content pulled in by conref (common-steps, common-notes)
    images/               illustrations referenced by <image>
    ```
   ````

2. **Format and check**

   ```bash
   dogsbay-xml format -i topics/*.dita *.ditamap
   scripts/check-stage.sh
   ```

   ```
   == validate-project  /home/you/audacity-guide ==
   41 file(s): 41 valid, 0 invalid.

   == project-health  /home/you/audacity-guide ==
   Root map: audacity-guide.ditamap (project config); house rules: none (none configured)
   Project is healthy: valid, no broken references, keys, orphans, or broken element ids.

   == validate-conditions  /home/you/audacity-guide  (scheme: subject-scheme.ditamap) ==
   41 file(s): 41 pass, 0 with violations.

   == build  project.json -> /tmp/check-stage-<pid> ==
   all deliverables built

   STAGE OK
   ```
::::

Now put the repetition separator where it reads naturally. Move the
`<repsep>` line out of the inner `<groupseq>` to directly after
`<delim>:</delim>` in the outer one, and run the gate with `SKIP_BUILD=1`:

```
== validate-project  /home/you/audacity-guide ==
/home/you/audacity-guide/topics/scripting-reference.dita:
  39:20  error: The content of element type "groupseq" does not match its content model.
41 file(s): 40 valid, 1 invalid.
FAIL: validation errors
```

A `<repsep>` is the first child of the group it applies to, or it is
nowhere. The message names the element it found instead of what the model
allows.

## What you learned

- The programming domain: `<syntaxdiagram>`, `<groupseq>`, `<kwd>`,
  `<delim>`, `<var>`, `<repsep>`, `<synph>`; `<parml>`, `<plentry>`,
  `<pt>`, `<pd>`; `<parmname>`, `<option>`, `<apiname>`, `<codeph>`;
  `<codeblock outputclass="language-…">` with `<coderef>`.
- The software domain: `<cmdname>`, `<varname>`, `<msgph>`, `<msgblock>`,
  `<msgnum>`, `<screen>`, `<userinput>`, `<systemoutput>`.
- `<refsyn>` and `<properties>` with `<prophead>`, `<property>`,
  `<proptype>`, `<propvalue>`, `<propdesc>`, and a profiling attribute
  on a row.
- A `<repsep>` comes first in its group.
- Code lives in a file and the topic references it.

## Where to go next

:::cards
- **[Stage 22: Learning](/part-4-books/stage-22-learning)** {icon="arrow-right"}
  A learning assessment with four kinds of question, and what the gate
  has to do about the Learning and Training DTDs.

- **[Compare 20 to 21 on GitHub](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/20-hazards-and-safety...tutorial/21-software-domains)** {icon="github"}
  Exactly what this stage added.
:::
