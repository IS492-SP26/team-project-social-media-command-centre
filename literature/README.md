# Literature Review



Our literature review spans four interconnected domains that directly inform the design of the Social Media Command Centre.



## 1. Transformer-Based Sentiment Analysis



RoBERTa (Liu et al., 2019) demonstrates the effectiveness of large-scale pretraining for language understanding tasks. TweetEval (Barbieri et al., 2020) further shows that domain-specific benchmarks significantly improve sentiment performance on noisy social media text. In contrast, VADER (Hutto \& Gilbert, 2014) represents lexicon-based approaches that are interpretable but less robust to informal language.



These works justify our choice of a transformer-based model over rule-based sentiment systems.



## 2. Aspect-Based Sentiment Analysis (ABSA)



Sun et al. (2019) demonstrate how BERT can be adapted for aspect-based sentiment analysis by constructing auxiliary sentences. This is particularly relevant for our system, which analyzes customer feedback across aspects such as service quality, food, and ambiance.



This literature informs our multi-dimensional analysis beyond overall sentiment scores.



## 3. Bias in Online Reviews



Li \& Hitt (2008) identify the J-shaped distribution of product reviews, where extreme ratings dominate. Hu, Pavlou, and Zhang (2009) further examine mechanisms for mitigating bias in online review systems.



These findings inform our interpretation strategy: sentiment signals are treated as relative indicators rather than unbiased population measures.



## 4. Calibration, Human-AI Interaction \& Visualization



Guo et al. (2017) show that modern neural networks are often overconfident, motivating our use of confidence thresholding to flag uncertain predictions. Amershi et al. (2019) provide human-AI interaction guidelines that emphasize transparency and user oversight. Tufte (2001) highlights principles of clear and responsible visual communication, guiding our dashboard design.



Together, this body of work reinforces that high predictive accuracy alone is insufficient. Decision-support systems must communicate uncertainty, acknowledge bias, and support human judgment.



