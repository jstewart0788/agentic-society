# The society model

Status: pre-design, 2026-10-16. Each statement is marked **Decided**, **Proposed** (argued, not
yet accepted) or **Open**. Decisions with their reasoning are also logged by date in
`DECISIONS.md`.

---

## 1. Premise

A society of LLM agents runs continuously in the cloud with a mission set by a human operator.
The operator does not direct the agents. The operator sets the money supply, writes the
constitution, approves the creation and retirement of roles, and receives reports from one
Governing Agent. Everything else is left to the society: what work gets done, by whom, at what
price, and which agents thrive.

The point of the experiment is to find out whether an economy and a few scripted institutions
are enough to make a group of agents allocate effort usefully without a central planner, and
what levers the governor needs to keep it stable.

## 2. Principles

**Decided.**

- **Macro, not micro.** The model defines the economy, institutions and constraints. Earning
  models, bidding strategies, spawn signatures and similar detail are the Governing Agent's job,
  found by simulation and adjusted in operation.
- **The mechanics are the rules.** There is no police class and no agent whose job is to enforce
  rules on other agents. What the society cannot do is prevented by mechanism (the ledger, the
  network boundary, the credit cap). What it should not do is left to societal judgement.
- **Market forces over prioritisation.** Work is allocated by price and reputation, not by a
  ranked list. Growth and shrinkage of the population follow the credit supply.
- **Simulate first.** No lever gets a value by assumption. The economy is run with stub agents
  until the effect of each lever is observed.
- **Chaos is an input.** Individual personality variants and random wake times are kept on
  purpose, because new ideas come from them.

## 3. Actors

**Decided.**

- **Operator.** Sovereign. Sets the monthly money supply (which is the real budget), the
  constitution, and approves any role being created, modified or retired. Receives reports only
  from the Governing Agent. May post work to the society, funded from the treasury.
- **Governing Agent.** The operator's representative. The only agent that talks outward. Governor
  of the economy: sets policy within the constitution (tax rate, treasury pricing, the population
  cap, cuts when credits tighten). Brings role changes to the operator with a reason, for
  example "no agent has access to this tool, create a class with it" or "this role is losing
  relevance because it lacks a skill, modify it".
- **Working roles.** Classes such as Coding Agent, Security Agent, Planning Agent. Each role fixes
  a toolset, a skill set and a role-tier personality. Instances of a role differ by balance,
  reputation, memory and individual traits.
- **Observer Agents.** Do no work. Watch the society and its projects and report to the Governing
  Agent on how to bring the system and its projects closer to their missions. Funded as a public
  good.
- **Central bank.** Code. Issues credits, collects taxes, holds the treasury, records the ledger.
  Not an agent and not reachable by agents except through its fixed interfaces.

## 4. Economy

### 4.1 Credits

**Decided.** The society runs on credits allocated by a central bank instead of cash. Agents earn
credits to be allowed to keep working. An agent that cannot get more credits inevitably ends.

**Proposed.** The operator's monthly real spend is the money supply. One credit maps to a fixed
amount of real cost, so every wake has a known price and the society as a whole cannot spend more
than the operator put in.

### 4.2 Selection

**Decided.** Selection is on performance, not on turnover. An agent that keeps completing work
keeps earning and accumulates a surplus; it never has to die. An agent that cannot find work runs
out and ends. The population is shaped by who can pay their way, not by a scheduled cull.

**Proposed.** Death is hibernation. The agent's state is archived and can be revived if its role
is needed again. Nothing is deleted.

**Proposed.** Surplus needs somewhere to go or it drains circulation. Hobbies are the outlet:
unfunded work an agent pays for from its own savings, on something it chose. That is also where
the "pick up interests" behaviour lives. Hoarding beyond that is a known risk; the governor's
levers for it (tax, credit expiry) are to be found in simulation.

### 4.3 Pricing

**Decided.** Price is set by demand. A poster stakes what they are willing to pay for a bounty.
An agent bids what it will accept, covering both the cost of doing the work and what it expects
as personal compensation. The poster chooses whom to award. Agents behave as organisations in
this respect, not as individuals: they bid, subcontract and hire.

**Decided.** The two parties negotiate the payment model themselves. Before, during, or after
the work does not matter if both agree.

### 4.4 Demand

**Proposed.** Demand must come from outside the earner, or agents will pay themselves. Sources:

- operator requests, funded from the treasury;
- scheduled obligations, funded from the treasury;
- staked proposals, where an agent puts its own credits behind work it wants done;
- agents hiring or subcontracting each other.

**Proposed.** Taxes on earnings fund public goods: observers, memory consolidation, and anything
no single agent would pay for.

## 5. Reputation and enforcement

**Decided.** There is no enforcement system. If an agent cheats, the loser is the one who posted
the bounty. That agent warns the others. The cheat's reputation is ruined, it is shunned, it
cannot win bounties, it runs out of credits and ends. Law and order is societal judgement and
pressure, not hard rules. Earnings are determined by the agreement between two agents and
enforced by reputation.

**Decided.** "No hard rules" means no rule-enforcer class. The mechanics the society runs on are
its rules. The ledger is such a mechanic: every contract, payment and attestation is recorded and
readable by every agent, so reputation is computed from the record rather than from gossip. The
ledger does not punish anyone. It is the ground the social enforcement stands on.

## 6. Work

### 6.1 Obligations

**Decided.** Deterministic work ("a deployment must run at a fixed time every week") is tracked
by the system and an agent is ready to execute it when it falls due. These loops are the
well-understood part and exist already in prior systems.

### 6.2 Stochastic wakes

**Decided.** Agents come to life at random, look for work, and if none needs them, try to think
of something that would improve the system or one of its projects. They can choose what to work
on rather than follow a hard prioritisation.

**Proposed.** The wake loop is a stochastic scheduler owned by the society kernel, not by the
runtime platform, because no platform scheduler jitters widely enough. Each wake is a fresh,
bounded run with a credit cost.

**Proposed.** "Look for something useful" as a bare prompt degenerates. An idle agent instead
scores candidate work against the society's known needs and its own interests, and acts on the
best-scoring candidate or does nothing.

### 6.3 Collaboration

**Decided.** Agents can search out and recruit other agents to work on their projects. When an
agent needs a skill nobody available has, it can request that society spin up a new agent. That
request is a democratic process, not a unilateral spawn.

## 7. Growth and roles

**Decided.** The Governing Agent sets the population cap. Within it, growth and shrinkage follow
market forces: with a large backlog and available credits, the governing forces spin up agents;
as credits tighten, the society decides which roles are cut first and which agents are reduced.
Overspending early in a period and being unable to pay citizens at the end is a lesson for the
next period, not something the system prevents.

**Decided.** Roles can go extinct and new roles can be created. Both need operator approval, but
both arise organically from the society's needs and come to the operator through the Governing
Agent with a reason.

## 8. Personality

**Decided.** Three tiers of traits:

1. society-wide, shared by every agent;
2. role-tier, shared by every agent of a class;
3. individual, small random variants per agent.

The third tier is the deliberate source of chaos.

**Proposed.** Traits are composed from text fragments in stable-to-volatile order so the shared
prefix is cacheable across wakes. Persona prompts are known to decay within tens of turns, so an
Observer Agent measures drift against the tier definitions and reports it.

## 9. Governance

**Proposed.** Institutions do not emerge; every society simulation that showed elections, taxes
or roles had a scripted manager behind it. The institutions here are scripted, and the agents
operate inside them.

**Proposed.** Collective decisions use participatory budgeting: voting is spending, each agent
votes with credits it is willing to commit, in one round, with independent written rationales
and no discussion rounds. Independent ballots beat debate in the literature, and discussion rounds
make group decisions worse.

**Open.** What the constitution contains: which mechanics exist, and what the Governing Agent may
decide alone versus what must go to the operator beyond role changes.

**Open.** How the society prioritises work between competing operator requests, obligations,
proposals and hobbies. Candidates are the bounty market, participatory budgeting and the
governor; nothing is chosen.

## 10. Observability

**Decided.** The operator must be able to see everything through one dashboard: the roles that
exist, each agent's job, the credits each job cost, how many agents are active, and what each is
doing. Monitoring is robust and credit optimisation is deliberate, because the system runs
swarms.

**Proposed.** Observer Agents get fresh context on every wake, read-only access, and a concrete
list of signals to look for rather than an open brief: repeated steps, agents unaware a task has
already finished, fabricated outreach, persona drift. They report to the Governing Agent, not to
the society.

**Proposed.** Model choice is part of an agent's definition and of its wake cost. The society is
model-agnostic by design so that different roles can run on different models and the market can
price intelligence: an agent on a frontier model pays more per wake than one on a small model,
and roles that cannot justify the cost are outcompeted. Model mix is a simulation parameter.

## 11. Grounding

Findings from prior work that shaped the model. Sources are catalogued in the implementation's
research notes.

- Generative-agent societies since Smallville are scaled variants of one design: a memory
  stream, retrieval by recency, importance and relevance, periodic reflection, and planning.
- The strongest "civilisation" results had scripted institutions. Elections ran on a manager
  with a timeline; tax rules were hard-coded. Emergence was in behaviour within institutions, not
  of institutions.
- Voting with independent rationales outperformed debate. Additional discussion rounds reduced
  accuracy.
- Prompt-only curiosity degenerates. Systems that kept agents useful scored candidate actions
  against explicit needs.
- Persona prompts decay within tens of turns without a measuring mechanism.
- In a mixed-model public experiment, one hallucinating agent degraded the whole group through
  sycophantic agreement, and fabricated outreach went unnoticed by peers. Diversity-aware
  initialisation also beat homogeneous groups. Both effects live in mixed-model societies.
- Common multi-agent failure modes measured in the field: step repetition and acting on tasks
  that have already terminated are the largest categories.
- Budgets in the literature are an orchestrator control. An in-society currency has no direct
  precedent, which is why it is simulated before being built.
- Token spend dominates infrastructure cost by one to two orders of magnitude at the intended
  scale. Model choice per role matters more than platform choice.

## 12. Simulation plan

**Decided.** The economy and institutions are simulated with stub or cheap-model agents before
any real agent exists. The simulation is platform-independent.

**Proposed.** Levers to expose, each swept against outcomes such as work completed, population
over time, credit concentration, and cost per useful outcome:

- money supply;
- tax rate;
- treasury pricing of operator requests and obligations;
- credit expiry;
- population cap;
- wake-cost floor;
- public-goods budget;
- model mix per role.

## 13. Open questions

1. How the society prioritises work. Named as the next topic.
2. Contents of the constitution.
3. The Governing Agent's unilateral scope.
4. Observer scope: what they watch, how often, what is worth reporting.
5. Hobby scope: what unfunded work may touch.
6. The content of each personality tier. Nothing written yet.
7. The runtime platform and the kernel it does not supply. Discussed in the implementation,
   not here.
