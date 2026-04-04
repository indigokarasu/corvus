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

## [2.5.0] - 2026-04-02

### Added
- Memory file reading (`MEMORY.md`, `memory/*.md`) as pattern analysis data source
- Session log reading with machine-noise filtering (human/assistant messages only)
- Filesystem read permissions for Memory/ and session log paths

## [2.3.2] - 2026-03-30

### Added
- `## Ontology types` section per authoring rules v2.4.0
