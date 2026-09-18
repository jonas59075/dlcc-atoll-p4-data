# dlcc-atoll-p4-data

Dediziertes Git-Repository für Versionierung und Archivierung von P4-Gate-Artefakten aus dem dlcc-atoll Projekt.

## Was ist P4-Gate?

P4-Gate ist der Messpunkt für Plane-Issue-Umsetzung durch Multi-Modell-AI-Agenten in der DLCC-Workspace. 
Es erfasst Mitschnitte (Snapshots) vom initialen Issue-State über Branch-Erstellung, Code-Commits, PR-Submission bis zur finalen Plane-State-Transition.

P4-Gate-Daten ermöglichen:
- **Cost-Vergleich:** Kosten/Token pro Agent-Modell (Claude 3.5 Sonnet vs. OpenAI o1 vs. Andere)
- **Latency-Analyse:** Wall-Clock-Zeit vom Issue-Pickup bis zur Merge/Done
- **Quality-Assessment:** Code-Review-Outcomes, Fehlerquoten, Rework-Zyklen
- **Compliance-Audit:** Welcher Agent, wann, was committed — für Betriebshistorie

## Dateiformat

Jedes P4-Gate-Artefakt ist **JSON Lines** (`.jsonl`):
```
{"timestamp": "2026-09-18T14:23:45Z", "event": "issue_picked", "issue_id": "...", ...}
{"timestamp": "2026-09-18T14:24:10Z", "event": "branch_created", "branch": "...", ...}
{"timestamp": "2026-09-18T14:25:30Z", "event": "commit", "sha": "...", ...}
...
```

Ein Artefakt pro Issue. Nicht komprimiert (Git handled Whitespace-Kompression).

## Naming-Schema

```
<PLANE_ID>-<ISSUE_SEQ>-<TITLE_SLUG>-<BENCH_PREFIX>.jsonl
```

Beispiele:
- `DLCCMVP-123-add-websocket-handler-cc.jsonl` (CC-Bench, Sonnet, Workspace ws-dlcc-cc)
- `INFRA-456-fix-bridge-loop-oc.jsonl` (OC-Bench, OpenRouter-Multi, Workspace ws-dlcc-oc)
- `BRIDGE-789-adagio-refactor.jsonl` (Production-Run, keine Bench-Suffix)

**Bench-Präfixe:**
- `cc` → Claude Code Pipeline (Anthropic)
- `oc` → OpenCode Pipeline (OpenRouter / Multi-Modell)
- (keine Suffix) → Production-Läufe

## Struktur im Repo

```
dlcc-atoll-p4-data/
├── README.md (diese Datei)
├── .gitignore
├── 2026-09/ (Ordner nach Monat YYYY-MM)
│   ├── 2026-09-18_cc_batch-001/ (Batch-Runs)
│   │   ├── DLCCMVP-123-*.jsonl
│   │   ├── INFRA-456-*.jsonl
│   │   └── ...
│   └── 2026-09-19_oc_batch-002/
│       └── ...
└── index.json (optional: Manifest aller Dateien + Metadaten)
```

## Revision & Retention

- **Aufbewahrung:** Mind. 12 Monate (Audit-Trail für Betreiber)
- **Git-Tiefe:** Linear, keine Rebases (Commit-SHA ist Beweismittel)
- **Cleanup:** Manuell oder per Script — größere Batches nach 24 Monaten in ein Archiv-Tag und aus dem Repo raus

## Zugriff

Public Repository. Keine Credentials oder PII in Artefakten.

---
**Verwaltung:** Claudius Deployer | **Letzte Änderung:** 2026-09-18
