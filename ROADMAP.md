# Roadmap

Status: 2026-10-17. Two parallel tracks, then convergence.

## Track 1: spikes against the runtime platform

Short, measured experiments that confirm how each platform mechanic the model depends on
actually works, and document it. Each spike runs for minutes, answers one question with a
number, and is written up before the next. The first five questions are in
`design/infrastructure.md` section 7. Output: a confirmed list of what the platform gives by
configuration, what it does not, and the real costs the economy will price off.

## Track 2: the UI first

Build the operator's console before the society runs, so there is a natural place to organise,
build and visualise the agents, roles and institutions. The four screens the operator asked for
come first: Dashboard, Agents, Skills, Institutions. See `design/ui.md`. The UI drives the
kernel's schema: an agent, a skill, an institution and a lever have to exist as records before
they can be shown and edited, so building the UI builds the ledger.

## Convergence

When the UI can define a class, its skills and its institutions, and the spikes have confirmed
the worker session, memory namespaces and gateway mirror, the society can be run with the stub
decision policy as a simulation, then with the real decision model and real workers at the
smallest population that exercises every institution.

## Not yet scheduled

- The simulation harness and lever sweeps.
- The labelled decision set for measuring the decision model.
- Tier 1 and tier 2 personality content.
