# Feature Coverage — McDonald's Social Media Command Centre

---

## 1. Data Ingestion & Processing

| Feature | Details | File |
|---|---|---|
| Kaggle dataset download | Uses `kagglehub` to auto-download and cache the dataset at runtime | `data_loader.py` |
| Streamlit data caching | `@st.cache_data` prevents re-downloading on every rerun | `data_loader.py` |
| Rating normalization | Extracts numeric value from strings like `"4 stars"` using regex | `data_loader.py` |
| Address parsing | Splits `store_address` into `street`, `city`, `zip`, `country` components | `data_loader.py` |
| Time-ago computation | Combines `years_ago`, `months_ago`, `days_ago`, `hours_ago` into a single `total_days_ago` float | `data_loader.py` |
| Review length feature | Computes `review_length` (character count) for each review | `data_loader.py` |
| Rating count cleaning | Strips commas from `rating_count` strings and casts to numeric | `data_loader.py` |
| Null row removal | Drops any rows with missing values after all transformations | `data_loader.py` |
| Branch-level filtering | `filter_reviews(df, street, days)` — exact match on `street` + rolling day window | `data_loader.py` |

---

## 2. Review Dashboard (Tab 1)

| Feature | Details | File |
|---|---|---|
| Multi-city filter | Sidebar multiselect to include/exclude cities; defaults to all | `app.py` |
| KPI cards | Three metric tiles: Total Reviews, Avg Star Rating, Cities Covered | `app.py` |
| Rating distribution chart | Bar chart of review counts per star rating (1–5) | `app.py` |
| Review length boxplot | Box-and-whisker chart showing review length distribution per rating | `app.py` |
| Top 10 cities chart | Horizontal bar chart of cities by review volume; top city highlighted in yellow | `app.py` |
| Feature correlation heatmap | Seaborn heatmap of `rating`, `rating_count`, `review_length`, `total_days_ago` | `app.py` |
| McDonald's chart theme | Custom colours (red/yellow/cream), fonts, and spine styling on all charts | `app.py` |

---

## 3. Branch Health Overview (Tab 2)

| Feature | Details | File |
|---|---|---|
| Health table | Aggregates all branches: Avg Rating, Total Reviews, Status, Trend | `app.py` |
| Colour-coded status | 🔴 Needs Attention (`< 2.5`), 🟡 Monitor (`2.5–3.5`), 🟢 Healthy (`> 3.5`) | `app.py` |
| Trend indicator | Compares last 30-day avg vs 31–60-day avg; ⬆️ / ➡️ / ⬇️ (±0.1 threshold) | `app.py` |
| Row-click → branch selector | Clicking a health table row sets `session_state["copilot_branch"]` so the sidebar selectbox updates on the same rerun | `app.py` |

---

## 4. AI Manager Co-Pilot (Tab 2)

| Feature | Details | File |
|---|---|---|
| Branch selectbox | Sidebar dropdown listing all unique branch streets | `app.py` |
| Time range slider | Sidebar slider (30–365 days) to control the review window | `app.py` |
| Active branch display | Styled badge in sidebar showing the currently selected branch | `app.py` |
| Review count feedback | Inline message showing how many reviews match the current filter | `app.py` |
| Generate Insights button | Triggers LLM call; disabled path shows warning when no reviews found | `app.py` |
| Top-20 review cap | Only the 20 most recent reviews are sent to Claude to stay within token limits | `llm.py` |
| Structured LLM output | System prompt enforces three sections: SUMMARY / ACTION ITEMS / STAFF BRIEFING | `prompts/manager_copilot.txt` |
| Summary card | Displays Claude's 2–3 sentence theme overview in a styled red card | `app.py` |
| Action items card | Parses numbered action items into structured HTML: problem, evidence quote, directive | `app.py` |
| Staff briefing card | Full-width dark card showing the memo ready for team distribution | `app.py` |
| Download staff briefing | One-click `.txt` download; filename sanitized as `briefing_{branch}_{YYYY-MM-DD}.txt` | `app.py` |
| Reviews preview table | Shows up to 50 matching reviews (rating, text, days ago, length) sorted by recency | `app.py` |

---

## 5. LLM Integration

| Feature | Details | File |
|---|---|---|
| Anthropic Claude API call | `client.messages.create()` with system + user message | `llm.py` |
| Model | `claude-haiku-4-5-20251001` | `llm.py` |
| System prompt loading | Reads `prompts/manager_copilot.txt` at call time | `llm.py` |
| Review formatting | Reviews formatted as numbered list `[rating/5] review_text` | `llm.py` |
| Error handling | Catches `AuthenticationError`, `APIConnectionError`, `APIStatusError` and returns user-friendly messages | `llm.py` |
| SQLite call logging | Every LLM call logged to `llm_logs.db`: timestamp, branch, days_range, num_reviews, response_text | `llm.py` |
| dotenv key loading | `.env` resolved relative to project root regardless of working directory | `llm.py` |

---

## 6. UI & Branding

| Feature | Details | File |
|---|---|---|
| McDonald's colour palette | Red `#DA291C`, Yellow `#FFC72C`, Dark `#1A1A1A`, Cream `#FFF8F0` | `app.py` |
| CSS injection | Custom styles injected via `st.markdown()` for sidebar, tabs, buttons, cards, charts | `app.py` |
| Inter font | Google Fonts `Inter` applied globally via `@import` | `app.py` |
| Page header banner | Gradient red banner with logo and subtitle on every page load | `app.py` |
| Dark sidebar | Black sidebar with yellow border and section separators | `app.py` |
| Styled tab bar | Active tab highlighted red; hover state highlights yellow | `app.py` |
| Primary/secondary buttons | Red primary button with shadow; yellow-bordered secondary button | `app.py` |
| Responsive wide layout | `layout="wide"` with sidebar expanded by default | `app.py` |

---

## 7. Configuration & Infrastructure

| Feature | Details | File |
|---|---|---|
| Environment variable management | `python-dotenv` loads `.env` from project root | `.env` / `llm.py` |
| `.gitignore` | Excludes `.env`, `__pycache__`, `.venv`, `llm_logs.db`, notebook checkpoints | `.gitignore` |
| Dependency manifest | All Python dependencies pinned in `requirements.txt` | `requirements.txt` |
| SQLite persistence | `llm_logs.db` auto-created at project root on first LLM call | `llm.py` |

---

## Feature Count Summary

| Area | Features |
|---|---|
| Data Ingestion & Processing | 9 |
| Review Dashboard | 7 |
| Branch Health Overview | 4 |
| AI Manager Co-Pilot | 11 |
| LLM Integration | 7 |
| UI & Branding | 8 |
| Configuration & Infrastructure | 4 |
| **Total** | **50** |
