# Vision

## The World We Are Building Toward

Imagine a set of agents that work on your behalf. They handle your Amazon purchases — finding the right product, comparing prices, tracking shipping. They coordinate with your work colleagues' agents to schedule meetings, share context, move projects forward. They communicate with your friends' agents to make plans, surface recommendations, and share things you'd each care about. You don't manage each of these interactions individually. Instead, you have a single layer that shows you what your agents are doing, ensures they're acting within the scope you've defined, flags decisions that need your judgment, and tells you when outcomes aren't tracking right.

Your friends and colleagues use different AI vendors — Anthropic, OpenAI, Google, others. That doesn't matter. Their agents and yours interoperate, and the quality of the interaction doesn't depend on which platform anyone chose.

This is what Interlock is for.

---

## The Infrastructure Gap

The protocols that make this possible are converging. A2A (Agent-to-Agent) defines how agents discover each other and manage task lifecycles. MCP (Model Context Protocol) defines how agents reach tools and data sources. OpenTelemetry's GenAI conventions define how agent behavior is observed. These are open, multi-vendor standards gaining real adoption.

What they don't define is the governance layer: who authorized this agent to act? Is this action within the delegated scope? What happened, in an auditable form? Was the outcome actually correct? And how do you do all of this across vendor boundaries, without requiring every participant to share a platform?

That is the control plane gap — and it is structural, not incidental.

Orchestration frameworks (LangGraph, CrewAI, OpenAI Agents SDK) answer "what should happen next?" The agent control plane answers something different: **is what's happening safe, authorized, correct, and attributable?**

---

## Why Vendor Neutrality Matters

The agent control plane market is forming. Enterprise control towers are being built by cloud providers and platform vendors. Each embeds proprietary assumptions about identity, policy, and evaluation. Each creates lock-in at the governance layer — the layer that, if it works correctly, should outlast any individual AI vendor.

A control plane owned by a platform is not a neutral governance layer. It is a governance layer with a commercial interest in a specific ecosystem's success.

Interlock is designed on the premise that governance infrastructure for the agentic web should be:

- **Open in protocol** — built on standards that any agent runtime can speak
- **Independent of any single vendor's model or runtime** — so the infrastructure survives vendor transitions
- **Applicable to individuals, teams, and organizations** — not just enterprise deployments with dedicated AI operations teams
- **Extensible at the policy layer** — so that operators can define governance rules appropriate to their context, rather than accepting a platform's defaults

---

## The Human Dimension

There is a real risk worth taking seriously.

COVID gave us a preview of what it looks like when human-to-human interaction drops sharply. The effects were not uniform. Productivity adapted faster than wellbeing. Adults with established social networks found workarounds. But younger people, whose social and developmental needs depend on unmediated physical interaction, faced measurable harm: elevated anxiety, disrupted identity formation, weakened peer relationships, a collapse of the casual spontaneous contact that doesn't survive being scheduled on a calendar.

The risk with a heavily agent-mediated world isn't that people stop interacting. It's that the texture of interaction changes in ways we don't fully account for. If agents handle the friction and coordination of social life, do people lose the practice of navigating that friction themselves? If your agent can always find you a better deal faster, do you lose the serendipity of browsing — the unplanned discovery, the conversation it starts? If coordination becomes frictionless, does it also become shallow?

These are not arguments against building this infrastructure. They are arguments for building it with intention.

**Interlock's design commitment:** the control plane layer should make human contact easier to choose, not harder to maintain. Agents should handle the transactional so that human attention can concentrate on what is actually relational. The goal is not to replace human interaction — it is to reduce the overhead that competes with it.

This commitment has concrete implications for how the system behaves:

- Agents should surface "a human may want to handle this" as a first-class outcome, not a failure mode
- The principal — the person or organization the agents work for — should always be able to see what has been delegated and step back in
- Agents communicating across principal boundaries should not obscure the humans behind them — identity and representation matter
- The system should be auditable enough that you can always understand what was done on your behalf and why
- Delegation patterns should be visible over time — not because oversight is surveillance, but because informed principals make better choices about how much to delegate

---

## The Competitive Framing

The agent control plane is a real category. Several enterprise products exist. None currently combine vendor neutrality, open-protocol foundations, and applicability at every scale — individual, team, and multi-organization. That combination is what Interlock is designed to provide.

The goal is not to be another control tower. It is to be the coordination substrate that makes it possible for any agent, from any vendor, running on any runtime, to operate in a way that is governable, observable, and trustworthy — without requiring the humans behind those agents to share a platform.

---

## Reference Standards and Context

- [A2A Protocol — Linux Foundation](https://github.com/a2aproject/A2A/blob/main/docs/specification.md)
- [Model Context Protocol specification](https://modelcontextprotocol.io/specification/2025-11-25)
- [OpenTelemetry GenAI Semantic Conventions](https://opentelemetry.io/docs/specs/semconv/gen-ai/)
- [Forrester: Announcing Our Evaluation of the Agent Control Plane Market](https://www.forrester.com/blogs/announcing-our-evaluation-of-the-agent-control-plane-market/)
- [OWASP Top 10 for LLM Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
- [AGNTCY — Agent directory and messaging infrastructure](https://docs.agntcy.org/)
