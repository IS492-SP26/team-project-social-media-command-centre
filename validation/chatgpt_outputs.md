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



\# Sentiment \& Issue Classification



| Review | Sentiment | Main Issue                                  |

| ------ | --------- | ------------------------------------------- |

| 1      | Negative  | Food quality / possible contamination       |

| 2      | Positive  | Good customer service                       |

| 3      | Negative  | Refund / mobile order issue                 |

| 4      | Positive  | Food quality + quick service                |

| 5      | Negative  | Order accuracy                              |

| 6      | Negative  | Delivery partner handling / service process |

| 7      | Negative  | Long wait time                              |

| 8      | Negative  | Incorrect portion / product quality         |

| 9      | Negative  | Missing order items                         |

| 10     | Positive  | Friendly staff / order accuracy             |



---



\# Top Operational Issues Identified



\### 1️⃣ Order Accuracy Problems



Multiple reviews report incorrect or missing items:



\* Drive-thru order mistakes

\* Missing nuggets

\* Incorrect drinks/portions



This suggests issues in \*\*order preparation or verification before handing food to customers\*\*.



---



\### 2️⃣ Long Wait Times / Operational Inefficiency



Several reviews indicate delays:



\* Drive-thru waiting \*\*25 minutes\*\*

\* Mobile orders \*\*not being processed quickly\*\*

\* Delivery drivers waiting in long lines



This may indicate:



\* understaffing

\* poor queue management

\* inefficient drive-thru workflow



---



\### 3️⃣ Food Quality Concerns



Some customers mention:



\* possible contamination

\* half-filled drinks

\* inconsistent food quality



Food safety perceptions can seriously damage customer trust and require immediate attention.



---



\# Recommended Areas for Store Manager Investigation



\### Priority 1 — Food Safety \& Quality Control



The review mentioning possible contamination is serious and should be investigated immediately. Staff training and food handling procedures should be reviewed.



---



\### Priority 2 — Drive-Thru \& Order Fulfillment Process



Repeated complaints about wrong or missing items suggest problems in order preparation or handoff. Implementing order verification steps may help reduce mistakes.



---



\### Priority 3 — Service Speed \& Queue Management



Long wait times indicate operational bottlenecks. The store should examine staffing levels, drive-thru workflow, and mobile order processing efficiency.



---



\# Short Management Summary



Customer feedback indicates \*\*three primary operational challenges\*\* at this location: order accuracy problems, long drive-thru wait times, and occasional food quality concerns. While several reviews praise friendly staff, operational execution issues—particularly incorrect orders and slow service—are negatively impacting customer experience. Addressing these workflow and quality control issues should be the store’s immediate priority.



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



\### Output



\#### 1) Why rating and text sentiment might differ



Star ratings and written text can diverge for several common reasons:



\* \*\*Overall vs. specific experience:\*\* A customer may rate the \*overall visit\* positively (or negatively) but mention one specific issue in the text.

\* \*\*Different weighting of factors:\*\* Some people care more about service friendliness than food quality (or vice versa), so the rating reflects what they value most.

\* \*\*Habitual / default ratings:\*\* Some users give 4–5 stars by default unless something was extremely bad, even if they complain in the text.

\* \*\*Mixed sentiment:\*\* The text may contain both positive and negative points; the star rating becomes an “average” in the customer’s mind.

\* \*\*Timing / context:\*\* The review text might describe one incident, but the rating could be influenced by past experiences at the same location.



---



\#### 2) Example review (5 stars, negative text)



\*\*Rating:\*\* 5 stars

\*\*Text:\*\* “The fries were cold and the wait was 30 minutes.”



\* \*\*Likely true sentiment (text):\*\* \*\*Negative\*\*

\* \*\*Why mismatch:\*\* The rating does not align with the complaint severity. Possible explanations: the reviewer clicked 5 stars accidentally, is rating based on something else (e.g., friendly staff), or has a personal bias toward giving high ratings.

\* \*\*How to interpret:\*\* Treat this as a \*\*high-risk mismatch\*\* because it could hide serious operational issues if you rely only on star averages.



---



\#### 3) Apply to real reviews



\##### Review A



\*\*Rating:\*\* 4 stars

\*\*Text:\*\* “The staff here does make a difference. They are friendly and always smiling.”



\* \*\*Text sentiment:\*\* \*\*Positive\*\*

\* \*\*Mismatch?\*\* \*\*No\*\* (rating and text both indicate a good experience)

\* \*\*Interpretation:\*\* Positive signal for customer service quality.



\##### Review B



\*\*Rating:\*\* 1 star

\*\*Text:\*\* “I repeat my order three times in the drive thru and they still mess it up.”



\* \*\*Text sentiment:\*\* \*\*Negative\*\*

\* \*\*Mismatch?\*\* \*\*No\*\* (rating and text strongly align)

\* \*\*Interpretation:\*\* Clear operational issue: \*\*order accuracy + drive-thru communication\*\*.



\##### Review C



\*\*Rating:\*\* 4 stars

\*\*Text:\*\* “The staff is always friendly and the orders are correct.”



\* \*\*Text sentiment:\*\* \*\*Positive\*\*

\* \*\*Mismatch?\*\* \*\*No\*\* (aligned)

\* \*\*Interpretation:\*\* Consistent positive signal for service and order accuracy.



---



\#### 4) Dashboard recommendation: how to flag mismatches



A dashboard should automatically detect and surface \*\*rating–text inconsistencies\*\* using a simple rule:



\* Compute \*\*text sentiment score\*\* (LLM/transformer prediction)

\* Compare against \*\*star rating polarity\*\*



&nbsp; \* 1–2 stars → expected negative

&nbsp; \* 3 stars → expected neutral/mixed

&nbsp; \* 4–5 stars → expected positive



\*\*Flag cases when:\*\*



\* 4–5 stars + negative text sentiment

\* 1–2 stars + positive text sentiment

\* High confidence mismatch (model is confident the text is opposite of the rating)



\*\*UI suggestion for managers:\*\*



\* Add a “⚠ Rating–Text Mismatch” tag

\* Show the review in a “Needs Review” queue

\* Aggregate mismatch counts by store to identify locations where star ratings may be misleading



This helps managers avoid relying only on star averages and instead catch hidden operational problems.



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



\### Output



\#### 1) Most Common Operational Problems



From the provided reviews, several recurring operational issues can be identified:



\* \*\*Order fulfillment errors\*\* – incorrect or missing items in orders.

\* \*\*Service delays\*\* – long wait times in the drive-thru.

\* \*\*Product quality concerns\*\* – contaminated-looking food or improperly prepared items.

\* \*\*Process issues with digital orders and delivery partners\*\* – problems with mobile refunds and handling DoorDash drivers.



These issues suggest that the store may be experiencing \*\*workflow inefficiencies and quality control problems\*\* during order preparation and service.



---



\#### 2) Categorization of Issues



| Issue                         | Category                                  |

| ----------------------------- | ----------------------------------------- |

| Food looked contaminated      | Food Quality                              |

| Mobile order refund issue     | Customer Service / Digital Order Handling |

| Order incorrect in drive thru | Order Accuracy                            |

| Long wait times in drive thru | Wait Time / Operational Efficiency        |

| Missing items in orders       | Order Accuracy                            |

| Half-filled coffee            | Food Quality / Product Preparation        |

| Locked out DoorDash drivers   | Customer Service / Delivery Process       |



---



\#### 3) Top 3 Operational Improvements



\*\*1. Improve Order Verification Processes\*\*

Introduce a quick order-check step before handing food to customers to reduce missing items and incorrect orders.



\*\*2. Optimize Drive-Thru Operations\*\*

Evaluate staffing levels and workflow during peak hours to reduce wait times and improve service speed.



\*\*3. Strengthen Quality Control During Food Preparation\*\*

Ensure staff follow preparation standards for portion sizes and food handling to avoid complaints about food quality or incomplete beverages.



---



\### Management Summary



Customer feedback indicates several operational challenges at this McDonald’s location. The most common problems relate to \*\*order accuracy, slow drive-thru service, and inconsistent product preparation\*\*. Additionally, issues with \*\*mobile order refunds and delivery driver handling\*\* suggest gaps in digital order management. Addressing order verification procedures, improving drive-thru workflow efficiency, and reinforcing food preparation standards should significantly improve customer satisfaction and operational performance at this store.





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



\### Output



\#### 1) Sentiment Classification



\*\*Sentiment:\*\* Neutral (mixed)



\#### 2) Confidence Score



\*\*Confidence:\*\* 55%



\#### 3) Why the Sentiment is Ambiguous



The review contains \*\*mixed signals\*\*:



\* \*\*Mildly positive phrase:\*\* “the food was okay” suggests the food quality was acceptable.

\* \*\*Negative/uncertain phrase:\*\* “the experience felt strange” implies something about the visit was uncomfortable or unusual.



Because the statement does not clearly describe a specific issue or strong opinion, the overall sentiment falls between positive and negative. The language is vague and subjective, which makes automated classification less reliable.



\#### 4) Should the Dashboard Flag This Review?



\*\*Yes, it should be flagged for human review.\*\*



A decision-support dashboard should flag reviews when:



\* The sentiment model has \*\*low confidence\*\*

\* The review contains \*\*mixed or ambiguous language\*\*

\* The issue is \*\*unclear or not easily categorized\*\*



Flagging such reviews allows store managers or analysts to manually interpret the feedback and determine whether it reflects a minor comment, an unusual experience, or a potential operational issue that automated systems cannot clearly classify.



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



\### Output



\#### 1) Is sarcasm present?



\*\*Yes.\*\*

The phrase \*\*“Great job McDonald’s”\*\* appears positive, but the second sentence describes a negative experience: \*\*waiting 25 minutes and receiving cold fries\*\*. The positive phrase is likely used sarcastically to emphasize dissatisfaction.



---



\#### 2) Real Sentiment



\*\*Negative\*\*



Although the first phrase sounds positive, the overall meaning of the review clearly expresses frustration with \*\*long wait times and poor food quality\*\*.



---



\#### 3) Why Simple Keyword Methods Might Misclassify It



Basic sentiment analysis methods such as \*\*bag-of-words or lexicon-based approaches\*\* rely heavily on counting positive or negative words. In this review:



\* The phrase \*\*“Great job”\*\* is strongly positive in many sentiment dictionaries.

\* Keyword-based models may assign a \*\*positive score\*\* because of this phrase.

\* These models typically \*\*do not understand sarcasm, tone, or context\*\*.



More advanced models (e.g., transformer-based models) consider the \*\*relationship between sentences and overall context\*\*, allowing them to detect that the positive phrase is actually used sarcastically to describe a negative experience.



---



\#### Implication for Decision Support Systems



Sarcastic reviews should be \*\*flagged carefully\*\*, because simple sentiment systems may misinterpret them and produce misleading insights in customer feedback dashboards.



---





