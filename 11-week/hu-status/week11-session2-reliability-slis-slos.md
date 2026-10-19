# Week 11 — Session 2: Reliability Targets — SLIs, SLOs, Error Budgets and Alerting

**Topic:** Planning for reliability (Site Reliability Engineering)
**Course:** Sistemas Distribuidos (2026-B, G1)

## Introduction

Observability explains *what is going on* inside a system. The follow-up question is *how reliable does the system need to be, and how do we defend that level?* That is where **SLIs, SLOs, error budgets and alerting** come in — the main planning tools of Site Reliability Engineering (SRE). Combined, they convert a vague wish like "the system should be reliable" into specific, measurable goals, plus a disciplined rule for deciding when to keep releasing features and when to pause and stabilize.

## Why Reliability Needs Explicit Targets

Aiming for 100% reliability is unrealistic and also a poor use of money. Distributed systems will always experience failures — broken hardware, network problems, outages in dependencies — and chasing the final fraction of a percent demands huge effort for improvements users seldom notice.

For that reason, teams replace "never fail" with a clear definition of **how reliable is reliable enough**: an explicit goal that weighs reliability against how fast new features can be delivered. *Reliability is a feature that has a price; setting targets lets you decide that price on purpose.*

## SLI — Service Level Indicator

An **SLI** is a number that measures one aspect of a service's health, taken from the user's perspective.

- **Typical SLIs:** availability (% of requests that succeed), latency (how quickly requests finish), error rate and throughput.
- **Strong SLIs** are measured where the user interacts with the system, are objective, and are difficult to manipulate.
- **Example:** "the share of requests answered in less than 300 ms" is a latency SLI.

## SLO — Service Level Objective

An **SLO** sets a target value (or range) for an SLI over a defined period of time.

A well-formed SLO specifies four elements: the **SLI** being measured, the **target** (e.g. 99.9%), the **time window** (e.g. a rolling 30 days) and the **scope** (the service or endpoint it covers).

- **Example:** "99.9% of requests succeed across a rolling 30-day window."
- An SLO should match what users and the business actually need — not just the highest value the team can reach.

## SLA — Service Level Agreement

An **SLA** is a formal agreement with customers that defines **consequences** (for example service credits or penalties) when the promised level is missed.

- SLAs are normally *less strict* than the internal SLOs, giving the team a buffer to fix issues before the contract is broken.
- Not all services have an external SLA; internal services may rely on SLOs alone.

## Error Budget

The **error budget** is the amount of unreliability the team is *permitted*: `error budget = 100% − SLO`.

- **Example:** with a 99.9% SLO, the error budget is 0.1% — about **43 minutes of downtime per month** (over 30 days, 0.1% ≈ 43.2 minutes).
- The budget is meant to be **used**, not saved: it can be spent on releases, experiments and deliberate risk.
- **Policy:** while there is budget left, the team can release quickly; once it is used up, the team slows down or stops risky changes and concentrates on stability until reliability is restored.

This turns the classic conflict between development (which wants to move fast) and operations (which wants stability) into a common rule based on data.

## Alerting Based on SLOs

An alert should call a person when **users are actually being affected**, not every time something small happens internally.

- **Alert on symptoms rather than causes:** page someone when user-facing SLIs get worse, not for every CPU spike.
- **Burn-rate alerts:** alert on the speed at which the error budget is being used. A fast burn (e.g. 2% of the monthly budget within an hour) is urgent; a slow burn should become a ticket, not a 3 a.m. page.
- **Make alerts actionable:** each alert should demand a human decision or action. Large numbers of noisy, non-actionable alerts lead to **alert fatigue**, and real problems end up being ignored.
- A smaller set of meaningful alerts means quicker responses and a healthier on-call rotation.

## The Reliability Loop

All of these ideas fit into an ongoing cycle:

1. **Measure** — gather the SLI.
2. **Set a target** — define the SLO.
3. **Track** — follow the error budget.
4. **Alert** — notify based on symptoms and burn rate.
5. **Decide** — release new features if budget remains; stabilize if it is used up.

The cycle then repeats, with targets adjusted as the system and user expectations change.

## Key Takeaways

- **SLI** = what is measured; **SLO** = the goal for it; **SLA** = the contract that carries consequences.
- **Error budget** = 100% − SLO = the unreliability you are allowed; spend it on change and freeze when it is gone.
- **Alert** on user-facing symptoms and burn rate, and keep alerts actionable to prevent fatigue.
- The reliability loop connects everything: measure → target → track → alert → decide.

## Infographic

See [`reliability-slis-slos-vertical.png`](./reliability-slis-slos-vertical.png) in this folder for a visual summary.

## OptiView mapping (Week 11)

For OptiView after the MVP 2 classroom expo, the first candidate SLIs sit at the **user boundary**: the gateway's request success rate and latency (p95), together with the place-order / payment flows that span sales + workflow. An internal SLO (e.g. 99.9% successful gateway requests over 30 days) would set the **error budget** that controls how quickly we deliver the next evolutionary slices versus when we freeze and stabilize. Burn-rate alerts should page on user-facing symptoms, not on every small CPU blip in a container.
