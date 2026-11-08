# An action in an agent's interface instructs that agent

## The shape

A human clicks an option an agent offered: "Review it for me", "Draft a reply", "Chase this". A second agent, or an operator session overseeing the system, sees the request and does the work directly, because that's quicker and it isn't clear what is blocking the first agent.

The work gets done, and the system is now wrong in three silent ways:

1. **No record.** The instructed agent has nothing in its logs. The item is still open to it, so it raises it again, which reads as nagging.
2. **No attribution.** The output exists under the wrong name. If it is then presented as the agent's work, nobody can say who actually did it.
3. **No measurement.** Whatever stopped the agent is still there, now hidden behind a human success. The one clean signal, whether the agent could have done it and how well, is gone.

## What happened

I clicked "Review it for me" on three pull requests. An operator session had a subagent review them instead. The reviews were good, but none of it reached the agent's records, and the three requests stayed open.

The reason given for stepping in was also wrong. The assumed blocker was "no access to the code host." The real blocker was that the review job checked the wrong remote, a mirror with no open pull requests, so the job had never started for that client. Access had been granted weeks earlier. Doing the work by hand would have hidden that.

A day later the same thing happened in a quieter form. I asked whether the agent had finished something. The operator checked the mailbox and logs directly and gave a correct answer within minutes. That correct answer hid the real defect: the agent couldn't report on that item, because the link it used to track the work had been overwritten.

## The rule

1. **Route it, don't absorb it.** The deliverable is "the agent did the work", not "the work exists". After you act, will the agent's own records show what happened?
2. **Check the reported blocker before working around it.** "It can't do X" is a claim to verify. Check the permission, the connection, and the input the job actually reads.
3. **The operator removes blockers and never does the task.** Credentials and misdirected inputs are the operator's job. The judgement the agent exists to apply is not.
4. **If the wrong hand already did the work, don't import it.** Keep it as a benchmark.
5. **When asked for a status the agent should own, ask what the agent reported.** If its answer is wrong or missing, that is the finding.
