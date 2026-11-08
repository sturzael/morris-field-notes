# In an unattended system, a question is a failure

## The shape

A scheduled, unattended job ends its run by asking for something:

> "The remote workbench tool needs explicit permission before I can use it. Could you approve that tool use so I can continue?"

> "I haven't received a specific task yet. What would you like me to do?"

## What happened

Both came from real runs of two unrelated jobs. Both exited 0. Neither did any work, and neither reported that it hadn't. One was a monthly finance job that did nothing for weeks. The other was, for a while, 13% of all runs: each one a full model call that produced a question nobody would answer.

In an interactive session, asking before acting is careful behaviour, and it looks like diligence in a log. But nobody is there. The run is over and nothing happened. The process exited cleanly, so every caller, exit-code check and dashboard recorded a success. The politeness hid the failure.

One cause of the "what would you like me to do?" form turned up in the same system. A long shared rules preamble was placed before every task, and the instruction to act sat well below it. The model read the whole thing as a document to review, summarised the rules back, and asked what to do. Putting a short imperative at the very top did, and it named the situation outright: you are running headless, there is no human here to answer, and asking a question ends the run having done nothing.

## The rule

**Any run whose final act is a question has failed.** Detect it and treat it that way:

- Check only the closing lines, so runs that merely quote a question don't match.
- Mark the run failed. Don't let the exit code stand.
- Log what it asked for as a named capability gap. "Job X stops because it needs permission Y" can be acted on. "Job X produced nothing" can't.
- Escalate once, plainly: the job did nothing, and has been silently doing nothing.

Then fix the design. If a headless job can need permission at runtime, either the permission must be standing or the job must be changed so it never needs to ask. A prompt that sometimes needs human approval isn't an unattended job. It's an interactive job on a timer, and it will fail whenever nobody is watching.

Tell the model there is no human. It can't infer that, and otherwise "ask if unsure" looks like the safer choice.

**The tell:** a question mark in the last line of a job log. A well-behaved unattended run ends with a statement of what it did.
