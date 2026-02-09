# release-management-notes
Practical notes on release management, environment drift, and deployment visibility in complex systems.

# Release management notes

This repository collects practical notes, patterns, and observations related to
release management in multi-repository and microservice-based systems.

The focus is not on tooling, but on **failure modes** - situations where releases
appear correct on paper, yet environments behave differently in practice.

These notes are based on hands-on release management work across complex delivery
setups involving multiple teams, CI/CD pipelines, and environments.

---

## Scope

The problems described here commonly appear in systems that have:
- multiple independently deployed repositories
- shared environments (QA, UAT, PROD)
- parallel development streams
- partial ownership across teams

They are not tied to any specific technology stack or vendor.

---

## Common failure patterns

### 1. Missing deployments

One of the most frequent root causes of production issues is not defective code,
but code that was **never deployed** to a given environment.

Typical scenario:
- a release spans multiple repositories
- most services are deployed successfully
- one service is silently skipped
- the release is considered “complete” because all pull requests are merged

Since no explicit failure occurs, this often remains unnoticed until runtime
behavior diverges.

---

### 2. False release confidence

Release artifacts such as branches, tags, and version numbers often create
a false sense of certainty.

Common assumptions include:
- “All changes are merged”
- “The release branch exists”
- “The pipeline is green”

None of these guarantee that the **same artifacts** are actually running
in the same environment.

---

### 3. Fragmented visibility

Release information is typically scattered across:
- CI/CD systems
- version control platforms
- ticketing tools
- spreadsheets
- chat conversations

No single source reflects the actual state of deployments across environments,
which forces teams to reconstruct reality manually.

---

## Environment drift

Environment drift describes a situation where environments expected to be equivalent
(QA, UAT, PROD) slowly diverge over time.

Common causes:
- partial deployments
- hotfixes applied to a single environment
- manual interventions
- emergency rollbacks
- environment-specific configuration changes

Drift is rarely intentional. Once introduced, it often persists unnoticed.

---

## Multi-repository releases

In multi-repository systems:
- releases are coordinated implicitly rather than explicitly
- ownership is split across teams
- responsibility for “the whole release” is unclear

This increases cognitive load, especially during:
- incident response
- regression testing
- release approvals
- audits and post-mortems

---

## Signals teams often miss

In many cases, early warning signs exist but are easy to overlook:
- one repository lagging behind others
- long gaps since the last deployment to a given environment
- build numbers that differ without a clear reason
- reliance on assumptions instead of verification

These signals usually surface too late to be useful.

---

## Why these problems persist

Most delivery tooling focuses on:
- individual repositories
- individual pipelines
- individual deployments

Release management problems, however, emerge at the **system level** -
between repositories, environments, and teams.

As a result, many organizations rely on manual processes and institutional knowledge
to bridge these gaps.

---

## Motivation

These notes exist because the same classes of problems appear repeatedly across
different organizations and projects.

In practice, teams often discover release issues:
- during late-stage testing
- during user acceptance
- or directly in production

By the time they surface, the cost of fixing them is already high.

---

## Related work

As a response to these recurring patterns, we are experimenting with a tool that
aims to improve visibility around what is **actually deployed** across environments,
rather than what was planned or assumed.

More context:
https://watchturm.com
