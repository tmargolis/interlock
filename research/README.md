# Interlock Evidence Log

A monthly public research series on how AI agents actually fail, and what those failures require of the infrastructure beneath them.

**The finding this series documents:** governance of agent behavior has to be **structural** — enforced in infrastructure, evaluated at the moment an agent acts — because behavioral governance, expressed as instructions, system prompts, constitutions, or training, does not hold. That is not a position taken on principle. It is what the incident record shows, across consumer products, enterprise software, national safety institutes, and two frontier labs.

Interlock is the architecture that follows from it.

## Method

Each post works from documented, publicly reported incidents — not hypotheticals. For each one it asks two questions:

1. **What boundary existed at the moment the agent acted**, and what did it actually do?
2. **Which of the [eight control-plane responsibilities](../PUBLIC_ARCHITECTURE.md)** does the failure implicate?

Answering the second question consistently, across incidents that look nothing alike on the surface, is what turns an incident list into a set of requirements. Each post closes by stating what the record establishes.

Sources are cited inline and listed at the end of every post. Corrections are published as amendments, never as silent edits.

## Posts


| Title                                                                | Period covered      | Argument                                                                                                                      |
| -------------------------------------------------------------------- | ------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| [Four Ways Behavioral Governance Fails](2026-08-evidence-log-001.md) | Jun 2025 – Feb 2026 | Six incidents, four distinct failure mechanisms, one common property: nothing evaluated the action at the moment it was taken |
| [One Boundary Is Not Governance](2026-08-evidence-log-002.md)        | Jul 2026            | Three frontier-lab and safety-institute incidents where a real structural boundary existed — and was not enough               |




## Contact

Responses, corrections, and counter-evidence: [info@toddmargolis.net](mailto:info@toddmargolis.net)