Research-Backed Enhancement Strategy for McDonald's Sentiment Dashboard
This document outlines core findings from three seminal works in Machine Learning and Data Visualization, providing actionable strategies to improve sentiment analysis and dashboard design.

1. RoBERTa: A Robustly Optimized BERT Pretraining Approach
Authors: Y. Liu et al. (2019)

Core Summary
RoBERTa improves upon the original BERT model by identifying that BERT was significantly undertrained. The authors modified the pretraining process by removing the Next Sentence Prediction (NSP) task, utilizing larger batch sizes, and implementing dynamic masking to change masking patterns across epochs.

Key Insights
Hyperparameter Sensitivity: Model performance scales significantly with training duration and data volume.

Dynamic Masking: Varying masked tokens during training creates more robust linguistic representations.

NSP Redundancy: Removing the NSP objective improves performance on downstream tasks.

Dashboard Application
Upgrade the Engine: Replace standard BERT or VADER models with a fine-tuned RoBERTa-base or RoBERTa-large model.

Handle Slang: Leverage the robust training of RoBERTa to better capture sentiment in informal customer reviews and emojis.

2. Utilizing BERT for Aspect-Based Sentiment Analysis (ABSA)
Authors: Z. Sun et al. (2019)

Core Summary
The researchers propose transforming Aspect-Based Sentiment Analysis (ABSA) into a sentence-pair classification task. The model processes the original review alongside an auxiliary "query" sentence (e.g., "What is the sentiment towards the food?") to focus its attention.

Key Insights
Contextual Targeting: Auxiliary sentences guide the model to focus on specific attributes like price or service.

State-of-the-Art Results: This method outperforms models attempting to extract multiple sentiments simultaneously without guidance.

Dashboard Application
Granular Metrics: Implement a Categorical Heatmap by running reviews against specific pairs:

[Review] + "The French Fries were good."

[Review] + "The Drive-Thru speed was fast."

Identify Pain Points: Separate "Food Quality" scores from "Wait Time" scores to provide actionable insights for store managers.

3. The Visual Display of Quantitative Information
Author: Edward R. Tufte (2001)

Core Summary
A foundational text advocating for graphical excellence. Tufte introduces the Data-Ink Ratio, arguing that every visual element in a chart must communicate specific data, and all "non-data-ink" should be removed.

Key Insights
Minimize Chartjunk: Avoid unnecessary grids, 3D effects, and heavy borders.

Data-Ink Ratio: Prioritize a high ratio where every pixel represents a data point.

Small Multiples: Use a series of similar, small graphs to allow users to compare trends across categories quickly.

Dashboard Application
Redesign for Clarity: Remove branding elements that create visual clutter or distract from the data.

Regional Small Multiples: Instead of one line chart with 20 overlapping lines, use 20 clean sparklines side-by-side to compare different store locations.

Bullet Graphs: Replace "Speedometer" gauges with bullet graphs or bar charts to increase the data-ink ratio.
