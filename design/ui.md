# UI design

The operator's console. Status: pre-design, 2026-10-17. The UI is built first (see
`ROADMAP.md`) so there is a natural place to organise, build and visualise the agents, roles and
institutions before the society runs. Marks as in MODEL.md.

## Principles

**Decided.** The operator can see and modify everything: every agent by class, individually, or
the society as a whole; logs, memories, system prompts, skills and tools; every institution and
its levers. Any individual stat can be changed through the UI, even where that risks compromising
the experiment.

**Proposed.** Every manual change is written to the ledger as an operator intervention, with the
old value, the new value and a note. Not to stop the operator, but so the experiment stays
interpretable afterwards: a population crash on day 12 should be attributable to the tax change
on day 11 if that is what caused it.

**Proposed.** The UI reads and writes the kernel's Postgres directly through the kernel's API.
It has no state of its own. Stack is chosen in the infrastructure track.

## Screens the operator asked for

### Dashboard

**Decided.** Everything at a glance. Population by class, agents awake now and what each is
doing, credits committed per tranche today and how far down the seniority order the day's money
reached, spend against the gateway cap, obligations due today and their holders, open bounties,
the current vote, latest observer escalations.

### Agents

**Decided.** The society as a whole, a class, or one agent. For each agent: balance, reputation,
wake windows, traits, self-file, memories by tier, system prompt as composed for the next wake,
skills and tools linked, action history with the decision model's probabilities at each step,
worker transcripts, logs. Every field editable. Class view edits apply to every member; class
tier edits apply to the role.

### Skills

**Decided.** Craft the work units agents are allowed to use: prompt template, tools, model,
budget ceiling, typed inputs and outputs. Link a skill to a class or to an individual agent;
unlink it. See which agents hold which skills and what each skill has cost in total.

### Institutions

**Decided.** Every institution and its levers: current value, allowed range, who may change it
(operator, governor, vote), history of changes. Treasury tranches and the seniority order; income
and wealth tax; class dues; match ratio; spawn fee curve; seed balances; public goods minimum per
role; proposal pricing; reserve cap; the representatives' trait-bias budget. Editable in place.

## Screens proposed in addition

### Ledger

Every transaction, contract, bounty, loan, attestation and warning, searchable by agent, class,
counterparty, time and kind. This is the source reputation is computed from, so it should be
readable as a record, not only as aggregates.

### Boards and votes

The world, class and contact boards as the agents see them. The current amendment cycle:
proposals, their cost, who proposed, the ballot, turnout. Past cycles and outcomes.

### Projects and obligations

Each project with its immutable goal statement, its bounties, the agents working it, observer
reports filed against it, and whether it is hobby-eligible. The obligations calendar: every
scheduled job, its holder and backup, execution history, misses.

### Observer reports

Reports filed by observers, grouped by subject (agent, class, society, infrastructure, project),
what the Observer Representative escalated, what the Governing Agent passed on. Without this the
operator only ever sees the top of the chain.

### Governing Agent console

The one place the operator and the Governing Agent talk. The approvals queue: role creation,
role modification, role extinction, escalations. The operator's priority list, edited here.

### Archives

Dead agents: their final state, memories, cause of death (balance history), and which living
agents have read them.

### Simulation

Run the economy with the stub policy: pick lever values, run N days, compare runs side by side
on population, credit concentration, work completed, loan volume, cost per outcome. Promote a
lever set to the live institutions in one action.

### Interventions

The log of every manual change the operator made, as a timeline next to the society's metrics.

## Open

1. Which of the proposed screens are their own screens and which are tabs of the four decided
   ones. The ledger, boards and archives could live inside Agents and Institutions.
2. Whether the operator can post to the boards. The model says only the Governing Agent talks
   outward from the society; the operator posting inward is a different question.
3. Stack. Decided in the infrastructure track once the kernel's API exists.
