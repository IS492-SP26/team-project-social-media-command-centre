# McDonald’s Social Media Command Centre

## Abstract

Customer review platforms generate large volumes of unstructured text that branch managers rarely read in full. The McDonald’s Social Media Command Centre is a generative AI system that:

* Ingests 32,736 public reviews from a Kaggle dataset.
* Surfaces signals at the level of an individual store.
* Uses the Anthropic Claude Haiku model to produce a plain English summary.
* Generates three prioritized action items and a ready to send staff briefing memo.
* Enforces evidence grounding through a strict system prompt requiring cited quotes.
* Caps model calls at the twenty most recent reviews to control cost and latency.

The system features a Streamlit interface with a review dashboard and an AI Co-Pilot that converts raw feedback into operational language within seconds. This report describes the architecture, prompting strategy, and telemetry layer.

---

## 1. Introduction and Related Work

### 1.1 Motivation

* Generative AI allows for the conversion of unstructured text into operational decision making tools.
* Prior natural language tools focused on sentiment scores or topic clusters; LLMs can now produce actionable memos.
* McDonald’s branches accumulate thousands of reviews that are difficult for area managers to synthesize manually.
* The system identifies specific failures in service speed, cleanliness, or accuracy to provide targeted interventions.

### 1.2 Contributions

* **Streamlit Application:** A working tool for processing 32,736 reviews into branch level signals.
* **System Prompt Design:** A schema that enforces grounding by requiring verbatim review quotes for every action item.
* **Telemetry Layer:** A persistent SQLite database that logs model calls for cost monitoring and debugging.
* **User Study Design:** A framework to measure time saved and memo quality against a no tool baseline.

### 1.3 Related Work

* **Attribute Extraction:** Modeled after Katsaros et al. (2025), the system produces structured attributes rather than general summaries.
* **Model Selection:** While Cabrio et al. (2025) question the need for LLMs in simple analysis, this project uses Claude Haiku for its ability to produce reliable, operator ready language.
* **Interaction Patterns:** Following Desolda et al. (2025), the design ensures the human remains in the loop by providing verifiable evidence for every AI suggestion.

---

## 2. Method

### 2.1 System Overview

The architecture consists of three distinct layers:

1.  **Data Layer:** Downloads and cleans the Kaggle dataset.
2.  **LLM Layer:** Formats reviews, calls the Anthropic API, and logs results.
3.  **Presentation Layer:** A Streamlit application with a Dashboard and a Co-Pilot tab.

### 2.2 Data Pipeline

* The dataset is downloaded via the `kagglehub` library and cached.
* Non ASCII bytes are stripped from review text.
* Store addresses are split into street, city, zip, and country.
* A "total days ago" metric is computed to weight recency.
* Reviews are filtered by street and date range for specific branch analysis.

### 2.3 LLM Integration

* The system loads prompts from external text files to allow updates without redeploying code.
* Input is capped at the twenty most recent reviews to manage token limits and costs.
* The model used is `claude-haiku-4-5-20251001`.
* Error handling catches authentication or connection issues to prevent application crashes.

### 2.4 Prompt Design

The system prompt enforces a three section response:
1.  **SUMMARY:** A high level overview.
2.  **ACTION ITEMS:** Three items following a "problem, evidence, action" structure.
3.  **STAFF BRIEFING:** A memo ready for distribution.

### 2.5 Presentation Layer

* **Visual Identity:** Customized CSS uses the McDonald’s color palette (red, yellow, and cream).
* **Review Dashboard:** Displays KPI cards and charts for rating distributions and city performance.
* **AI Manager Co-Pilot:** Provides a "Branch Health Overview" table with trend arrows and color coded status.

### 2.6 Telemetry and Evaluation

* All calls are logged to `llm_logs.db` with timestamps and response text.
* A planned study with five participants will measure time to completion and issue identification accuracy.

---

## 3. Results, Analysis, and Discussion

### 3.1 Implementation Status

| ID | Feature | Implementation | Status |
| :--- | :--- | :--- | :--- |
| F1 | Review Dashboard | Charts for ratings, cities, and branch performance. | Implemented |
| F2 | Branch Health Table | Ranked branch list with status and trend indicators. | Implemented |
| F3 | AI Insight Generation | Claude Haiku outputs summary, items, and memo. | Implemented |
| F4 | Evidence Grounding | Requires verbatim quotes for every action item. | Implemented |
| F5 | Memo Download | One click .txt export for staff communications. | Implemented |

### 3.2 Discussion Points

* **Structural Value:** The primary benefit is the structured schema, not just the text generation.
* **Prompting as Grounding:** Evidence grounding was achieved through strict prompting rather than complex RAG architectures.
* **Granularity:** Moving from city level to branch level analysis was critical for operational relevance.

---

## 4. Limitations and Ethics

* **Static Data:** The dataset is a snapshot and does not provide real time updates.
* **Sampling Bias:** Using only twenty reviews may miss broader trends in high volume branches.
* **Hallucination Risk:** While not observed, there is no programmatic verification that quotes exist in the source text.
* **Privacy:** Reviewer names are not used, but employee names appearing in reviews may be surfaced.

---

## 5. Conclusion and Future Work

The McDonald’s Social Media Command Centre demonstrates that a grounded LLM can convert raw feedback into useful management tools. Future work will include:

* Expanding the user study to gather quantitative performance metrics.
* Implementing a retrieval step to allow managers to query specific topics like "cleanliness."
* Adding an automated evaluation harness to verify grounding for every response.

---

## 6. References

* Anthropic. (2025). Claude documentation: Prompt engineering.
* Brooke, J. (1996). SUS: A quick and dirty usability scale.
* Cabrio, G., et al. (2025). Do you actually need an LLM?
* Desolda, G., et al. (2025). Human-AI collaboration is not very collaborative yet.
* Katsaros, A., et al. (2025). From reviews to actionable insights.
* Nelgiriyewithana, A. (2024). McDonald’s store reviews [Data set].

---

## Appendices

### Appendix A: System Prompt

> You are an operations co pilot for a McDonald’s area manager. The user will give you a numbered list of recent customer reviews for one branch. produce exactly three sections: SUMMARY, ACTION ITEMS, STAFF BRIEFING.
>
> Constraints: Base every claim strictly on the reviews provided. Do not reference issues not present. Do not invent quotes. Keep the briefing under 150 words.

### Appendix B: Architecture Overview

| Layer | File | Responsibility |
| :--- | :--- | :--- |
| Data | `data_loader.py` | Cleaning and parsing the Kaggle dataset. |
| LLM | `llm.py` | API calls and SQLite logging. |
| Prompt | `manager_copilot.txt` | Instructions for structure and grounding. |
| Presentation | `app.py` | The Streamlit user interface. |
| Persistence | `llm_logs.db` | Storage for every model call. |

### Appendix E: Telemetry Schema

| Column | Type | Description |
| :--- | :--- | :--- |
| id | INTEGER | Primary key. |
| timestamp | TEXT | UTC ISO 8601 time. |
| branch | TEXT | The street address of the branch. |
| days_range | INTEGER | The selected time window. |
| num_reviews | INTEGER | Count of reviews sent to the model. |
| response_text | TEXT | The raw output from Claude. |
