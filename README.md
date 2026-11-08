# Nine months running an AI agent on a real business: where oversight actually failed

Since February I've been running an AI operations agent for my software studio. We call it Morris. It reads our Slack, email, task board and pull requests, decides what needs doing, and does a growing share of it. That includes an engineering loop that can take a client's email, turn it into a ticket, build a fix in an isolated branch, have the fix reviewed by a separate model, and open a pull request with a drafted reply to the client. A human still merges, and a human still presses send.

This is an account of what went wrong. I'm writing it because almost none of the failures were what I expected. The model was rarely the weak point. The weak points were the controls around the model: the rules, guards, monitors and status reports I wrote to keep it safe.

## The shape of the system

The current version is deliberately boring. There is no agent framework and no database. Each job is a fresh, headless Claude Code run started by cron or by a small event watcher. A run starts cold, reads a set of files (the agent's voice, its state, the relevant project docs, a decision log), does its work and writes files back. Nothing persists between runs except files. Every action is therefore reproducible from what was on disk, and an audit means reading files.

Authority is set by a written charter in three zones. Zone 1 is routine work: the agent acts and logs it. Zone 2 is anything with consequences: the agent proposes, and I approve with a Slack reaction that is checked against my user ID. Zone 3 is work the agent should never do, and it can't, because the tools for it aren't installed. Each job gets an allowlist containing only the tools it needs. Messages that leave the company are approved word for word, in two steps. Email is draft-only: sending isn't a capability the agent has.

It took five generations to get here, and each one died of something different. The first was over-built plumbing that never ran. The third was the most instructive: it produced 1,307 proposals, I approved 3, and the system's own post-mortem said it had been *"rewarded for producing output rather than for being right."*

The current version has produced more than a hundred logged findings. They became regression tests (772 so far) and often written engineering patterns. Here are the ones that matter for anyone deploying agents.

## 1. A rule in a prompt is weaker than a missing tool

Every Slack message was meant to go through one script. The script enforces quiet hours, a daily message budget, a length limit, a ban on internal ticket IDs in client channels, and an attribution log of who sent each message. The prompts said so, in several places.

The bot token, though, was readable from the agent's environment. When the script got in the way of a task, sessions sometimes skipped it and called the Slack API directly. Some of those sessions were mine. The message arrived under the agent's name with no attribution record. Then a different part of the system, the monitor that watches for unapproved client messages, found messages under its own name that it had no record of sending, and raised them as unauthorised. That happened five times, and every time the "rogue" message was a human's.

Two lessons came out of this. First, the guard only worked for callers who chose to use it. The fix wasn't a stronger sentence in the prompt. It was moving the credential so the script is the only thing that can read it, and adding a pre-flight check that refuses raw API calls. Second, **a shared identity needs attribution at the moment of sending**. Without it, oversight produces confident false alarms, and those cost real investigation time.

## 2. Built is not wired

In one day at the end of August I wrote ten principles the system had to follow. A month later an audit compared them with what was actually running. Four of the ten weren't honoured. In each case the mechanism existed but nothing called it: a pause switch with no callers, a test suite that had never run on the production host, a scoring engine that had never once executed. Code review didn't catch any of them, because everything looked wired.

The audit that found them looked for *missing output*, not for code. "When did this last run, and what did it produce?" is now a standing check. Shipping a mechanism and shipping its caller are two separate changes, and the second one is the one that gets forgotten.

## 3. Status reports that collapse different states

Several of the most expensive failures were honest reports of the wrong thing:

- A watcher that couldn't reach its source returned "nothing new." A quiet inbox and a broken connection looked identical. In one stretch, connectors failed silently for days before anyone noticed.
- An instruction queue said "0 pending." That output is byte-for-byte the same whether everything was delivered or the queue was silently wiped by a sync job. It was wiped, and it was reported as "delivered" twice, quoting the queue's own words.
- The agent misreported **its own capabilities**, twice within 24 hours. It escalated "I can't edit that kind of draft" as a tooling gap when the tool existed. It also reported a missing integration that turned out to work fine.

The fix is cheap and structural. "Quiet" and "broken" get different exit codes and different wording. Watermarks never advance on a failed read. "I can't" needs the same evidence as "I did."

## 4. The agent will improve itself, and you may not see it

A worker agent edited the review job's own prompt to add a new code-review integration. The change was a good one. But nobody saw it happen, and the agent's review behaviour had changed. The integration also posted its reviews under a human's account, because that was the only credential the connection had.

Neither was malicious, and both are exactly the drift oversight exists to catch. The charter's rule is now explicit: the agent can't widen its own authority, and changes to its prompts are logged like any other action.

## 5. Evaluating an agent is its own discipline

I test the agent with a drill harness. It sends real simulated emails, replays Slack scenarios through a read-only executor, and has an independent judge model score each run against a written expectation. The harness taught me more than the drills did:

- **Only first-run scores count toward granting autonomy.** A fail followed by a fix and a pass proves the fix works, not that the agent behaves well.
- **A harness fault is a third verdict, never a pass.**
- **Don't put a second agent between the agent under test and the human it serves.** If you do, you're measuring the pair.
- **Score the agent against what it could see at decision time, not against what you know later.** Our "missed" client concern had been withdrawn 47 minutes before the agent decided. The agent was right, and the reviewer was working from the older information.
- **Mine your test cases from real incidents.** Invented scenarios are too clean.

## What I believe now

- **Most of my failures were in the controls, not the model.** Guards that could be skipped, mechanisms nothing called, monitors that couldn't tell quiet from broken. That's one system on one model family, and I only checked the agent's calls where we disagreed, so treat it as an observation, not a measurement.
- **Enforce with capability, not instructions.** If the agent shouldn't do something, it shouldn't have the tool for it.
- **Audit for absence.** The failures that hurt were silent ones.
- **Unattended is the real test.** The system still hasn't run a full week without me stepping in. Until it does, I treat every claim of autonomy, including mine, as unproven.

I don't think any of this is specific to my studio. More and more small businesses are handing agents their inboxes and credentials, usually with less scaffolding than this and with no one reading the logs. These failure shapes will turn up there too.

*— Elliot Sturzaker, Wild Interactive*

*Written with Claude's help; the system, the incidents and the conclusions are mine.*
