# Architecture Diagram — McDonald's Social Media Command Centre

```mermaid
flowchart TD

    %% ── External Sources ──────────────────────────────────────────────
    subgraph EXT["External Sources"]
        KAGGLE["🗄️ Kaggle Dataset\nnelgiriyewithana/\nmcdonalds-store-reviews"]
        ANTHROPIC["🤖 Anthropic API\nclaude-haiku-4-5-20251001"]
        ENV[".env\nANTHROPIC_API_KEY"]
    end

    %% ── Data Layer ────────────────────────────────────────────────────
    subgraph DATA["Data Layer — data_loader.py"]
        DL_LOAD["kagglehub.dataset_download()\nDownload & cache CSV"]
        DL_CLEAN["Clean & Parse\n• Extract numeric rating\n• Parse store_address → street/city/zip\n• Compute total_days_ago\n• Drop nulls"]
        DL_FILTER["filter_reviews(df, street, days)\nFilter by branch + time window"]
        CACHE["@st.cache_data\nIn-memory DataFrame"]
    end

    %% ── LLM Layer ────────────────────────────────────────────────────
    subgraph LLM["LLM Layer — llm.py"]
        LLM_PROMPT["Load System Prompt\nprompts/manager_copilot.txt"]
        LLM_FORMAT["Format top-20 reviews\ninto numbered text"]
        LLM_CALL["anthropic.Anthropic()\nclient.messages.create()"]
        LLM_LOG["SQLite Logging\nllm_logs.db\n(timestamp, branch, days,\nnum_reviews, response_text)"]
    end

    %% ── Prompt ───────────────────────────────────────────────────────
    PROMPT["📄 prompts/manager_copilot.txt\nSystem prompt:\nSUMMARY / ACTION ITEMS /\nSTAFF BRIEFING format"]

    %% ── App Layer ────────────────────────────────────────────────────
    subgraph APP["Presentation Layer — app.py (Streamlit)"]

        subgraph TAB1["Tab 1 — Review Dashboard"]
            T1_FILTER["Sidebar: Multi-city filter"]
            T1_KPI["KPI Cards\nTotal Reviews · Avg Rating · Cities"]
            T1_CHARTS["Charts (Matplotlib / Seaborn)\n• Rating Distribution\n• Review Length Boxplot\n• Top 10 Cities\n• Feature Correlation Heatmap"]
        end

        subgraph TAB2["Tab 2 — AI Manager Co-Pilot"]
            T2_HEALTH["Branch Health Overview Table\n_build_health_table()\n🔴 <2.5 · 🟡 2.5-3.5 · 🟢 >3.5\nTrend: ⬆️ ➡️ ⬇️ (30-day vs 31-60 day)"]
            T2_FILTER["Sidebar: Branch selectbox + Day slider\n(row-click → session_state['copilot_branch'])"]
            T2_BTN["Generate Insights button"]
            T2_CARDS["Insight Cards\n_render_insights_cards()\n• Summary\n• Action Items (styled HTML)\n• Staff Briefing"]
            T2_DL["Download Staff Briefing\nbriefing_{branch}_{date}.txt"]
            T2_TABLE["Reviews Preview Table\n(top 50, sorted by recency)"]
        end

        CSS["McDonald's Brand Theme\n_inject_css()\nRed #DA291C · Yellow #FFC72C"]
    end

    %% ── Flow ──────────────────────────────────────────────────────────
    KAGGLE -->|"kagglehub download"| DL_LOAD
    DL_LOAD --> DL_CLEAN --> CACHE
    ENV -->|"load_dotenv()"| LLM_CALL
    CACHE --> TAB1
    CACHE --> TAB2

    T1_FILTER --> T1_KPI --> T1_CHARTS

    T2_HEALTH --> T2_FILTER
    T2_FILTER -->|"filter_reviews()"| DL_FILTER
    DL_FILTER --> T2_BTN
    T2_BTN -->|"generate_insights()"| LLM_FORMAT
    PROMPT --> LLM_PROMPT --> LLM_CALL
    LLM_FORMAT --> LLM_CALL
    LLM_CALL -->|"API request"| ANTHROPIC
    ANTHROPIC -->|"response text"| LLM_CALL
    LLM_CALL --> LLM_LOG
    LLM_CALL -->|"structured text"| T2_CARDS
    T2_CARDS --> T2_DL
    T2_CARDS --> T2_TABLE

    CSS -.->|"injected into all tabs"| APP

    %% ── Styles ───────────────────────────────────────────────────────
    classDef external  fill:#fff3cd,stroke:#ffc72c,color:#1a1a1a
    classDef data      fill:#e8f4f8,stroke:#5b9bd5,color:#1a1a1a
    classDef llm       fill:#e8f0e8,stroke:#5b9b5b,color:#1a1a1a
    classDef app       fill:#fce8e8,stroke:#da291c,color:#1a1a1a
    classDef prompt    fill:#f0e8f8,stroke:#9b5bd5,color:#1a1a1a

    class KAGGLE,ANTHROPIC,ENV external
    class DL_LOAD,DL_CLEAN,DL_FILTER,CACHE data
    class LLM_PROMPT,LLM_FORMAT,LLM_CALL,LLM_LOG llm
    class T1_FILTER,T1_KPI,T1_CHARTS,T2_HEALTH,T2_FILTER,T2_BTN,T2_CARDS,T2_DL,T2_TABLE,CSS app
    class PROMPT prompt
```

## Component Summary

| Layer | File | Responsibility |
|---|---|---|
| **Data** | `app/data_loader.py` | Downloads dataset via kagglehub, cleans & parses, exposes `get_df()` and `filter_reviews()` |
| **LLM** | `app/llm.py` | Loads system prompt, formats reviews, calls Claude API, logs to SQLite |
| **Prompt** | `prompts/manager_copilot.txt` | Instructs Claude to output SUMMARY / ACTION ITEMS / STAFF BRIEFING |
| **Presentation** | `app/app.py` | Streamlit UI — two tabs, sidebar filters, charts, insight cards, briefing download |
| **Persistence** | `llm_logs.db` | SQLite — logs every LLM call (branch, days, num_reviews, response) |
| **Config** | `.env` | Holds `ANTHROPIC_API_KEY` (never committed) |

## Data Flow (Co-Pilot Tab)

```
Kaggle CSV
  └─► data_loader.get_df()          # clean + cache
        └─► filter_reviews()        # branch + time window
              └─► llm.generate_insights()
                    ├─► manager_copilot.txt  (system prompt)
                    ├─► top-20 reviews       (user message)
                    └─► Claude Haiku API
                          └─► SUMMARY / ACTION ITEMS / STAFF BRIEFING
                                ├─► Rendered as styled cards in Streamlit
                                ├─► Logged to llm_logs.db
                                └─► Downloadable as .txt briefing
```
