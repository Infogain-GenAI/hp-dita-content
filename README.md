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
├── content/
│   ├── V2/                         ← HP SW Solutions UG Version 2 (Tridion export)
│   │   └── p_SW-Solutions_UG/
│   │       └── rm_sw-solutions_ug/
│   │           └── c_sw-solutions_ug_Title/
│   │               ├── m_sw-solutions_Drivers/
│   │               ├── m_sw-solutions_HP-click/
│   │               ├── m_sw-solutions_HP-smartstream/
│   │               └── m_sw-solutions_HP-smarttracker/
│   └── V3/                         ← HP SW Solutions UG Version 3 (Tridion export)
│       └── p_SW-Solutions_UG/
│           └── rm_sw-solutions_ug/
│               └── c_sw-solutions_ug_Title/
│                   ├── m_sw-solutions_Drivers/
│                   ├── m_sw-solutions_HP-click/
│                   ├── m_sw-solutions_HP-smartstream/
│                   └── m_sw-solutions_HP-smarttracker/
├── Printers/                       ← HP Printer documentation (manuals, topics, images)
│   └── Manuals/
│       ├── Images/
│       ├── Libraries/
│       ├── Maps/
│       └── Topics/
├── index.json                      ← Auto-generated catalogue (664 topics)
└── README.md
```

---

## Content Statistics

| Version | Topics | Type |
|---------|--------|------|
| V2 | 312 | SW Solutions UG (Tridion Docs export) |
| V3 | 319 | SW Solutions UG (Tridion Docs export) |
| Printers | 33 | Printer manuals & guides |
| **Total** | **664** | |

---

## Workflow: AI Agent → Published Doc

```
User request via Streamlit UI
       ↓
AI Agent searches RAG (ChromaDB) for matching topics
       ↓
GPT-4o edits DITA XML based on user instructions
       ↓
File saved to GitHub: content/{V2|V3}/p_SW-Solutions_UG/...
       ↓
Git commit: "[publish] Updated: {topic_title}"
       ↓
RAG index auto-updated (ChromaDB sync)
       ↓
DITA-OT publishes → PDF
       ↓
PDF uploaded to GitHub
```

---

## File Naming Convention

DITA topics follow the Tridion Docs export naming:
```
{topic_type}_{slug}=GUID-{uuid}={version}={language}=.xml
e.g.  c_sw-solutions_sm_Drivers=GUID-7D77B9C1-5432-4B3A-AA27-DDA9E8D7C921=2=en-US=.xml
```

---

## For HP Integration

Content is sourced from HP Tridion Docs via structured export:
1. Export from Tridion Docs in structured XML format
2. Place V2 content under `content/V2/`
3. Place V3 content under `content/V3/`
4. Run `python _rebuild_index.py` to regenerate the index
5. Agent automatically picks up new content via RAG sync
