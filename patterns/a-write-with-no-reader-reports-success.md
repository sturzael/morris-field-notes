# A write with no reader reports success

## The shape

A handler writes a record and reports success. The write really does succeed: the row lands, the API returns 200, the screen shows a confirmation. But nothing reads the store it wrote to, so the thing the user was promised never happens.

Every component works. Only the purpose failed, and nothing notices, because the promise and its delivery came from the same code path.

## What happened

An admin screen let me answer the agent's open questions. The handler wrote each answer to an activity log and the screen said "Recorded." The activity log was a display surface and nothing consumed it. Eleven answers sat there, including a typed instruction, each marked "Recorded", and the agent never saw any of them.

When a real delivery path was built, it only carried new answers. The old ones stayed stranded, so the bug became harder to see, not gone.

It came back later in another form. On the live host, answers went to a built-in fallback location because one setting was unset, while the agent read a different file. Neither side raised an error.

**The same shape, pointed at the operator.** The agent kept a ledger of capability gaps: walls it had hit, one entry per wall, written so that a human would act on them. When I finally looked, it held 66 open entries out of 82 ever recorded, going back weeks. Nobody had read it. The agent was writing diligently into a channel with no reader. A self-report channel nobody reads makes the reports worthless, and the first sign is a count that only goes up.

## The rule

1. **Name the reader before writing the writer.** If you can't say which process reads this record, and when, you are building a sink.
2. **Derive the confirmation from the reader, not the writer.** "Recorded" should come from the delivery path, not from the write returning 200.
3. **A new delivery path for an existing record type needs a backfill in the same change.** Forward-only fixes turn a known bug into an invisible one.
4. **Make the honest state visible.** "Recorded, not yet picked up" should be something the screen can say, driven by real state and never by a timer.
5. **When writer and reader live in different processes, test them together.** In a later build, each component's tests passed on its own. The first end-to-end run found eleven breaks, four of them this pattern.
6. **Give agent self-reports a reader**, and ask "still true?" on a schedule.

**The tell:** for any "saved", "recorded" or "noted" message, ask what process reads it and what the user will see when it does.
