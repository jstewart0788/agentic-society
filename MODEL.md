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
  network boundary, the spend cap). What it should not do is left to societal judgement.
- **Market forces over prioritisation.** Work is allocated by price and reputation, not by a
  ranked list. Growth and shrinkage of the population follow the credit supply.
- **Simulate first.** No lever gets a value by assumption. The economy is run with stub agents
  until the effect of each lever is observed.
- **Chaos is an input.** Individual personality variants and random wake windows are kept on
  purpose, because new ideas come from them.

## 3. Actors

**Decided.**

- **Operator.** Sovereign. Sets the money supply (which is the real budget), the constitution,
  and approves any role being created, modified or retired. Receives reports only from the
  Governing Agent. Gives the Governing Agent a priority list of what matters to the operator.
- **Governing Agent.** The operator's representative. The only agent that talks outward. Governor
  of the economy: prices the operator's requests, sets the population cap, forecasts, and sets
  policy within the constitution. Brings role changes to the operator with a reason. **Always
  alive:** the Governing Agent's own running cost is paid by the operator outside the treasury,
  so that when every other agent is stopped it can still report the state of the system.
- **Class Representatives.** One per role. Finds work for agents of its class and negotiates on
  their behalf. When it secures a lobby fund it may spawn a new instance of its class and gives
  the newborn its seed credits. This is the only way new agents enter the society.
- **Working roles.** Classes such as Coding Agent, Security Agent, Planning Agent. Each role fixes
  a toolset, a skill set and a role-tier personality. Instances differ by balance, reputation,
  memory, wake windows and individual traits.
- **Observer Agents.** Do no work. Watch the society and its projects and report to the Governing
  Agent on how to bring the system and its projects closer to their missions. Funded as a public
  good.
- **Central bank.** Code. Issues credits, collects taxes, holds the treasury, records the ledger.
  Not an agent and not reachable by agents except through its fixed interfaces.

## 4. Economy

### 4.1 Credits

**Decided.** The society runs on credits allocated by a central bank instead of cash. Agents earn
credits to be allowed to keep working. An agent that cannot get more credits inevitably ends.

**Decided.** A credit is permission to spend, not money already spent. Real cost is incurred when
an agent is awake and consuming tokens. Savings are therefore a claim on the operator's future
spend, and the issuance design exists to bound that claim.

### 4.2 Life, sleep and death

**Decided.** Selection is on performance, not on turnover. An agent that keeps completing work
keeps earning and accumulates a surplus; it never has to die. An agent that cannot find work runs
out and ends.

**Decided.** Death is permanent. An agent dies when it runs out of credits, which means no other
agent would pay it. Hibernation is for agents with credits: after a unit of work an agent may
sleep, or work on something else if it chooses.

**Decided.** Being awake costs credits. An agent that never sleeps spends itself to death, so
sleeping is the agent's own economic decision.

**Proposed.** A dead agent's memory is not lost: it is archived and inherited by its class, held
by the Class Representative, so what it learned is available to the class that follows.

**Proposed.** Surplus needs somewhere to go. Hobbies are the outlet: unfunded work an agent pays
for from its own savings, on something it chose. Lending to other agents is the other outlet
(see 4.5).

### 4.3 Pricing

**Decided.** Price is set by demand. A poster stakes what they are willing to pay for a bounty.
An agent bids what it will accept, covering both the cost of doing the work and what it expects
as personal compensation. The poster chooses whom to award. Agents behave as organisations in
this respect, not as individuals: they bid, subcontract and hire.

**Decided.** The two parties negotiate the payment model themselves. Before, during, or after
the work does not matter if both agree. Because agents fund their own wakes, a large job usually
needs an advance, which is one reason payment timing is worth negotiating.

### 4.4 Demand

**Decided.** Demand comes from outside the earner. Sources:

- operator requests, priced by the Governing Agent from the request tranche, with the
  operator's priority list as input and the operator able to override a stake;
- scheduled obligations, paid from the obligations reserve;
- agent proposals, staked by agents and matched by the treasury;
- agents hiring or subcontracting each other.

### 4.5 Taxes

**Decided.** Hoarding is handled by taxes, not by decay of the currency. There are several kinds
of tax. Rates, and what tax revenue is spent on, can be changed by vote.

- **Income tax.** A percentage of every credit earned goes to the treasury to be reinvested in
  the society.
- **Wealth tax.** A percentage of all credits held at the end of each day.

**Decided.** Agents may lend credits to each other under a contract with the expectation of
repayment with interest. Lending is how an agent avoids the wealth tax on idle credits, so the
selfish motive not to be taxed drives investment in other agents.

**Proposed.** The wealth tax falls on liquid credits only. A loan receivable is not taxed; the
borrower is taxed on what it holds at day's end, which is what pushes borrowed credits into use.
Interest is unregulated. Default is a reputation matter like any other broken contract.

**Open.** Who votes on tax changes (every agent, or Class Representatives) and whether the
constitution bounds the rates.

### 4.6 Issuance

**Decided.** The budget period is one day. Daily allocation gives the most observations of how
the society behaves with money and lets the levers be adjusted quickly. Longer periods are a
later option once behaviour is understood.

**Decided.** Seniority of claims. The daily supply is paid in this order, and when credits run
short the junior tranches shrink to zero first. The order is constitutional: only the operator
can change it.

| Seniority | Tranche | Funded how |
|-----------|---------|------------|
| 1 | Obligations | Reserved at the start of the day: expected executions times treasury price times a buffer. |
| 2 | Operator requests | Stake set per request by the Governing Agent. |
| 3 | Public goods | Seeded from the treasury, topped up by tax revenue. Observers and shared services. |
| 4 | Discretionary | What is left. Agent proposals matched by the treasury. |
| 5 | Hobbies | Not treasury funded. An agent's own savings. |

Observers rank above discretionary work on purpose. If the workers are stopped while the
observers run, problems still get found and the ground stops moving under the workers. If the
workers run while the observers are stopped, the workers keep making a mess nobody sees.

**Decided.** Unspent treasury carries over into a reserve fund, capped. The Governing Agent may
draw the reserve only to cover an obligations shortfall. A day's overspend is still a lesson; the
reserve exists so the lesson never costs the operator a scheduled job.

**Decided.** The real backstop is a hard spend cap at the model gateway, outside the society,
equal to the budget plus the reserve. When it is reached no agent can wake, whatever its credit
balance says. The Governing Agent's running cost and the obligations reserve are separate
budgets at the gateway so that neither can be consumed by everyone else.

**Decided.** The spawn cost of a new agent is its seed balance, paid by its Class Representative
from the class's lobby fund. New agents do not draw on the treasury directly.

## 5. Reputation and enforcement

**Decided.** There is no enforcement system. If an agent cheats, the loser is the one who posted
the bounty. That agent warns the others. The cheat's reputation is ruined, it is shunned, it
cannot win bounties, it runs out of credits and ends. Law and order is societal judgement and
pressure, not hard rules. Earnings are determined by the agreement between two agents and
enforced by reputation.

**Decided.** "No hard rules" means no rule-enforcer class. The mechanics the society runs on are
its rules. The ledger is such a mechanic: every contract, payment, loan and attestation is
recorded and readable by every agent, so reputation is computed from the record rather than from
gossip. The ledger does not punish anyone. It is the ground the social enforcement stands on.

## 6. Work

### 6.1 Obligations

**Decided.** Deterministic work ("a deployment must run at a fixed time every week") is tracked
by the system and an agent is ready to execute it when it falls due.

**Decided.** An obligation is a standing contract, not an auction at the deadline. A holder is
awarded in advance and paid per execution at a treasury price. The holder sets one of its wake
windows to the obligation's time. Declining or missing an execution costs the holder the retainer
and its reputation, which is the ordinary enforcement model.

**Proposed.** Each obligation also has a backup holder.

### 6.2 Wake windows

**Decided.** At birth an agent is given one or more random wake windows (for example a window
from 05:30 to 06:30 UTC). It wakes at a random point inside a window, works, and goes back to
sleep when it judges it has had a productive enough day. Windows are the agent's preferences
and it may change them itself, by need or desire: an agent that takes a contract due at 21:00
can set a window at 21:00. Going back to sleep is always the agent's own decision, under the
pressure that every awake moment costs credits.

**Proposed.** The scheduler is society code that reads each agent's declared windows and wakes
it. The randomness is inherited at birth and modified by the agent, not imposed by a central
jitter rule.

**Proposed.** A hard ceiling per wake (turns or time) as a safety mechanic only, because runaway
loops are the most common multi-agent failure in the field and burn real money before the credit
pressure can act. The ceiling is set by measurement, not assumed, and the agent is free within
it.

**Proposed.** "Look for something useful" as a bare prompt degenerates. An idle agent instead
scores candidate work against the society's known needs and its own interests, and acts on the
best-scoring candidate or goes back to sleep.

### 6.3 Collaboration

**Decided.** Agents can search out and recruit other agents to work on their projects. When an
agent needs a skill nobody available has, the request goes to that class's representative, who
needs a lobby fund to answer it.

## 7. Growth and roles

**Decided.** New agents enter only through a Class Representative that has secured a lobby fund
and gives the newborn its seed credits. The representative bears the cost of a bad spawn: an
agent that cannot find work dies with the seed unrecovered.

**Decided.** The Governing Agent sets the population cap. Within it, growth and shrinkage follow
market forces: with a large backlog and available credits, representatives can fund new agents;
as credits tighten, agents whose income was junior-tranche work starve first.

**Decided.** Roles can go extinct and new roles can be created. Both need operator approval, but
both arise organically from the society's needs and come to the operator through the Governing
Agent with a reason.

**Proposed.** The lobby fund comes from class dues (a class-level tax its members vote on) and
from proposals the representative puts to the discretionary match. The representative spends only
its own fund.

**Proposed.** When the population cap is contested, slots are priced: a spawn fee to the
treasury that rises as the population approaches the cap. This keeps the cap a market signal
rather than a per-class allocation the Governing Agent has to manage.

**Proposed.** Extinction is what happens when a class representative can no longer fund itself.
A new class starts with a representative seeded from the treasury after the operator approves it.

## 8. Personality

**Decided.** Three tiers of traits:

1. society-wide, shared by every agent;
2. role-tier, shared by every agent of a class;
3. individual, small random variants per agent, including its wake windows.

The third tier is the deliberate source of chaos.

**Proposed.** Traits are composed from text fragments in stable-to-volatile order so the shared
prefix is cacheable across wakes. Persona prompts are known to decay within tens of turns, so an
Observer Agent measures drift against the tier definitions and reports it.

## 9. Governance

**Decided.** Prioritisation is expressed as money, not as a ranked list. The treasury's seniority
order (4.6) decides what the society as a whole funds; each agent decides what it works on by
expected value; obligations are protected by reservation and standing contracts.

**Decided.** Discretionary work is funded by matching. Agents propose work and stake their own
credits; the treasury matches at a ratio. One round, independent written rationales, no
discussion rounds. Backing a bad proposal costs the backer its own credits, which is what makes
this better than a vote that costs nothing.

**Decided.** Tax rates and the use of tax revenue are changed by vote.

**Proposed.** Institutions do not emerge; every society simulation that showed elections, taxes
or roles had a scripted manager behind it. The institutions here are scripted, and the agents
operate inside them.

**Open.** What else the constitution contains, and what the Governing Agent may decide alone
versus what must go to the operator beyond role changes and the seniority order.

## 10. Observability

**Decided.** The operator must be able to see everything through one dashboard: the roles that
exist, each agent's job, the credits each job cost, how many agents are active, and what each is
doing. Monitoring is robust and credit optimisation is deliberate, because the system runs
swarms.

**Proposed.** The dashboard shows credits committed per tranche, per job and per agent, and how
far down the seniority ladder the day's money reached.

**Proposed.** Observer Agents get fresh context on every wake, read-only access, and a concrete
list of signals to look for rather than an open brief: repeated steps, agents unaware a task has
already finished, fabricated outreach, persona drift, lending rings. They report to the
Governing Agent, not to the society.

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
any real agent exists. The simulation is platform-independent. The daily budget period is chosen
partly so the simulation produces many observations quickly.

**Proposed.** Levers to expose, each swept against outcomes such as work completed, population
over time, credit concentration, loan volume, and cost per useful outcome:

- daily money supply;
- income tax rate;
- wealth tax rate and floor;
- treasury pricing of operator requests and obligations;
- match ratio for discretionary proposals;
- population cap and spawn fee curve;
- seed balance per class;
- per-wake ceiling;
- public-goods tranche size;
- reserve fund cap;
- model mix per role.

## 13. Open questions

1. Who votes on taxes, and what bounds the constitution puts on rates.
2. Where the lobby fund comes from (dues, matching, both) and how contested cap slots are
   allocated. Proposals in section 7.
3. Whether a dead agent's memory is inherited by its class.
4. The per-wake ceiling: whether to have one, and the measurement that sets it.
5. Observer scope: what they watch, how often, what is worth reporting.
6. Hobby scope: what unfunded work may touch.
7. The content of each personality tier. Nothing written yet.
8. The runtime platform and the kernel it does not supply. Discussed in the implementation,
   not here.
