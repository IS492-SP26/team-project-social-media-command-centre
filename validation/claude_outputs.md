PROMPT #1 - Sentiment + Issue Detection (Typical Case)





You are an operations analyst for McDonald's.



Below are 10 customer reviews from a McDonald's location in Austin, Texas.



Your tasks:



1\. Classify the sentiment of each review (Positive / Negative / Neutral).



2\. Identify the main issue mentioned in each review (food quality, service, wait time, order accuracy, cleanliness, etc.).



3\. Provide a short summary of the top 3 recurring problems customers are experiencing at this location.







Reviews:







1\. "Why does it look like someone spit on my food? I had a normal transaction, everyone was chill and polite, but now I dont want to eat this. Im trying not to think about what this milky white/clear substance is all over my food."



2\. "It'd McDonalds. It is what it is as far as the food and atmosphere go. The staff here does make a difference. They are all friendly, accommodating and always smiling."



3\. "Made a mobile order. Line was not moving so I had to leave otherwise I’d be late for work. Never got the refund in the app."



4\. "My McCrispy chicken sandwich was good and customer service was quick."



5\. "I repeat my order 3 times in the drive thru and she still managed to mess it up."



6\. "I work for DoorDash and they locked us out to wait in a long line for no reason."



7\. "Pulled into drive through at 8:07 and waited 25 minutes for a simple order."



8\. "Ordered a large coffee with no ice and the cup was less than half full."



9\. "Ordered nuggets but they were missing when I got home."



10\. "The staff here is always friendly and always get their orders correct."



Finally:



Based on these reviews, what operational issues should the store manager investigate first?



---



OUTPUT



---



\*\*Review-by-Review Analysis:\*\*



| # | Sentiment | Main Issue |

|---|-----------|------------|

| 1 | Negative | Food quality / potential contamination |

| 2 | Positive | Customer service (staff friendliness) |

| 3 | Negative | Mobile order / refund failure |

| 4 | Positive | Food quality + service speed |

| 5 | Negative | Order accuracy (drive-thru) |

| 6 | Negative | Third-party delivery (DoorDash) access / service policy |

| 7 | Negative | Wait time (drive-thru) |

| 8 | Negative | Order fulfillment / portion accuracy |

| 9 | Negative | Missing items + unresponsive staff |

| 10 | Positive | Order accuracy + staff friendliness |



\*\*Top 3 Recurring Problems:\*\*



1\. \*\*Order Accuracy \& Missing Items\*\* — Reviews 5, 9, and 8 all describe orders that were wrong, incomplete, or not fulfilled as requested. This is the most frequent and operationally concrete complaint pattern.



2\. \*\*Wait Times \& Drive-Thru Inefficiency\*\* — Reviews 3, 6, and 7 all describe excessive wait times or flow breakdowns in the drive-thru and at peak hours. Review 7 reports 25 minutes with no resolution; Review 3 describes a line so unmoving the customer had to leave.



3\. \*\*Food Quality \& Presentation Concerns\*\* — Review 1 describes what appears to be a contamination incident (an unknown substance on the food), and Review 8 describes an underfilled coffee that was not corrected. These suggest inconsistent food handling and quality control.



\*\*What the Store Manager Should Investigate First:\*\*



The highest-priority investigation should be \*\*order accuracy and fulfillment processes in the drive-thru\*\*, since it appears in multiple reviews and represents both a customer experience failure and a potential food safety risk (Review 1). Immediately after, the manager should audit \*\*drive-thru throughput and staffing during peak hours\*\*, and review \*\*mobile order handling and refund workflows\*\*, since customers who lose money through app failures are among the most likely to leave permanent public negative reviews.



---





PROMPT #2 - Rating vs Text Mismatch (Important for Your Project)



You are analyzing customer feedback for McDonald's stores.



Some reviews contain star ratings that do not match the sentiment expressed in the text.



Example review:

Rating: 5 stars

Review text: "The fries were cold and the wait was 30 minutes."





Tasks:



1\. Explain why rating and text sentiment might differ.

2\. Identify the likely true sentiment of the review.

3\. Suggest how a dashboard should flag these cases for store managers.



Then apply this reasoning to the following real reviews:





Review A:

Rating: 4 stars

Text: "The staff here does make a difference. They are friendly and always smiling."





Review B:

Rating: 1 star

Text: "I repeat my order three times in the drive thru and they still mess it up."





Review C:

Rating: 4 stars

Text: "The staff is always friendly and the orders are correct."



Explain your reasoning clearly.



---



OUTPUT



\*\*Why Do Ratings and Text Sentiment Diverge?\*\*



Several psychological and behavioral factors explain this mismatch. Customers sometimes assign a star rating based on habit, loyalty, or overall brand feeling — not the specific experience they just described. Others rate based on expectations: a 5-star reviewer at a fast food chain might have a lower baseline than someone reviewing a fine dining restaurant. Rating fatigue is also common — customers click stars quickly before writing more considered text. Finally, some reviewers intend the rating sarcastically or ironically, especially in informal platforms.



\*\*Applied Analysis:\*\*



\*\*Review A — 4 stars, positive text about friendly staff:\*\*

The rating and text are \*\*aligned\*\*. Both signal a genuinely positive experience focused on service quality. The 4-star rating (rather than 5) likely reflects an implicit acknowledgment that the baseline McDonald's experience is average, but the staff elevates it. True sentiment: \*\*Positive\*\*. No flagging needed; this is a reliable signal.



\*\*Review B — 1 star, text about repeated order errors:\*\*

The rating and text are \*\*aligned\*\*. Both signal a genuinely negative experience centered on order accuracy and staff attentiveness. True sentiment: \*\*Negative\*\*. No mismatch flag needed, but the specific operational failure (repeating order 3 times, still wrong) should be extracted as an actionable issue.



\*\*Review C — 4 stars, text praising friendly staff and correct orders:\*\*

Similar to Review A, this is \*\*aligned\*\* — consistent positive sentiment across rating and text. True sentiment: \*\*Positive\*\*. Worth noting that 4 stars for a review with no complaints may indicate the customer reserves 5 stars for extraordinary experiences. No flag needed.



\*\*Dashboard Flagging Recommendation:\*\*



The dashboard should automatically flag any review where the star rating and the model's predicted sentiment fall into \*\*opposing categories\*\* — for example, a 4 or 5-star rating paired with a predicted negative sentiment score above a set threshold (e.g., 70% confidence negative). These should appear in a dedicated "Mismatch Review Queue" visible to store managers, with the rating, text, and model confidence score all displayed side by side. Human reviewers should make the final call on whether to weight the text or the rating as the operative signal. This is especially important because aggregate star averages used in dashboards will be distorted if high ratings mask negative text.



---



PROMPT #3 - Trend + Operational Decision Support



You are a regional operations manager analyzing McDonald's store performance.



Below are customer reviews from a specific store.



Based on these reviews:



1\. Identify the most common operational problems.

2\. Categorize them into types (food quality, order accuracy, customer service, wait time).

3\. Recommend the top 3 operational improvements the store should prioritize.





Reviews:





\- Food looked contaminated.

\- Mobile order refund issue.

\- Order incorrect in drive thru.

\- Long wait times in drive thru.

\- Missing items in orders.

\- Half-filled coffee.

\- Locked out DoorDash drivers.



Provide a short management summary.



---



OUTPUT



\*\*Issue Categorization:\*\*



| Review Signal | Category |

|---|---|

| Food looked contaminated | Food Quality |

| Mobile order refund issue | Technology / Digital Operations |

| Order incorrect in drive-thru | Order Accuracy |

| Long wait times in drive-thru | Wait Time |

| Missing items in orders | Order Accuracy |

| Half-filled coffee | Order Fulfillment / Food Quality |

| Locked out DoorDash drivers | Third-Party Delivery / Customer Service |



\*\*Most Common Problems by Category:\*\*

\- \*\*Order Accuracy\*\* appears most frequently (incorrect orders, missing items)

\- \*\*Wait Time\*\* is the second most prominent theme

\- \*\*Food Quality\*\* and \*\*Fulfillment\*\* are tied for third



\*\*Management Summary:\*\*



This location is exhibiting a pattern of systemic execution failures concentrated in the drive-thru channel. Order accuracy is the most urgent concern, with multiple reports of incorrect and incomplete orders suggesting either a training gap, a communication breakdown between ordering and prep stations, or both. Drive-thru wait times are also critically elevated, which compounds customer frustration and likely contributes to the order errors (staff rushing under pressure).



\*\*Top 3 Recommended Operational Improvements:\*\*



1\. \*\*Implement a double-check protocol at the drive-thru window\*\* — require staff to verify each bag against the printed receipt before handing it to the customer. This directly addresses order accuracy and missing item complaints.



2\. \*\*Conduct a drive-thru throughput audit during peak hours\*\* — identify the specific bottleneck (ordering, prep, or window handoff) causing the 20–25 minute wait times and adjust staffing or sequencing accordingly.



3\. \*\*Establish a clear third-party delivery pickup process\*\* — DoorDash and similar drivers need a designated workflow (separate queue or window) to prevent blocking or conflict with dine-in and drive-thru customers. Locking them out creates public-facing negative reviews and disrupts delivery SLAs.



---



PROMPT #4 - Ambiguous Sentiment (Failure Case)



Analyze the sentiment of the following review:



"I guess the food was okay but the experience felt strange."



Tasks:



1\. Classify the sentiment (positive, negative, neutral).

2\. Provide a confidence score from 0–100%.

3\. Explain why the sentiment might be ambiguous.



Then explain whether a decision-support dashboard should flag this review for human review.



---



OUTPUT



\*\*Review:\*\* \*"I guess the food was okay but the experience felt strange."\*



\*\*Sentiment Classification:\*\* Neutral (leaning mildly negative)



\*\*Confidence Score:\*\* ~45%



\*\*Why This Review Is Ambiguous:\*\*



This review resists clean classification for several reasons. The phrase "I guess" signals hedging and low conviction — the customer is neither endorsing nor condemning the food, just tolerating it. "Okay" is a weak positive that barely clears neutral. The second clause — "the experience felt strange" — introduces negative affect but is entirely vague: "strange" could refer to atmosphere, staff behavior, an unusual interaction, or something the customer couldn't articulate. There is no specific complaint, no praise, and no clear emotional valence. A keyword-based system might classify "okay" as positive and "strange" as negative and produce a spurious neutral by averaging them, without capturing that the customer seems uneasy in a way they couldn't name.



\*\*Should the Dashboard Flag This for Human Review?\*\*



Yes, and for two reasons. First, the low confidence score (below whatever threshold the system sets, likely 60–70%) means the model itself is uncertain, which is exactly the condition under which human review adds the most value. Second, the vagueness of "experience felt strange" is a signal that warrants investigation — customers who feel something was "off" but can't explain it sometimes represent early signals of a recurring issue (e.g., an uncomfortable interaction with staff, unusual wait dynamics, or food that looked wrong but wasn't bad enough to complain about explicitly). The dashboard should surface this with a tag like \*\*"Low Confidence — Ambiguous Sentiment"\*\* and optionally cluster it with other vague or hedged reviews to look for patterns.



---



PROMPT #5 - Sarcasm / Edge Case





Analyze the sentiment of the following review:



"Great job McDonald's. I waited 25 minutes for cold fries."



Tasks:



1\. Identify whether sarcasm is present.

2\. Classify the real sentiment.

3\. Explain why simple keyword methods might misclassify this review.



---



OUTPUT



\*\*Review:\*\* \*"Great job McDonald's. I waited 25 minutes for cold fries."\*



\*\*1. Is Sarcasm Present?\*\*



Yes, sarcasm is clearly present. The opening "Great job McDonald's" reads as praise but is immediately undermined by the specific, negative details that follow — a 25-minute wait and cold fries. The juxtaposition of effusive congratulation with a complaint is a textbook sarcasm structure: the positive framing is used to amplify rather than soften the negative content.



\*\*2. Real Sentiment:\*\*



Negative. The customer is expressing frustration about both wait time and food quality. The "Great job" functions as mockery, not genuine approval.



\*\*3. Why Simple Keyword Methods Would Fail:\*\*



A lexicon-based system like VADER would likely assign positive weight to "Great job" and potentially treat it as a dominant sentiment signal, producing a neutral or even mildly positive overall score. VADER and similar tools operate on word-level polarity and don't have robust mechanisms to detect when positive phrasing is negated by surrounding context. The model would see "Great," score it positive, then see "cold" and "25 minutes," score those negative, and potentially average them into a misleading neutral. It cannot understand that "Great job" here is not descriptive praise — it's a rhetorical device used to express the opposite. This is exactly the kind of case where transformer-based models trained on informal social media text (like RoBERTa fine-tuned on Twitter/Yelp data) outperform keyword approaches, because they learn contextual relationships across the full sentence rather than scoring words in isolation.



---

