

\# Prompting Protocol



\## Overview



This project evaluates how large language models can assist with interpreting large volumes of customer feedback for operational decision-making.

We designed a structured prompting protocol to test whether LLMs can extract meaningful insights from McDonald’s customer reviews and support the design of a \*\*Social Media Command Centre (SMCC)\*\*.



The goal of this protocol is not only to test model accuracy but also to examine how LLMs handle:



\* typical sentiment classification tasks

\* operational issue extraction

\* rating–text mismatches

\* ambiguous feedback

\* sarcasm and informal language



These scenarios reflect real challenges in analyzing social media and review platforms.



Three models were tested:



\* \*\*ChatGPT\*\*

\* \*\*Claude\*\*

\* \*\*Gemini\*\*



Each model received identical prompts to ensure consistent comparison.



---



\# Prompt Design Strategy



The prompts were intentionally designed to simulate \*\*real operational decision scenarios\*\* rather than simple NLP benchmarks.



Five prompt types were used:



1\. \*\*Sentiment + issue extraction\*\*

2\. \*\*Rating–text mismatch reasoning\*\*

3\. \*\*Operational trend identification\*\*

4\. \*\*Ambiguous sentiment detection\*\*

5\. \*\*Sarcasm detection\*\*



Together, these prompts evaluate whether LLMs can move beyond simple classification and support \*\*decision-making contexts\*\*.



---



\# Prompt Categories



\## Prompt 1 — Sentiment + Issue Detection (Typical Case)



This prompt tests whether the model can perform the core task of the system: interpreting customer feedback.



The model must:



\* classify sentiment

\* extract operational issues

\* identify recurring problems across reviews

\* recommend investigation priorities



This reflects the main capability required for the SMCC dashboard.



---



\## Prompt 2 — Rating vs Text Mismatch



Online reviews often contain \*\*conflicting signals\*\*, where the star rating does not match the written text.



This prompt tests whether the model can:



\* recognize rating–text inconsistencies

\* infer the true sentiment

\* suggest how a dashboard should flag these cases



Handling these mismatches is important because relying only on star ratings can hide operational problems.



---



\## Prompt 3 — Operational Trend Analysis



This prompt evaluates whether the model can move from individual reviews to \*\*managerial insights\*\*.



The model must:



\* identify common operational issues

\* categorize them into operational domains

\* recommend practical improvements



This simulates how regional managers might use aggregated review data to identify systemic problems.



---



\## Prompt 4 — Ambiguous Sentiment (Failure Case)



Some reviews are vague or mixed in tone.



This prompt tests whether the model can:



\* recognize ambiguity

\* provide a confidence estimate

\* determine whether human review is required



This reflects the need for \*\*uncertainty-aware decision support\*\*, which is a core design principle of the SMCC.



---



\## Prompt 5 — Sarcasm / Edge Case



Sarcasm is a common failure case for simple sentiment tools.



This prompt evaluates whether the model can:



\* detect sarcasm

\* correctly interpret the real sentiment

\* explain why rule-based or keyword methods would fail



This demonstrates the advantage of contextual language models over traditional sentiment analysis tools.



---



\# Evaluation Procedure



Each prompt was executed using three different models:



\* ChatGPT

\* Gemini

\* Claude



Outputs were saved separately in the repository:



\* `chatgpt\_outputs.md`

\* `gemini\_outputs.md`

\* `claude\_outputs.md`



This structure allows direct comparison of how different models interpret the same inputs.



The outputs were evaluated qualitatively based on:



\* correctness of sentiment interpretation

\* ability to identify operational issues

\* reasoning quality

\* handling of ambiguity and sarcasm

\* usefulness for managerial decision support



---



\# Observations



Across models, several patterns emerged:



\* LLMs consistently performed well on \*\*clear sentiment classification tasks\*\*.

\* Models were generally able to identify \*\*recurring operational issues\*\* such as order accuracy problems and long drive-thru wait times.

\* \*\*Ambiguous and sarcastic reviews\*\* required more reasoning, but models could often explain why these cases are difficult.

\* All models demonstrated the ability to provide \*\*decision-support style summaries\*\*, which aligns with the goals of the SMCC system.



---



\# Implications for the Project



The prompting experiments demonstrate that LLMs can support several critical capabilities required for the Social Media Command Centre:



\* interpreting noisy customer feedback

\* identifying operational patterns

\* explaining uncertainty in ambiguous cases

\* supporting human decision-makers rather than replacing them



These results suggest that generative AI can meaningfully assist organizations in transforming unstructured review data into actionable operational insights.





