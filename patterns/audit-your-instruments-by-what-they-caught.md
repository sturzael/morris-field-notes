# Audit your instruments by what they have actually caught

## The shape

Each incident produces a mechanism to stop it recurring: a guard, a ledger, a detector. Each is justified by a real failure.

Nobody counts them. The code is small and the tests pass. What builds up is surface area that can fail silently, and attention drifts from the work to the machinery.

## What happened

Sixteen instruments were built around the agent in five days. Then I counted two things.

**Which instruments had ever fired?**

| Earning their keep | Never done anything |
|---|---|
| capability-gap log: 15 recorded | health engine: never run, no data source connected |
| run telemetry: 692 runs measured | event store: 195 events in, 0 processed, nothing reads it |
| receipts 20, requests 6, alerts 6 | corrections ledger: 0 uses, built for a problem that had happened three times |
| regression suite: 57 tests | instruction queue: nothing ever queued |

**Where did the week's incidents come from?** Of eleven findings, seven were the new oversight machinery malfunctioning: a guard that silently muted a scheduled job for four days, a verification gate that matched on category instead of identity, a health probe blanked by a bulk edit, a detector keyed on the wrong field, a monitor broken by a delimiter collision, a floor that opened rows for billing addresses, and a detector that inferred a fault from a symptom. Two were infrastructure. One was the agent's own judgement.

Six of the seven were things built to catch problems, which then either raised false alarms or went blind.

## The rule

**A mechanism that has never fired isn't protection. It is unvalidated surface area.** Periodically list every guard, ledger and detector, and next to each write how many times it has actually caught something. Then act on it:

- **Fired repeatedly:** keep it. It has earned trust.
- **Never fired, and couldn't have** (no data source, no reader, no trigger connected): finish it or delete it. It looks like protection on paper and does nothing.
- **Fires constantly:** it's noise. Fix the threshold or remove it.

When most incidents are your own instruments misfiring, the fix isn't another instrument. Fix or remove what exists first.

Two habits would have prevented most of it:

- **Don't ship a guard the same day you debug the fault.** Written while the incident is fresh, it encodes the instance rather than the class, and it hasn't been tested on cases where it should stay quiet.
- **Prefer removing a failure mode to detecting it.** Replacing an expiring token with one that doesn't expire ends a whole class of failure. A monitor that notices the expiry only shortens each outage.
