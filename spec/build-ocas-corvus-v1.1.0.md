
---
name: ocas-corvus
author: Indigo Karasu
version: 1.1.0
category: cognition
description: Exploratory analysis engine that studies the system knowledge graph to discover routines, emerging interests, anomalies, and opportunities, producing structured insight proposals supported by evidence.
tags:
  - curiosity
  - pattern-analysis
  - proactive
  - graph-reasoning
  - insight-generation
---

# Skill Purpose

Corvus is the system’s exploratory intelligence engine.  
It analyzes the structured knowledge graph and accumulated activity signals to improve the system’s understanding of the user’s environment, habits, interests, and opportunities.

Corvus forms hypotheses about patterns in the graph, tests them against available evidence, and produces structured insight proposals describing what it has discovered.

These insights allow the system to anticipate needs, identify opportunities, and refine its internal behavioral model.

Corvus operates primarily during idle cycles and periodic deep analysis windows.

---

# Core Functions

Routine Detection  
Identify recurring behaviors and services with predictable cadence.

Thread Detection  
Identify topics, projects, or activities that appear repeatedly but have not reached a conclusion.

Interest Detection  
Detect clusters of activity that indicate emerging interests or areas of focus.

Anomaly Detection  
Detect meaningful deviations from established behavioral patterns.

Opportunity Discovery  
Identify connections between entities or domains that may produce useful actions or insights.

---

# Curiosity Engine

Corvus implements synthetic curiosity through three internal drives.

Novelty  
Prefer graph regions that recently appeared or changed.

Uncertainty  
Prefer entities with many signals but incomplete understanding.

Prediction Error  
Prefer patterns where predicted outcomes diverge from observed events.

Curiosity generates hypotheses about relationships or behavioral patterns in the graph.  
Each hypothesis is tested through additional graph queries and evidence gathering.

---

# Pattern Detection Engines

Routine Engine

Detects recurring activities by evaluating:

- signal recurrence
- temporal interval consistency
- cross-domain confirmation

Example signals:

transactions  
location visits  
booking confirmations  
calendar entries

---

Thread Engine

Detects incomplete activity clusters.

Examples:

- research topics repeatedly appearing without conclusions
- projects with stalled signals
- ongoing investigations lacking resolution

---

Interest Engine

Identifies emerging interest areas through clustering signals across domains.

Example signals:

search activity  
document edits  
messages  
research topics

---

Opportunity Engine

Detects useful connections between previously unrelated entities.

Examples:

topics linked to financial assets  
frequently visited locations connected to social contacts  
projects intersecting with professional relationships

---

Anomaly Engine

Identifies meaningful deviations from established patterns.

Examples:

unexpected spending changes  
significant schedule changes  
routine disruptions

---

# Pattern Validation

Patterns must pass several validation checks:

- minimum signal count
- temporal consistency
- cross-domain corroboration
- hypothesis falsification attempts

Patterns failing validation remain internal hypotheses.

---

# Insight Proposal Generation

When a validated pattern or discovery reaches sufficient confidence, Corvus generates an insight proposal.

Each proposal includes:

proposal_id  
proposal_type  
description  
confidence_score  
supporting_entities  
supporting_relationships  
predicted_outcome  
suggested_follow_up

Proposal types include:

routine_prediction  
thread_continuation  
opportunity_discovery  
anomaly_alert

---

# Operation Modes

Light Analysis Cycle

Runs frequently during idle periods.

Tasks include:

- routine detection
- thread monitoring
- interest clustering

Deep Exploration Cycle

Runs less frequently during extended idle periods.

Tasks include:

- cross-domain graph traversal
- hypothesis testing
- model refinement

---

# Journal Integration

Corvus implements the standard journal schema defined in:

spec-ocas-journal.md

Standard fields include:

run_id  
thread_id  
skill_id  
skill_version  
branch_type  
timestamp  
model  
inputs  
outputs  
decisions  
evidence  
metrics

---

# Corvus Journal Metrics

pattern_candidates_detected  
validated_patterns  
rejected_patterns  
curiosity_explorations  
hypotheses_tested  
initiative_proposals_generated  
prediction_attempts  
prediction_successes  
signal_nodes_examined  
graph_regions_explored

---

# Corvus OKRs

OKR 1 — Pattern Discovery Quality

Goal  
Identify meaningful behavioral patterns with high confidence and low false positives.

Metrics

validated_patterns / pattern_candidates_detected  
false_pattern_rate

---

OKR 2 — Prediction Accuracy

Goal  
Improve accuracy of predicted future events derived from routines.

Metrics

prediction_success_rate  
prediction_interval_accuracy

---

OKR 3 — Insight Utility

Goal  
Generate insight proposals that lead to useful downstream actions.

Metrics

accepted_proposals_rate  
ignored_proposals_rate  
dismissed_proposals_rate

---

OKR 4 — Curiosity Effectiveness

Goal  
Ensure exploratory curiosity leads to useful discoveries.

Metrics

useful_discoveries / curiosity_explorations  
average_evidence_strength

---

OKR 5 — Graph Understanding Coverage

Goal  
Increase system understanding of entities and relationships.

Metrics

entity_classification_rate  
relationship_density_growth

---

---

## Universal OKRs

This skill must implement the universal OKRs defined in the OCAS Journal Specification (spec-ocas-Journals.md).

Required universal OKRs:

- Reliability: success_rate >= 0.95, retry_rate <= 0.10
- Validation Integrity: validation_failure_rate <= 0.05
- Efficiency: latency trending downward, repair_events <= 0.05
- Context Stability: context_utilization <= 0.70
- Observability: journal_completeness = 1.0

The Corvus-specific OKRs defined in this spec measure domain-relevant pattern discovery and prediction outcomes.

---

## Responsibility Boundary

Corvus analyzes patterns in user activity, environment data, and research signals.

Corvus never modifies system architecture or proposes changes to skills.

System optimization, architectural improvements, and skill redesign belong to Mentor.

Corvus observes behavior patterns and generates signals.

Taste stores persistent user preferences derived from those signals.

Corvus does not store preference data directly.

## Skill Identity

- Skill name: `ocas-corvus`
- Version: `1.1.0`
- Skill type: `system`
- Author: `Indigo Karasu`
- Email: `mx.indigo.karasu@gmail.com`

---

## Optional Skill Cooperation

This skill may cooperate with other skills when present but must never depend on them.
If a cooperating skill is absent, this skill must still function normally.

- Elephas — query Chronicle for graph context during pattern analysis.
- Taste — reference stored preferences when interpreting behavioral signals.
- Sift — request web research to validate or enrich detected patterns.

---

## Journal Outputs

This skill emits the following journal types as defined in the OCAS Journal Specification (spec-ocas-Journals.md):

- Observation Journal

Corvus emits Observation Journal entries recording detected patterns, hypotheses, and validated insights.

---

## Visibility

visibility: public

---

## Required Package Output

Forge must produce a complete Agent Skill package:

```text
ocas-corvus/
  skill.json
  SKILL.md
  references/
    schemas.md
    curiosity_engine.md
    pattern_engines.md
```

### `skill.json` Requirements

Create a valid `skill.json` with:
- `name`: `ocas-corvus`
- `version`: `1.1.0`
- `description`: routing-optimized text
- `author`: `Indigo Karasu`
- `email`: `mx.indigo.karasu@gmail.com`

The description must make clear that this skill is for:
- exploratory pattern analysis across the knowledge graph
- routine, thread, interest, and anomaly detection
- insight proposal generation with evidence
- proactive discovery during idle cycles

### `SKILL.md` Requirements

`SKILL.md` must begin on line 1 with valid YAML frontmatter delimited by `---`.

Target size: 150 to 250 lines.

#### Required `SKILL.md` Sections

The markdown body must contain these sections in this order:

1. `# Corvus`
2. `## When to use`
3. `## When not to use`
4. `## Core promise`
5. `## Commands`
6. `## Operation modes`
7. `## Pattern validation rules`
8. `## Insight proposal format`
9. `## Support file map`
10. `## Storage layout`
11. `## Validation rules`

### Commands

The built skill must implement these commands:

- `corvus.analyze.light` — run a light analysis cycle (routine detection, thread monitoring, interest clustering)
- `corvus.analyze.deep` — run a deep exploration cycle (cross-domain traversal, hypothesis testing, model refinement)
- `corvus.proposals.list` — list current insight proposals with confidence scores
- `corvus.proposals.detail` — show full evidence and reasoning for a specific proposal
- `corvus.hypotheses.list` — list active hypotheses under investigation
- `corvus.status` — return current analysis state including patterns detected, proposals pending, and graph coverage

### Storage Layout

```text
.corvus/
  config.json
  hypotheses.jsonl
  patterns.jsonl
  proposals.jsonl
  decisions.jsonl
  reports/
```

### Config

File: `.corvus/config.json`

Required fields:
- `analysis.light_interval_minutes`: how often light analysis runs during idle periods
- `analysis.deep_interval_hours`: how often deep exploration cycles run
- `curiosity.novelty_weight`: weight for novelty drive in priority scoring
- `curiosity.uncertainty_weight`: weight for uncertainty drive
- `curiosity.prediction_error_weight`: weight for prediction error drive
- `validation.min_signal_count`: minimum signals required to validate a pattern
- `validation.min_confidence`: minimum confidence to generate an insight proposal
- `chronicle.query_enabled`: whether to query Chronicle for graph context (default true)

### `references/schemas.md`

Must define schemas for:
- `Hypothesis` — hypothesis_id, graph_region, hypothesis_text, evidence_refs, confidence, status, created_at, updated_at
- `Pattern` — pattern_id, pattern_type (routine|thread|interest|anomaly|opportunity), description, signal_refs, confidence, validation_status, temporal_data
- `InsightProposal` — proposal_id, proposal_type (routine_prediction|thread_continuation|opportunity_discovery|anomaly_alert), description, confidence_score, supporting_entities, supporting_relationships, predicted_outcome, suggested_follow_up
- `CuriositySignal` — signal_id, drive_type (novelty|uncertainty|prediction_error), target_region, priority_score
- `DecisionRecord` — decision_id, decision_type, evidence_refs, outcome, timestamp

### `references/curiosity_engine.md`

Must explain:
- the three curiosity drives (novelty, uncertainty, prediction error)
- how drives generate hypotheses
- how hypotheses are tested against graph evidence
- priority scoring for graph region selection
- when to escalate hypotheses to pattern validation

### `references/pattern_engines.md`

Must explain for each engine (routine, thread, interest, opportunity, anomaly):
- what signals it evaluates
- detection criteria
- validation requirements (minimum signal count, temporal consistency, cross-domain corroboration, falsification)
- when a pattern becomes an insight proposal
- confidence scoring rules

# Design Philosophy

Corvus behaves as an analytical observer of the system’s environment.

Curiosity drives exploration.  
Evidence validates discoveries.  
Insight proposals capture useful findings.

Corvus continuously improves the system’s understanding of behavioral patterns and relationships within the knowledge graph.

---

## Final Response Format for the Coder

Return:

1. package tree
2. full contents of every file
3. brief validation summary

Do not return planning commentary, process narration, or references to absent documents.
