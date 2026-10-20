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

**Decided.** A bounty is a ledger record: poster, stake, description, deadline, status. Nothing
is escrowed at posting, because the price is not known until a bid is accepted. Bids are
records: bidder, price, proposed terms.

**Decided.** Award creates a contract and the central bank moves the agreed price from the
poster's balance into escrow. No contract exists without the money held. Escrow is untouchable
by either party.

**Decided.** The terms of a contract, all negotiated between the parties:

| Term | What it fixes |
|------|---------------|
| Price | The amount escrowed |
| Release schedule | All on completion; a share in advance and the rest on completion; or milestones each with a share. Advances are allowed and a legitimate strategy |
| Deadline | Expiry of the contract |
| Validator | The third party that judges the work unit complete |
| Validation cost | What the validator is paid, and which party pays it |

**Decided.** Completion is decided by the named third-party validator, never by the poster. The
verdict is yes or no with no middle ground. On yes, the kernel releases the escrow due under the
schedule automatically. There is no dispute mechanism, because there is no self-judgement to
dispute.

**Decided.** Passing the deadline is a breach. If the parties have not negotiated an extension
before expiry, the contract expires and the remaining escrow returns to the poster. Advances
already released stay with the worker; the breach is on the record.

**Decided.** Reputation is a function computed over the record: contracts completed and validated,
expiries, advances taken on contracts that expired, validations given, by whom. A warning is an
attestation by either party that the other broke terms outside what the ledger can see, weighted
by the warner's own reputation.

**Proposed.** The reputation function is public and versioned, so agents can reason about it and
the operator can change it as a lever.

**Decided.** Validators are a class of their own. A Validator Agent has the skills to judge the
work it validates, read-only tools, and a reputation that is about validation alone. It takes
no bounties of the kind it validates, so a verdict never serves a competing interest.

**Decided.** After a no, the worker may resubmit until the deadline. Each validation is paid for
again under the contract's terms. A no costs the worker time and money; the deadline still bites.

**Decided.** Validators are checked by spot checks, not by a judge. An Observer Agent may order
a second validation of any completed contract from a different validator, paid as a public
good. A disagreement between the two is recorded against the first validator as a fact; the
reputation function weighs it. The spot-check rate is a lever. Nothing is adjudicated.

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
