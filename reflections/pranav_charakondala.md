# Literature Reflections – Pranav Charakondala

---

## TweetEval: Unified Benchmark and Comparative Evaluation for Tweet Classification  
Barbieri et al., 2020

### Summary

TweetEval introduces a unified benchmark for evaluating transformer-based models on social media classification tasks, including sentiment analysis. The paper demonstrates that domain-specific pretraining significantly improves performance compared to generic language models. It also standardizes evaluation across multiple datasets, highlighting performance variation depending on domain and task formulation. The results show that models trained on large-scale Twitter corpora are particularly effective for noisy and informal text.

### Insights

1. Domain-specific pretraining is critical for real-world social media sentiment tasks.
2. Benchmark design strongly influences how model performance is interpreted.
3. High accuracy in one dataset does not guarantee robustness across contexts.

### Limitations / Risks

1. Focuses primarily on model accuracy rather than deployment considerations.
2. Does not address uncertainty or interpretability in downstream systems.

### Project Inspiration

This paper supports our decision to use a transformer-based model trained on social media data and reinforces the importance of evaluating performance carefully across different locations and contexts.

---

## On Calibration of Modern Neural Networks  
Guo et al., 2017

### Summary

Guo et al. demonstrate that modern deep neural networks are often poorly calibrated, meaning their predicted probabilities do not reflect true likelihoods. Even highly accurate models can be overconfident when wrong, which is problematic for decision-support applications. The paper introduces temperature scaling as a simple yet effective post-hoc calibration method. Their findings highlight the distinction between accuracy and reliability.

### Insights

1. Miscalibration is common in deep learning systems.
2. Confidence scores should not be interpreted as absolute truth.
3. Simple calibration techniques can significantly improve reliability.

### Limitations / Risks

1. Calibration performance may vary across domains.
2. End users may still misinterpret confidence without proper UX support.

### Project Inspiration

This paper directly motivates our use of confidence thresholding to flag low-certainty sentiment predictions for human review within the Social Media Command Centre.

---

## Guidelines for Human-AI Interaction  
Amershi et al., 2019

### Summary

Amershi et al. present 18 evidence-based guidelines for designing effective human-AI systems. The paper emphasizes transparency, uncertainty communication, user control, and iterative feedback. It argues that AI systems should support human decision-making rather than replace it. The guidelines are grounded in empirical studies across multiple deployed AI products.

### Insights

1. AI systems should communicate uncertainty clearly.
2. Users must remain in control of high-stakes decisions.
3. Transparency improves trust and system adoption.

### Limitations / Risks

1. Guidelines are high-level and require contextual interpretation.
2. Implementation quality depends heavily on interface design.

### Project Inspiration

This work shapes our dashboard design philosophy: the SMCC is structured as a decision-support system where sentiment insights are surfaced transparently, and human managers retain interpretive authority.
