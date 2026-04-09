# Feature Coverage Report
**McDonald's Social Media Command Centre**

IS492 — GenAI for Human-AI Collaboration | Spring 2026 | UIUC

Authors: Pranav C, Jeet T, Ashish G

Submitted: April 2026

---

## 1. Project Overview

The McDonald's Social Media Command Centre is an AI-powered Streamlit web application designed for area managers and regional operations leads. It ingests 32,736 customer reviews from a public Kaggle dataset, surfaces branch-level performance issues, and uses Claude Haiku via the Anthropic API to generate plain-English summaries, prioritized action items, and staff briefing memos. The tool is live at [mcd-command-centre.streamlit.app](https://mcd-command-centre.streamlit.app).

**Core problem addressed:**
- 32,736 customer reviews go largely unread by branch management
- Managers rely on aggregate star ratings and miss granular, actionable issues
- No existing tool generates staff-ready communications from review data

---

## 2. Proposed Features

At the outset of the project, five major features were scoped and committed to. All five are implemented in the live demo.

| Feature | Status | Implementation Notes |
|---|---|---|
| F1 — Review Dashboard | ✅ Implemented | Rating distribution, top 10 cities by volume, best/worst branch charts, city filter with live updates |
| F2 — Branch Health Table | ✅ Implemented | Worst-performing locations ranked first; managers select branch + time range to drill down |
| F3 — AI Insight Generation | ✅ Implemented | Claude Haiku reads top 20 reviews per branch call; returns structured SUMMARY / ACTION ITEMS / STAFF BRIEFING output |
| F4 — Evidence Grounding | ✅ Implemented | System prompt requires all action items to cite actual review quotes; hallucination explicitly forbidden |
| F5 — Memo Download | ✅ Implemented | One-click .txt file export of the staff briefing memo, ready to send to branch staff |

**Feature coverage: 5 of 5 proposed features implemented (100%).**

---

## 3. Feature Implementation Detail

### F1 — Review Dashboard

Tab 1 of the Streamlit app. Powered by `data_loader.py` which handles the Kaggle ingestion pipeline, non-ASCII character stripping, and schema normalization at load time. Charts are rendered using Streamlit native components and update live when the city filter changes. Displays rating distribution across all branches, top 10 cities by review volume, and a best-vs-worst performing branches comparison chart.

| | |
|---|---|
| **File** | `data_loader.py`, `app.py` (Tab 1) |
| **Data source** | Kaggle public McDonald's review dataset (32,736 reviews) |
| **Interactivity** | City dropdown; all charts update on selection |
| **Key outputs** | Rating histogram, city volume bar chart, branch ranking chart |

### F2 — Branch Health Table

Tab 2 opens with a ranked table of all branches sorted worst-first by average star rating. This design choice followed professor feedback that city-level aggregation was too broad for operational use. Managers can identify the most urgent location at a glance without reading through charts. Selecting a branch and time range activates the AI generation call.

| | |
|---|---|
| **File** | `app.py` (Tab 2) |
| **Ranking logic** | Ascending sort by average star rating across all reviews |
| **Filter** | Branch (street address level) + date range selector |
| **Design rationale** | Switched from city-level to branch-level based on professor feedback; managers care about specific addresses |

### F3 — AI Insight Generation

The core AI feature. On clicking Generate Insights, the app calls `llm.py` which constructs a prompt from the top 20 reviews for the selected branch and time window, sends it to Claude Haiku via the Anthropic API, and returns a structured three-section response. Claude Haiku was chosen over a local Llama3 model (4.7GB, timeout issues on GTX 1650) for speed, cost (~$0.001/call), and output reliability.

| | |
|---|---|
| **File** | `llm.py` |
| **Model** | Claude Haiku (Anthropic API) |
| **Input** | Top 20 reviews for selected branch + time range |
| **Output sections** | SUMMARY of main issues / 3 ACTION ITEMS with evidence / STAFF BRIEFING memo |
| **Rate limiting** | 20 reviews per call cap; keeps response time under 5 seconds |

### F4 — Evidence Grounding and Hallucination Prevention

The system prompt passed to Claude explicitly forbids the model from referencing issues not present in the provided review text. Every action item must cite a direct quote from the reviews supplied in that call. This was a deliberate prompt engineering decision to ensure outputs are defensible and audit-ready for operational use. Additionally, only review text and star rating are sent to the API — no reviewer names, IDs, coordinates, or addresses — preserving PII hygiene on public data.

| | |
|---|---|
| **File** | `llm.py` (system prompt) |
| **Technique** | Strict system prompt grounding + evidence citation requirement |
| **PII policy** | Only review text + star rating sent to Claude API |
| **Non-ASCII handling** | Stripped at load time in `data_loader.py` |

### F5 — Staff Briefing Memo Download

The STAFF BRIEFING section of the AI output is exposed as a downloadable `.txt` file via Streamlit's `st.download_button` component. The memo is formatted for immediate distribution to branch staff without editing. This closes the loop between insight generation and operational action, which was a gap in all prior tools identified in the problem statement.

| | |
|---|---|
| **File** | `app.py` |
| **Format** | `.txt` file download |
| **Content** | AI-generated staff briefing section from Claude output |
| **UX** | One-click download, no copy-paste required |

---

## 4. Observability and Telemetry

Every API call is logged to a local SQLite database via `llm.py`. Each record captures timestamp, branch identifier, date range selected, number of reviews sent, and the full model response. This telemetry layer was built specifically to support CP4 evaluation — it enables usage analysis, cost monitoring, and debugging without any third-party logging service.

| | |
|---|---|
| **Storage** | SQLite (local, gitignored) |
| **Fields logged** | `timestamp`, `branch`, `days_range`, `num_reviews`, `response` |
| **Purpose** | CP4 evaluation support, cost monitoring, debugging |
| **Secrets** | API key in `.env` (gitignored); `.env.example` with placeholder only; no secrets in GitHub |

---

## 5. Feature Coverage Summary

All five features proposed at the start of the project are fully implemented in the live Streamlit deployment.

| ID | Feature | File(s) | Status |
|---|---|---|---|
| F1 | Review Dashboard | `data_loader.py`, `app.py` | ✅ Implemented |
| F2 | Branch Health Table | `app.py` | ✅ Implemented |
| F3 | AI Insight Generation | `llm.py`, `app.py` | ✅ Implemented |
| F4 | Evidence Grounding | `llm.py` (system prompt) | ✅ Implemented |
| F5 | Memo Download | `app.py` | ✅ Implemented |

**Live App:** https://mcd-command-centre.streamlit.app

**GitHub:** https://github.com/PranavCR01/mcd-copilot
