# Give judgement a mechanical floor, or it produces nothing

## The shape

You build a tracking mechanism (a ledger, a register, a checklist) and tell the agent to fill it in: "open a row whenever someone asks you for something." The instruction is clear and correct.

Then you check, and the ledger is empty. Not wrong, empty. Nothing errored, and the emptiness looks exactly like a quiet period.

## What happened

Three instruments shipped into the agent on the same day:

| Instrument | How it is driven | After 14 hours |
|---|---|---|
| Receipt log | a script writes it when work is dispatched | working, 3 rows |
| Fact sheet | the prompt runs a script | working, generated on schedule |
| Request ledger | judgement only: "open a row when asked" | empty |

In between, a five-item client email arrived and was classified, specified and worked on. The request ledger stayed at zero. The two mechanical instruments fired. The judgement-driven one never happened at all, and the instruction had been written by someone who had documented this exact failure twice that week.

The first fix made it worse in a quieter way. A floor was added to one of the three ways requests arrive. Requests from the other two still relied on judgement, and the ledger stayed nearly empty for two more days. Now the emptiness looked like the rule working.

## The rule

**Anything left entirely to judgement gets a mechanical floor, or it gets nothing.**

The floor is deliberately crude: one row per inbound item, built from data already to hand. It will often be wrong: undercounted, roughly worded, imperfectly attributed. That's acceptable, because wrong is visible and absent isn't. A thin row invites correction. Zero rows invite nothing.

Two things keep a floor from replacing the judgement it backs up:

- **Call it a floor in the instruction**, next to the judgement: "one row is automatic; a five-item email is five requests, and splitting them is still your job."
- **Make the floor undercount, never merge.** An obviously thin row invites correction. A plausible merged row hides what it swallowed.

And **floor every path**. If requests can arrive three ways and only one is floored, an empty store still means "not instrumented" rather than "nothing owed", which is the ambiguity the floor was meant to remove.

**The test:** for every rule of the form "the agent should record or track X", ask what the store looks like if the agent simply never does it, and how long it would take anyone to notice. If the answers are "identical to a quiet day" and "indefinitely", it needs a floor.
