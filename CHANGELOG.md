## [2026-04-08] Filesystem path normalization

### Fixed
- Corrected filesystem read paths to use `$OCAS_DATA_ROOT/` prefix consistently instead of `$OCAS_WORKSPACE_ROOT/`
- Aligned workspace and memory paths with OCAS storage layout conventions

### Validation
- ✓ Version: 2.5.2 → 2.5.3
- ✓ All filesystem paths now conform to spec-ocas-skill-authoring-rules.md

## [2026-04-05] Ontology types fix

### Fixed
- Added missing `## Ontology types` section to SKILL.md; declares Concept/Idea and Signal extraction per spec-ocas-ontology.md

### Validation
- ✓ Version: 2.5.1 → 2.5.2

## [2026-04-04] Spec Compliance Update

### Changes
- Added missing SKILL.md sections per ocas-skill-authoring-rules.md
- Updated skill.json with required metadata fields
- Ensured all storage layouts and journal paths are properly declared
- Aligned ontology and background task declarations with spec-ocas-ontology.md

### Validation
- ✓ All required SKILL.md sections present
- ✓ All skill.json fields complete
- ✓ Storage layout properly declared
- ✓ Journal output paths configured
- ✓ Version: 2.5.0 → 2.5.1

# Changelog

## [2.6.0] - 2026-04-08

### Multi-Platform Compatibility Migration

- Adopted agentskills.io open standard for skill packaging
- Replaced skill.json with YAML frontmatter in SKILL.md
- Replaced hardcoded ~/openclaw/ paths with $OCAS_DATA_ROOT/ for platform portability
- Abstracted cron/heartbeat registration to declarative metadata pattern
- Added metadata.hermes and metadata.openclaw extension points
- Compatible with both OpenClaw and Hermes Agent


## [2.5.0] - 2026-04-02

### Added
- Memory file reading (`MEMORY.md`, `memory/*.md`) as pattern analysis data source
- Session log reading with machine-noise filtering (human/assistant messages only)
- Filesystem read permissions for Memory/ and session log paths

## [2.3.2] - 2026-03-30

### Added
- `## Ontology types` section per authoring rules v2.4.0
