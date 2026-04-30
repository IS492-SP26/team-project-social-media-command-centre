# User Study: McDonald's Social Media Command Centre

**IS492 — Generative AI for Human-AI Collaboration**
**Study Version:** 1.0 | **Date:** April 2026

---

## 1. Study Overview

### Purpose
Evaluate the usability, usefulness, and trustworthiness of the McDonald's Social Media Command Centre — a Streamlit app that combines review analytics with an AI-powered manager co-pilot — with target users (restaurant operations students, business analysts, or fast-food managers).

### Research Questions
1. Can users successfully complete core analytics and AI co-pilot tasks without assistance?
2. How much time do users spend on each task, and where do they get stuck?
3. Do users trust the AI-generated insights enough to act on them?
4. What are the dominant usability frustrations and perceived strengths?

### Participants
- **Target N:** 5–8 participants (sufficient for qualitative saturation; Nielsen's law of diminishing returns)
- **Profile:** MSIM classmates, business/operations students, or anyone who might use a restaurant analytics dashboard in a work context
- **Screener criteria:** Basic computer literacy; no prior exposure to this specific app

### Session Format
- **Mode:** In-person (preferred) or remote via screen share (Zoom/Teams)
- **Duration:** ~20-25 minutes per session
- **Compensation:** None required (course context); optional coffee/snack if in-person

---

## 2. Consent Script

> Read aloud (or share as a written form) before the session begins.

---

"Thank you for agreeing to participate in this study. My name is Ashish and I'm a student at the University of Illinois Urbana-Champaign along wiht my friends Jeet & Pranav working on a class project for IS492.

Today I'm going to ask you to use a web application we built that analyzes McDonald's customer reviews. The session will take about 35–45 minutes.

A few things I want you to know:

**Your participation is voluntary.** You can stop at any time without any consequences.

**We are testing the app, not you.** There are no right or wrong answers. If something is confusing, that's useful feedback for us — it means we need to improve the design.

**We may take notes and, with your permission, record your screen and audio** so we can review what happened later. Recordings will only be seen by our project team and will not be shared publicly.

**Your responses will be kept confidential.** We will report only aggregated or anonymized findings.

Do you have any questions before we begin?

Do you consent to participate and to having your screen recorded? [Wait for verbal yes/no. Do not proceed without consent.]"

---

## 3. Pre-Study Questionnaire

*Administer before showing the app. Collect on paper or a Google Form.*

| # | Question | Scale |
|---|----------|-------|
| P1 | What is your current role / field of study? | Open text |
| P2 | How often do you use data dashboards or analytics tools at work or school? | Never / Rarely / Monthly / Weekly / Daily |
| P3 | How comfortable are you using AI tools (e.g. ChatGPT, Copilot) to help with decisions? | 1 (Not at all) – 5 (Very comfortable) |
| P4 | Have you ever worked in food service, retail, or operations management? | Yes / No |
| P5 | How familiar are you with online customer reviews as a business tool? | 1 (Not at all) – 5 (Very familiar) |

---

## 4. Task Protocol

### Facilitator Instructions
- Open the app at **https://mcd-command-centre.streamlit.app** before the session.
- Use **think-aloud protocol**: ask the participant to narrate what they are doing and why.
  - Prompt: *"Please think out loud as you work — tell me what you're looking at, what you're trying to do, and what you expect to happen."*
- **Do not help** unless the participant is completely stuck for more than 90 seconds. If you must intervene, note it as an assisted completion.
- Start a timer at the beginning of each task. Stop when the participant says they are done or when 3 minutes elapse (mark as incomplete if time runs out).
- Record: ✅ Success | ⚠️ Assisted | ❌ Failed | ⏱️ Time (seconds) | 📝 Observations

---

### Task Set

#### Task 1 — Find the Most-Reviewed City
> *"Imagine you're a regional manager. Without changing any filters, look at the dashboard and tell me: which city has the most customer reviews?"*

- **Success criterion:** Participant correctly identifies the top city from the "Top 10 Cities by Review Count" chart.
- **Expected time:** 30–60 s
- **Common errors:** Looking at wrong chart; trying to filter first.

---

#### Task 2 — Identify the Worst Performing Branch
> *"A senior executive has asked you to flag branches that are underperforming. Find the branch with the lowest average rating and tell me its name and rating."*

- **Success criterion:** Participant reads off the correct branch name and rating from the "Worst Performing Branches" chart.
- **Expected time:** 30–60 s
- **Common errors:** Confusing "worst branch" chart with "top cities" chart.

---

#### Task 3 — Filter the Dashboard to a Single City
> *"Now filter the dashboard to show only reviews from Chicago. Tell me the average star rating for Chicago."*

- **Success criterion:** Participant successfully uses the city multiselect to deselect all cities except Chicago and reads the KPI card.
- **Expected time:** 45–90 s
- **Common errors:** Not knowing how to deselect all cities first; dropdown visibility issues.

---

#### Task 4 — Navigate to the Co-Pilot and Select a Branch
> *"Switch to the AI Manager Co-Pilot tab. Find a branch that has a 'Needs Attention' status in the health table and select it."*

- **Success criterion:** Participant clicks the Co-Pilot tab, locates a 🔴 branch in the health table, and either clicks the row or selects it from the dropdown.
- **Expected time:** 45–90 s
- **Common errors:** Not noticing the health table; not realizing rows are clickable.

---

#### Task 5 — Generate AI Insights
> *"For the branch you just selected, generate AI insights using the last 90 days of reviews. Tell me the top problem the AI identified."*

- **Success criterion:** Participant clicks "Generate Insights" and correctly reads back the first action item from the output.
- **Expected time:** 60–120 s (includes API response time ~5–10 s)
- **Common errors:** Not clicking the button; misreading SUMMARY vs ACTION ITEMS.

---

#### Task 6 — Download the Staff Briefing
> *"The AI has produced a staff briefing memo. Download it as a text file."*

- **Success criterion:** Participant clicks "Download Staff Briefing (.txt)" and the file downloads successfully.
- **Expected time:** 15–30 s
- **Common errors:** Not scrolling down to find the button; confusing it with the insights cards.

---

#### Task 7 — Adjust Time Range and Observe Change
> *"Now change the time range to the last 30 days and regenerate insights. Did the number of reviews change? Generate new insights and tell me if the AI's recommendations are different."*

- **Success criterion:** Participant adjusts the slider, clicks Generate again, and notices/describes any difference in review count or output.
- **Expected time:** 60–120 s
- **Common errors:** Not regenerating after changing the slider; not noticing the review count label.

---

## 5. Post-Task Questionnaires

### 5a — System Usability Scale (SUS)
*Administer after all tasks. 5-point Likert: 1 = Strongly Disagree, 5 = Strongly Agree.*

| # | Statement |
|---|-----------|
| 1 | I think that I would like to use this system frequently. |
| 2 | I found the system unnecessarily complex. |
| 3 | I thought the system was easy to use. |
| 4 | I think that I would need the support of a technical person to be able to use this system. |
| 5 | I found the various functions in this system were well integrated. |
| 6 | I thought there was too much inconsistency in this system. |
| 7 | I would imagine that most people would learn to use this system very quickly. |
| 8 | I found the system very cumbersome to use. |
| 9 | I felt very confident using the system. |
| 10 | I needed to learn a lot of things before I could get going with this system. |

**SUS Scoring:**
- Odd items (1,3,5,7,9): subtract 1 from the response.
- Even items (2,4,6,8,10): subtract the response from 5.
- Sum all 10 adjusted scores × 2.5 → score out of 100.
- Benchmarks: ≥ 85 = Excellent | 71–84 = Good | 50–70 = OK | < 50 = Poor

---

### 5b — UMUX-Lite
*Two items, 7-point Likert: 1 = Strongly Disagree, 7 = Strongly Agree.*

| # | Statement |
|---|-----------|
| U1 | This system's capabilities meet my requirements. |
| U2 | This system is easy to use. |

**UMUX-Lite Score:** `((U1 − 1) + (U2 − 1)) / 12 × 100` → score out of 100. Scores ≥ 70 are considered acceptable.

---

### 5c — Trust & Usefulness (Custom)
*5-point Likert: 1 = Strongly Disagree, 5 = Strongly Agree.*

| # | Statement |
|---|-----------|
| T1 | I trust the AI-generated insights enough to share them with my team. |
| T2 | The AI's action items are grounded in the actual reviews (not made up). |
| T3 | The staff briefing memo is something I could use directly without editing. |
| T4 | The dashboard charts helped me understand branch performance quickly. |
| T5 | I would use this tool in a real restaurant operations role. |

---

### 5d — Post-Study Interview (5 minutes)

Ask at least 3 of the following:

1. *"What was the most useful part of the app for you?"*
2. *"Was there anything you expected the app to do that it didn't?"*
3. *"Did anything about the AI output make you hesitant or unsure? Why?"*
4. *"If you were a branch manager, how would this change your Monday morning routine?"*
5. *"What is the one thing you would change about this app?"*
6. *"On a scale of 1–10, how much do you trust the AI's recommendations? What would increase that trust?"*

---

## 6. Observer Recording Sheet

*One sheet per participant. Fill in during the session.*

```
Participant ID: ________   Date: ________   Facilitator: ________
Mode: In-person / Remote   Think-aloud compliance: High / Medium / Low

PRE-STUDY
  Role/background: ________________________________________________
  Analytics tool experience (P2): _________________________________
  AI comfort (P3, 1–5): ___

TASK LOG
┌──────┬────────┬──────────────┬────────────────────────────────────────┐
│ Task │ Result │ Time (sec)   │ Observations / Errors / Quotes         │
├──────┼────────┼──────────────┼────────────────────────────────────────┤
│  T1  │        │              │                                        │
│  T2  │        │              │                                        │
│  T3  │        │              │                                        │
│  T4  │        │              │                                        │
│  T5  │        │              │                                        │
│  T6  │        │              │                                        │
│  T7  │        │              │                                        │
└──────┴────────┴──────────────┴────────────────────────────────────────┘
Result codes: ✅ Success  ⚠️ Assisted  ❌ Failed

NOTABLE QUOTES
1. ___________________________________________________________________
2. ___________________________________________________________________
3. ___________________________________________________________________

FRUSTRATIONS OBSERVED
___________________________________________________________________

MOMENTS OF DELIGHT / SURPRISE
___________________________________________________________________
```

---

## 7. Analysis Plan

### 7a — Quantitative Metrics

| Metric | How to Calculate | Target |
|--------|-----------------|--------|
| **Task Success Rate** | (✅ successes) / (total tasks × N participants) × 100 | ≥ 80% |
| **Assisted Completion Rate** | (⚠️ assisted) / total attempts × 100 | ≤ 15% |
| **Error Rate** | (❌ failures + ⚠️ assisted) / total attempts × 100 | ≤ 20% |
| **Mean Time on Task** | Average seconds per task across all participants | Per-task baseline |
| **SUS Score** | Per formula above; average across participants | ≥ 71 (Good) |
| **UMUX-Lite Score** | Per formula above; average across participants | ≥ 70 |
| **Trust Score (T1–T5)** | Mean of T1–T5 scaled to 100 | ≥ 60% |

**Basic Stats to Report:** Mean, Median, Min, Max for all timed tasks and scale scores. With N < 10, do not use inferential statistics — report descriptives and patterns only.

---

### 7b — Qualitative Analysis (Thematic)

After all sessions, pool all observer notes and interview transcripts and apply the following steps:

1. **Affinity mapping** — Write each observation/quote on a sticky note (or Miro/FigJam). Group into clusters.
2. **Label themes** — Name each cluster with a short theme label (e.g., "AI Trust Uncertainty", "Chart Readability", "Dropdown Confusion").
3. **Count mentions** — How many participants raised each theme? (frequency ≠ importance, but flags priority)
4. **Prioritize** — Themes mentioned by 3+ participants warrant design changes.

**Seed themes to watch for (based on app design):**
- Discoverability of the Co-Pilot tab
- Trust in AI-generated action items
- Confusion between city filter (Tab 1) and branch filter (Tab 2)
- Row-click interaction on the health table
- Usefulness of the staff briefing memo format
- Time taken for AI response (spinner UX)

---

## 8. Results

*Based on 5 participants. Sessions conducted April 2026.*

### 8a — Participant Summary

| ID | Role | Analytics Exp. (P2) | AI Comfort (P3, 1–5) | Food Svc Exp. |
|----|------|---------------------|----------------------|---------------|
| P1 | MSIM student (Information Management) | Weekly | 4 | No |
| P2 | Business Analytics student | Monthly | 3 | Yes (former shift supervisor) |
| P3 | Data Science student | Daily | 5 | No |
| P4 | MBA student (Operations focus) | Rarely | 2 | Yes (managed a café) |
| P5 | MSIM student (UX track) | Weekly | 4 | No |

---

### 8b — Task Results

| Task | Success % | Assisted % | Failed % | Mean Time (s) | Median Time (s) |
|------|-----------|------------|----------|---------------|-----------------|
| T1 — Most-reviewed city | 100% | 0% | 0% | 32 | 30 |
| T2 — Worst branch | 80% | 20% | 0% | 44 | 38 |
| T3 — Filter to city | 60% | 20% | 20% | 94 | 72 |
| T4 — Co-Pilot + select branch | 60% | 40% | 0% | 97 | 95 |
| T5 — Generate insights | 100% | 0% | 0% | 93 | 90 |
| T6 — Download briefing | 80% | 20% | 0% | 24 | 20 |
| T7 — Adjust time range | 80% | 20% | 0% | 110 | 105 |
| **Overall** | **80%** | **17%** | **3%** | **71** | **72** |

**Per-participant task log:**

| Task | P1 | P2 | P3 | P4 | P5 |
|------|----|----|----|----|-----|
| T1 | ✅ 32s | ✅ 28s | ✅ 25s | ✅ 45s | ✅ 30s |
| T2 | ✅ 38s | ✅ 42s | ✅ 30s | ⚠️ 75s | ✅ 35s |
| T3 | ✅ 68s | ⚠️ 95s | ✅ 55s | ❌ 180s | ✅ 72s |
| T4 | ✅ 88s | ✅ 95s | ✅ 70s | ⚠️ 125s | ⚠️ 105s |
| T5 | ✅ 95s | ✅ 88s | ✅ 82s | ✅ 110s | ✅ 90s |
| T6 | ✅ 18s | ✅ 22s | ✅ 15s | ⚠️ 45s | ✅ 20s |
| T7 | ✅ 105s | ✅ 115s | ✅ 88s | ⚠️ 145s | ✅ 98s |

Result codes: ✅ Success &nbsp; ⚠️ Assisted &nbsp; ❌ Failed

**Notable task observations:**
- **T3 (Filter to city):** P4 (least tech-savvy) could not figure out how to deselect all cities from the multiselect before picking one; marked as failed after 3 minutes. P2 needed a hint about the "clear all" interaction.
- **T4 (Row-click on health table):** Neither P4 nor P5 initially realized the health table rows were clickable. Both tried the dropdown first, then scrolled looking for another control. Prompted with: *"Is there another way to select a branch?"*
- **T6 (Download briefing):** P4 scrolled past the download button after insights loaded and needed pointing to it.

---

### 8c — Scale Scores

**SUS Raw Responses** *(1=Strongly Disagree, 5=Strongly Agree)*

| Item | P1 | P2 | P3 | P4 | P5 |
|------|----|----|----|----|-----|
| 1. Would use frequently | 4 | 4 | 5 | 4 | 4 |
| 2. Unnecessarily complex | 2 | 2 | 2 | 3 | 2 |
| 3. Easy to use | 4 | 4 | 4 | 3 | 4 |
| 4. Need technical support | 1 | 2 | 1 | 3 | 1 |
| 5. Functions well integrated | 4 | 3 | 4 | 3 | 4 |
| 6. Too much inconsistency | 2 | 2 | 2 | 3 | 2 |
| 7. Most people learn quickly | 4 | 4 | 5 | 3 | 4 |
| 8. Very cumbersome | 1 | 2 | 1 | 2 | 2 |
| 9. Felt confident using it | 4 | 4 | 5 | 3 | 4 |
| 10. Needed to learn a lot | 2 | 2 | 1 | 3 | 2 |

**Scale Scores Summary**

| Participant | SUS | UMUX-Lite | Trust (avg T1–T5, scaled to 100%) |
|-------------|-----|-----------|-----------------------------------|
| P1 | 80.0 | 66.7 | 80% |
| P2 | 72.5 | 58.3 | 68% |
| P3 | 90.0 | 83.3 | 92% |
| P4 | 57.5 | 50.0 | 64% |
| P5 | 77.5 | 66.7 | 76% |
| **Mean** | **75.5** | **65.0** | **76%** |
| **Median** | **77.5** | **66.7** | **76%** |
| Min | 57.5 | 50.0 | 64% |
| Max | 90.0 | 83.3 | 92% |

**UMUX-Lite raw responses** *(1–7 scale)*

| Participant | U1 — Capabilities meet my requirements | U2 — Easy to use |
|-------------|---------------------------------------|------------------|
| P1 | 5 | 5 |
| P2 | 5 | 4 |
| P3 | 6 | 6 |
| P4 | 4 | 4 |
| P5 | 5 | 5 |

**Trust items raw responses** *(1–5 scale)*

| Participant | T1 Trust AI insights | T2 Grounded in reviews | T3 Briefing usable directly | T4 Charts helpful | T5 Would use in role |
|-------------|---------------------|----------------------|----------------------------|------------------|----------------------|
| P1 | 4 | 4 | 3 | 5 | 4 |
| P2 | 3 | 3 | 3 | 4 | 4 |
| P3 | 4 | 5 | 4 | 5 | 5 |
| P4 | 3 | 3 | 2 | 4 | 4 |
| P5 | 4 | 4 | 3 | 4 | 4 |
| **Mean** | **3.6** | **3.8** | **3.0** | **4.4** | **4.2** |

---

### 8d — Themes

| Theme | Mentions (out of 5) | Sentiment | Recommended Action |
|-------|--------------------|-----------|--------------------|
| Health table row-click not discoverable | 4/5 | Negative | Add a visible "Click a row to load branch" tooltip or highlight on hover |
| Dashboard charts are clear and informative | 4/5 | Positive | Maintain current chart design |
| City multiselect interaction is non-obvious | 3/5 | Negative | Add placeholder text: "Select cities (default: all)" or a Select All / Clear All button |
| AI wait time causes anxiety | 3/5 | Negative | Improve spinner copy: show "Analysing X reviews with Claude…" with a progress cue |
| AI action items are trustworthy because they quote reviews | 3/5 | Positive | Maintain evidence-quote format in prompt |
| Staff briefing memo needs minor editing before use | 3/5 | Mixed | Add editable text area so users can tweak before downloading |
| Tab structure (Dashboard vs Co-Pilot) is intuitive | 2/5 | Positive | No change needed |
| City filter (Tab 1) vs branch filter (Tab 2) link is unclear | 2/5 | Negative | Add a caption explaining that the city filter also narrows the Co-Pilot branch list |

---

### 8e — Key Quotes

> **P2 (T4 — health table):** *"Oh wait, can I click on this? I wasn't sure if it was just a display table or if it did something. That's actually really cool once you know."*

> **P4 (T3 — city filter):** *"I clicked on Chicago but everything else is still there. I don't know how to remove the others. Do I have to click each one individually?"*

> **P3 (T5 — AI insights):** *"I like that it actually quotes the review in the action item. It feels honest — it's not just making stuff up, it's showing me where it got the idea from."*

> **P1 (post-study interview):** *"If I were a branch manager, I'd run this every Monday before my team huddle. The staff briefing is basically a pre-written talking points doc — I'd just read through it and adapt."*

> **P4 (T1 — trust question):** *"I'd trust the summary, but I'd verify the action items myself before telling my team. I'd want to read a few of the reviews it used."*

> **P5 (post-study interview):** *"The spinner just said 'Analysing reviews with Claude' and then nothing for about 8 seconds. I wasn't sure if it froze. A progress bar or a note about how long it takes would help."*

> **P2 (post-study interview):** *"I'd use this if it was connected to live data — like Google reviews pulling in daily. Right now the dataset feels historical."*

---

### 8f — Summary Findings & Recommendations

**Benchmark comparison:**

| Metric | Result | Target | Status |
|--------|--------|--------|--------|
| Overall task success rate | 80% | ≥ 80% | ✅ Met |
| Assisted completion rate | 17% | ≤ 15% | ⚠️ Just over |
| Error/failure rate | 20% | ≤ 20% | ✅ Met |
| Mean SUS score | 75.5 | ≥ 71 | ✅ Good |
| Mean UMUX-Lite score | 65.0 | ≥ 70 | ⚠️ Below target |
| Mean trust score | 76% | ≥ 60% | ✅ Met |

---

**What worked well:**
- Dashboard charts (rating distribution, top/bottom branches, top cities) were understood quickly and without instruction by all 5 participants.
- AI action items that include quoted evidence from reviews significantly increased perceived trust (mentioned by 3/5 participants).
- The two-tab structure (Dashboard / Co-Pilot) was intuitive once participants were oriented.
- The staff briefing memo format was valued — 4/5 participants said they could see themselves using it in a real operations role with minor edits.

**Top usability issues:**
1. **Health table row-click is not discoverable (4/5 participants)** — Users treat the table as read-only display. No visual affordance signals that rows are interactive.
2. **City multiselect "deselect all" interaction (3/5 participants)** — Users do not know they must first remove existing selections before choosing a single city. The default "all selected" state is confusing.
3. **AI spinner provides no time expectation (3/5 participants)** — 8–10 second API wait without a progress cue caused uncertainty about whether the app had frozen.

**Trust & AI-specific findings:**
- Overall AI trust score of 76% is above the 60% target, but **T3 (staff briefing usable directly) scored lowest (mean 3.0/5)**, indicating users want to review and edit before sharing with staff.
- Participants who had food service experience (P2, P4) were notably more cautious about acting on AI recommendations without reading the underlying reviews — suggesting a desire for a "show me the source reviews" link alongside each action item.
- No participant expressed concern about AI hallucination unprompted, but when asked directly, 3/5 said the evidence quotes were the main reason they trusted the output.

**Recommended design changes (priority order):**
1. **Add row hover highlight + tooltip on health table** — e.g., cursor changes to pointer and a tooltip reads "Click to load in Co-Pilot" to signal interactivity.
2. **Add "Clear all / Select all" buttons to city multiselect** — or change the default to no cities selected with a prompt: "Select one or more cities to filter."
3. **Improve AI spinner copy** — Replace generic spinner with: *"Sending 20 reviews to Claude… this takes about 8–10 seconds."*
4. **Make staff briefing editable before download** — Replace static display card with an editable `st.text_area` pre-filled with the briefing text, so users can tweak before saving.
5. **Link source reviews to action items** — Add a "See reviews" expander under each action item showing the 2–3 reviews the point draws from.
