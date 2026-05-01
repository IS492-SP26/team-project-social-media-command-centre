# McDonald's Social Media Command Centre: An LLM-Powered Decision-Support Tool for Restaurant Operations Management

**Authors:** Pranav C, Jeet T, Ashish G
**Course:** IS492 — Generative AI for Human-AI Collaboration
**Institution:** University of Illinois Urbana-Champaign
**Date:** April 2026

---

## Abstract

Fast-food chains generate thousands of customer reviews each week, yet branch managers lack practical tools to extract actionable intelligence from this unstructured feedback at scale. This paper presents the McDonald's Social Media Command Centre (SMCC), a Streamlit web application that combines an interactive analytics dashboard with an AI-powered manager co-pilot built on Anthropic Claude Haiku. The system ingests 32,736 publicly available McDonald's customer reviews, surfaces branch-level performance signals through visualizations, and — on demand — generates structured management briefs comprising a plain-English summary, evidence-grounded action items, and a downloadable staff briefing memo. A think-aloud usability study with five participants yielded an overall task success rate of 80%, a mean System Usability Scale score of 75.5 (rated "Good"), and an AI trust score of 76%. Results indicate the tool successfully bridges the gap between raw review data and operational action, while surfacing key interaction design improvements for future iterations.

---

## 1. Introduction

Customer reviews posted on platforms such as Google Maps represent one of the richest — and most underutilized — sources of operational intelligence available to restaurant managers. For a chain like McDonald's, which operates over 40,000 locations globally, each branch receives a continuous stream of text feedback describing food quality, order accuracy, drive-thru wait times, and staff behavior. Yet the scale of this data makes systematic human reading infeasible: a regional manager overseeing dozens of branches could spend hours reading reviews that yield only a handful of actionable insights.

Existing analytics tools address this gap incompletely. Most reduce the entire content of review text to a single aggregate star rating, discarding the granular operational signals contained in the prose. Research on online review dynamics has further shown that rating distributions are systematically J-shaped — skewed toward extreme values (Li & Hitt, 2008; Hu et al., 2009) — meaning averages are structurally misleading as a performance proxy. Sentiment analysis tools offer a partial remedy but introduce their own problems: they classify polarity without identifying *which* operational dimension drove the sentiment, they mishandle sarcasm and mixed-sentiment language, and they rarely communicate model uncertainty to the human decision-maker (Guo et al., 2017).

The emergence of large language models (LLMs) creates a new design opportunity. Unlike classification pipelines that map text to labels, LLMs can reason over review content in context, identify specific operational themes, produce natural-language summaries, and — critically — generate structured communications ready for immediate operational use. Applied through careful prompt engineering, this capability can close the loop between customer feedback and management action in a way that prior tools could not.

This paper describes the design, implementation, and evaluation of the McDonald's Social Media Command Centre (SMCC), an application built for IS492 — Generative AI for Human-AI Collaboration at the University of Illinois Urbana-Champaign. The system makes three primary contributions:

1. A branch-level analytics dashboard that surfaces performance trends across a large real-world review dataset
2. An AI co-pilot that generates evidence-grounded, three-section management briefs from filtered review subsets using Claude Haiku via the Anthropic API
3. A usability evaluation protocol with quantitative metrics (task success, SUS, UMUX-Lite, trust) and qualitative findings that identify interaction design priorities for future iterations

---

## 2. Related Work

### 2.1 Sentiment Analysis for Customer Feedback

Sentiment analysis research has progressed from lexicon-based approaches to deep neural architectures. VADER (Hutto & Gilbert, 2014) introduced a rule-based model effective on short social media text but limited in handling implicit or mixed-sentiment language. Transformer-based models such as RoBERTa (Liu et al., 2019) and domain-adapted variants evaluated in the TweetEval benchmark (Barbieri et al., 2020) achieved substantially higher accuracy on social media corpora. Aspect-based sentiment analysis (ABSA) extended this work by disaggregating overall sentiment into facet-level signals — for example, separating food quality from service quality within a single review — as formalized in SemEval-2014 Task 4 (Pontiki et al., 2014) and advanced via BERT-based auxiliary sentence construction (Sun et al., 2019).

Despite these advances, three gaps persist in the practical application of sentiment analysis to restaurant operations. First, classification models produce labels without explanations, making it difficult for a manager to know *why* a branch's sentiment has declined or *which* operational factor to address. Second, high-accuracy models are often poorly calibrated — their confidence scores do not reliably reflect prediction correctness (Guo et al., 2017) — yet calibration is rarely surfaced to end users. Third, the J-shaped distribution of online review ratings (Li & Hitt, 2008) means aggregate scores systematically over-represent extreme opinions, a bias that existing dashboards do not acknowledge or correct for.

### 2.2 Human-AI Interaction in Decision-Support Tools

The Guidelines for Human-AI Interaction (Amershi et al., 2019) provide an evidence-based framework for designing AI systems that effectively collaborate with human decision-makers. Several guidelines are directly relevant to the SMCC design: AI systems should make clear what they can and cannot do (G1), support efficient error correction (G6), and explain their outputs with sufficient evidence for the user to evaluate reliability (G8). These principles informed the design of the Co-Pilot's evidence-grounded output format, which requires every action item to cite a direct quote from the reviews provided — making the AI's reasoning auditable and correctable by the manager.

Tufte's (2001) principles of data visualization informed the dashboard design: maximizing the data-to-ink ratio, using color purposefully rather than decoratively, and enabling multi-level views from summary (KPI cards) to detail (branch-level charts).

### 2.3 LLMs for Operational Decision Support

Checkpoint 2 prompting experiments compared Claude, ChatGPT, and Gemini across five prompt types: sentiment and issue extraction, rating-text mismatch reasoning, operational trend identification, ambiguous sentiment handling, and sarcasm detection. All three models demonstrated strong performance on clear sentiment classification tasks and trend identification. LLMs proved particularly advantageous over rule-based tools in handling sarcasm (e.g., correctly interpreting "Great job McDonald's. I waited 25 minutes for cold fries." as negative) and in providing decision-support style summaries that named specific operational categories rather than generic polarity labels. Claude was selected as the production model based on output consistency, adherence to structured format instructions, and cost efficiency (~$0.001 per call).

---

## 3. Method

### 3.1 System Description

The SMCC is a two-tab Streamlit web application deployed at https://mcd-command-centre.streamlit.app. The system architecture separates concerns across three layers: a data layer (`data_loader.py`), an LLM layer (`llm.py`), and a presentation layer (`app.py`).

**Data Layer.** The Kaggle dataset (`nelgiriyewithana/mcdonalds-store-reviews`, 32,736 reviews) is downloaded automatically on first run via `kagglehub` and cached in memory using `@st.cache_data`. Key transformations include: regex extraction of numeric ratings from strings like "3 stars" stored as nullable `Int64`; address parsing splitting `store_address` into `street`, `city`, `zip`, and `country` columns (with `street` serving as the branch identifier throughout); computation of a `total_days_ago` float column enabling simple `<=` comparisons for all date-range filters; and stripping of non-ASCII characters from review text before LLM transmission.

**Tab 1 — Review Dashboard.** Four Matplotlib charts give managers a visual pulse on performance: rating distribution across all reviews, top 10 best-performing branches (minimum 10 reviews), top 10 cities by review volume, and the five worst-performing branches by average rating. A city multiselect filter drives all four charts simultaneously. Three KPI cards (Total Reviews, Avg Star Rating, Cities Covered) provide headline figures above the chart grid.

**Tab 2 — AI Manager Co-Pilot.** The Co-Pilot tab opens with a Branch Health Overview table aggregated from the full dataset. Each row shows a branch's average rating, total review count, color-coded status (🔴 < 2.5 / 🟡 2.5–3.5 / 🟢 > 3.5), and a trend indicator comparing the last 30 days versus the previous 30-day window (±0.1 threshold). Rows are clickable: selecting a row sets `st.session_state["copilot_branch"]` and triggers a rerun, pre-loading that branch in the filter controls. A time-range slider (30–365 days) narrows the review window. Clicking "Generate Insights" calls `generate_insights()` in `llm.py`.

**LLM Layer.** `generate_insights()` loads the system prompt from `prompts/manager_copilot.txt`, formats the 20 most recent filtered reviews as numbered `[rating/5] review_text` lines, and calls `claude-haiku-4-5-20251001` with `max_tokens=1000`. Error handling covers authentication failures, connection errors, and API status errors. Every successful call is logged to `llm_logs.db` (SQLite) with fields: `timestamp`, `branch`, `days_range`, `num_reviews`, `response_text`.

**Prompt Design.** The system prompt instructs Claude to act as an operations assistant for branch managers and enforces a strict three-section output format (SUMMARY / ACTION ITEMS / STAFF BRIEFING). Each action item must follow the format: `[Problem] — Evidence: "[quote]" — Action: [directive]`. The prompt explicitly forbids referencing issues absent from the provided reviews, fabricating quotes, and adding sections beyond the three required ones. These constraints were designed to make outputs auditable (every claim is traceable to a specific review), consistent (regex section parsing is reliable), and safe for operational use. The STAFF BRIEFING section produces a ≤150-word internal memo beginning with "Team," ready for the manager to send directly to staff.

**Output Rendering.** `_parse_sections()` splits the Claude response on the three headers using case-insensitive regex. `_render_insights_cards()` renders SUMMARY and ACTION ITEMS side-by-side in styled HTML cards; `_format_action_items()` further parses each item into a problem headline, a yellow-bordered evidence quote block, and a red directive line. STAFF BRIEFING renders full-width in a serif card with a "Ready to share" badge. A download button exports the briefing as a sanitized `.txt` file.

### 3.2 Evaluation Design

A think-aloud usability study was conducted with five participants in April 2026 to evaluate task performance, system usability, and AI trust. Participants were recruited from IS492 and adjacent graduate programs at UIUC and screened for basic computer literacy and no prior exposure to the SMCC app. Sessions ran approximately 40 minutes each in an in-person format.

**Task Set.** Seven tasks were designed to cover the critical-path scenarios in both tabs:

| Task | Description |
|------|-------------|
| T1 | Find the most-reviewed city from the dashboard (no filter change) |
| T2 | Identify the worst-performing branch by average rating |
| T3 | Filter the dashboard to a single city using the multiselect |
| T4 | Navigate to the Co-Pilot tab and select a branch with "Needs Attention" status |
| T5 | Generate AI insights for the selected branch |
| T6 | Download the staff briefing as a .txt file |
| T7 | Adjust the time range slider and regenerate insights |

Tasks were timed from prompt delivery to participant declaration of completion. A 3-minute timeout applied; tasks exceeding this limit were coded as failed. Facilitated completions (where the facilitator provided a hint after 90 seconds of complete blockage) were coded as "assisted."

**Quantitative Measures.** Task success rate, assisted completion rate, error/failure rate, and mean time-on-task were computed per task and overall. Post-task questionnaires administered the System Usability Scale (SUS; Brooke, 1996), UMUX-Lite (2-item adaptation), and five custom Likert items measuring AI trust across dimensions of confidence, grounding, briefing usability, chart clarity, and real-world applicability.

**Qualitative Measures.** Think-aloud verbalizations and facilitator observations were recorded per session. After all sessions, observations were affinity-mapped into themes, labeled, and ranked by frequency. Themes mentioned by three or more participants were flagged as priority findings.

---

## 4. Results

### 4.1 Task Performance

Across 35 task attempts (7 tasks × 5 participants), 28 were completed successfully (80%), 6 were assisted (17%), and 1 failed (3%).

| Task | Success | Assisted | Failed | Mean Time (s) |
|------|---------|----------|--------|---------------|
| T1 — Most-reviewed city | 100% | 0% | 0% | 32 |
| T2 — Worst branch | 80% | 20% | 0% | 44 |
| T3 — Filter to single city | 60% | 20% | 20% | 94 |
| T4 — Co-Pilot + select branch | 60% | 40% | 0% | 97 |
| T5 — Generate insights | 100% | 0% | 0% | 93 |
| T6 — Download briefing | 80% | 20% | 0% | 24 |
| T7 — Adjust time range | 80% | 20% | 0% | 110 |
| **Overall** | **80%** | **17%** | **3%** | **71** |

Tasks T1 and T5 achieved 100% unaided success, confirming that locating high-level chart information and operating the AI generation button are intuitive. Tasks T3 and T4 showed the lowest success rates. T3 failures were driven by participants not knowing they needed to first deselect all cities before selecting a single one — the multiselect defaulting to "all selected" was a consistent source of confusion. T4 difficulties arose because 4 of 5 participants did not initially recognize the health table rows as clickable; all attempted the dropdown first and only discovered the row-click interaction with assistance.

### 4.2 Scale Scores

| Metric | Mean | Median | Min | Max | Target |
|--------|------|--------|-----|-----|--------|
| SUS (0–100) | 75.5 | 77.5 | 57.5 | 90.0 | ≥ 71 |
| UMUX-Lite (0–100) | 65.0 | 66.7 | 50.0 | 83.3 | ≥ 70 |
| AI Trust (0–100%) | 76% | 76% | 64% | 92% | ≥ 60% |

The mean SUS of 75.5 falls in the "Good" range (71–84) per Bangor et al. (2008) benchmarks, indicating the system is broadly usable. UMUX-Lite fell slightly below the 70-point acceptable threshold at 65.0, consistent with the specific friction points identified in T3 and T4. The overall AI trust score of 76% exceeded the 60% target. Notably, P4 (the least technically experienced participant, an MBA student with food service background) contributed the lowest scores across all three scales, suggesting the tool's current interaction design is better suited to users with moderate analytics experience.

Individual AI trust item means revealed an important nuance: dashboard chart helpfulness scored highest (M = 4.4/5) while the briefing's direct usability scored lowest (M = 3.0/5). The latter finding indicates that while participants valued the staff briefing concept, most wanted to edit the memo before sending it rather than using it verbatim.

### 4.3 Qualitative Themes

| Theme | Frequency | Sentiment |
|-------|-----------|-----------|
| Health table row-click not discoverable | 4/5 | Negative |
| Dashboard charts clear and informative | 4/5 | Positive |
| AI action items trustworthy due to evidence quotes | 3/5 | Positive |
| City multiselect deselect-all interaction unclear | 3/5 | Negative |
| AI wait time causes uncertainty | 3/5 | Negative |
| Staff briefing needs editing before use | 3/5 | Mixed |
| City vs. branch filter relationship unclear | 2/5 | Negative |

Four themes reached the three-participant priority threshold. The most prevalent negative finding — health table row-click discoverability — was confirmed by the T4 performance data. The most prevalent positive finding — trust in evidence-grounded action items — validates a core prompt engineering decision made during system design.

### 4.4 Notable Participant Observations

Participant P3 (Data Science, AI comfort 5/5) completed all tasks unaided with a mean time of 62 seconds and awarded a SUS of 90, representing the ceiling of the current design's potential for experienced users. Participant P4 (MBA, AI comfort 2/5, food service experience) failed T3, required assistance on T4 and T7, and awarded a SUS of 57.5 — the only score falling below the "OK" threshold of 50. This polarization suggests the system's learnability, rather than its core utility, is the primary adoption risk for the target user population of restaurant operations managers, who may have limited analytics tool experience.

---

## 5. Discussion

### 5.1 Evidence Grounding as a Trust Mechanism

The most significant positive finding from the user study is that evidence-grounded action items — where Claude is instructed to cite a direct quote from the reviews for each recommendation — meaningfully increased participant trust. Three of five participants spontaneously mentioned the quoting behavior as the primary reason they found the output credible. This finding directly validates Amershi et al.'s (2019) Guideline G8 (support user verification of AI outputs) as a trust-building mechanism in operational AI tools. It also addresses the hallucination concern that is central to responsible LLM deployment: by requiring quoted evidence, the system makes fabrication immediately detectable.

### 5.2 The Interaction Design Gap Between Discoverability and Value

The study exposed a meaningful gap between the utility of the health table and its discoverability as an interactive element. All five participants recognized the table's informational value immediately; four did not initially recognize its click affordance. This pattern illustrates a broader challenge in Streamlit-based interfaces: the framework's native `st.dataframe` component with `on_select="rerun"` does not provide a cursor change or visual hover state that would conventionally signal interactivity. The fix is straightforward — a hover highlight and tooltip — but the finding underscores that AI capability alone does not determine user experience quality; micro-interaction design carries significant weight.

### 5.3 Tension Between AI Speed and Perceived Reliability

The ~8–10 second API response time, while acceptable in absolute terms, produced anxiety in three participants who verbalized uncertainty about whether the app had frozen. This "spinner anxiety" is a known issue in AI-assisted tools where latency is non-deterministic. Importantly, the fix is not to make the AI faster (Claude Haiku is already among the fastest available models) but to give users a time expectation. A simple copy change — "Sending 20 reviews to Claude, takes about 8–10 seconds" — is sufficient to reframe latency as expected behavior rather than a failure signal.

### 5.4 Comparison to Original Proposal Goals

The original proposal scoped a RoBERTa-based classification pipeline with confidence thresholding and aspect-level ABSA. The final system diverges from this plan by using Claude Haiku to generate natural-language outputs rather than classification labels. This substitution was driven by a practical constraint (Llama3 local deployment had timeout issues on available hardware) but produced a qualitatively superior outcome: where RoBERTa would have output a label ("negative") and a confidence score (0.84), Claude Haiku outputs a three-paragraph brief with quoted evidence and a ready-to-send memo. The aspect-level tracking goal was addressed implicitly — Claude's SUMMARY naturally disaggregates themes by operational dimension — rather than through an explicit classification taxonomy. The uncertainty-awareness goal was addressed through the evidence-citation requirement rather than through probability reporting.

---

## 6. Limitations, Risks, and Ethical Considerations

### 6.1 Dataset Limitations

The Kaggle dataset is a static snapshot of reviews and does not update in real time. Actual deployment would require integration with live review APIs (Google Places, Yelp). The dataset also exhibits well-documented biases in online review platforms: extreme experiences are systematically over-represented (Li & Hitt, 2008), reviews are written by self-selected subsets of customers, and the linguistic style of reviews may not represent the full diversity of customer experiences across McDonald's demographic markets. Any operational decisions based on this data should account for these structural biases.

### 6.2 Hallucination Risk

Despite the evidence-citation constraint in the system prompt, Claude Haiku can produce paraphrases that subtly misrepresent the source review, or cite a real quote to support a claim that goes beyond what the evidence strictly supports. The prompt includes explicit instructions against fabrication, but these are soft constraints. In a production deployment, outputs should be reviewed by a human manager before being acted upon — a principle stated in the SAFETY.md documentation and reinforced in the app's responsible use note.

### 6.3 Small Study Sample

The usability study was conducted with N=5 participants, which is sufficient to identify major usability issues (Nielsen & Molich, 1990) but insufficient for statistically significant quantitative comparisons. The polarized scores between P3 and P4 suggest the system's usability varies substantially by user experience level, but confirming this with statistical confidence would require a larger, stratified sample.

### 6.4 PII and Data Privacy

All reviews in the dataset are publicly posted and contain no direct identifiers. The app transmits only `review` text and `rating` values to the Claude API — no store addresses, reviewer metadata, or session information. The app does not collect any information about the person using it. LLM call logs stored in `llm_logs.db` remain local and are excluded from version control. These measures address the data minimization principle under standard privacy frameworks.

### 6.5 Responsible AI Considerations

The SMCC is explicitly positioned as decision-support, not decision-automation. AI outputs are recommendations to be reviewed, not directives to be executed. This framing aligns with Amershi et al.'s (2019) principle that AI systems should support human agency rather than supplant it. The system does not make any employment-related inferences, does not generate outputs that could constitute defamation of specific staff members (the system prompt instructs Claude not to name individuals), and does not use review data for any purpose other than the operational summary it was collected for.

---

## 7. Conclusion and Future Work

This paper presented the McDonald's Social Media Command Centre, an LLM-powered operational decision-support tool that transforms unstructured customer reviews into structured, evidence-grounded management briefs. The system achieved 80% overall task success, a SUS of 75.5 ("Good"), and an AI trust score of 76% in a five-participant usability study — meeting four of six quantitative targets and identifying three high-priority interaction design improvements.

The primary contribution of this work is demonstrating that the combination of careful prompt engineering (strict output format, evidence citation requirement, hallucination prohibition) and a fast, low-cost LLM (Claude Haiku) can produce operational AI outputs that branch managers trust and want to act on, without requiring any machine learning expertise from the user.

**Future work** should prioritize the following:

1. **Row-click discoverability** — Add hover highlight and tooltip to the health table to signal interactivity.
2. **Multiselect UX** — Add "Select All / Clear All" buttons to the city filter.
3. **Spinner copy** — Replace the generic spinner with an estimated wait time message.
4. **Editable briefing** — Replace the static briefing card with an editable `st.text_area` pre-filled with Claude's output, so managers can personalize before downloading.
5. **Live data integration** — Connect to the Google Places API for real-time review ingestion.
6. **Expanded user study** — Conduct a larger study (N ≥ 15) with stratified recruitment across analytics experience levels to quantify the usability gap observed between P3 and P4.
7. **Longitudinal evaluation** — Measure whether managers who use the tool weekly show measurable improvement in branch ratings over a 60-day period.

---

## References

Amershi, S., Weld, D., Vorvoreanu, M., Fourney, A., Nushi, B., Collisson, P., Suh, J., Iqbal, S., Bennett, P., Inkpen, K., Teevan, J., Kikin-Gil, R., & Horvitz, E. (2019). Guidelines for Human-AI Interaction. *Proceedings of the 2019 CHI Conference on Human Factors in Computing Systems*, 1–13. https://doi.org/10.1145/3290605.3300233

Bangor, A., Kortum, P., & Miller, J. (2008). An empirical evaluation of the System Usability Scale. *International Journal of Human–Computer Interaction*, 24(6), 574–594.

Barbieri, F., Camacho-Collados, J., Espinosa-Anke, L., & Neves, L. (2020). TweetEval: Unified benchmark and comparative evaluation for tweet classification. *Findings of EMNLP 2020*, 1644–1650.

Brooke, J. (1996). SUS: A "quick and dirty" usability scale. In P. W. Jordan, B. Thomas, B. A. Weerdmeester, & I. L. McClelland (Eds.), *Usability Evaluation in Industry* (pp. 189–194). Taylor & Francis.

Guo, C., Pleiss, G., Sun, Y., & Weinberger, K. Q. (2017). On calibration of modern neural networks. *Proceedings of the 34th International Conference on Machine Learning (ICML)*, 70, 1321–1330.

Hu, N., Liu, L., & Zhang, J. J. (2009). Do online reviews affect product sales? The role of reviewer characteristics and temporal effects. *Information Technology & Management*, 9(3), 201–214.

Hutto, C. J., & Gilbert, E. (2014). VADER: A parsimonious rule-based model for sentiment analysis of social media text. *Proceedings of the International AAAI Conference on Web and Social Media (ICWSM)*, 8(1), 216–225.

Li, X., & Hitt, L. M. (2008). Self-selection and information role of online product reviews. *Information Systems Research*, 19(4), 456–474.

Liu, Y., Ott, M., Goyal, N., Du, J., Joshi, M., Chen, D., Levy, O., Lewis, M., Zettlemoyer, L., & Stoyanov, V. (2019). RoBERTa: A robustly optimized BERT pretraining approach. *arXiv preprint arXiv:1907.11692*.

Nielsen, J., & Molich, R. (1990). Heuristic evaluation of user interfaces. *Proceedings of the SIGCHI Conference on Human Factors in Computing Systems*, 249–256.

Pontiki, M., Galanis, D., Pavlopoulos, J., Papageorgiou, H., Androutsopoulos, I., & Manandhar, S. (2014). SemEval-2014 Task 4: Aspect based sentiment analysis. *Proceedings of the 8th International Workshop on Semantic Evaluation (SemEval 2014)*, 27–35.

Sun, C., Huang, L., & Qiu, X. (2019). Utilizing BERT for aspect-based sentiment analysis via constructing auxiliary sentence. *Proceedings of NAACL-HLT 2019*, 380–385.

Tufte, E. R. (2001). *The Visual Display of Quantitative Information* (2nd ed.). Graphics Press.

---

## Appendices

### Appendix A — System Prompt (Final Version, `prompts/manager_copilot.txt`)

```
You are an operations assistant for McDonald's branch managers.
You will be given a set of real customer reviews for a specific store location.
Your job is to help the manager understand what is going wrong and take action.

## OUTPUT FORMAT
You must always respond with exactly three sections, in this order, using these exact headers:

SUMMARY
ACTION ITEMS
STAFF BRIEFING

Do not add any text before SUMMARY or after the STAFF BRIEFING section.

---

### SUMMARY
Write 2-3 sentences in plain English.
State the dominant themes across the reviews (e.g. slow service, order errors, cleanliness).
Mention how widespread or severe the issues appear to be based on the reviews provided.

### ACTION ITEMS
List exactly 3 recommendations, numbered 1-3, ordered from highest to lowest priority.
Each item must:
- Name the specific problem being addressed.
- Quote or closely paraphrase at least one real complaint from the reviews as evidence.
- State a concrete corrective action the manager can take (e.g. retrain, audit, implement a checklist).

Format:
1. [Problem] — Evidence: "[quote or paraphrase]" — Action: [what to do]
2. ...
3. ...

### STAFF BRIEFING
Write a short internal memo the manager can send directly to their team.
- Maximum 150 words.
- Professional but direct tone.
- Do not mention customer names or quote reviews verbatim.
- Focus on what the team needs to do differently, not on blame.
- Begin with: "Team,"

---

## STRICT RULES
- Only reference issues that appear in the reviews you are given. Do not invent complaints.
- Do not fabricate quotes. If you paraphrase, make it clear it is a paraphrase.
- Do not mention issues absent from the provided reviews, even if they are common McDonald's problems.
- Do not add sections, headers, bullet points, or commentary outside the three required sections.
- If the reviews are overwhelmingly positive, say so honestly in SUMMARY and adjust ACTION ITEMS
  to focus on maintaining strengths rather than fixing problems — but still ground every point in
  actual review evidence.
```

---

### Appendix B — Prompt Evolution Across Checkpoints

**CP1 Prompt (exploratory, used in validation experiments):**
Initial prompts tested across Claude, ChatGPT, and Gemini covered five scenarios:
1. *Sentiment + issue extraction* — "Analyze these 5 reviews. Classify sentiment, extract operational issues, and recommend investigation priorities."
2. *Rating-text mismatch* — "This review has 5 stars but says: [negative text]. How would a dashboard flag this?"
3. *Operational trend analysis* — "Here are 10 reviews from one branch over 30 days. What operational trends do you see?"
4. *Ambiguous sentiment* — "Rate your confidence on this review: 'The food was okay but the experience felt strange.' Should it be flagged for human review?"
5. *Sarcasm detection* — "Classify the sentiment of: 'Great job McDonald's. I waited 25 minutes for cold fries.' Explain why a keyword tool would fail."

**CP2 Prompt (structured but unparsed):**
A single-turn prompt asking for a summary and bullet-point action items. Output format was unspecified, leading to inconsistent section headers across API calls. The `_extract_briefing()` function was developed during this phase.

**CP3 Prompt (final production version):**
Added exact header names (`SUMMARY`, `ACTION ITEMS`, `STAFF BRIEFING`) on their own lines to enable reliable regex parsing. Added the evidence citation format (`Evidence: "[quote]" — Action: [directive]`). Added the positive-review contingency rule. Removed all format flexibility to ensure structural consistency across API calls.

---

### Appendix C — Usability Study Materials

#### Participant Consent Script
*See `docs/Evaluation.md` § 2 for the full consent script read aloud before each session.*

#### Pre-Study Questionnaire (5 items)
- P1: Current role / field of study (open text)
- P2: Analytics tool usage frequency (Never / Rarely / Monthly / Weekly / Daily)
- P3: AI tool comfort level (1–5 scale)
- P4: Food service / operations management experience (Yes / No)
- P5: Familiarity with customer reviews as a business tool (1–5 scale)

#### Task Prompts (verbatim)

| Task | Prompt Read to Participant |
|------|---------------------------|
| T1 | "Imagine you're a regional manager. Without changing any filters, look at the dashboard and tell me: which city has the most customer reviews?" |
| T2 | "A senior executive has asked you to flag branches that are underperforming. Find the branch with the lowest average rating and tell me its name and rating." |
| T3 | "Now filter the dashboard to show only reviews from Chicago. Tell me the average star rating for Chicago." |
| T4 | "Switch to the AI Manager Co-Pilot tab. Find a branch that has a 'Needs Attention' status in the health table and select it." |
| T5 | "For the branch you just selected, generate AI insights using the last 90 days of reviews. Tell me the top problem the AI identified." |
| T6 | "The AI has produced a staff briefing memo. Download it as a text file." |
| T7 | "Now change the time range to the last 30 days and regenerate insights. Did the number of reviews change? Generate new insights and tell me if the AI's recommendations are different." |

#### Post-Study Scales
- **SUS:** 10 items, 5-point Likert. Score = Σ(adjusted scores) × 2.5. *(See Evaluation.md §5a)*
- **UMUX-Lite:** 2 items, 7-point Likert. Score = ((U1−1)+(U2−1))/12×100. *(See Evaluation.md §5b)*
- **Trust & Usefulness:** 5 custom items, 5-point Likert. *(See Evaluation.md §5c)*

---

### Appendix D — Key App Screenshots (Descriptions)

*Note: Screenshots are embedded in the CP3 presentation deck (`presentation/MCD_CP3_Presentation.pdf`). Text descriptions are provided here for reference.*

**Screenshot 1 — Review Dashboard (Tab 1)**
Four-chart grid with KPI row above. Top-left: bar chart of 1–5 star rating counts in red/yellow. Top-right: horizontal bar chart of top 10 branches, bars colored yellow (≥4.5 avg) or red. Bottom-left: city volume horizontal bars, top bar highlighted yellow. Bottom-right: bottom 5 branches in red, with a dashed threshold line at 2.5.

**Screenshot 2 — Branch Health Overview Table (Tab 2)**
Full-width Streamlit dataframe showing all branches sorted ascending by Avg Rating. Status column shows 🔴🟡🟢 emoji labels. Trend column shows ⬆️➡️⬇️ arrows. Selected row highlighted in blue.

**Screenshot 3 — AI Co-Pilot Insights Output**
Two-card grid: left card (red header) shows SUMMARY text; right card (yellow header) shows ACTION ITEMS with problem headline, yellow-bordered evidence quote, and red directive arrow. Below: full-width dark-header STAFF BRIEFING card with "Ready to share" badge. Download button beneath.

**Screenshot 4 — Staff Briefing Download**
Yellow "⬇️ Download Staff Briefing (.txt)" button after insight generation, with filename format `briefing_{branch}_{date}.txt`.

---

### Appendix E — SQLite Log Schema

```sql
CREATE TABLE IF NOT EXISTS llm_logs (
    id            INTEGER PRIMARY KEY AUTOINCREMENT,
    timestamp     TEXT    NOT NULL,  -- UTC ISO-8601
    branch        TEXT    NOT NULL,  -- street address of selected branch
    days_range    INTEGER NOT NULL,  -- time window slider value
    num_reviews   INTEGER NOT NULL,  -- reviews sent to Claude (max 20)
    response_text TEXT    NOT NULL   -- full Claude response text
);
```

This schema supports post-hoc analysis of API usage patterns, cost estimation (num_reviews × avg tokens/review × token price), and output quality auditing by branch and time window.
