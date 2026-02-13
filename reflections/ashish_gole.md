# Literature Reflection – Ashish Gole

---

## Pontiki et al. (2014) – SemEval-2014 Task 4: Aspect-Based Sentiment Analysis

Pontiki, M., Galanis, D., Papageorgiou, H., Androutsopoulos, I., Pavlopoulos, J., & Manandhar, S. (2014). *SemEval-2014 Task 4: Aspect Based Sentiment Analysis*. Proceedings of the 8th International Workshop on Semantic Evaluation (SemEval 2014), 27–35.

### Summary

This paper introduced a benchmark task for Aspect-Based Sentiment Analysis, shifting sentiment modeling from overall polarity to fine-grained aspect-level sentiment detection. The authors created annotated datasets in the restaurant and laptop domains and defined subtasks such as aspect term extraction, aspect category detection, and polarity classification. They evaluated systems using F1 for extraction tasks and accuracy for polarity classification. Results showed that aspect category detection generally performed better than aspect term extraction, highlighting the complexity of fine-grained sentiment modeling. The work established standardized datasets and evaluation procedures that significantly shaped future ABSA research.

### Insights

1. Sentiment at the sentence level is often misleading because different aspects within the same review can carry opposing polarities.
2. Clear annotation guidelines are critical, especially when distinguishing between explicit aspect terms and implied categories.
3. Domain-specific differences matter, for example restaurant reviews contain more explicit aspect mentions than laptop reviews.

### Limitations / Risks

1. The dataset relies on manual annotation, which introduces subjectivity and potential disagreement in ambiguous cases.
2. The structured benchmark setting does not fully reflect noisy, real-world social media data.

### Concrete Idea for Our Project

Instead of treating McDonald’s reviews as purely overall positive or negative, we can extend our RoBERTa pipeline to detect aspect categories such as SERVICE, PRICE, FOOD QUALITY, and CLEANLINESS. This would transform our dashboard from sentiment tracking to operational diagnostics at the aspect level, especially for regions like Florida where systemic issues were observed.

---

## Sun et al. (2019) – Utilizing BERT for Aspect-Based Sentiment Analysis via Constructing Auxiliary Sentence

Sun, C., Huang, L., & Qiu, X. (2019). *Utilizing BERT for Aspect-Based Sentiment Analysis via Constructing Auxiliary Sentence*. NAACL-HLT 2019, 380–385.

### Summary

This paper proposes converting aspect-based sentiment analysis into a sentence-pair classification task by constructing an auxiliary sentence from the target-aspect pair. Instead of treating ABSA as single-sentence classification, they leverage BERT’s strength in sentence-pair tasks such as QA and NLI. They fine-tune BERT on the transformed input and achieve state-of-the-art results on SentiHood and SemEval-2014 datasets. Their experiments demonstrate that the improvement is not just from using BERT, but from restructuring the task in a way that aligns with pre-training objectives. The approach significantly improves both aspect detection and sentiment classification performance.

### Insights

1. Model performance improves not only from architecture but from how the task is framed relative to pre-training objectives.
2. Sentence-pair classification better leverages contextual modeling capabilities of transformer models.
3. Label-aware reformulation, such as binary yes or no classification, can improve calibration and robustness.

### Limitations / Risks

1. The approach increases computational cost due to expansion of input pairs.
2. It depends heavily on well-defined aspect categories, which may not always be available in open-domain reviews.

### Concrete Idea for Our Project

We selected a Twitter-pretrained RoBERTa model because it performs well on noisy social media text. Inspired by this paper, we could reformulate ambiguous reviews into structured prompts such as “The sentiment about service in this review is positive.” This may improve detection of sarcastic or mixed reviews, especially the 11 percent anomaly we observed in 5-star reviews with non-positive text.

---

## Hutto & Gilbert (2014) – VADER: A Parsimonious Rule-Based Model for Sentiment Analysis of Social Media Text

Hutto, C. J., & Gilbert, E. (2014). *VADER: A Parsimonious Rule-based Model for Sentiment Analysis of Social Media Text*. Proceedings of ICWSM 2014.

### Summary

This paper presents VADER, a rule-based sentiment analysis model specifically designed for social media text. It combines a human-validated sentiment lexicon with grammatical and syntactic heuristics such as capitalization, degree modifiers, punctuation, and negation handling. Unlike purely lexicon-based approaches, VADER incorporates intensity adjustments and contextual cues to better capture sentiment strength. The model achieves strong performance across multiple domains while remaining computationally efficient. The authors emphasize interpretability and transparency compared to black-box machine learning models.

### Insights

1. Sentiment intensity is influenced by punctuation, capitalization, and degree modifiers, not just word polarity.
2. Lightweight rule-based systems can remain competitive across domains if carefully engineered.
3. Interpretability is a key advantage when models are deployed in business environments.

### Limitations / Risks

1. Rule-based systems struggle with sarcasm and implicit sentiment.
2. Lexicon coverage may not generalize well to evolving language or regional slang.

### Concrete Idea for Our Project

We compared RoBERTa against VADER and found RoBERTa performed better on noisy reviews. However, VADER’s interpretability suggests an opportunity to combine both approaches. We could integrate VADER-style intensity heuristics into our dashboard to explain why a review is classified as highly negative, increasing executive trust in our sentiment tracking system.
