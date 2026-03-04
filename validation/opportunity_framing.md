\# Opportunity Framing



\## Problem with Current Tools



Most existing customer feedback analytics tools rely on \*\*simple sentiment analysis and aggregate ratings\*\* to summarize customer experiences. While these tools are useful for high-level trends, they fail to capture several important aspects of real customer feedback:



1\. \*\*Lack of Operational Context\*\*



Traditional dashboards typically classify reviews only as \*positive, neutral, or negative\*. However, store managers need to understand \*\*what operational issue caused the sentiment\*\*, such as order accuracy problems, long drive-thru wait times, or food quality concerns.



Without extracting operational signals from text, managers cannot easily translate feedback into concrete actions.



---



2\. \*\*Failure to Handle Complex Language\*\*



Customer reviews often contain:



\* sarcasm

\* mixed sentiment

\* vague complaints

\* contextual statements



Keyword-based sentiment tools frequently misclassify these cases.



Example:



> “Great job McDonald's. I waited 25 minutes for cold fries.”



A traditional sentiment model might classify this as \*\*positive\*\* due to the phrase \*“Great job”\*, while the true meaning is strongly negative.



---



3\. \*\*Rating–Text Mismatch\*\*



Many review systems rely heavily on \*\*star ratings\*\*, but ratings do not always reflect the sentiment expressed in the written text.



For example:



\* A review may have \*\*5 stars but describe a bad experience\*\*

\* A review may contain \*\*mixed feedback but a neutral rating\*\*



If dashboards rely only on rating averages, important operational issues may remain hidden.



---



4\. \*\*No Uncertainty Awareness\*\*



Existing tools rarely communicate \*\*model uncertainty\*\*.



Some reviews are inherently ambiguous:



> “The food was okay but the experience felt strange.”



A robust system should recognize uncertainty and \*\*flag these cases for human review\*\* rather than forcing a confident classification.



---



\# Proposed Opportunity



The Social Media Command Centre (SMCC) addresses these limitations by using \*\*LLM-based analysis to transform unstructured reviews into operational insights.\*\*



Key capabilities include:



\### 1. Operational Issue Extraction



Instead of only predicting sentiment, the system identifies \*\*specific operational categories\*\*, such as:



\* food quality

\* order accuracy

\* customer service

\* wait time

\* digital order issues



This allows store managers to quickly identify \*\*actionable operational problems\*\*.



---



\### 2. Context-Aware Language Understanding



Large language models can interpret:



\* sarcasm

\* mixed sentiment

\* conversational language

\* implicit complaints



This improves the reliability of sentiment interpretation compared to keyword-based tools.



---



\### 3. Rating–Text Mismatch Detection



The system compares:



\* predicted text sentiment

\* star rating polarity



When they conflict, the dashboard flags the review for investigation.



This helps managers detect \*\*hidden operational issues masked by star ratings\*\*.



---



\### 4. Uncertainty-Aware Decision Support



Rather than forcing a confident prediction for every review, the system identifies:



\* ambiguous reviews

\* low-confidence classifications

\* unclear operational signals



These cases are \*\*flagged for human review\*\*, ensuring that automated analysis supports—not replaces—human judgment.



---



\# Product Impact



By combining \*\*LLM reasoning with structured operational categorization\*\*, the Social Media Command Centre transforms raw review data into actionable insights.



Compared to traditional sentiment dashboards, this system enables managers to:



\* detect operational problems earlier

\* understand \*why\* customers are dissatisfied

\* identify recurring issues across locations

\* prioritize operational improvements more effectively



Ultimately, this approach turns unstructured customer feedback into a \*\*practical decision-support tool for restaurant operations management.\*\*



