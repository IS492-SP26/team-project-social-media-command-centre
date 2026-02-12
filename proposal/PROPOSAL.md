\# Project Proposal: McDonald’s Social Media Command Centre (SMCC)



\## Problem \& Importance



Fast-food chains like McDonald’s operate at massive scale across diverse geographic regions. Public platforms such as Google Maps generate continuous streams of customer feedback describing food quality, service behavior, wait times, and cleanliness. While this data contains rich operational signals, its scale and unstructured format make systematic human interpretation infeasible.



Existing analytics systems frequently reduce feedback to aggregate star ratings or overall sentiment averages. However, research on online reviews (Li \& Hitt, 2008; Hu et al., 2009) shows that ratings are typically J-shaped, meaning extreme experiences are disproportionately represented. As a result, simple averages may mislead decision-makers. Furthermore, modern transformer-based sentiment models, while accurate (Liu et al., 2019; Barbieri et al., 2020), often fail to communicate uncertainty, and deep neural networks are known to be overconfident (Guo et al., 2017).



This creates a human-centered problem: operational managers need reliable, interpretable signals—not just predictions. Our goal is to build a Social Media Command Centre (SMCC) that transforms unstructured review text into calibrated, uncertainty-aware decision support.



---



\## Prior Systems \& Gaps



Sentiment analysis systems range from lexicon-based approaches such as VADER (Hutto \& Gilbert, 2014) to transformer-based models such as RoBERTa (Liu et al., 2019). Benchmark research such as TweetEval (Barbieri et al., 2020) demonstrates that domain-specific pretraining improves performance on social media text. Aspect-based sentiment research (Sun et al., 2019) further shows that sentiment signals can be decomposed into dimensions such as service and product quality.



However, three major gaps remain:



1\. \*\*Calibration Gap:\*\* High accuracy does not imply reliable confidence estimates (Guo et al., 2017).

2\. \*\*Bias Gap:\*\* Online reviews are systematically skewed toward extreme ratings (Li \& Hitt, 2008; Hu et al., 2009).

3\. \*\*Human-AI Gap:\*\* Many systems prioritize automation over transparent decision support (Amershi et al., 2019).



Current dashboards aggregate metrics but rarely communicate uncertainty, bias, or model reliability. As a result, decision-makers may overinterpret noisy signals.



---



\## Proposed Approach



We propose a Social Media Command Centre (SMCC) that integrates modeling, calibration, and human-centered interface design.



The system will include:



\- Transformer-based sentiment classification (RoBERTa)

\- Confidence thresholding to flag low-certainty predictions

\- Aspect-level signal tracking (e.g., service vs. food)

\- Geographic and temporal performance analysis

\- Transparent dashboard visualization guided by human-AI interaction principles (Amershi et al., 2019) and visual clarity principles (Tufte, 2001)



Rather than prescribing actions, the SMCC surfaces interpretable signals such as sentiment shifts, rating–text mismatches, and emerging location-level patterns. The objective is calibrated decision support—not automation.



---



\## Plan for Checkpoint 2 Validation via Prompting



For Checkpoint 2, we will validate the usefulness of the SMCC using structured prompt-based evaluation:



1\. Generate region-level summaries that synthesize recent sentiment trends.

2\. Produce explanations for sentiment-rating inconsistencies.

3\. Generate uncertainty-aware recommendations that prioritize investigation areas.



Outputs will be evaluated on:

\- Factual consistency with dashboard metrics

\- Clarity and interpretability

\- Alignment with uncertainty signaling

\- Usefulness to a hypothetical operations manager



This evaluation ensures that generative components enhance, rather than distort, decision-making.



---



\## Risks \& Mitigation



\*\*Bias Risk:\*\* Online reviews are not representative samples. We mitigate this by emphasizing relative comparisons rather than absolute judgments and explicitly documenting sampling limitations.



\*\*Reliability Risk:\*\* Neural models may produce overconfident predictions. We mitigate this through confidence thresholding and transparency in probability reporting.



\*\*Interpretation Risk:\*\* Users may misinterpret automated summaries. We address this through uncertainty-aware language and interface design grounded in human-AI interaction principles.



\*\*Privacy Risk:\*\* The dataset consists of publicly available reviews. Where possible, identifiable information will be anonymized.



---



\## Expected Contribution



The SMCC demonstrates how large-scale sentiment modeling can be embedded within a calibrated, human-centered decision-support framework. The contribution is not merely predictive performance, but responsible operational intelligence.



