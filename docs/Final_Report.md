# McDonald's Social Media Command Centre — Complete Project Context

> This document is a single-source summary of the entire project, intended to be passed to an AI model for generating a presentation. It covers the problem, solution, technical implementation, features, design decisions, safety, and user study results end-to-end.

---

## 1. Project Identity

| Field | Detail |
|-------|--------|
| **Project Name** | McDonald's Social Media Command Centre (SMCC) |
| **Course** | IS492 — Generative AI for Human-AI Collaboration |
| **Institution** | University of Illinois Urbana-Champaign (UIUC) |
| **Semester** | Spring 2026 |
| **Live App** | https://mcd-command-centre.streamlit.app |
| **GitHub** | https://github.com/IS492-SP26/team-project-social-media-command-centre |

### Team

| Name | Role |
|------|------|
| Pranav C | Backend & AI integration |
| Jeet T | Frontend & UI/UX |
| Ashish G | Data Engineering & Visualizations |

---

## 2. Problem Statement

### Context
McDonald's branch managers receive hundreds of customer reviews on Google Maps and social platforms every week. The reviews contain rich operational signals — service speed, cleanliness, order accuracy, staff behavior — but the sheer volume makes systematic human reading infeasible.

### The Gap
Existing analytics tools reduce all feedback to a single aggregate star rating. Research shows online reviews are J-shaped (extreme ratings over-represented), meaning simple averages mislead decision-makers. No existing tool:
- Extracts granular, branch-level issues from review text
- Generates ready-to-use staff communications from that analysis
- Grounds AI recommendations in actual review evidence to prevent hallucination

### Core Problem (in one sentence)
**32,736 customer reviews go largely unread by branch management, and no tool transforms that feedback into direct operational action.**

---

## 3. Solution Overview

The McDonald's Social Media Command Centre is a two-tab Streamlit web application that:

1. **Visualizes** branch performance across all locations using interactive charts (Review Dashboard)
2. **Generates** AI-powered, evidence-grounded management briefs for any branch on demand (AI Manager Co-Pilot)

The AI Co-Pilot uses **Anthropic Claude Haiku** to read up to 20 recent reviews for a selected branch and produce three structured outputs: a plain-English summary, prioritized action items with quoted evidence, and a staff briefing memo the manager can send directly to their team.

---

## 4. Dataset

| Field | Detail |
|-------|--------|
| **Source** | Kaggle — `nelgiriyewithana/mcdonalds-store-reviews` |
| **Size** | 32,736 customer reviews |
| **Coverage** | McDonald's locations across the United States |
| **Key columns** | `rating` (1–5 stars, stored as "X star/stars" strings), `review` (free text), `store_address`, `rating_count`, `years_ago`, `months_ago`, `days_ago`, `hours_ago` |
| **Access** | Auto-downloaded at first run via `kagglehub` library |

### Data Cleaning Pipeline (`data_loader.py`)
- **Rating parsing:** Regex extracts integer from strings like "3 stars" → stored as nullable `Int64`
- **Address parsing:** `store_address` split into `street`, `city`, `zip`, `country` — `street` is the branch identifier
- **Time normalization:** `total_days_ago` = `years_ago × 365 + months_ago × 30 + days_ago + hours_ago / 24`
- **Non-ASCII stripping:** Review text cleaned of encoding artifacts before LLM use
- **Caching:** Full pipeline runs once and is cached via `@st.cache_data`

---

## 5. Tech Stack

| Layer | Technology | Version |
|-------|-----------|---------|
| Frontend / App framework | Streamlit | ≥ 1.40 |
| AI Model | Anthropic Claude Haiku | `claude-haiku-4-5-20251001` |
| AI SDK | Anthropic Python SDK | ≥ 0.40 |
| Data processing | Pandas | ≥ 2.0 |
| Visualizations | Matplotlib, Seaborn | ≥ 3.8, ≥ 0.13 |
| Dataset access | kagglehub | ≥ 0.3 |
| Persistence / logging | SQLite (built-in) | — |
| Environment secrets | python-dotenv | ≥ 1.0 |
| Language | Python | 3.10+ |

---

## 6. System Architecture

### Three-Layer Design

```
[ Kaggle Dataset ]  →  [ data_loader.py ]  →  [ app.py (Streamlit UI) ]
                                                      ↓
                                              [ llm.py ] → [ Anthropic Claude API ]
                                                      ↓
                                              [ llm_logs.db (SQLite) ]
```

### Component Responsibilities

| File | Layer | What it does |
|------|-------|--------------|
| `app/data_loader.py` | Data | Downloads, cleans, caches dataset; exposes `get_df()` and `filter_reviews()` |
| `app/llm.py` | LLM | Loads system prompt, formats reviews, calls Claude API, logs to SQLite |
| `prompts/manager_copilot.txt` | Prompt | Instructs Claude to output SUMMARY / ACTION ITEMS / STAFF BRIEFING |
| `app/app.py` | Presentation | Streamlit UI — two tabs, charts, health table, insight cards, briefing download |
| `llm_logs.db` | Persistence | SQLite — logs every LLM call for auditability |
| `.env` | Config | Holds `ANTHROPIC_API_KEY` (never committed to git) |

### Co-Pilot Data Flow (Step by Step)
1. User selects a branch and time range in Tab 2
2. `filter_reviews(df, street, days)` filters the cached DataFrame
3. `generate_insights()` in `llm.py` takes the top 20 most recent filtered reviews
4. Reviews are formatted as numbered `[rating/5] review_text` lines
5. System prompt from `manager_copilot.txt` + formatted reviews → Claude Haiku API
6. Claude returns structured text with three sections
7. `_parse_sections()` splits the response on SUMMARY / ACTION ITEMS / STAFF BRIEFING headers
8. Sections rendered as styled HTML cards in the UI
9. Full response logged to `llm_logs.db`
10. Staff briefing exposed as a downloadable `.txt` file

---

## 7. Features (All 5 Implemented — 100% Coverage)

### F1 — Review Dashboard (Tab 1)

An interactive analytics layer giving managers a visual pulse on branch performance across all locations.

**Charts included:**
- **Rating Distribution** — Bar chart of 1–5 star counts across all reviews
- **Top 10 Best Performing Branches** — Horizontal bar, min. 10 reviews, yellow ≥ 4.5 stars
- **Top 10 Cities by Review Count** — Horizontal bar, top city highlighted in yellow
- **Worst 5 Performing Branches** — Horizontal bar, red < 2.5 stars threshold line

**Interactivity:**
- City multiselect filter (default: all cities) — all charts update live on selection
- KPI row: Total Reviews | Avg Star Rating | Cities Covered

---

### F2 — Branch Health Overview Table (Tab 2)

A ranked table of all branches with health status indicators, designed for quick triage.

**Columns:** Branch (street), City, Avg Rating, Total Reviews, Status, Trend

**Status thresholds:**
- 🔴 Needs Attention: avg rating < 2.5
- 🟡 Monitor: avg rating 2.5–3.5
- 🟢 Healthy: avg rating > 3.5

**Trend logic:**
- Compares last-30-day average vs 31–60 day average
- ⬆️ Improving if diff > +0.1 | ⬇️ Declining if diff < –0.1 | ➡️ Stable otherwise

**UX detail:** Rows are clickable — clicking any row instantly loads that branch into the Co-Pilot filter via `st.session_state`, avoiding the need to manually find the branch in a long dropdown.

---

### F3 — AI Insight Generation (Tab 2)

The core AI feature. One click sends up to 20 recent reviews to Claude Haiku and returns a structured management brief.

**Model:** `claude-haiku-4-5-20251001`
- Chosen over Llama3 (local) for speed, reliability, and cost (~$0.001/call)
- `max_tokens = 1000` — sufficient for all three sections

**Why 20 reviews:**
- Stays within predictable token budget
- Covers recent operational reality without historical noise
- Keeps API response time under ~10 seconds

**Output sections:**
| Section | Content |
|---------|---------|
| SUMMARY | 2–3 sentences on dominant themes across reviews |
| ACTION ITEMS | Exactly 3 numbered items, each with: problem → evidence quote → corrective action |
| STAFF BRIEFING | ≤150-word internal memo starting with "Team," ready to send to staff |

---

### F4 — Evidence Grounding & Hallucination Prevention

The system prompt enforces strict output rules to make the AI defensible in an operational setting:

- **Only reference issues that appear in the provided reviews** — fabrication explicitly forbidden
- **Every action item must cite a real quote or close paraphrase** from the reviews
- **If reviews are positive**, Claude acknowledges that and reframes action items as maintaining strengths
- **No extra sections, headers, or commentary** outside the three required sections

**PII hygiene:** Only `review` text and `rating` value are sent to the API — no reviewer names, store coordinates, or metadata.

---

### F5 — Staff Briefing Download

The STAFF BRIEFING section is exposed as a one-click downloadable `.txt` file via Streamlit's `st.download_button`.

- **Filename format:** `briefing_{branch_name}_{YYYY-MM-DD}.txt`
- **Sanitization:** Street name cleaned with `re.sub(r"[^\w\-]", "_", street)` before use in filename
- **Content:** Markdown formatting stripped (`**` and `*`) for clean plain text
- **Purpose:** Closes the loop — manager can share directly with staff without copy-pasting

---

## 8. System Prompt Design

**File:** `prompts/manager_copilot.txt`

**Role assigned to Claude:** Operations assistant for McDonald's branch managers

**Output format enforced:**
```
SUMMARY
[2-3 sentences]

ACTION ITEMS
1. [Problem] — Evidence: "[quote]" — Action: [what to do]
2. ...
3. ...

STAFF BRIEFING
Team,
[≤150 words]
```

**Key prompt engineering decisions:**
- Headers are exact and on their own line → enables regex parsing in `_parse_sections()`
- "Ordered from highest to lowest priority" → ensures most urgent issue is always #1
- "Do not add any text before SUMMARY" → prevents preamble that breaks parsing
- Evidence citation requirement → makes outputs auditable and builds user trust

---

## 9. UI Design

### Branding
McDonald's red/yellow palette applied throughout via injected CSS:
- `MCD_RED = #DA291C` — headers, buttons, active tabs, KPI card borders
- `MCD_YELLOW = #FFC72C` — sidebar accent, hover states, secondary buttons
- `MCD_DARK = #1A1A1A` — body text
- `MCD_LIGHT = #FFF8F0` — warm off-white page background

### Insight Cards Layout
After generating insights, the three sections render as styled cards:
- SUMMARY and ACTION ITEMS side-by-side in a 2-column grid
- STAFF BRIEFING full-width below, in a serif font ("Ready to share" badge)
- Each action item rendered with `#N — Problem`, quoted evidence in a yellow left-border blockquote, and a red `→ Action` directive line

---

## 10. Safety & Responsible AI

- **AI is advisory, not prescriptive** — all outputs are recommendations to be reviewed by a human manager before acting
- **No PII in API calls** — only review text + star rating sent to Claude; no names, addresses, or reviewer IDs
- **Grounded outputs** — system prompt forbids fabrication; Claude cannot invent complaints absent from the provided reviews
- **Prompt injection prevention** — user input (branch name, time range) is used only to filter data, never injected into the prompt string
- **Audit log** — every LLM call logged to local SQLite (`llm_logs.db`) for accountability; not transmitted anywhere
- **Token cap** — hard limit of 20 reviews per call; prevents accidental large payloads

---

## 11. Observability & Telemetry

Every API call is logged to `llm_logs.db` (SQLite, project root, gitignored).

**Schema:**
```sql
llm_logs(
  id            INTEGER PRIMARY KEY AUTOINCREMENT,
  timestamp     TEXT,     -- UTC ISO format
  branch        TEXT,     -- street address of selected branch
  days_range    INTEGER,  -- time window selected
  num_reviews   INTEGER,  -- actual reviews sent (max 20)
  response_text TEXT      -- full Claude response
)
```

**Use cases:** Usage analysis, cost monitoring, debugging incorrect outputs, CP4 evaluation.

---

## 12. Key Technical Decisions & Why

| Decision | Reason |
|----------|--------|
| Claude Haiku over local Llama3 | Llama3 (4.7GB) had timeout issues on development hardware (GTX 1650); Haiku is faster, cheaper (~$0.001/call), and more reliable |
| Branch-level (street) not city-level filtering | Professor feedback — managers care about specific addresses, not city aggregates |
| 20-review cap | Balances token cost, response time, and recency; avoids historical noise |
| SQLite over a cloud log | No third-party dependency; sufficient for course-scale evaluation |
| `total_days_ago` computed column | Single float enables all date-range filters as simple `<=` comparisons |
| Regex section parsing (not JSON) | Claude outputs plain text reliably; JSON format adds parsing overhead and prompt complexity |
| Row-click → session_state pattern | Lets health table row selection update the branch selectbox on the same rerun without extra UI elements |

---

## 13. User Study Results

**Study conducted:** April 2026 | **N = 5 participants** | **Format:** In-person think-aloud, ~40 min sessions

### Participants
| ID | Role | Analytics Experience | AI Comfort (1–5) | Food Service Exp. |
|----|------|---------------------|-----------------|-------------------|
| P1 | MSIM student | Weekly | 4 | No |
| P2 | Business Analytics student | Monthly | 3 | Yes |
| P3 | Data Science student | Daily | 5 | No |
| P4 | MBA student (Operations) | Rarely | 2 | Yes |
| P5 | MSIM student (UX track) | Weekly | 4 | No |

### Task Results

| Task | Success % | Assisted % | Failed % | Mean Time |
|------|-----------|------------|----------|-----------|
| T1 — Find most-reviewed city | 100% | 0% | 0% | 32s |
| T2 — Identify worst branch | 80% | 20% | 0% | 44s |
| T3 — Filter to single city | 60% | 20% | 20% | 94s |
| T4 — Navigate Co-Pilot + select branch | 60% | 40% | 0% | 97s |
| T5 — Generate AI insights | 100% | 0% | 0% | 93s |
| T6 — Download staff briefing | 80% | 20% | 0% | 24s |
| T7 — Adjust time range + regenerate | 80% | 20% | 0% | 110s |
| **Overall** | **80%** | **17%** | **3%** | **71s** |

### Scale Scores

| Metric | Score | Benchmark | Status |
|--------|-------|-----------|--------|
| SUS (System Usability Scale) | **75.5 / 100** | ≥ 71 = Good | ✅ Good |
| UMUX-Lite | **65.0 / 100** | ≥ 70 = Acceptable | ⚠️ Slightly below |
| AI Trust Score | **76%** | ≥ 60% | ✅ Met |

### Key Usability Findings

**What worked:**
- Dashboard charts were understood by all 5 participants without instruction
- AI action items citing evidence from reviews significantly increased trust (3/5 mentioned this)
- Staff briefing memo format was valued — 4/5 said they'd use it in a real operations role

**Top 3 problems:**
1. **Health table row-click not discoverable (4/5)** — Users treated the table as read-only; no visual affordance signals interactivity
2. **City multiselect deselect-all interaction (3/5)** — Users didn't know they had to clear all cities before selecting one
3. **AI spinner provides no time expectation (3/5)** — 8–10 second wait without a progress cue caused uncertainty about whether the app had frozen

### Key Participant Quotes

> *"I like that it actually quotes the review in the action item. It feels honest — it's not just making stuff up."* — P3

> *"If I were a branch manager, I'd run this every Monday before my team huddle."* — P1

> *"I'd trust the summary, but I'd verify the action items myself before telling my team."* — P4

> *"The spinner just said 'Analysing reviews with Claude' and then nothing for 8 seconds. I wasn't sure if it froze."* — P5

### Recommended Design Changes (Priority Order)
1. Add row hover highlight + tooltip on health table ("Click to load in Co-Pilot")
2. Add Clear All / Select All button to city multiselect
3. Improve spinner copy to show estimated wait time
4. Make staff briefing editable in a text area before download
5. Link source reviews to each action item via an expander

---

## 14. Comparison to Original Proposal

| Proposal Goal | Final Implementation | Status |
|--------------|---------------------|--------|
| Transformer-based sentiment classification (RoBERTa) | Replaced by Claude Haiku — faster, more interpretable, and generates natural language rather than labels | ✅ Exceeded (richer output) |
| Confidence thresholding for low-certainty predictions | Evidence citation requirement in prompt achieves transparency differently — every claim is grounded in actual quotes | ✅ Addressed differently |
| Aspect-level tracking (service vs. food) | Claude's SUMMARY naturally surfaces aspect-level themes without explicit classification | ✅ Implicit |
| Geographic and temporal analysis | City filter + branch health table + time-range slider | ✅ Implemented |
| Human-centered decision support, not automation | AI is advisory; outputs must be reviewed; grounding prevents fabrication | ✅ Core design principle |

---

## 15. Project Milestones

| Checkpoint | Deliverable | Status |
|-----------|-------------|--------|
| CP1 | Proposal, literature review, repo setup | ✅ Done |
| CP2 | Prompting experiments, validation across Claude/GPT/Gemini, initial app | ✅ Done |
| CP3 | Full feature implementation, live Streamlit deployment, feature coverage report | ✅ Done |
| CP4 | User study (N=5), SUS/UMUX-Lite/trust evaluation, Evaluation.md | ✅ Done |

---

## 16. Literature Referenced

- Amershi et al. (2019) — Guidelines for Human-AI Interaction
- Barbieri et al. (2020) — TweetEval: Unified Benchmark for Tweet Classification
- Guo et al. (2017) — On Calibration of Modern Neural Networks
- Hu et al. (2009) — Overcoming the J-Shaped Distribution of Product Reviews
- Hutto & Gilbert (2014) — VADER: A Parsimonious Rule-based Model for Sentiment Analysis
- Li & Hitt (2008) — Self-selection and Information Role of Online Product Reviews
- Liu et al. (2019) — RoBERTa: A Robustly Optimized BERT Pretraining Approach
- Sun et al. (2019) — Utilizing BERT for Aspect-Based Sentiment Analysis

---

## 17. Quick Reference: Key Numbers

| Metric | Value |
|--------|-------|
| Total reviews in dataset | 32,736 |
| Reviews sent per AI call (max) | 20 |
| Avg API cost per call | ~$0.001 |
| AI model | Claude Haiku (`claude-haiku-4-5-20251001`) |
| App response time (AI) | 8–10 seconds |
| Features implemented | 5 of 5 (100%) |
| User study participants | 5 |
| Overall task success rate | 80% |
| Mean SUS score | 75.5 / 100 |
| Mean AI trust score | 76% |
| Live app URL | https://mcd-command-centre.streamlit.app |
