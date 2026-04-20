---
name: ocas-corvus
description: >
  Corvus: exploratory pattern analysis engine for the system knowledge graph,
  skill journals, Memory files, and session logs. Detects routines, emerging
  interests, anomalies, stalled threads, and cross-domain opportunities from
  accumulated activity signals. Reads Memory and session logs (human/assistant
  messages only) as additional pattern sources. Trigger phrases: 'analyze
  patterns', 'detect routines', 'find anomalies', 'what patterns do you see',
  'exploration cycle', 'run analysis', 'update corvus'. Do not use for web
  research (use Sift), person investigations (use Scout), or system
  architecture changes (use Mentor).
metadata:
  author: Indigo Karasu
  email: mx.indigo.karasu@gmail.com
  version: "2.6.3"
  hermes:
    tags: [analysis, patterns, knowledge-graph]
    category: signal
    cron:
      - name: "corvus:update"
        schedule: "0 0 * * *"
        command: "corvus.update"
  openclaw:
    skill_type: system
    visibility: public
    filesystem:
      read:
        - "{agent_root}/commons/data/ocas-corvus/"
        - "{agent_root}/commons/journals/ocas-corvus/"
        - "{agent_root}/commons/db/ocas-elephas/chronicle.lbug"
        - "{agent_root}/commons/journals/*/"
        - "{agent_root}/commons/workspace/MEMORY.md"
        - "{agent_root}/commons/workspace/memory/"
        - "{agent_root}/commons/agents/*/sessions/"
      write:
        - "{agent_root}/commons/data/ocas-corvus/"
        - "{agent_root}/commons/journals/ocas-corvus/"
    self_update:
      source: "https://github.com/indigokarasu/corvus"
      mechanism: "version-checked tarball from GitHub via gh CLI"
      command: "corvus.update"
      requires_binaries: [gh, tar, python3]
    cron:
      - name: "corvus:update"
        schedule: "0 0 * * *"
        command: "corvus.update"
---

# Corvus

Corvus is the system's curiosity engine — it continuously scans the knowledge graph, skill journals, Memory files, and session logs to surface behavioral patterns, emerging interests, stalled threads, and cross-domain opportunities that no single skill would notice on its own. It works by forming hypotheses, testing them against accumulated signals, and emitting validated proposals downstream to Praxis and Vesper for action and briefing.

Memory files (`{agent_root}/MEMORY.md` and `{agent_root}/memory/*.md`) provide persistent user context, preferences, and accumulated knowledge. Session logs (`{agent_root}/agents/*/sessions/*.jsonl`) provide raw conversational history — Corvus reads only `human` and `assistant` role message entries, skipping all machine noise (toolResult, compaction, custom entries, etc.). Both sources are read-only.

## When to use

- Detect recurring behavioral patterns and routines
- Identify emerging interests from activity clusters
- Discover anomalies or meaningful deviations from established patterns
- Find cross-domain opportunities connecting previously unrelated entities
- Monitor stalled threads and incomplete activity clusters
- Run periodic analysis during idle cycles

## When not to use

- Web research or fact-checking — use Sift
- OSINT investigations on people — use Scout
- System architecture changes or skill evaluation — use Mentor
- Storing user preferences — use Taste
- Direct communication — use Dispatch

## Responsibility boundary

Corvus owns exploratory pattern analysis across the knowledge graph, skill journals, Memory files, and session logs.

Corvus does not own: skill evaluation (Mentor), behavioral refinement (Praxis), web research (Sift), knowledge graph writes (Elephas), preference persistence (Taste), browsing interpretation (Thread), Memory writes (the agent platform), session log writes (the agent platform).

Corvus emits BehavioralSignal files to Praxis and InsightProposal files to Vesper. Corvus receives research thread signals from Thread.

## Ontology types

Corvus works with these types from `spec-ocas-ontology.md`:

- **Concept/Idea** — patterns, topics, emerging interests, and anomalies detected from accumulated signals.
- **Signal** — behavioral signals emitted to Praxis for refinement.

Corvus does not emit entity Signals to Elephas/Chronicle. Behavioral signals are written as BehavioralSignal files to `{agent_root}/commons/data/ocas-corvus/signals/` for Praxis to consume directly.

## Commands

- `corvus.analyze.light` — run a light analysis cycle: routine detection, thread monitoring, interest clustering
- `corvus.analyze.deep` — run a deep exploration cycle: cross-domain traversal, hypothesis testing, model refinement
- `corvus.proposals.list` — list current insight proposals with confidence scores
- `corvus.proposals.detail` — show full evidence and reasoning for a specific proposal
- `corvus.hypotheses.list` — list active hypotheses under investigation
- `corvus.status` — return current analysis state: patterns detected, proposals pending, graph coverage
- `corvus.journal` — write journal for the current run; called at end of every run
- `corvus.update` — pull latest from GitHub source; preserves journals and data

## Operation modes

### Light Analysis Cycle
Runs frequently during idle periods. Focuses on routine detection, thread monitoring, and interest clustering. Low cost, fast execution.

### Deep Exploration Cycle
Runs less frequently during extended idle periods. Performs cross-domain graph traversal, hypothesis testing, and model refinement. Higher cost, produces richer insight proposals.

## Curiosity engine

Corvus prioritizes graph regions for exploration using three internal drives:

- **Novelty** — prefer regions that recently appeared or changed
- **Uncertainty** — prefer entities with many signals but incomplete understanding
- **Prediction error** — prefer patterns where predicted outcomes diverge from observed events

Each drive generates hypotheses. Hypotheses are tested through graph queries and evidence gathering. Validated hypotheses become insight proposals.

Read `references/curiosity_engine.md` for drive mechanics and priority scoring.

## Pattern validation rules

Patterns must pass all validation checks before becoming proposals:

- Minimum signal count met
- Temporal consistency confirmed
- Cross-domain corroboration present
- Falsification attempt completed without contradiction

Patterns failing validation remain internal hypotheses for future evaluation.

Read `references/pattern_engines.md` for per-engine detection criteria and validation rules.

## Insight proposal format

Each proposal includes: proposal_id, proposal_type, description, confidence_score, supporting_entities, supporting_relationships, predicted_outcome, suggested_follow_up.

Proposal types: routine_prediction, thread_continuation, opportunity_discovery, anomaly_alert, behavioral_signal.

Read `references/schemas.md` for exact proposal schema.

## Analysis cycle completion

After every analysis cycle (light or deep):

1. Persist hypotheses, patterns, and proposals to local JSONL files
2. For each validated pattern with `proposal_type: behavioral_signal`: write a BehavioralSignal file to `{agent_root}/commons/data/ocas-corvus/signals/{signal_id}.json`. Praxis reads from this directory.
3. For each validated proposal reaching sufficient confidence (all types except `behavioral_signal`): write an InsightProposal file to `{agent_root}/commons/data/ocas-corvus/proposals/{proposal_id}.json`. Vesper reads from this directory.
4. Scan Thread journals for entries with `research_thread` payload; track processed run_ids in ingestion log
5. Write journal via `corvus.journal`

## Inter-skill interfaces

**Corvus → Praxis (cooperative read):** Corvus writes BehavioralSignal files to `{agent_root}/commons/data/ocas-corvus/signals/{signal_id}.json`. Praxis reads from this directory. Corvus does not write to Praxis's directories.

**Corvus → Vesper (cooperative read):** Corvus writes InsightProposal files to `{agent_root}/commons/data/ocas-corvus/proposals/{proposal_id}.json`. Vesper reads from this directory during briefing generation. Corvus does not write to Vesper's directories.

**Thread → Corvus (journal payload):** Thread includes `research_thread` payload in journal entries. Corvus scans Thread journals during analysis cycles and tracks processed run_ids in its ingestion log.

See `spec-ocas-interfaces.md` for schemas and handoff contracts.

## Read-only data sources

```
{agent_root}/MEMORY.md              — primary Memory file (read-only)
{agent_root}/memory/*.md            — supplemental Memory files (read-only)
{agent_root}/sessions/*.jsonl       — session logs (read-only; parse human/assistant messages only)
{agent_root}/commons/db/ocas-elephas/chronicle.lbug    — Chronicle knowledge graph (read-only, NOT SQLite — see pitfall)
{agent_root}/commons/journals/*/                       — skill journals (read-only)
```

When reading session logs, Corvus filters each JSONL entry by role. Only entries with `"role": "human"` or `"role": "assistant"` are processed. All other entry types — `toolResult`, `toolUse`, `compaction`, `custom`, and any other machine-generated entries — are skipped.

### Data source pitfalls

**Chronicle (`chronicle.lbug`)** is NOT a SQLite database despite its file extension. Attempting `sqlite3.connect()` will raise `DatabaseError: file is not a database`. Use the MemPalace MCP tools (`mcp_mempalace_mempalace_kg_query`, `mcp_mempalace_mempalace_kg_stats`, `mcp_mempalace_mempalace_kg_timeline`) to query the knowledge graph. Alternatively, `mcp_mempalace_mempalace_search` provides semantic search over stored drawers.

**Session logs** live at `{agent_root}/sessions/*.jsonl` (flat directory), not under `agents/*/sessions/`. The `agents/` path from earlier skill versions is deprecated.

**When using `execute_code` with heredocs and `terminal()`**, avoid nested f-strings with shell variable interpolation — use separate tool calls or write Python scripts to disk instead. The execute_code sandbox's string escaping breaks with complex heredoc nesting.

## Storage layout

```
{agent_root}/commons/data/ocas-corvus/
  config.json
  hypotheses.jsonl
  patterns.jsonl
  proposals.jsonl
  decisions.jsonl
    {thread_id}.json
    processed/
  proposals/
    {proposal_id}.json
  signals/
    {signal_id}.json
  reports/

{agent_root}/commons/journals/ocas-corvus/
  YYYY-MM-DD/
    {run_id}.json
```


Default config.json:
```json
{
  "skill_id": "ocas-corvus",
  "skill_version": "2.4.0",
  "config_version": "1",
  "created_at": "",
  "updated_at": "",
  "curiosity": {
    "novelty_weight": 0.4,
    "uncertainty_weight": 0.3,
    "prediction_error_weight": 0.3
  },
  "validation": {
    "min_signal_count": 3,
    "min_confidence_for_proposal": 0.5
  },
  "retention": {
    "days": 0,
    "max_records": 10000
  }
}
```

## OKRs

Universal OKRs from spec-ocas-journal.md apply to all runs.

```yaml
skill_okrs:
  - name: proposal_precision
    metric: fraction of proposals confirmed as useful within 30 days
    direction: maximize
    target: 0.70
    evaluation_window: 30_runs
  - name: pattern_validation_rate
    metric: fraction of detected patterns passing all validation checks
    direction: maximize
    target: 0.80
    evaluation_window: 30_runs
  - name: graph_coverage
    metric: fraction of active graph regions analyzed within one deep cycle
    direction: maximize
    target: 0.90
    evaluation_window: 30_runs
  - name: false_anomaly_rate
    metric: fraction of anomaly alerts dismissed as noise
    direction: minimize
    target: 0.15
    evaluation_window: 30_runs
```

## Optional skill cooperation

- Elephas — read Chronicle (read-only) for graph context during pattern analysis
- Agent Memory — read `{agent_root}/MEMORY.md` and `{agent_root}/memory/*.md` (read-only) for persistent user context, preferences, and accumulated knowledge
- Agent Sessions — read `{agent_root}/agents/*/sessions/*.jsonl` (read-only) for conversational history; only human/assistant message entries are consumed
- Thread — receives research thread signals via journal payload
- Vesper — reads InsightProposal files from Corvus's `proposals/` directory (cooperative read; Corvus owns)
- Praxis — reads BehavioralSignal files from Corvus's `signals/` directory (cooperative read; Corvus owns)
- Mentor — Mentor may read Corvus data for evaluation context

## Journal outputs

Observation Journal — all analysis cycles (light and deep).

## Initialization

On first invocation of any Corvus command, run `corvus.init`:

1. Create `{agent_root}/commons/data/ocas-corvus/` and subdirectories (`proposals/`, `signals/`, `reports/`)
2. Write default `config.json` with ConfigBase fields if absent
3. Create empty JSONL files: `hypotheses.jsonl`, `patterns.jsonl`, `proposals.jsonl`, `decisions.jsonl`
4. Create `{agent_root}/commons/journals/ocas-corvus/`
5. Register cron job `corvus:deep` if not already present (check the platform scheduling registry first)
6. Register heartbeat entry `corvus:light` in `HEARTBEAT.md` if not already present
7. Register cron job `corvus:update` if not already present (check the platform scheduling registry first)
8. Log initialization as a DecisionRecord in `decisions.jsonl`

## Background tasks

| Job name | Mechanism | Schedule | Command |
|---|---|---|---|
| `corvus:deep` | cron | `0 3 * * *` (daily 3am) | `corvus.analyze.deep` — full exploration cycle |
| `corvus:light` | heartbeat | every heartbeat pass | `corvus.analyze.light` — routine detection, thread monitoring |
| `corvus:update` | cron | `0 0 * * *` (midnight daily) | `corvus.update` |

Cron options for `corvus:deep`: `sessionTarget: isolated`, `lightContext: true`, `wakeMode: next-heartbeat`.

Registration during `corvus.init`:
```
# Check platform scheduling registry for existing tasks
# Task declared in SKILL.md frontmatter metadata.{platform}.cron
# If corvus:update absent:
# Task declared in SKILL.md frontmatter metadata.{platform}.cron
```

Heartbeat registration: append `corvus:light` entry to `{agent_root}/HEARTBEAT.md` if not already present.


## Self-update

`corvus.update` pulls the latest package from the `source:` URL in this file's frontmatter. Runs silently — no output unless the version changed or an error occurred.

1. Read `source:` from frontmatter → extract `{owner}/{repo}` from URL
2. Read local version from SKILL.md frontmatter `metadata.version`
3. Fetch remote version from SKILL.md frontmatter: `gh api "repos/{owner}/{repo}/contents/SKILL.md" --jq '.content' | base64 -d | grep 'version:' | head -1 | sed 's/.*"\(.*\)".*/\1/'`
4. If remote version equals local version → stop silently
5. Download and install:
   ```bash
   TMPDIR=$(mktemp -d)
   gh api "repos/{owner}/{repo}/tarball/main" > "$TMPDIR/archive.tar.gz"
   mkdir "$TMPDIR/extracted"
   tar xzf "$TMPDIR/archive.tar.gz" -C "$TMPDIR/extracted" --strip-components=1
   cp -R "$TMPDIR/extracted/"* ./
   rm -rf "$TMPDIR"
   ```
6. On failure → retry once. If second attempt fails, report the error and stop.
7. Output exactly: `I updated Corvus from version {old} to {new}`


## Visibility

public

## Support file map

| File | When to read |
|---|---|
| `references/schemas.md` | Before creating hypotheses, patterns, or proposals |
| `references/curiosity_engine.md` | Before drive scoring or hypothesis generation |
| `references/pattern_engines.md` | Before pattern detection or validation |
| `references/journal.md` | Before corvus.journal; at end of every run |

## Update command

This skill self-updates every 24 hours via:

```bash
corvus.update
```

This pulls the latest version from GitHub and restarts the skill's background tasks if applicable.
