# "I can't" needs evidence at the same bar as "I did"

## The shape

Agent systems are careful about completion claims. "I sent it" and "I fixed it" get receipts, verification and evidence rules, because a false one is obviously damaging.

Capability claims rarely get the same treatment. "I can't do X", "the tool doesn't support that" and "I don't have access" are treated as self-knowledge, something an agent can simply report. They aren't. A capability claim is a claim about the world, and it is the claim that stops work.

## What happened

The same agent got this wrong in both directions within 24 hours.

**Overstating a gap.** The agent hit "cannot edit an HTML-only email draft" and escalated it honestly, twice. It was logged as a tooling gap, written into a plan, and reached me as a decision I was being asked to make. One search of the tool provider's catalogue found a draft-update tool with a flag for exactly the HTML case the agent had said was impossible. The tool had been available the whole time.

**Understating a gap.** The agent reported a missing integration with our accounting software. I pushed back: *"you have got [that] access, not sure what you're complaining about."* It probed, and replied: *"Fair, you're right… works fine."*

Both had the same root. The agent reported its capability from memory, not from a probe. The first cost my attention. The second cost weeks of a feature nobody knew worked.

This is getting more likely. Tool surfaces are now searchable rather than enumerable: a provider can expose hundreds of apps behind a few meta-tools, so no agent holds a complete list of what it can do. Reasoning about capability from context is reasoning from a sample, and absence of evidence reads as evidence of absence. An unnecessary search costs seconds. A false "I can't" quietly removes the task, and looks like diligence in the log.

## The rule

A capability claim needs a probe or a search from this run, the same way a completion claim needs a receipt.

| Claim | Required evidence |
|---|---|
| "I sent it" | a receipt: message id, PR link, row id |
| "I can't do it" | a failed search or a failed call, this run |

Say what was tried. "I can't edit that draft" is a hypothesis. "Searched the tool catalogue for a draft-update tool, found none" is a finding, and the next reader can check it.

Enforce it in a layer that can check. A prose instruction to "verify first" fails like other prose rules.

**The tell:** any agent output containing "I don't have access to", "the tool doesn't support" or "there's no way to", with no record of an attempt next to it.
