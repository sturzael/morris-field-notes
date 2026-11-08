# One verdict per message loses the minority signal, and re-classification destroys it

## The shape

An agent that triages inbound messages usually writes one verdict per message plus a free-text note. Two silent failures follow.

1. **A message with several items collapses to one verdict.** Real messages carry several requests. When one is resolved and another isn't, a single verdict takes the majority reading and the remaining request disappears.
2. **Re-processing overwrites what the first pass found.** A later run works out the verdict and the note from scratch, with no idea a previous record existed. A refinement can improve the verdict while deleting the detail that made the item matter.

## What happened

A client email carried three items: a broken page (later cancelled by the client), a live public page showing information that should not have been there, and a small content change the client said they would make themselves. The agent classified the whole thread as "ignore", with a note saying the client had cancelled and would fix things themselves. The note showed it had seen the exposure item. The verdict dropped it.

Hours later a second pass reclassified the thread into a daily digest, which was a better verdict. Its new note no longer mentioned the exposure item at all.

Separately, about 25 items kept a "parked: client paused" verdict after the pause had been lifted, and a note still said the client was paused while the actual flag was empty.

The tell for both failures: **the note captured what the classification could not.** When a model's prose names something its schema has no room for, the schema is wrong, and the next pass will overwrite the prose.

## The rule

1. **Split the message into requests before classifying.** One verdict per request, not per message.
2. **Classification writes add a new record that supersedes the old one, never overwrite it.** Losing the history is how a refinement becomes a regression nobody can see.
3. **A note is not a record.** Anything that matters needs its own field.
4. **When control state changes, re-check what was derived from it.** Lifting a pause must clear every item parked because of it, not just stop parking new ones.
5. **Re-processing must give the same result for the same input**: refined, never silently different.
6. **Never let the model write a fact the system can compute.** Timestamps, ids and counts should come from code. The agent was once asked for "UTC now" in a prompt and wrote local time labelled as UTC, putting first-seen times about twelve hours in the future, and a cleanup job then ran off those values. Check model-written fields against a computable bound at write time.
