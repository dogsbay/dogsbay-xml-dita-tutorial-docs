---
title: "Stage 22: Software domains"
description: Document a command-line interface with the software and programming domains, a syntax diagram, a parameter list, messages, and code pulled from a file.
type: tutorial
---

# Stage 22: Software domains

Document the `mod-script-pipe` interface with elements from DITA's
software and programming domains. Create a scripting reference for command
syntax, parameters, messages, and platform-specific pipe locations.

Create a task that references `samples/export-mp3.py` through
`<coderef>`. Mark up a terminal session with `<screen>`,
`<userinput>`, and `<systemoutput>`.

**Optional module.** See [Choose a learning path](/start-here/learning-path)
for the starting checkpoint and the next core lesson.

**Time:** about 35 minutes.
**You need:** stage 21 complete.


Recorded output below is an example. File counts, paths, and stage numbers can differ. [Check your work](/start-here/run-the-gate) to see the result for your own project.

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
         <p>The <apiname>send</apiname> function in the sample script (<filepath>export-mp3.py</filepath>, listed in <xref href="exporting-from-a-script.dita"/>) returns the whole reply; check it for <msgnum>Failed!</msgnum> before sending the next command.</p>
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

   The *Responses* section names the sample script with `<filepath>` and
   links to the task that lists it, `exporting-from-a-script.dita`. The
   link goes to the task and not to the `.py` file, because the build
   does not copy `samples/` into the output: the script reaches the
   reader as the listing that `<coderef>` pulls into the task.

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
     selects a behavior, `<codeph>` a literal value.
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
   attribute, so a filtered build keeps only the rows for its platform.
   No Windows-filtered deliverable includes the scripting reference: the
   `podcaster-linux` build drops the Windows row, and the unfiltered builds
   show both rows. `<filepath>` with a `<varname>`
   inside it names the pipe with the user id as a variable.
::::

## Step 2: The task and the sample

::::steps
1. **Add `samples/export-mp3.py`**
   The task in the next step pulls this Python script into its listing.
   In the **Explorer**, right-click `my-audacity-guide`, choose
   **New Folder**, and enter `samples`. Then add the script to the new
   folder in one of these ways:

   - Add the file from the checkpoint `tutorial/22-software-domains` of
     the [sample project](/start-here/set-up#the-sample-project). Download
     the file from its
     [GitHub page](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/blob/tutorial/22-software-domains/samples/export-mp3.py),
     or save it from a copy of the sample project on that branch. Then
     right-click `samples` in the **Explorer** and choose
     **Add Files...**. In the file chooser, use **Look In** to go to the
     folder that holds `export-mp3.py`, select the file, and click
     **Add**. The editor copies the file into `samples`. You can also drag
     the file from your desktop onto `samples` in the **Explorer**.
   - Right-click `samples`, choose **New File**, and enter
     `export-mp3.py`. The editor creates the file empty. Type or paste the
     listing, and save the file.

   ```text title="samples/export-mp3.py"
   #!/usr/bin/env python3
   """Export the open Audacity project as MP3 through mod-script-pipe."""
   import os
   import sys
   
   if sys.platform == "win32":
       TO_PIPE = "\\\\.\\pipe\\ToSrvPipe"
       FROM_PIPE = "\\\\.\\pipe\\FromSrvPipe"
       EOL = "\r\n\0"
   else:
       TO_PIPE = f"/tmp/audacity_script_pipe.to.{os.getuid()}"
       FROM_PIPE = f"/tmp/audacity_script_pipe.from.{os.getuid()}"
       EOL = "\n"
   
   
   def send(to_pipe, from_pipe, command: str) -> str:
       """Send one command and return the reply, which ends with an empty line."""
       to_pipe.write(command + EOL)
       to_pipe.flush()
       reply = ""
       while True:
           line = from_pipe.readline()
           if line == "" or (line == "\n" and reply):
               return reply
           reply += line
   
   
   if __name__ == "__main__":
       target = sys.argv[1] if len(sys.argv) > 1 else "episode.mp3"
       with open(TO_PIPE, "w") as to_pipe, open(FROM_PIPE) as from_pipe:
           for command in ("SelectAll:", "Normalize: PeakLevel=-1",
                           f"Export2: Filename={target} NumChannels=1"):
               print(send(to_pipe, from_pipe, command).strip())
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
             <p>If the script stops with <msgph>FileNotFoundError</msgph>, the pipe does not exist: <keyword keyref="product-name"/> is not running or the module is not enabled.
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
   the `<codeblock>` becomes a class on the `<pre>` in the HTML5 output,
   `language-python`, the convention that syntax highlighters such as
   highlight.js and Prism read. In the check's build of the full guide,
   `out/full/topics/exporting-from-a-script.html`, the listing is in
   place:

   ```
   <pre class="pre codeblock language-python"><code>#!/usr/bin/env python3
   """Export the open Audacity project as MP3 through mod-script-pipe."""
   import os
   import sys

   if sys.platform == "win32":
       TO_PIPE = "\\\\.\\pipe\\ToSrvPipe"
       FROM_PIPE = "\\\\.\\pipe\\FromSrvPipe"
       EOL = "\r\n\0"
   else:
       TO_PIPE = f"/tmp/audacity_script_pipe.to.{os.getuid()}"
       FROM_PIPE = f"/tmp/audacity_script_pipe.from.{os.getuid()}"
       EOL = "\n"


   def send(to_pipe, from_pipe, command: str) -&gt; str:
       """Send one command and return the reply, which ends with an empty line."""
       to_pipe.write(command + EOL)
       to_pipe.flush()
       reply = ""
       while True:
           line = from_pipe.readline()
           if line == "" or (line == "\n" and reply):
               return reply
           reply += line


   if __name__ == "__main__":
       target = sys.argv[1] if len(sys.argv) &gt; 1 else "episode.mp3"
       with open(TO_PIPE, "w") as to_pipe, open(FROM_PIPE) as from_pipe:
           for command in ("SelectAll:", "Normalize: PeakLevel=-1",
                           f"Export2: Filename={target} NumChannels=1"):
               print(send(to_pipe, from_pipe, command).strip())</code></pre>
   ```

4. **Read the script**
   The script follows the pipe example that Audacity publishes for
   `mod-script-pipe`:

   - Audacity creates two pipes, one for commands and one for replies.
     On Linux and macOS they are
     `/tmp/audacity_script_pipe.to.<uid>` and
     `/tmp/audacity_script_pipe.from.<uid>`, where `<uid>` is your user
     id, which `os.getuid()` returns. On Windows they are the named pipes
     `\\.\pipe\ToSrvPipe` and `\\.\pipe\FromSrvPipe`, written with
     doubled backslashes in a Python string. Each command on Windows ends
     with `\r\n\0`, and on the other platforms with `\n`.
   - The script opens both pipes once, in one `with` statement, and
     sends the three commands through them.
   - Each reply is the command's output, the status line, and then an
     empty line. `send()` reads the reply line by line and returns when
     it reaches that empty line. A single `read()` would wait for the end
     of the file, and that never comes while Audacity holds the pipe
     open, so the script would hang after the first command.

5. **Read the screen**
   `<screen>`, from the user interface domain, is a block for what a
   terminal shows, whitespace preserved. Inside it, `<userinput>` and
   `<systemoutput>`, from the software domain, mark what the reader typed
   and what came back, so a stylesheet can tell them apart. The
   `<steptroubleshooting>` from stage 06 covers the common way the step
   fails: if the pipe does not exist, opening it raises
   `FileNotFoundError`, marked with `<msgph>`. `<cmdname>` and
   `<filepath>` mark the command that checks for the pipes.

6. **Edit the maps**

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
        <topicref href="topics/effects-reference.dita" keys="effects"/>
        <topicref href="topics/effect-presets.dita"/>
   +    <topicref href="topics/scripting-reference.dita" keys="scripting"/>
      </topichead>
      <topichead navtitle="Glossary">
        <topicref href="topics/glossary/audio-units.dita"/>
   ```

   ```diff title="podcaster-guide.ditamap"
   --- a/podcaster-guide.ditamap
   +++ b/podcaster-guide.ditamap
   @@ -36,6 +36,8 @@
        <topicref href="topics/supported-audio-formats.dita" keys="formats"/>
        <topicref href="topics/effects-reference.dita" keys="effects"/>
        <topicref href="topics/effect-presets.dita"/>
   +    <topicref href="topics/scripting-reference.dita" keys="scripting"/>
   +    <topicref href="topics/exporting-from-a-script.dita"/>
        <topicref href="topics/glossary/audio-units.dita"/>
        <topicref href="topics/glossary/g-compression.dita"/>
        <topicref href="topics/glossary/g-normalization.dita"/>
   ```

   ```diff title="audacity-book.ditamap"
   --- a/audacity-book.ditamap
   +++ b/audacity-book.ditamap
   @@ -69,6 +69,9 @@
      <appendix href="topics/supported-audio-formats.dita" keys="formats"/>
      <appendix href="topics/effects-reference.dita" keys="effects"/>
      <appendix href="topics/effect-presets.dita"/>
   +  <appendix href="topics/scripting-reference.dita" keys="scripting">
   +    <topicref href="topics/exporting-from-a-script.dita"/>
   +  </appendix>
    
      <backmatter>
        <booklists>
   ```

   The full guide gets an *Automation* head for the task and the
   reference under *Reference* with the key `scripting`; the podcaster
   guide takes both under its *Reference*; in the book the reference is a
   fourth appendix with the task as its section, and carries the key
   `scripting` too, so the podcast workflow's link to it resolves in the
   PDF. The beginner guide does not get them.

7. **Edit `topics/podcast-production-workflow.dita`**

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

## Step 3: Check your work

::::steps
1. **Format and check your work**

   Format each file that you changed: in the editor, choose **XML** >
   **Format** and save the file. From the command line, format them all:

   ```bash
   dogsbay-xml format -i topics/*.dita *.ditamap
   ```

   Then check the project. In the editor, choose **Project** >
   **Check Project** and read the result in the **Project Validation**
   panel. From the command line, run:

   ```bash
   dogsbay-xml check .
   ```

   Example output:

   ```
   health   clean
   build    full                 ok  /home/you/my-audacity-guide/out/full
     5 note(s) — run with --verbose to see them
   build    beginner-mac         ok  /home/you/my-audacity-guide/out/beginner-mac
   build    beginner-windows     ok  /home/you/my-audacity-guide/out/beginner-windows
   build    podcaster-linux      ok  /home/you/my-audacity-guide/out/podcaster-linux
     1 note(s) — run with --verbose to see them
   build    review               ok  /home/you/my-audacity-guide/out/review
     4 note(s) — run with --verbose to see them
   build    install-variants     ok  /home/you/my-audacity-guide/out/install-variants
     7 note(s) — run with --verbose to see them
   build    collection           ok  /home/you/my-audacity-guide/out/collection
     18 note(s) — run with --verbose to see them
   build    book-pdf             ok  /home/you/my-audacity-guide/out/book-pdf
     WARN  /home/you/my-audacity-guide/audacity-book.ditamap  PDF rendering reported 19 warnings (12 The following feature isn't implemented by Apache FOP, yet: table-layout=… (on fo:table) (…, 2 The contents of fo:inline line n exceed the available area in the inline-progression direc…, 2 The contents of fo:block line n exceed the available area in the inline-progression direct…, and 3 other kinds)
     6 note(s) — run with --verbose to see them
   output   wrote a file, no pages to check links in book-pdf
   output   clean (full, beginner-mac, beginner-windows, podcaster-linux, review, install-variants, collection)
   Ready: the project is healthy, every deliverable built, and the output of full, beginner-mac, beginner-windows, podcaster-linux, review, install-variants, collection holds together.
   ```

   The note lines count DITA-OT notes from each build. To print them, run
   `dogsbay-xml check -v .`, or read them in the **Project Validation**
   panel.
::::

Test the position of the repetition separator. Move the
`<repsep>` line out of the inner `<groupseq>` to directly after
`<delim>:</delim>` in the outer one, and check your work. The check stops
at health. Example output:

```
health   NOT CLEAN
  invalid: /home/you/my-audacity-guide/topics/scripting-reference.dita
    39:20  The content of element type "groupseq" does not match its content model.
  (run project-health for the full report)
Not ready: the project itself has faults. The build and the built output were not checked.
```

A `<repsep>` follows the optional title and precedes the items in its
group. The message names the element it found instead of what the model
allows.

Undo the change and check again. Confirm that the check reports `Ready`
before you continue.

## What you learned

- The programming domain: `<syntaxdiagram>`, `<groupseq>`, `<kwd>`,
  `<delim>`, `<var>`, `<repsep>`, `<synph>`; `<parml>`, `<plentry>`,
  `<pt>`, `<pd>`; `<parmname>`, `<option>`, `<apiname>`, `<codeph>`;
  `<codeblock outputclass="language-…">` with `<coderef>`.
- The software domain: `<cmdname>`, `<varname>`, `<msgph>`, `<msgblock>`,
  `<msgnum>`, `<userinput>`, `<systemoutput>`.
- The user interface domain: `<screen>`.
- `<refsyn>` and `<properties>` with `<prophead>`, `<property>`,
  `<proptype>`, `<propvalue>`, `<propdesc>`, and a profiling attribute
  on a row.
- A `<repsep>` precedes the items in its group, after any title.
- Code lives in a file and the topic references it.

## Next lesson

**Checkpoint:** `tutorial/22-software-domains`. If you use Git, commit your work.

Continue with [Stage 23: learning](/part-4-books/stage-23-learning).

For the core course, use the [learning path](/start-here/learning-path) to skip optional modules.

[Compare this stage with its predecessor](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/21-hazards-and-safety...tutorial/22-software-domains).
