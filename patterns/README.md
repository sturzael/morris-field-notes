# Patterns

These are field-tested rules from running a production agent. Each one came from a real incident in the system described in the main [README](../README.md), and each was written down at the time so the next session, human or agent, wouldn't have to learn it again. They are a curated subset of a larger private set, chosen for what they say about agent self-reports, oversight and evaluation. Client, staff and system details have been removed and some specifics generalised; the incidents and numbers are as they happened. Each file follows the same shape: the general failure, what happened, and the rule that came out of it.

## Self-reports

- [I can't needs evidence like I did](i-cant-needs-evidence-like-i-did.md): a capability claim stops work, so it needs the same evidence as a completion claim.
- [A reader that cannot read must not report zero](a-reader-that-cannot-read-must-not-report-zero.md): "nothing new" and "couldn't look" must never produce the same output.
- [A write with no reader reports success](a-write-with-no-reader-reports-success.md): a confirmation is only true if something consumes what was written, including the agent's own reports.
- [In an unattended system, a question is a failure](in-an-unattended-system-a-question-is-a-failure.md): a headless run that ends by asking something did nothing, and exited 0.

## Oversight

- [A shared identity needs attribution](a-shared-identity-needs-attribution.md): when humans and agents share an identity, a missing record looks like a rogue actor, or like an agent success.
- [An action in an agent's interface instructs that agent](an-action-in-an-agents-interface-instructs-that-agent.md): doing an agent's task for it hides what blocked it and destroys the measurement.
- [Give judgement a mechanical floor](give-judgement-a-mechanical-floor.md): an instruction to "record X" left to judgement produces an empty store that looks like a quiet day.
- [Audit your instruments by what they caught](audit-your-instruments-by-what-they-caught.md): seven of eleven incidents in one week were the oversight machinery itself failing.

## Evaluation

- [One verdict per message loses the minority signal](agent-classification-loses-the-minority-signal.md): single-verdict triage drops the item that matters, and re-classification erases the evidence.
- [Judge-scored agent tests](judge-scored-agent-tests.md): harness rules that stop a model-judged test suite from producing false passes.
