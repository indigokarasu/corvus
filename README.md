# 🐦‍⬛ Corvus

Corvus is the system's curiosity engine -- it continuously scans the knowledge graph and skill journals to surface behavioral patterns, emerging interests, stalled threads, and cross-domain opportunities that no single skill would notice on its own. It works by forming hypotheses, testing them against accumulated signals, and emitting validated proposals downstream to Praxis and Vesper for action and briefing.


Skill packages follow the [agentskills.io](https://agentskills.io/specification) open standard and are compatible with OpenClaw, Hermes Agent, and any agentskills.io-compliant client.

---

## Overview

In a system that accumulates signals from browsing, research, communications, and portfolio events, Corvus provides the connective intelligence layer. It runs periodic exploration cycles -- light passes for routine detection, deep passes for cross-domain graph traversal -- testing hypotheses against Chronicle and skill journals. Validated patterns become structured proposals that flow to Praxis for behavioral refinement and Vesper for daily briefing, making the whole system smarter without explicit instruction. Every proposal requires multi-signal corroboration and a falsification attempt before it leaves Corvus.

## Commands

| Command | Description |
|---|---|
| `corvus.analyze.light` | Light analysis cycle: routine detection, thread monitoring, interest clustering |
| `corvus.analyze.deep` | Deep exploration cycle: cross-domain traversal, hypothesis testing, model refinement |
| `corvus.proposals.list` | List current insight proposals with confidence scores |
| `corvus.proposals.detail` | Show full evidence and reasoning for a specific proposal |
| `corvus.hypotheses.list` | List active hypotheses under investigation |
| `corvus.status` | Current analysis state: patterns detected, proposals pending, graph coverage |
| `corvus.journal` | Write journal for the current run |
| `corvus.update` | Pull latest from GitHub source (preserves journals and data) |

## Setup

`corvus.init` runs automatically on first invocation and creates all required directories, config.json, and JSONL files. It also registers the `corvus:deep` cron job (daily 3am) and the `corvus:light` heartbeat entry and `corvus:update` (midnight daily, self-update). No manual setup is required.

## Dependencies

**OCAS Skills**
- [Elephas](https://github.com/indigokarasu/elephas) -- Chronicle read-only, graph context during pattern analysis
- [Praxis](https://github.com/indigokarasu/praxis) -- receives BehavioralSignal files via journal payload
- [Vesper](https://github.com/indigokarasu/vesper) -- receives InsightProposal files via journal payload
- [Thread](https://github.com/indigokarasu/thread) -- sends research thread signals via Corvus journal payload

**External**
- None

## Scheduled Tasks

| Job | Mechanism | Schedule | Command |
|---|---|---|---|
| `corvus:deep` | cron | `0 3 * * *` (daily 3am) | Full exploration cycle |
| `corvus:light` | heartbeat | Every heartbeat pass | Routine detection and thread monitoring |
| `corvus:update` | cron | `0 0 * * *` (midnight daily) | Self-update from GitHub source |

## Changelog

### v2.5.0 -- April 2, 2026
- Added Memory file reading (`MEMORY.md`, `memory/*.md`) as pattern analysis data source
- Added session log reading with machine-noise filtering (human/assistant messages only)
- Added filesystem read permissions for Memory/ and session log paths

### v2.3.0 -- March 27, 2026
- Added `corvus.update` command and midnight cron for automatic version-checked self-updates

### v2.2.0 -- March 22, 2026
- Routing improvements and short-name trigger aliases

### v2.1.0 -- March 22, 2026
- Analysis cycle completion with full Praxis and Vesper signal emission
- Initialization with cron and heartbeat registration
- Background task definitions

### v2.0.0 -- March 18, 2026
- Initial release as part of the unified OCAS skill suite
---

*Corvus is part of the [OCAS Agent Suite](https://github.com/indigokarasu) -- a collection of interconnected skills for personal intelligence, autonomous research, and continuous self-improvement. Each skill owns a narrow responsibility and communicates with others through structured signal files, shared journals, and Chronicle, a long-term knowledge graph that accumulates verified facts over time.*
