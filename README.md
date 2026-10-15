# talos-sdlc-jtbd

This repo is Terem's working attempt at automating the full software development lifecycle. It starts by defining what "the full SDLC" means. We break delivery into 81 jobs, from the first signal that something is worth building to the learning that feeds the next cycle. Each job is rated on how much of it an AI agent can carry today.

The aim is a shared, inspectable map of what agents can take over and what still needs a person. Nine decision gates stay with humans in every configuration.

The framework is part of [Project Talos](https://github.com/terem-tech), Terem's open-source plugins, skills and tools for mid-market agentic engineering.

## What's in the repo

| File | What it holds |
|------|---------------|
| `terem-agentic-sdlc-81-jobs.csv` | The 81 jobs, with who runs each one under three team mixes |
| `terem-agentic-sdlc-81-jobs-full.csv` | The full source data: descriptions, agent ratings, dependencies and feedback loops |

## The model

### 81 jobs in four groups and seven stages

| Group | Stages | Jobs |
|-------|--------|------|
| Discovery | Discover | 12 |
| Definition | Define | 10 |
| Development | Plan, Build, Verify, Release | 47 |
| Operate | Operate | 12 |

Each job is named as a job to be done (for example "Formalise the spec", "Steer the agent fleet" or "Plan rollback & recovery"). Jobs depend on earlier jobs, so the set forms a dependency graph rather than a checklist.

### The SDLC is a loop

Three jobs feed back into earlier stages:

1. **Triage and route defects** loops back to **Implement the change-set**.
2. **Update context & harness** loops back to **Set standards & guardrails**.
3. **Feed the next cycle** loops back to **Triage the signal**.

### Nine decision gates, always human

| Stage | Gate | Decision |
|-------|------|----------|
| Discover | Decide to pursue it | Go / no-go: worth defining, park it, or kill it |
| Define | Agree the definition | Sign-off on scope and business case before planning |
| Plan | Agree the plan | Sign-off before the build is delegated |
| Build | Re-plan on surprises | Whether a build discovery changes scope or approach |
| Verify | Review the diff | An experienced engineer judges the change at the PR boundary |
| Verify | Run UAT with real users | Business owners and users try it and sign off |
| Release | Approve the release | Go / no-go with evidence, risk and rollback on the table |
| Operate | Call keep or roll back | Whether the release stays in or gets pulled |
| Operate | Measure the outcome | Keep, pivot or kill, judged on real numbers |

Agents can prepare the evidence for every gate. The decision and the accountability stay with a person.

### Agent ratings and team mixes

Every non-gate job has an agent rating from 0 to 1: how much of the job an agent can carry with current tooling. Decision gates are rated 0.1 and are always human, whatever the mix.

A mix is a threshold on that rating. A job goes to an agent when its rating meets the threshold.

| Mix | Threshold | Jobs run by agents |
|-----|-----------|--------------------|
| People-led | 0.70 and above | 17 of 81 |
| Balanced | 0.50 and above | 43 of 81 |
| Fully agentic | every non-gate job | 72 of 81 |

The ratings are Terem's judgement as of October 2026. They will move as models and tooling improve. Treat them as a starting point to argue with, not a measurement.

## Column reference

### `terem-agentic-sdlc-81-jobs.csv`

| Column | Meaning |
|--------|---------|
| `id` | Stable job identifier |
| `group` | Discovery, Definition, Development or Operate |
| `stage` | discover, define, plan, build, verify, release or operate |
| `job` | Job name |
| `one_liner` | One-sentence summary |
| `decision_gate` | `yes` for the nine human gates |
| `runs_by_people_led` | `agent` or `human` under the People-led mix |
| `runs_by_balanced` | `agent` or `human` under the Balanced mix |
| `runs_by_fully_agentic` | `agent` or `human` under the Fully agentic mix |
| `depends_on` | Space-separated ids of the jobs this one needs first |

### `terem-agentic-sdlc-81-jobs-full.csv`

| Column | Meaning |
|--------|---------|
| `id` | Stable job identifier |
| `order` | Position in delivery order (1–81) |
| `group`, `stage`, `job`, `one_liner` | As above |
| `description` | What the job involves and how agents and people share it |
| `decision_gate` | `yes` for the nine human gates |
| `agent_rating_0_to_1` | How much of the job an agent can carry today |
| `depends_on` | Space-separated ids of upstream jobs |
| `loops_back_to` | Ids of earlier jobs this one feeds back into |

## How to use it

1. **Audit your own delivery.** Map your team's current work against the 81 jobs. The gaps show the jobs nobody owns today.
2. **Pick a starting mix.** Filter the summary CSV on one of the `runs_by_*` columns to see which jobs you'd hand to agents first.
3. **Set your own thresholds.** Apply your own cut-off to `agent_rating_0_to_1` in the full CSV, or re-rate jobs for your stack and risk appetite.
4. **Build the graph.** Use `depends_on` and `loops_back_to` to draw the pipeline, plan sequencing, or find the critical path.

## Talk to Terem

Terem is an Australian product engineering and strategy consultancy. If you want help working out which jobs to hand to agents first, book 30 minutes with [James Piskorz](https://calendly.com/james-piskorz-terem/30min) or email [info@terem.com.au](mailto:info@terem.com.au).

[terem.tech](https://terem.tech)

## License

To be confirmed.
