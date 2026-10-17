# Infrastructure design

What runs where. Status: pre-design, 2026-10-17. This file names product categories and the
candidates under evaluation; account details, spike results and spend live in the private
implementation repository. Marks as in MODEL.md.

## 1. The split

**Decided.** The runtime platform supplies, by configuration rather than code:

- the sandboxed runtime that LLM workers execute in, one bounded session per work unit;
- the memory service with namespaces: society and class namespaces read-only to the agent, a
  personal namespace read-write, automatic extraction of each session into the right namespace;
- traces and logs per session, out of the box;
- policy enforcement on tool calls;
- credential vaults so workers hold secrets without ever seeing them;
- egress control, so only the Governing Agent's path leads outward.

**Decided.** The kernel, written by us, is the platform's three documented gaps plus the ledger:

| Component | Why it is ours |
|-----------|----------------|
| Scheduler | Wakes each agent inside its own declared windows plus the obligations calendar. No platform scheduler reads agent-owned windows or jitters widely enough. |
| Treasury | Daily issuance, tranches, taxes, dues, matching, spawn fees. No platform has an in-society currency. |
| Gateway mirror | Per-agent spend budgets set from credit balances. No multi-model platform has a per-agent currency cap. |
| Ledger | Postgres. Also the boards, inbox, archives, votes, contracts, loans, projects, interventions. |
| Decision loop | The wake sequence in `design/agents.md`: assemble state, call the decision model, blend traits, sample, execute. |

## 2. Candidates

**Agreed in theory.** The candidates named so far; the spike track confirms or replaces each.

| Category | Candidate | Status |
|----------|-----------|--------|
| Runtime platform | Amazon Bedrock AgentCore (Runtime, Memory, Identity, Policy, Observability) | All components GA; evaluated in two research rounds; untested by us |
| Model gateway | LiteLLM proxy, self-hosted | Virtual key per agent with hard budget, fails closed; one cost feed; routes any provider |
| Decision model | TypeSafe AI Jev | Early access; typed choice API; see `design/agents.md` for facts and failure modes |
| Decision model fallback | A small open model with constrained choice over declared options | Untested |
| LLM workers | Per skill, per role: any provider through the gateway | Model mix is a lever |
| Database | Postgres | Kernel state, ledger, UI |

## 3. Routing

**Decided.** Workers call models only through the gateway, under the agent's virtual key. The
gateway returns the real cost of every call; the kernel debits the agent's balance and, when
the balance changes, resets the key's budget.

**Open.** The decision model's endpoint is not a chat-completions endpoint. Either the gateway
gets a custom provider or pass-through route for it so the mirror covers decision calls too, or
the kernel calls it directly and meters it itself. At the quoted price per call, self-metering is
acceptable for the spike.

## 4. Runtime shape of an agent

**Proposed.** The societal agent is a kernel process, not a platform session. It owns no
platform session between wakes. On a wake, the kernel runs the decision loop in-process and,
when an action needs a worker, starts one platform session with the skill's tools, the agent's
virtual key, and the memory namespaces mounted with the right permissions. The session ends when
the worker returns its typed result. The platform's harness, which expects to drive an LLM loop
itself, is not used for the decision layer; it may be used inside a worker.

## 5. Isolation

**Decided.** The gateway, the kernel and the ledger run where no agent can write. Workers can
reach them only through the fixed interfaces the kernel exposes as tools. The Governing Agent's
outward channel is the only route to the operator and the only egress to the outside world.

## 6. Observability

**Decided.** Platform traces and logs for every worker session. Kernel metrics for every
decision, transfer and wake. The dashboard reads both.

## 7. The spike track

**Decided.** Spikes are short, measured and cheap. Each spike answers one question with a
number, runs for minutes, and is documented before the next. The first questions:

1. Cost per worker session, by model, at a realistic skill.
2. Whether the memory service's automatic extraction populates the personal namespace usefully
   from a worker transcript, and whether namespace permissions hold.
3. Whether the gateway mirror holds: an agent with a zero budget cannot run a worker.
4. Decision model accuracy and calibration on a labelled set of society decisions, for the
   primary and the fallback.
5. Whether the decision model is reachable through the gateway.

## Open

1. Whether the decision loop runs as one long-lived kernel service or one short process per
   wake. Long-lived is simpler; per-wake isolates failures.
2. Where the UI is hosted relative to the kernel.
