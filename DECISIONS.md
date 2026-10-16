# Decision log

Newest at the bottom. Each entry gives the decision and the reason, so a later reader can tell
whether the reason still holds.

## 2026-10-15

**Credits as the economy.** Agents earn credits to keep working and end when they cannot get
more. Reason: it turns cost control into a selection pressure instead of a cap imposed from
outside, and it gives the society a reason to allocate effort.

**Infrastructure is bought, not built.** Context, memory persistence, communication and security
come from a platform. Reason: a previous system became unmaintainable because too many
half-finished subsystems were hand-built.

**Simulate before building.** Reason: the in-society currency has no precedent in the literature,
and every lever value would otherwise be a guess.

## 2026-10-16

**Selection is on performance, not turnover.** Successful agents accumulate surplus and need
never die; unsuccessful ones starve. Reason: the operator corrected an earlier framing in which
death was scheduled. Hoarding is accepted as a risk to be handled by governor levers.

**Price by negotiation.** Poster stakes, bidders offer, poster awards, parties agree the payment
model. Reason: the price should be set by demand, not by a tariff.

**No enforcement class.** Cheating is punished by reputation and shunning. Reason: the operator
does not want a police system; social pressure is the enforcement.

**Mechanics are the rules.** "No hard rules" means no enforcer, not no mechanism. The ledger is a
mechanic that records every contract, payment and attestation and is readable by all agents.
Reason: reputation computed from a record is robust to gossip and to one agent fabricating
claims about another.

**Governor sets the cap; market sets the population.** Reason: the operator wants macro levers
in one place and the micro decisions (spawn signatures, earning models) left to the governor.

**Roles change with operator approval.** Extinction and creation arise from the society's needs
and come through the Governing Agent with a reason. Reason: role definitions bind tools and
credentials, which are the operator's to grant.

**Prioritisation is money, not a list.** The treasury pays claims in a constitutional seniority
order (obligations, operator requests, public goods, discretionary, then hobbies from savings);
agents pick work by expected value. Reason: a ranked list needs a planner; a seniority order
decides cuts without anyone choosing victims.

**Observers outrank discretionary work.** Reason: if workers stop while observers run, problems
are still found and the ground stops moving under the workers. The reverse lets workers make a
mess nobody sees.

**The Governing Agent prices operator requests.** The operator supplies a priority list and may
override a stake, but leaves stakes to the governor. Reason: an operator who prices everything
becomes the planner.

**Discretionary work is funded by matching, not by a free vote.** Reason: backing a bad
proposal must cost the backer; costless ballots reward sycophantic agreement.

**Budget period is one day.** Reason: daily allocation yields many more observations of the
society's behaviour with money while the levers are still being learned.

**Class Representatives are the only source of new agents.** A representative with a lobby fund
spawns an instance and donates its seed credits. Reason: an agent that does not exist cannot
negotiate for its own first credits, and a representative that pays for bad spawns has the right
incentive not to make them.

**Death is permanent; hibernation is for the solvent.** An agent dies when nobody will pay it.
An agent with credits may sleep after a unit of work. Reason: keeps selection on performance
while giving successful agents a way to stop spending.

**Agents own their wake windows.** Random at birth, editable by the agent, sleep is the agent's
call, and every awake moment costs credits. Reason: puts the stochastic wake under the economy
rather than under a central scheduler rule, and lets an agent align itself to a contract.

**Hoarding is handled by tax, not currency decay.** Income tax on earnings and a daily wealth
tax on holdings; both votable. Lending with interest is the tax-driven outlet for idle credits.
Reason: taxes are visible, votable and fund the treasury; decay is none of those.

**Hard spend cap at the gateway equals budget plus reserve; the Governing Agent is always
funded.** Reason: the Governing Agent must be able to report the state of the system even when
every other agent is stopped, so its running cost is the operator's, outside the treasury.

**Lobby funds come from class dues and matched proposals; every approved role gets a public
goods minimum.** Enough to keep the representative and one worker alive. Reason: a role the
operator and governor judged worth having must always have one agent able to wake. Consequence:
extinction becomes a decision to withdraw the minimum, not starvation.

**No hard population cap; a spawn fee curve instead.** The fee rises with population until one
more agent is unaffordable. Reason: a cap is an allocation someone has to manage; a rising price
removes artificial demand and stops the population spiralling with no one deciding.

**Direct democracy on a weekly cycle.** One agent, one vote, on every issue; one week to propose
amendments, one day to vote; proposing costs credits scaled by effect; votes arrive in an inbox
and voting is optional. Reason: agents are not all awake at once, so a cycle is needed; pricing
proposals stops noise.

**Dead agents' memories are public.** Any agent may become a historian with its own credits.
Reason: a keeper of past lessons could be an essential guide for the society.

**No per-wake ceiling.** An agent that spends itself to death in one wake is natural selection
and an example to the others. The mechanic that makes this safe: per-agent gateway budgets
mirror credit balances, so a runaway wake can burn only what the agent owns.

**Model-agnostic by design.** Different roles may run on different vendors' models. Reason: the
operator does not want to be locked into one company's models and wants to experiment with
model per role. Consequence: no single-vendor managed-agent platform can be the spine, and
per-agent cost caps have to be enforced at a gateway or in the kernel rather than by the platform.
