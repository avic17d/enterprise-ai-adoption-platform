# Enterprise AI Assessment & Recommendation Agent — P0 Pilot

> An agentic AI pipeline that assesses enterprise AI readiness across three dimensions, synthesizes a scored executive brief, and outputs a formatted PDF report — fully automated via n8n.

---

## Overview

This platform automates the AI readiness assessment process for enterprise clients. Given a structured Excel intake file, it runs three parallel specialist AI agents (Industry & Scale, Tech Stack, Governance), synthesizes results via a fourth synthesizer agent, and delivers a formatted executive PDF report.

**Designed for:** Strategy consultants, AI advisory firms, CIO/AI head leaders  
**Stack:** n8n (orchestration) · Anthropic Claude (LLM) · Microsoft OneDrive (data) · PDFBolt (output)  
**Status:** P0 Prototype — fully functional end-to-end pipeline
**Future Scope** Future versions will expand to real-time data ingestion, multi-framework benchmarking, and autonomous roadmap generation — enabling continuous, scalable AI readiness intelligence across the enterprise.

---

## Architecture

```
Schedule Trigger
       │
       ├──→ Industry & Scale (Excel/OneDrive)   ─┐
       ├──→ Tech Stack & Data Readiness (Excel)  ─┼──→ Merge1 (sync) → Code Node (JS transform)
       └──→ Governance Posture (Excel)           ─┘
                                                           │
                   ┌───────────────────────────────────────┤
                   │                                       │                           │
            Agent 1 (Haiku)                        Agent 2 (Haiku)             Agent 3 (Haiku)
            Industry & Scale                       Tech Stack                  Governance
                   │                                       │                           │
                   └───────────────────────────────────────┴───────────────────────────┘
                                                           │
                                                    Merge (Append)
                                                           │
                                                    Limit (1 item)
                                                           │
                                               Agent 4 — Synthesizer (Sonnet)
                                                           │
                                                Message a Model (HTML formatter)
                                                           │
                                                        PDFBolt
                                                           │
                                                     PDF Report ✅
```

### Agent Roles

| Agent | Model | Input | Output |
|-------|-------|-------|--------|
| Agent 1 — Industry & Scale | claude-haiku-4-5 | Org profile, employees, revenue, AI budget | Org readiness scores + narrative |
| Agent 2 — Tech Stack | claude-haiku-4-5 | Data infrastructure, integration, AI platform | Tech readiness scores + narrative |
| Agent 3 — Governance Posture | claude-haiku-4-5 | Regulatory frameworks, AI policy, data privacy | Governance scores + narrative |
| Agent 4 — Synthesizer | claude-sonnet-4-6 | All 3 agent outputs | Executive AI readiness score + brief |

---

## Repository Structure

```
enterprise-ai-adoption-platform/
├── README.md                           ← This file
├── workflow/
│   └── n8n_workflow.json               ← Export from n8n (see below)
└── data/
    └── Master_Input_Data_Template.xlsx ← Excel intake template (3 sheets)
```

---

## Excel Intake Format

The intake file contains **3 sheets**, each formatted as an Excel Table with two columns:

| Column | Description |
|--------|-------------|
| `Category` | Field name (e.g., "Company", "Industry", "Employees") |
| `Current State` | The client's current value for that field |

> **To add a new data point:** Add a new row in the relevant sheet. The `kvToObject()` JS function reads all rows dynamically — no code changes needed for domain-specific fields.

---

## n8n Workflow Setup

### Prerequisites
- n8n Cloud account
- Anthropic API key (or n8n Connect credits)
- Microsoft 365 / OneDrive access
- PDFBolt account (free tier: 100 generations/month)

### Steps

1. Clone this repo and place the Excel template in OneDrive
2. In n8n: import `workflow/n8n_workflow.json` via top-right menu → Import
3. Configure credentials: Anthropic, Microsoft Excel 365, PDFBolt
4. In each Excel (Get rows) node: select your Workbook, Sheet, and Table
5. Run via **Execute workflow** button or set Schedule Trigger

### Exporting workflow for version control
In n8n canvas → top-right (⋯) → Download → save as `workflow/n8n_workflow.json`

---

## Key Design Decisions

**Two Merge nodes (fork-join pattern)**
- Merge1 before Code node: waits for all 3 Excel reads to complete
- Merge2 before Agent 4: aggregates all 3 agent outputs for synthesis

**Limit node before Agent 4**
Prevents Agent 4 from running 3x. Agent 4 still accesses all 3 outputs via `$('Merge').all()[0/1/2]`.

**JS Code node replaces LLM Agent 0**
Pure JavaScript `kvToObject()` transform is faster, cheaper, and more reliable than an LLM-based intake parser.

---

## Roadmap (Post-P0)

- [ ] Auto-save PDF to OneDrive after generation
- [ ] Multi-client batch processing
- [ ] Web intake form (Typeform → n8n webhook)
- [ ] Scoring calibration across industries
- [ ] Client self-service portal
- [ ] Re-assessment delta tracking

---

## Author

**Avinendra Chauhan** — AI Product Builder  
Enterprise AI Adoption Intelligence Platform — Built with n8n + Anthropic Claude
