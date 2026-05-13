# HP DITA Content Repository

> **This repository is the single source of truth for all HP technical documentation.**
> It replaces the role of Tridion Docs as the content store, using a Git-based
> "DITA-as-Code" approach — the modern industry standard (used by Microsoft, Red Hat, Google).

---

## Why Git instead of Tridion Docs?

| Feature | Tridion Docs | This Repo (Git) |
|---|---|---|
| Version history | ✅ | ✅ Full commit log |
| Who changed what & why | ✅ | ✅ Commit messages w/ ticket IDs |
| Branching per ticket | ❌ | ✅ `feature/HP-101` branches |
| Review before publish | Via workflow | ✅ Pull Request review |
| Diff/compare versions | Limited | ✅ Native XML diff |
| Cost | Licensed ($$$) | Free |
| CI/CD integration | Complex | ✅ GitHub Actions / Azure Pipelines |
| RAG auto-sync | ❌ | ✅ Post-commit hook triggers re-ingestion |
| No admin rights needed | ❌ | ✅ |

---

## Repository Structure

```
dita-content-repo/
├── content/                        ← ALL DITA source files live here
│   ├── laserjet/                   ← Product line
│   │   ├── v1.0/                   ← Product version
│   │   │   ├── concepts/           ← Topic type
│   │   │   ├── tasks/
│   │   │   ├── references/
│   │   │   └── maps/               ← .ditamap files
│   │   └── v2.0/
│   ├── officejet/
│   ├── designjet/
│   └── shared/                     ← Reusable topics across products
│       ├── concepts/
│       ├── tasks/
│       └── references/
├── published/                      ← Built output (committed for history)
│   ├── pdf/
│   └── html/
├── index.json                      ← Auto-generated catalogue of all topics
└── README.md
```

---

## Workflow: JIRA Ticket → Published Doc

```
JIRA Ticket Created
       ↓
AI Agent fetches ticket + searches RAG for context
       ↓
GPT-4o generates DITA XML
       ↓
File saved to  content/{product}/{version}/{type}/{ticket_id}_{slug}.dita
       ↓
Git commit:  "[HP-101] Add task: Install printer driver"
       ↓
RAG index auto-updated (sync_to_rag.py)
       ↓
DITA-OT publishes → PDF / HTML5
       ↓
Published output committed to published/
       ↓
JIRA ticket updated → In Review
```

---

## Naming Convention

```
{ticket_id}_{slug}_{date}.dita
e.g.  HP-101_install-printer-driver_20260513.dita
```

## Commit Message Convention

```
[{ticket_id}] {action} {topic_type}: {short_description}
e.g. [HP-101] Add task: Install HP LaserJet printer driver on Windows 11
     [HP-102] Update concept: HP LaserJet Pro M404 Overview
     [HP-103] Fix reference: Configuration parameters table
```

---

## For HP Integration (Phase 2)

When HP provides access to their real DITA content:
1. Copy all `.dita` / `.ditamap` files into `content/`
2. `git add . && git commit -m "Import HP production content"`
3. Delete `../hp-dita-agent/chroma_db/`
4. Run `python ../hp-dita-agent/rag/sync_to_rag.py`
5. Zero code changes required — agent automatically picks up new content
