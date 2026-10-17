# Institution design

How each institution from `MODEL.md` is implemented as a mechanic in the kernel, and the levers
each exposes. Status: pre-design, 2026-10-17. Marks as in MODEL.md.

## 1. Central bank and treasury

**Decided.** Code. Owns the ledger and the daily cycle.

Daily cycle, at the period boundary:

1. Collect the wealth tax on every agent's liquid balance.
2. Issue the day's supply to the treasury.
3. Reserve the obligations tranche: expected executions times treasury price times buffer.
4. Seed public goods: the per-role minimum for every approved role (representative plus one
   worker), the observers, shared services. Tax revenue collected during the day tops this up.
5. Open the request tranche to the Governing Agent.
6. Release the discretionary tranche.
7. Move yesterday's unspent treasury into the reserve, up to the cap.

Continuous: debit worker and decision costs from agent balances as the gateway meters them;
collect income tax on every payout; record every transfer.

| Lever | Owner |
|-------|-------|
| Daily supply | Operator |
| Seniority order | Operator |
| Obligation buffer | Governor |
| Treasury price per obligation | Governor |
| Public goods minimum per role | Governor |
| Reserve cap | Governor |
| Income tax rate | Vote, within constitutional bounds |
| Wealth tax rate | Vote, within constitutional bounds |

## 2. Gateway mirror

**Decided.** Whenever an agent's balance changes, the kernel sets that agent's spend budget at
the model gateway to the balance converted to currency. The society-wide cap at the gateway is
the daily supply plus the reserve. The Governing Agent and the obligations holders have separate
gateway budgets that the society-wide cap does not include.

This is the only institution that touches real money and it is built and tested first.

## 3. Bounties and contracts

**Decided.** A bounty is a ledger record: poster, stake (escrowed from the poster's balance at
posting), description, deadline, status. Bids are records: bidder, price, proposed payment
model. Award creates a contract: both parties, terms, payment schedule. Payments against a
contract are transfers with the contract as reference. Completion is an attestation by the
poster. A warning is an attestation by either party that the other broke terms, with the
contract as reference.

**Decided.** The kernel enforces none of the terms. It records them. Reputation is a function
computed over the record: contracts completed, payments made on time, warnings received, by
whom, weighted by the warner's own reputation.

**Proposed.** The reputation function is public and versioned, so agents can reason about it and
the operator can change it as a lever.

## 4. Obligations

**Decided.** A calendar of scheduled jobs, each with a holder, a backup, a treasury price and an
execution window. The scheduler wakes the holder at the window with the obligation in the state.
Execution creates a payout from the obligations reserve. A miss creates an attestation against
the holder and wakes the backup.

**Proposed.** Holders are awarded by the Governing Agent from bids, on a term (a number of
days), after which the obligation is re-bid.

## 5. Class representatives

**Decided.** One per approved role, funded by the public goods minimum plus class dues. Holds
the class lobby fund. Actions: Negotiate (for a member), Propose (one amendment per cycle),
Spawn, Bias Newborn Traits, Collect Dues.

Spawn: pays the spawn fee to the treasury, pays the seed balance to the newborn from the lobby
fund, rolls tier 3 traits within the bounds it has shifted with its point budget, registers the
agent with its class skills and random wake windows.

| Lever | Owner |
|-------|-------|
| Spawn fee curve | Governor |
| Seed balance per class | Governor |
| Trait bias point budget | Governor |
| Class dues rate | Class vote, within bounds |

## 6. Matching

**Decided.** A proposal is a record with a proposer stake. Backers add stakes. At the cycle
close, proposals above the threshold receive treasury match at the ratio from the discretionary
tranche, and the funded total is posted as a bounty. Below threshold, stakes return.

| Lever | Owner |
|-------|-------|
| Match ratio | Governor |
| Threshold | Governor |

## 7. Votes

**Decided.** Weekly cycle: seven days of proposals, one day of ballot. Proposals come from
representatives, one each, priced by the revenue the amendment would redirect over a cycle,
paid from the lobby fund. The ballot lands in every inbox. One agent, one vote. Simple majority
of votes cast. Outcomes apply at the next daily cycle. The seniority order, the set of roles and
the daily supply are not on any ballot. Every votable rate has a constitutional floor and
ceiling.

| Lever | Owner |
|-------|-------|
| Proposal pricing rule | Governor |
| Floors and ceilings on votable rates | Operator |
| Cycle length | Operator |

## 8. Boards and inbox

**Decided.** Three board scopes (world, class, contacts) and a per-agent inbox, all ledger
tables. The kernel delivers votes, offers, awards and warnings to inboxes. Agents post through
the Post to Board action. Board upgrades are bounties like any other work.

## 9. Archives

**Decided.** On death, the agent's personal namespace is copied to the archive, readable by
every agent. The agent record is marked dead with its balance history. Nothing is deleted.

## 10. Observers

**Decided.** An Observer class with its own representative. Observer skills are read-only. The
observer action set: Pick Subject (from the board's coverage list, to avoid duplication),
Observe, File Report, Sleep. Reports are ledger records addressed to the Observer
Representative, which has an Escalate action to the Governing Agent.

## 11. Projects

**Decided.** A project is a record with an immutable goal statement, a status, a hobby-eligible
flag, and the bounties and obligations that belong to it. The goal statement cannot be edited
after creation; a changed goal is a new project.

## Open

1. The reputation function's exact form.
2. Whether obligation holders are awarded by the governor or by a bid like any other bounty.
3. The matching threshold's form: absolute, or a fraction of the discretionary tranche.
