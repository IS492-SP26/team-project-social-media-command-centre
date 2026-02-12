# McDonald’s Feedback Analysis: Social Media Command Centre (SMCC)

## Course
Introduction to Generative AI for Human-AI Collaboration  
Spring 2026  

## Overview
This project builds a Social Media Command Centre (SMCC) that transforms large-scale customer reviews into actionable, uncertainty-aware decision support.

Using McDonald’s store reviews, we apply transformer-based sentiment modeling and human-centered analysis to surface operational insights across locations. We also explore how GenAI prompting can generate clear, uncertainty-aware summaries and explanations to support decision-makers.

## The Problem
Organizations collect massive volumes of customer feedback but struggle with:
- Unstructured text at scale
- Overrepresentation of extreme reviews
- Lack of uncertainty communication in sentiment models
- Aggregated dashboards that hide local failures

The challenge is not data collection — it is responsible interpretation.

## Our Approach
The SMCC integrates:
- Transformer-based sentiment classification (RoBERTa)
- Confidence thresholding to flag low-certainty predictions
- Geographic and time-series performance analysis
- Interactive dashboard for operational teams
- GenAI prompting for human-readable summaries and explanations (validated in CP2)

The system supports human oversight rather than automated decision-making.

## Repository Structure
- `/literature/` → Related research and citations  
- `/reflections/` → Individual literature reflections  
- `/proposal/` → Formal CP1 proposal  
- `/presentation/` → Checkpoint slides  
- `/milestones.md` → Project roadmap
