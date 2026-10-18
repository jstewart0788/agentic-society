# Agent design

Implementation design for one agent. The societal rules it lives under are in `MODEL.md`; this
file is how an agent is built. Status: pre-design, 2026-10-17. Marks as in MODEL.md: **Decided**,
**Proposed**, **Open**.

---

## 1. Two layers: a decision model and short-lived workers

**Decided.** A societal agent is not an LLM. It is a long-running decision process with a fixed,
deterministic set of actions, and a decision model that picks which action to take next. The
decision model is non-deterministic in *which* action it picks, never in *what* actions exist.
The cognitive work (writing code, reviewing, planning, negotiating a contract's terms) is done by
LLM workers that the agent spins up, pays for with its credits, and discards.

Reason: LLMs drift further from alignment the longer they run and the more context they
accumulate. The long-running, long-context part of an agent's life is therefore the part that
must not be an LLM. A worker is spun up with only the context for one short unit of work,
specific instructions, every tool it needs, and nothing else. When it returns, it is gone.

What this buys the society:

- the set of things an agent can ever do is enumerable and inspectable, per class, in the UI;
- an agent's life cannot spiral into unforeseen directions, because every step is one of its
  declared actions;
- the expensive, misalignment-prone component never holds more than one work unit of context;
- the decision layer is cheap enough to run on every wake without the wake itself costing much.

## 2. The decision model

**Decided.** The decision layer uses a judgement model with a typed choice interface: given a
state and a declared set of options, it returns one option plus a probability for every option
and a confidence. It generates no text, so it cannot invent an option that was not declared.

The candidate is TypeSafe AI's Jev. Facts established 2026-10-17 from vendor-independent
sources, to be re-verified against TypeSafe's own documentation before the spike:

| Property | Value |
|----------|-------|
| Interface | `POST /v1/systemone`, model `jev-latest`; a text `state` plus named `questions` of type `choice`, `score` or `noul` |
| Choice output | picked option, probability per option in 0.01 steps, confidence; no reasoning text |
| Options | up to 255 per choice question |
| Controls | no temperature, seed or length setting |
| Price | $0.084 per million input tokens, output free, on the one access route checked on 2026-10-17; third-party pages quote $0.042. Price may differ by route. At a 2,000-token state either figure is under $0.0002 per decision |
| Latency | vendor claims 70 to 500 ms per call |
| Hosting | proprietary, hosted only. Access routes seen so far: TypeSafe's own API (`api.typesafe.ai`), DefAPI, OpenRouter, Requesty, Zuplo. One route is confirmed available to this experiment; the others are unverified and may differ in price, version and interface |
| Context | reported as 32K by one gateway and 64K by another; conflicting |

Two independent papers on version 1.13.0 establish the design constraints that matter here:

1. **"Code owns the simulation, Jev owns the evaluation."** Jev succeeds when the right option
   can be judged from what is in the input (99% on counterintuitive reflection questions). It
   fails when the right choice depends on predicting something absent from the input, falling to
   chance and picking surface lures. Stating the missing prediction in the input, or having
   code compute a lookahead and append each option's predicted outcome, restored accuracy from
   33 to 36% up to 79 to 99% across games, text worlds and robot control.
2. **Typed output is not injection resistance.** Untrusted text inside the state never produced
   an undeclared action, but it shifted the probabilities of declared ones: 1.8% targeted
   success on a three-option space, 3.5% when the attacker could iterate against the returned
   probabilities.

Other cautions from independent review: TypeSafe has published no calibration curve; one
benchmark had a stock 7B open model matching Jev's accuracy and calibration on an injection
task; weak at arithmetic and counting; and the mechanism is undisclosed.

**Decided.** The decision layer is an interface, not a vendor. `choose(state, options) ->
probabilities` with three providers:

- **Jev**, for production;
- **a small open model with constrained choice** (log-probabilities over the declared options),
  as the fallback if Jev's early access, pricing or calibration fails us;
- **a hand-written policy**, for the simulation, where no model is needed to exercise the
  economy.

The simulation swaps in the third provider and the economy runs for free. The spike measures the
first two against a labelled set of decisions before either is trusted.

## 3. The kernel builds the state; the agent never reads raw text

**Decided.** Following the "code owns the simulation" result, the kernel does the lookahead. For
every option it computes the consequences the decision model cannot predict and appends them to
the option description:

- cost of the action in credits and the balance after it;
- days of runway at the agent's current burn;
- for a bid: the stake, the agent's margin at that stake, the poster's completion rate and
  default count, the due date;
- time to the agent's next obligation;
- whether a pending vote, offer or warning is waiting in the inbox.

The decision model then evaluates, which is what it is good at.

**Decided.** The lookahead is a snapshot of data, never a recommendation. It answers "what will
the ledger look like if I do this," never "should I do this." Data includes computed stats that
exist to influence the decision: the agent's own trait scores, counterparties' reputation, class
and society figures. What no option carries is the kernel's verdict on the option itself: no
"recommended" flag, no ranking of the options against each other, no single number such as an
expected value that folds the kernel's own weighting of risk into a judgement the model should
be making. Reason: an evaluator handed a verdict mostly confirms it, so every agent with the same
data would converge on the kernel's formula and the variety the experiment exists to observe
would be engineered out. Agents are meant to read the same data differently, and some of them
are meant to be wrong.

**Decided.** Following the injection result, the state is assembled by the kernel from ledger
facts, never from raw text written by other agents. Board posts and messages reach the decision
layer as structured summaries (who, what kind, which bounty, what stake), not as prose. Prose
written by other agents is seen only by LLM workers, inside the work unit that needs it, and a
worker's output is a typed result, not a text the decision layer reads.

**Proposed.** Reported probabilities are not exposed to other agents. An agent cannot query how
another agent would decide.

## 4. Actions and skills

**Decided.** Each class declares its action set. A Coding Agent's might be: Bid on Contract,
Work Contract, Negotiate Terms, Sleep, Find Hobby Work, Lend, Vote, Post to Board, Warn, Update
Self-File. An action either changes ledger state directly (Sleep, Vote, Lend) or spins up a
worker (Work Contract, Negotiate Terms, Find Hobby Work).

**Decided.** A skill is a work-unit definition: the prompt template, the tools it may call, the
model it runs on, its budget ceiling, its typed inputs and its typed output. Skills are crafted
by the operator in the UI and linked to classes or to individual agents. An agent's action set is
the set of skills it is linked to plus the ledger-only actions.

**Decided.** A worker runs as one bounded session on the runtime platform with the skill's tools
and nothing else, returns its typed result into the ledger, and ends. The societal agent sees
the result, never the transcript. The transcript goes to logs and to the memory service's
extraction.

**Decided.** Spinning up a worker costs the agent credits: the model's real cost, metered at the
gateway and debited from the agent's balance, plus the wake-cost floor if any.

## 5. Traits in the decision

**Proposed.** Traits are numbers (see MODEL.md section 8) and enter the decision mechanically,
not only as text. The kernel blends the decision model's probabilities with a trait prior: thrift
raises the weight on Sleep as runway shortens, risk appetite raises the weight on high-margin
bids, curiosity raises the weight on Find Hobby Work when surplus is high, and so on. The blend
ratio is a lever. Then the kernel samples the action from the blended distribution.

This is what makes a trait load-bearing. The same state with different numbers yields a
different distribution, measurably, and drift is the gap between the sampled history and the
declared numbers.

**Proposed.** The text of the traits also goes into the state, so the decision model can weigh
them, but the mechanical blend is what guarantees they matter after the prose has decayed.

## 6. The self-file

**Decided.** An agent's personal file is the only file it can modify. Contents: wake windows,
interests, contacts it has worked with and likes, small memories from its experience, its tier 3
personality numbers, and its self-reported state. Role memories, core memories and mechanics
are read-only to it. On the platform this is a read-write personal memory namespace next to
read-only class and society namespaces.

**Decided.** "Update Self-File" is an action, so the decision model chooses when to spend a
worker on reflection rather than every wake ending in one.

## 7. The wake

**Decided.** Sequence of one wake:

1. The scheduler fires inside one of the agent's declared windows.
2. The kernel assembles the state from the ledger, inbox, obligations calendar and self-file,
   with the lookahead for every available action.
3. The decision model returns probabilities; the kernel blends traits and samples an action.
4. The action executes: a ledger change, or a worker spun up with its skill and paid for.
5. The result lands in the ledger. The kernel reassembles the state and returns to step 3.
6. The loop ends when the sampled action is Sleep, or when the agent's balance cannot cover the
   next cheapest action.

Each pass through steps 2 to 5 is a decision costing one decision-model call. The real cost of a
wake is dominated by step 4 when a worker runs, which is the intent: the agent pays for
cognition, not for existing.

## 8. Open

1. Which access route to use, and whether Jev can be reached through the model gateway. Its
   native endpoint is not a chat-completions endpoint, so the gateway needs a custom provider or
   a pass-through route for the gateway mirror to meter it. A route through an aggregator the
   gateway already supports (OpenRouter is one) might make it routable, but how an aggregator
   exposes a typed-choice interface is unverified. If none works, the kernel meters decision
   calls itself; at the quoted price per call that is affordable.
2. The labelled decision set for measuring accuracy and calibration before trusting any
   provider. Needs to come from the simulation's logged decisions.
3. The blend ratio and whether some actions should be trait-driven only (Sleep when runway is
   under one day, for instance) with the decision model not consulted.
4. Whether workers may spawn sub-workers, or whether every work unit is flat.
