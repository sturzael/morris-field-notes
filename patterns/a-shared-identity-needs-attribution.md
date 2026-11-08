# A shared identity needs attribution

## The shape

A human and one or more automated processes all act through the same identity: a bot user, a service account, a shared token. It works until someone asks who did something.

Then the identity gives the same answer for every actor. A missing approval record reads as "an unauthorised actor did this" when it really means "nothing here records who acts." The gap looks exactly like a breach.

## What happened

**Over-accusing the agent.** The agent posted a warning that client-facing messages had gone out under the company's name with no approval and no log entry. It raised three such alarms and proposed removing whatever was doing it from the client channel. An operator session investigated, matched the chat user to the agent's bot, and agreed: the agent was sending unapproved messages and didn't recognise its own sends.

Both were wrong. I send through that same bot identity from my terminal while mid-task. The agent's own decision log had no entry for those sends because it hadn't made them. The logging recorded who the caller was for refused posts, and recorded nothing for a post that succeeded.

The detector failed both ways: every human send looked unauthorised, and a genuinely unapproved send would have produced identical evidence.

**Over-crediting the agent.** The fix shipped: successful sends now record the kind of caller, and one command answers "who sent this?" Six weeks later an agent reviewing the channel found a message of mine and read it as proof that the ops agent had completed a delivery loop on its own. It built three conclusions on that and reported them. The lookup would have answered correctly on the first try. Nobody ran it until I asked.

The cause was the same both times: the display name was treated as the author.

## The rule

1. **Log the actor on success, not just on failure.** The successful path is the one someone will later need to audit.
2. **Record the kind of caller**: interactive human, model-driven, scheduled script. That one field turns an unanswerable question into a trivial one.
3. **Prefer attribution to a gate.** Blocking unattributed actions on a shared identity also blocks the human. Make it visible before making it forbidden.
4. **When a security-shaped alarm has no attribution behind it, suspect the instrumentation first.**
5. **A claim about who acted isn't evidence until the attribution log has been checked.** On a shared identity, the display name tells you the channel, not the author.

**The smell:** an alarm whose evidence is the absence of a record, on a system where you can't point to the code that would have written that record on success.
