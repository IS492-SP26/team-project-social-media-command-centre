# Speed Dating Interviews for Gap Analysis  
## Chicago McDonald’s Store Scenario

This README documents three speed dating interviews conducted to identify where current customer feedback and monitoring tools fail or underperform across accuracy, reliability, latency, UX friction, safety, and costs. The setting is a McDonald’s location in Chicago with high foot traffic, frequent mobile orders, and delivery pickups.

---

## 1. Objective

Identify gaps in existing tools that rely on sentiment and star ratings and propose opportunities for improvement using context aware understanding, operational issue extraction, mismatch detection, and uncertainty handling.

---

## 2. Interview Setup

### Location Context
Chicago McDonald’s store environment with:
- Lunch rush spikes
- High mobile order volume
- Delivery driver congestion near pickup area
- Frequent customer feedback in short, informal language

### Method
Three speed dating interviews with three unique user types:
1. Store Shift Manager
2. Frequent Mobile Order Customer
3. Delivery Driver

Each interview focused on real workflows and pain points, and each response was analyzed for tool failures and improvement opportunities.

---

## 3. Interview 1: Store Shift Manager (Chicago Loop, lunch rush)

### Scenario
Managing peak hour operations with walk in traffic, mobile orders, and delivery pickups.

### Questions and captured answers

**Q1. When you see negative reviews, what key context is missing that would explain what really happened operationally?**  
**Answer:** Reviews usually do not show staffing gaps, equipment issues, sudden delivery surges, register slowdowns, or kitchen bottlenecks. Without context, a complaint looks like poor service even when it is a volume spike or an outage.

**Q2. Have you seen reviews where the star rating and the written text do not match? What do you do with those today?**  
**Answer:** Yes. Examples include 1 star with “food was fine but took long” or 5 stars with “forgot my fries.” There is no system that detects mismatch, so these become noise instead of actionable signals.

**Q3. What kind of language causes tools or people to misinterpret the real issue?**  
**Answer:** Sarcasm like “amazing speed, only waited forever” and mixed statements like “staff was nice but order was wrong.” Tools that look for positive words can miss the operational failure.

**Q4. If a tool had to classify issues, what operational issue categories would be most useful?**  
**Answer:** Wait time, order accuracy, missing items, delivery handoff congestion, app pickup readiness mismatch, staffing shortage, equipment outage, lobby crowding.

**Q5. How should a tool handle uncertainty when a review is vague or ambiguous?**  
**Answer:** It should not force a confident label. It should flag the review as ambiguous and avoid driving the team toward false causes.

**Q6. Which current approach fails most and why?**  
**Answer:** Rating only fails by hiding the actual problem type. Keyword tools fail on sarcasm and phrasing. Sentiment dashboards fail by ignoring operational context. Traditional ML fails when it gives confident labels without uncertainty.

### Key takeaways
- Operational context is essential for correct interpretation
- Rating and text mismatch is frequent and currently unmanaged
- Uncertainty handling is required to reduce false prioritization

---

## 4. Interview 2: Frequent Mobile Order Customer (Downtown commuter)

### Scenario
Orders ahead and expects accurate pickup timing during commuting hours.

### Questions and captured answers

**Q1. What makes you leave a low rating even if the food quality is fine?**  
**Answer:** The pickup timing feels untrustworthy. If the app says ready but it is not, it breaks the promise and drives a low rating.

**Q2. Do you ever give a rating that does not match your written text? Why?**  
**Answer:** Yes. Ratings can reflect frustration while text reflects details. Sometimes a customer gives a low rating for delay but still writes that staff were polite.

**Q3. What kind of review text would a basic sentiment tool misunderstand?**  
**Answer:** Mixed feedback like “friendly staff, but the wait was ridiculous.” Positive words can hide the real complaint category.

**Q4. If you could choose one operational label for your complaint, what would it be most often?**  
**Answer:** Pickup readiness inaccurate or unexpected wait time.

**Q5. When your feedback is vague, what do you mean by it and what should a tool do?**  
**Answer:** Vague terms like “chaotic” often mean unclear queue, crowded pickup area, and no reliable estimate. The tool should mark uncertainty and connect it to likely operational themes rather than guessing.

**Q6. What would feel like a real improvement, not just a nicer dashboard?**  
**Answer:** Realistic ETA, visible reason for delay, and fewer repeated issues through actual operational changes.

### Key takeaways
- Ratings are emotional, text contains actionable detail
- Customers want transparency and realistic ETAs
- Vague feedback needs uncertainty handling and better interpretation

---

## 5. Interview 3: Delivery Driver (Third party pickup, River North corridor)

### Scenario
Relies on readiness accuracy and fast pickup to maximize earnings.

### Questions and captured answers

**Q1. What is the biggest hidden operational issue customers do not fully see?**  
**Answer:** Delivery pickup congestion and handoff process confusion. Crowding increases tension and slows operations.

**Q2. Have you seen sarcasm or venting that a tool might misclassify?**  
**Answer:** Yes. Sarcastic lines like “love waiting forever” can be misread as positive by keyword based tools.

**Q3. Do you see rating and text mismatches? Why does it happen?**  
**Answer:** Yes. Customers rate based on overall mood but describe specific failures in text, like delays or missing items.

**Q4. If the system had to extract operational issues, what are the top three that matter most to you?**  
**Answer:** True readiness status, accurate wait estimates, clear pickup queue management.

**Q5. How should a system treat ambiguous feedback like “slow today” without details?**  
**Answer:** It should not guess. It should mark uncertainty and correlate with operational signals like volume spikes, staffing, or equipment issues.

**Q6. What is the cost impact when tools misclassify issues or hide context?**  
**Answer:** The same problems repeat, drivers lose earnings, staff stress increases, and customers blame the wrong thing. Misclassification increases cost and conflict.

### Key takeaways
- Readiness truth and queue clarity are critical operational signals
- Sarcasm and venting create misclassification risk
- There is a direct cost impact from incorrect interpretation

---

## 6. Cross Interview Conclusions

### Theme A: Existing tools rely on sentiment and star ratings and miss operational context
Evidence:
- Shift manager needs staffing, surge, and equipment context
- Customer ratings reflect broken promises, not always food quality
- Driver highlights congestion and readiness truth as root causes

Conclusion:
Current tools treat feedback as emotion signals rather than operational signals. Operational issue extraction is needed.

### Theme B: Tools fail on sarcasm, mixed sentiment, and vague feedback leading to misclassification
Evidence:
- Sarcasm: “love waiting forever”
- Mixed sentiment: “staff was nice but order was wrong”
- Vague feedback: “chaotic” and “slow today”

Conclusion:
Keyword and basic sentiment approaches misread meaning and create false conclusions.

### Theme C: Rating and text mismatches hide real issues and create undetected problems
Evidence:
- 1 star with “food fine but took long”
- Higher star ratings with missing item complaints
- Ratings reflect mood while text carries the actionable issue

Conclusion:
Rating based systems fail to surface root cause categories without text interpretation.

### Theme D: No uncertainty awareness leads to wrong labels or ignored feedback
Evidence:
- Manager wants ambiguous reviews flagged, not forced
- Customer uses vague language that needs structured interpretation support
- Driver wants correlation with operational signals before labeling

Conclusion:
Uncertainty scoring and an ambiguity pathway are essential to reduce confident wrong outputs.

---

## 7. Gap Analysis and Opportunity Mapping

### Gap 1: Sentiment dashboards miss operational context
- Limitation: Only positive, neutral, negative
- Opportunity: Extract operational issues such as wait time, missing items, order accuracy, pickup readiness mismatch, delivery congestion

### Gap 2: Keyword based tools misinterpret language
- Limitation: Fails on sarcasm, mixed sentiment, phrasing variation
- Opportunity: Context aware LLM understanding that interprets intent and operational meaning

### Gap 3: Rating based systems hide issues through mismatch
- Limitation: Star rating does not align with text, causing hidden issues
- Opportunity: Detect rating and text mismatch and route to the true issue category

### Gap 4: Traditional ML lacks uncertainty awareness
- Limitation: Confident labels even when evidence is weak
- Opportunity: Add uncertainty scoring, flag ambiguous reviews, and trigger follow up or correlation checks

---

## 8. What We Should Validate Next

1. Define a consistent operational taxonomy for issue types for this store context  
2. Add mismatch detection between rating and text and measure how often it occurs  
3. Implement uncertainty scoring and evaluate reduction in false positives  
4. Test sarcasm and mixed sentiment handling on real Chicago area review samples  
5. Explore correlation of review clusters with operational signals such as order volume, staffing levels, and equipment status

---

## 9. Final Summary

Across three user types, the strongest recurring gaps are:
- Missing operational context
- Misclassification due to sarcasm, mixed sentiment, and vague text
- Rating and text mismatch hiding true root causes
- No uncertainty awareness causing confident wrong outputs
- Direct cost and friction impacts for customers, staff, and delivery drivers

The primary opportunity is a context aware system that extracts operational issues, detects mismatch patterns, and handles uncertainty safely.
