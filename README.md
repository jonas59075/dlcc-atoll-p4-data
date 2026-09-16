# dlcc-atoll-p4-data

Dedicated repository for P4 (Performance Recording) artifacts from DLCC ATOLL simulation benchmarks.

## Purpose

Stores playback recordings, frame-capture dumps, and profiling snapshots collected during multi-agent benchmark runs. Decoupled from production code to avoid cluttering the main repository with binary/large JSON datasets.

## Naming Schema

### Files
- **Timestamp-based**: `p4-<YYYY-MM-DD>T<HH-MM-SS>Z-<bench-id>-<descriptor>.jsonl`
- **Examples**:
  - `p4-2026-09-16T14-32-15Z-cc-run-001-frame-trace.jsonl`
  - `p4-2026-09-16T14-32-15Z-oc-run-001-perf-counters.jsonl`

### Directories
```
p4-data/
  └── <bench-id>/
      ├── runs/
      │   └── <run-id>/
      │       ├── traces/
      │       ├── counters/
      │       └── metadata.json
```

## Retention Policy

- **Keep**: Last 30 days of artifacts (rolling window)
- **Archive**: Older artifacts tagged and moved to cold storage (external, not in this repo)
- **Cleanup**: Automated via CI/CD or manual cleanup script every 30 days

## Format

All artifacts are JSON Lines (`.jsonl`), optionally gzipped (`.jsonl.gz`). No Git LFS — compression handles size.

## Access

- CI/CD: Pulls artifacts for post-run analysis
- Agentboard (AB): May render selected traces in dashboard
- Bridge: Stores benchmark metadata and result summaries

---

Generated: 2026-09-16
