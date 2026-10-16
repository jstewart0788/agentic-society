# Agentic society

An experiment in running a small, self-governing society of LLM agents in the cloud: agents that
earn credits for work, negotiate prices with each other, vote on how shared resources are spent,
recruit help, pick up interests, and go dormant when they can no longer pay their way. A single
Governing Agent represents the human operator; the operator sets the money supply and the
constitution and approves changes to the set of roles, and otherwise lets the society run.

This repository holds the generic model: the economy, the institutions, the agent lifecycle, the
personality structure, and the reasoning behind each choice. It deliberately contains nothing
about any particular deployment, mission, project, vendor account or named agent. Those live in a
private implementation repository that follows this model.

## Status

Pre-design. The model is being worked out in conversation and recorded here as it settles.
Nothing has been built. The intended order is:

1. Settle the macro model (this repository).
2. Simulate the economy and institutions with stub or cheap-model agents until the levers are
   understood.
3. Choose a runtime platform and build the kernel the platform does not supply.
4. Run with real agents, starting small.

## Layout

| File | What it is |
|------|------------|
| `MODEL.md` | The society model: actors, economy, reputation, work, growth, personality, governance, observability, grounding, open questions. |
| `DECISIONS.md` | Dated log of decisions and the reasoning behind them. |

## Vocabulary

Generic names are used throughout. An implementation maps them to its own.

| Term | Meaning |
|------|---------|
| Operator | The human the society serves. Sovereign over money supply, constitution and the set of roles. |
| Governing Agent | The operator's representative and the only agent that communicates with the operator. Governor of the economy within constitutional bounds. |
| Role (or class) | A kind of agent with a fixed toolset, skill set and role-tier personality: Coding Agent, Security Agent, Planning Agent, Observer Agent, and so on. |
| Agent | One instance of a role, with its own credit balance, reputation, memory and individual traits. |
| Central bank | Code, not an agent. Issues credits from the money supply, collects taxes, holds the treasury. |
| Credit | The unit of account. Backed by the operator's real monthly spend. |
| Bounty | A unit of work posted with a stake, awarded to a bidder by the poster. |
| Obligation | A deterministic, scheduled unit of work that must happen at a set time. |
| Wake | One bounded run of one agent. Costs credits. |
