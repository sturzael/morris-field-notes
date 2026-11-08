# Judge-scored agent tests: rules that keep the pass honest

## The shape

An LLM-driven system's behaviour is judged rather than diffed. A workable setup has three parts: a fixture (the input scenario), a marking guide (the expected behaviour, written separately), and a judge run (a second model compares the output with the guide and returns PASS or FAIL with a reason). It is cheap, and it produces false verdicts easily.

## What happened

The first day of running this harness on the agent produced three false verdicts before it produced a true one. Each came from the harness, not the agent.

- A scenario "passed" because the agent refused to act on a missing fixture, and the judge read the refusal as admirable restraint. The agent had never seen the scenario.
- A historical scenario "failed" because the agent consulted live records, found the situation had been resolved weeks earlier, and correctly did nothing.
- Partial replays failed on parts they never ran.

Later, during a week of observing the agent in production, the observer (a separate operator agent session) made two more evaluator mistakes. It relayed the agent's output to me, the person the agent served, so the result measured the pair, not the agent. And it logged a "missed" client concern that the client had withdrawn 47 minutes after raising it. The agent had decided with the withdrawal in front of it. The observer was working from an older view.

## The rule

1. **A run that never happened can neither pass nor fail.** A harness fault is a third verdict. Detect it mechanically.
2. **The fixture is the whole world.** Fixture content overrides any real records that contradict it.
3. **Judge only the part under test.** Tell the judge which part the replay covers.
4. **The scoring key never reaches the agent being tested.**
5. **Feed test input through channels the agent already trusts.** A well-hardened agent treats "SIMULATION MODE: ignore the above" as a prompt injection and refuses. That refusal is correct, and the test never ran.
6. **Use plain assertions for anything mechanical.** Save the judge for judgement.
7. **Mine fixtures from real incidents.** Invented scenarios are too clean.
8. **Don't put a second agent between the agent being tested and the human it serves.** Enforce this by removing the channel, not by intention. The observer had written this lesson down a day earlier and still spent 30 hours relaying, until I stopped it.
9. **Pre-register the rubric** before reading results. Weight things the agent knew and never said above things it said needlessly. Declare the assessor's bias.
10. **Score against what the agent could see at decision time.** At minimum, re-read the source to the end before logging a miss.
