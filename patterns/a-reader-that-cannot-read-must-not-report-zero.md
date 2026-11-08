# A reader that cannot read must not report zero

## The shape

A watcher has two kinds of silence, and from outside they look identical: the source is quiet, or the watcher could not reach the source. Collapse them and the system reports "nothing to do" with full confidence while a credential is missing, an API is down or a permission was revoked. Nothing errors, so nobody looks.

## What happened

Two watchers, a week apart, met the same choice.

One read a chat workspace the agent had not been invited to. Built naively, it would have returned an empty list and the summary would have said "nothing new." Instead it exits non-zero, says why, and leaves its watermark where it was, so the next run re-reads the window rather than skipping it. Its own code comment gives the reason: a watch that goes quiet is indistinguishable from a quiet workspace.

The other watched a client's project board. The host running it had never been given the credential for that board. An empty result would have read as "the client has raised nothing", which is exactly the false reassurance the watcher was built to remove. It exits with an error, says what is missing, and uses a separate exit code for the genuinely quiet case.

These were the good outcomes. Elsewhere in the same system, connectors failed silently for days before anyone noticed, which is why the rule exists.

## The rule

1. **Give "quiet" and "broken" different exit codes and different words.** Quiet is normal and should be cheap and silent. Broken is an error and must be loud. If a caller can't tell them apart from the exit status alone, the distinction doesn't exist.
2. **Never advance a watermark on a failed read.** A watermark claims the window was consumed. A failed read consumed nothing.
3. **Say what is now missing, not just that something failed.** "No credential for the board; cannot read it; watermark untouched" tells the operator the blast radius. "Error" doesn't.
4. **An agent relaying a watcher's result must relay its failures too.** A summary that says "nothing new from the client" while its reader failed has told the human something false. Put the unreadable state in the same sentence as the zero.
5. **Check the reader's health, not only the stream's.** A heartbeat written before the decision ("I ran and found nothing") separates an idle watcher from a dead one.

**The tell:** a monitor that has said "all clear" for an unusually long stretch while its credential, permissions or host changed. Or a watcher whose code has no branch that could ever print an error.
