---
title: "MentalManip: A Dataset For Fine-grained Analysis of Mental Manipulation in Conversations"
authors: [Yuxin Wang, Ivory Yang, Saeed Hassanpour, Soroush Vosoughi]
publication_date: 2024-08-11
conference: "Proceedings of the 62nd Annual Meeting of the Association for Computational Linguistics (Volume 1: Long Papers)"
pages: 3747–3764
tags: [research_paper, NLP, mental_manipulation, dataset, conversation_analysis, LLM_evaluation, toxicity_detection]
link_to_paper: # Add link to actual paper if available
github: https://github.com/audreycs/MentalManip
---

# MentalManip: Research Overview

## 1. Abstract Summary

Mental manipulation in interpersonal conversations is challenging to identify due to its context-dependent and subtle nature. Current Natural Language Processing (NLP) resources for detecting such manipulation are scarce. This paper introduces **MENTALMANIP**, a new dataset of 4,000 annotated fictional dialogues designed for comprehensive analysis of mental manipulation. The dataset identifies manipulation techniques and victim vulnerabilities. Experiments with leading-edge models show they inadequately identify and categorize manipulative content, even after fine-tuning with existing mental health and toxicity datasets. MENTALMANIP aims to spur further research in understanding and mitigating mental manipulation.

## 2. Introduction

### Problem Definition
*   **Mental manipulation**: A deceptive strategy to control or alter thoughts/feelings for personal objectives (Barnhill, 2014).
*   **Aggravation**: Digital technologies have increased the capacity to target and influence individuals, causing mental health distress (Ienca, 2023; Hamel et al., 2023).
*   **Challenge**: Manipulation is more insidious, deceitful, and context-dependent than overt verbal toxicity (e.g., hate speech), making it hard for individuals and automated tools to discern.
*   **Gap**: Existing NLP works focus on context-free or direct toxicity, insufficient for verbal mental manipulation. State-of-the-art Large Language Models (LLMs) also struggle (see Figure 1 example with GPT-4).

### Contribution
*   Introduction of **MENTALMANIP**: A dataset with multi-level annotations for mental manipulation detection and classification.
*   **Definition Used**: "Using language to influence, alter, or control an individual’s psychological state or perception for the manipulator’s benefit" (aligns with Barnhill, 2022).
*   **Dataset Details**: 4,000 multi-turn fictional dialogues from online movie scripts.
*   **Taxonomy**: Covers three dimensions:
    1.  Presence of manipulation
    2.  Manipulation technique (11 types)
    3.  Targeted vulnerability (5 types)
    (Illustrated in Figure 2; definitions in Appendix A).
*   **Goal**: Facilitate precise detection and nuanced classification.

### Experiments & Findings Preview
*   Conducted three classification tasks with SOTA generative and discriminative LLMs.
*   Fine-tuning with seven relevant datasets on toxic speech and mental health did not significantly improve performance.
*   Highlights the importance of MENTALMANIP for future studies.

## 3. Related Works

### 3.1. Mental Health Detection
*   NLP used for early detection of stress, depression, suicide.
*   Data resources exist (e.g., Dreaddit, SDCNL).
*   LLMs show promising but limited performance; lack explainability and need domain-specific fine-tuning.
*   Underscores need for nuanced annotations and addressing unaddressed mental health issues like manipulation.

### 3.2. Toxic Speech Detection
*   "Toxic speech": Rude, disrespectful, offensive language.
*   Benchmark datasets exist for explicit/implicit toxic speech (e.g., ToxiGen, DetexD).
*   Mental manipulation's subtlety is beyond context-free methods.
*   Context-aware dialogue systems (e.g., TalkDown, ToxiChat, MDRDC) focus on explicit toxicity, overlooking implicit manipulation.
*   **Table 1**: Compares MENTALMANIP with existing datasets.

## 4. Constructing MENTALMANIP

### 4.1. Taxonomy (Figure 2 & Appendix A)
*   Inspired by Simon’s research on psychological manipulation (Simon and Foley, 2011).
*   Multi-level:
    1.  **Presence of Manipulation**: Binary (Yes/No).
    2.  **Manipulation Technique**: Identifies specific techniques (11 options, e.g., Denial, Evasion, Shaming).
    3.  **Targeted Vulnerability**: Indicates victim vulnerabilities exploited (5 options, e.g., Naivete, Dependency).
*   Refined with expert psychological input and annotator feedback.
*   *See Appendix A for detailed definitions of each technique and vulnerability.*

### 4.2. Data Source and Preprocessing
*   **Source**: Cornell Movie Dialogs Corpus (Danescu-Niculescu-Mizil and Lee, 2011) - chosen for being human-crafted, semantically rich, stylistically diverse. Contains 220,579 exchanges from 617 movie scripts.
*   **Standardization**: Dialogues between two characters; speakers anonymized to "Person1" and "Person2".
*   **Filtering for Potential Manipulation**:
    1.  **Key phrase-based matching**:
        *   175 cleaned key phrases (sourced online, manually refined). Examples in Appendix B.
        *   Length-adaptive matching criterion (Table 2).
    2.  **BERT classification**:
        *   Fine-tuned BERT to differentiate manipulative from general toxic content.
        *   Training data: Dialogues flagged as "manipulative" by GPT-4 Turbo (zero-shot), then manually reviewed (464 true positives, 914 false positives from 1,378 flagged).
*   **Data Cleaning**:
    *   Initial: 1,406 (key phrase) + 3,739 (BERT) = 5,145 dialogues.
    *   After removing duplicates, low-quality dialogues, rephrasing for readability: 4,876 dialogues for annotation.

### 4.3. Human Annotation
*   **Platform**: Label Studio.
*   **Annotators**: 17 college students (native/fluent English, diverse backgrounds, preference for psychology/linguistics backgrounds).
*   **Process**: Tutorial sessions, written instructions, 3 annotators per dialogue (task).
*   **Annotation Task (per dialogue)**:
    *   Q1: Manipulation present? (Yes/No)
    *   Q2: Techniques used? (Multiple choice, max 3 from Figure 2, "cannot decide" option)
    *   Q3: Victims present? (Yes/No)
    *   Q4: Vulnerabilities targeted? (Multiple choice, max 2 from Figure 2, "cannot decide" option)
*   Annotators rated confidence (1-5) and could highlight manipulative sections.
*   **Outcome**: >13,000 annotations. 4,000 well-labeled dialogues after quality review.
*   **Inter-Annotator Agreement**: Fleiss’ Kappa = 0.596 (moderate agreement), expected due to subjectivity.
*   *See Appendix D for detailed annotation quality statistics and Appendix H for an example.*

### 4.4. Final Label Generation
*   Two strategies for final labels based on Q1 (Presence of Manipulation):
    1.  **Consensus agreement (MENTALMANIPcon)**: All three annotators agree.
    2.  **Majority agreement (MENTALMANIPmaj)**: Majority rule.
*   For techniques and vulnerabilities (on both versions): Included if annotated by at least two annotators for a task. (Some dialogues may lack these labels).

### 4.5. Dataset Statistics (Table 3, Figures 3, 4, 5, 6)
*   **MENTALMANIPcon & MENTALMANIPmaj**:
    *   Ratio of manipulative:non-manipulative is approx. 2.2-2.4 : 1.
    *   ~54-60% dialogues labeled with techniques, ~18-21% with vulnerabilities (Table 3).
*   **Technique/Vulnerability Distribution (Figure 3 left/center)**: Strong alignment between MENTALMANIPcon and MENTALMANIPmaj.
    *   Most common techniques: Persuasion/Seduction (P_S), Accusation (ACC), Intimidation (INT).
    *   Most common vulnerabilities: Over-responsibility (O_R), Naivete (NAT).
*   **Emotion Distribution (Figure 3 right - MENTALMANIPcon)**: Similar emotional profiles ("joy," "anger" common) for manipulative and non-manipulative dialogues. (MENTALMANIPmaj similar - Appendix E).
*   **Co-occurrence (Figure 4 - MENTALMANIPcon)**: Heatmaps show correlations (e.g., Accusation with Shaming/Belittlement). (MENTALMANIPmaj similar - Appendix E).
*   **Embedding Visualization (Figure 5 left - t-SNE of MENTALMANIPcon)**: Manipulative and non-manipulative dialogues have intertwined embeddings, hard to distinguish.
*   **Comparison with other dialogical datasets (Figure 5 right, Figure 6)**:
    *   MENTALMANIP has more conversational exchanges (richer context).
    *   Significant overlap in embedding space with other datasets (except Fox News).
*   **Key Insight**: Simple emotion classification or text embeddings alone are insufficient for distinguishing manipulation.

## 5. Experiments

### 5.1. Experiment Setting
*   **Tasks**:
    1.  Manipulation Detection (binary classification)
    2.  Technique Classification (multi-label)
    3.  Vulnerability Classification (multi-label)
*   **Datasets**: MENTALMANIPcon and MENTALMANIPmaj (split 6:2:2 for train/val/test).
*   **Models**:
    *   GPT-4 Turbo
    *   Llama-2-7B (Chat)
    *   Llama-2-13B (Chat)
    *   RoBERTa-base
*   **Experimental Settings**:
    1.  Zero-shot prompting (details in Appendix C)
    2.  Few-shot prompting (1 non-manipulative, 2 manipulative examples; details in Appendix C)
    3.  Fine-tuning (Llama-2-13B instruction-tuned, RoBERTa-base supervised fine-tuned on various datasets).
*   **Ignored Dataset**: TalkDown (too long for RoBERTa).
*   **Research Questions**:
    1.  Effectiveness of LLMs' inherent knowledge for identifying/categorizing manipulation.
    2.  Performance with prompted examples.
    3.  Performance post fine-tuning on relevant datasets.

### 5.2. Manipulation Detection (Section 4.2)
*   **Hypersensitivity of LLMs (Table 4)**:
    *   On 899 non-manipulative dialogues from MENTALMANIPcon:
        *   GPT-4 Turbo: Misclassified 312 (34.7%) as manipulative.
        *   Llama-2-7B: Misclassified 895 (99.5%).
        *   Llama-2-13B: Misclassified 879 (97.8%).
    *   Llama-2 models show very limited capability to discern non-manipulation.
*   **Overall Results (Tables 5 for MENTALMANIPcon, Table 6 for MENTALMANIPmaj)**:
    *   MENTALMANIPmaj is more challenging.
    *   **Zero/Few-shot**:
        *   Llama-2-13B often classifies nearly all as manipulative (high recall).
        *   Few-shot improves Accuracy & F1 for GPT-4 Turbo (increases Recall) and Llama-2-13B (makes it less sensitive).
        *   *See Appendix F for confusion matrices.*
    *   **Fine-tuning**:
        *   Fine-tuning Llama-2-13B on Dreaddit (mental stress) gave best results among existing datasets.
        *   Fine-tuning on existing relevant datasets did *not* notably outperform zero/few-shot.
        *   RoBERTa-base generally showed inferior Accuracy to Llama-2-13B. Fine-tuning on Fox News degraded RoBERTa performance significantly.
    *   **Fine-tuning on MENTALMANIP itself**: Best performance, as expected.

### 5.3. Technique and Vulnerability Classification (Section 4.3, Table 7 - MENTALMANIPcon)
*   Multi-label classification.
*   **Zero/Few-shot (GPT-4 Turbo, Llama-2-13B)**:
    *   GPT-4 Turbo performed better than Llama-2-13B.
    *   Few-shot prompting (2 examples) increased accuracy for both.
*   **Fine-tuning (Llama-2-13B, RoBERTa-base on MENTALMANIPcon)**:
    *   Llama-2-13B fine-tuned on MENTALMANIPcon performed better than RoBERTa-base fine-tuned on MENTALMANIPcon.
*   **Per-Category Precision/Recall (Table 8 - Zero-shot GPT-4 Turbo, Llama-2-13B)**:
    *   Varies significantly by technique/vulnerability.
    *   E.g., GPT-4 had high recall for "Playing the Servant Role" but low precision. High precision for "Persuasion or Seduction."

### 5.4. Analysis on Incorrect Predictions (Section 4.4, Figure 7)
*   Compared semantic distributions (t-SNE of Sentence Transformer embeddings) of correctly vs. incorrectly predicted dialogues by GPT-4 Turbo on MENTALMANIPcon test set.
*   **Finding**: Dialogues, whether correctly or incorrectly predicted, are semantically indistinguishable using these embeddings.
*   Underscores difficulty of differentiation based solely on lexical/semantic features.

## 6. Conclusion and Future Studies

### Conclusion
*   Introduced MENTALMANIP, a pioneering dataset for fine-grained identification and classification of mental manipulation.
*   Experiments with GPT-4 Turbo, Llama-2-13B, and RoBERTa-base reveal models' understanding of mental manipulation does not align well with human perspectives.
*   LLMs, especially smaller ones, tend to incorrectly identify general toxicity as manipulation.

### Future Studies
*   Expand dataset sources beyond movie scripts to include real-case interpersonal interaction data.
*   Investigate LLM performance with more complex prompting (e.g., chain-of-thought).
*   Acknowledge the inherent challenge due to implicitness, context-dependence, and subjectivity.
*   Dataset publicly available to foster research.

## 7. Limitations

*   **Language and Format**: English-only, dialogues between two individuals. Real-world interactions can be more complex.
*   **Data Source**: Movie scripts may not reflect natural, real-life communication styles.
*   **Data Annotation**:
    *   Inherent subjectivity.
    *   Annotator selection bias (e.g., gender demographic imbalance, though no notable difference in inter-gender agreement found).

## 8. Ethics and Broad Impact

*   **Annotator Well-being**: Profanity in R-rated movie dialogues rephrased to be milder while preserving context.
*   **Diversity**: Emphasized diverse annotator team (race, gender). Annotator feedback actively encouraged (summary in Appendix G).
*   **Data Sensitivity**: Dataset contains uncensored sensitive materials (hate speech, violence, etc.).
*   **Potential for Misuse**: Risk of data being used to train malicious generative AI (e.g., manipulative chatbots for scams). Crucial to address these risks for responsible use.

## 9. Appendices Overview (Key Information)

*   **Appendix A**: Definitions of 11 manipulation techniques and 5 targeted vulnerabilities.
*   **Appendix B**: Key phrase examples and online resources used for collection.
*   **Appendix C**: Prompting formats for GPT-4 and Llama-2 (Zero-shot, Few-shot for detection and classification).
*   **Appendix D**: Detailed analysis of annotation quality (inter-annotator agreement heatmaps, confidence plots, gender analysis).
*   **Appendix E**: Additional statistics for MENTALMANIPmaj (emotion distribution, co-occurrence heatmaps).
*   **Appendix F**: Confusion matrices for manipulation detection task.
*   **Appendix G**: Summary of annotator feedback (prior knowledge, mutual manipulation, cognitive fatigue).
*   **Appendix H**: An example of an annotated dialogue.
*   **Appendix I**: Screenshots of annotation platform and instructions.
*   **Appendix J**: More dataset statistics (counts for MENTALMANIPcon and MENTALMANIPmaj).

## 10. Personal Notes & Analysis Questions
*   *(Space for your own notes, connections to other research, potential follow-up experiments, etc.)*
*   How does the fictional nature of movie scripts impact the types/expressions of manipulation observed compared to real-world data?
*   Could a model trained on MENTALMANIP generalize to other forms of dialogue (e.g., social media, therapy sessions)?
*   What specific linguistic cues (beyond keywords) are most indicative of the different manipulation techniques?
*   How does the "moderate" Fleiss' Kappa of 0.596 affect the reliability of the ground truth labels and model evaluation?
*   The paper mentions LLMs misclassify general toxicity as manipulation. What are the distinguishing features?
*   Could the "cannot decide" option in annotation, if used frequently for certain dialogues, indicate a new category or ambiguity worth exploring?
*   How effective would [[Chain-of-Thought Prompting]] be on this dataset, given the context-dependent nature of manipulation?
*   Explore the specific examples in Table 8 (Precision/Recall per technique/vulnerability) - why might models perform well on "Persuasion/Seduction" but poorly on others?
*   The "hypersensitivity" of Llama-2 models (Table 4) is striking. What underlying reasons could cause this?
*   Are there cultural nuances in manipulation that this dataset (English, movie scripts) might not capture?
---
