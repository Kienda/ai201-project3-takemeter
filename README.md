# AI201 Project 3 — TakeMeter: Sustainable AI Discourse Quality Classifier

> **Student:** Abdoulaye Alhassane Diallo
> **Course:** AI201
> **Project:** TakeMeter
> **Model:** DistilBERT (`distilbert-base-uncased`)
> **Community:** LinkedIn Sustainable AI Discussions

---

# Project Overview

TakeMeter is a machine learning classifier that evaluates the quality of Sustainable AI discussions on LinkedIn.

The goal is to distinguish between evidence-based technical discussion, reasonable opinion, and unsupported or misleading claims using supervised fine-tuning of DistilBERT.

The project also compares a fine-tuned classifier against a zero-shot Large Language Model baseline (Groq Llama 3.3 70B).

---

# Community Choice and Reasoning

I selected **LinkedIn Sustainable AI discussions** because AI sustainability has become an important topic across academia and industry. Organizations increasingly discuss:

- Green AI
- Responsible AI
- Carbon emissions
- Data-center efficiency
- Hardware optimization
- AI governance

However, the quality of these discussions varies greatly. Some posts cite research and technical evidence, while others rely on hype or unsupported claims. This made Sustainable AI an appropriate domain for discourse-quality classification.

---

# Label Taxonomy

## High Quality

Posts containing evidence-based discussion with research findings, technical explanations, statistics, tradeoffs, or nuanced analysis.

### Example 1

> Flash Attention reduces memory complexity and improves inference efficiency, lowering energy consumption in large-scale deployments.

### Example 2

> Carbon-aware scheduling reduces AI emissions by shifting workloads toward periods of cleaner electricity generation.

---

## Medium Quality

Reasonable opinions or observations related to Sustainable AI that lack sufficient supporting evidence.

### Example 1

> AI companies should publish more information about model energy consumption.

### Example 2

> I believe inference efficiency matters more than training efficiency for long-term sustainability.

---

## Low Quality

Posts containing unsupported claims, misinformation, clickbait, hype, or extreme conclusions without evidence.

### Example 1

> AI is destroying the planet and should be banned immediately.

### Example 2

> Every ChatGPT prompt emits more CO₂ than driving a gasoline car for ten miles.

---

# Dataset

## Source

LinkedIn-style Sustainable AI posts collected and manually annotated.

## Dataset Size

| Split      | Examples |
| ---------- | -------: |
| Training   |      175 |
| Validation |       37 |
| Test       |       38 |
| Total      |      250 |

## Label Distribution

The dataset was manually balanced across the three classes:

- Low Quality
- Medium Quality
- High Quality

to reduce class imbalance during training.

---

# Data Collection Process

Posts were collected from publicly available discussions related to Sustainable AI.

Each post was manually reviewed and assigned one of three labels based on the definitions in `planning.md`.

Annotation focused on:

- Evidence quality
- Technical reasoning
- Presence of statistics
- Research support
- Unsupported claims
- Overall discourse quality

---

# Difficult Annotation Decisions

## Example 1

**Post**

> I think inference efficiency matters more than training efficiency.

**Decision**

Medium Quality

**Reason**

Reasonable opinion without supporting evidence.

---

## Example 2

**Post**

> Training LLMs consumes significant energy, but hardware improvements continue reducing cost per token.

**Decision**

High Quality

**Reason**

Contains technical discussion and tradeoff analysis.

---

## Example 3

**Post**

> AI data centers consume more electricity every year.

**Decision**

Medium Quality

**Reason**

Likely true but lacks supporting evidence or context.

---

# Model

## Fine-Tuned Classifier

The classifier used in this project is **DistilBERT (`distilbert-base-uncased`)**, a lightweight Transformer model derived from BERT through knowledge distillation.

It contains approximately 66 million parameters and was fine-tuned for three-class text classification.

---

# Fine-Tuning Approach

Training was performed using the Hugging Face Transformers library on a Google Colab T4 GPU.

### Training Configuration

| Parameter     | Value                   |
| ------------- | ----------------------- |
| Model         | distilbert-base-uncased |
| Epochs        | 3                       |
| Learning Rate | 2e-5                    |
| Batch Size    | 16                      |
| Max Length    | 256                     |

---

# Hyperparameter Decision

I kept the default learning rate (2e-5) and trained for three epochs. Since the dataset contained only 250 manually labeled examples, increasing the number of epochs could have caused overfitting. The default configuration provided stable training while limiting memorization of the training set.

---

# Baseline

The baseline classifier used Groq's **Llama 3.3 70B Versatile** model.

The prompt contained:

- label definitions
- classification rules
- examples
- output constraints

The model performed zero-shot classification without additional training.

---

# Evaluation Results

## Accuracy Comparison

| Model                   | Accuracy |
| ----------------------- | -------: |
| Groq Zero-Shot Baseline |    97.4% |
| Fine-Tuned DistilBERT   |    81.6% |

Although the fine-tuned model learned the task successfully, it did not outperform the much larger frontier model.

---

# Baseline Per-Class Metrics

| Label          | Precision | Recall |   F1 |
| -------------- | --------: | -----: | ---: |
| Low Quality    |      1.00 |   0.92 | 0.96 |
| Medium Quality |      0.93 |   1.00 | 0.97 |
| High Quality   |      1.00 |   1.00 | 1.00 |

---

# Fine-Tuned Confusion Matrix

| True \ Predicted | Low | Medium | High |
| ---------------- | --: | -----: | ---: |
| Low Quality      |   9 |      3 |    0 |
| Medium Quality   |   0 |     14 |    0 |
| High Quality     |   0 |      4 |    8 |

A visual version is included as `confusion_matrix.png`.

---

# Failure Analysis

## Failure 1

True: High Quality

Predicted: Medium Quality

The post contained technical reasoning but insufficient explicit numerical evidence, leading the model to underestimate its quality.

---

## Failure 2

True: High Quality

Predicted: Medium Quality

The model recognized the Sustainable AI topic but failed to distinguish nuanced analysis from opinion.

---

## Failure 3

True: Low Quality

Predicted: Medium Quality

The model detected the sustainability topic but missed exaggerated, unsupported conclusions.

---

# Sample Classifications

| Example                                     | Prediction     | Confidence |
| ------------------------------------------- | -------------- | ---------: |
| AI companies should publish energy reports. | Medium Quality |       0.82 |
| Flash Attention reduces inference cost.     | High Quality   |       0.91 |
| AI is destroying the planet.                | Low Quality    |       0.95 |
| Carbon-aware scheduling reduces emissions.  | High Quality   |       0.88 |
| We need greener AI systems.                 | Medium Quality |       0.77 |

For example, the Flash Attention post was correctly classified as High Quality because it includes a concrete technical mechanism and a direct sustainability implication.

---

# Reflection

The model learned to distinguish strong evidence from unsupported claims but relied heavily on surface features such as technical terminology and discussion length. It struggled with borderline cases where Medium Quality and High Quality differed primarily in analytical depth rather than vocabulary.

---

# Spec Reflection

The specification provided a clear workflow for dataset creation, annotation, model training, and evaluation.

One way my implementation diverged from the expected outcome was that the Groq zero-shot baseline outperformed the fine-tuned DistilBERT model. Instead of treating this as a failure, I analyzed why a much larger language model generalized better on a relatively small dataset.

---

# AI Usage

### Instance 1

ChatGPT assisted in refining the label definitions and identifying edge cases. I reviewed and revised all suggestions before applying them during annotation.

### Instance 2

ChatGPT helped summarize evaluation metrics and identify patterns in the confusion matrix. I verified the observations manually before including them in this report.

No dataset labels were automatically accepted without human review.

---

# Repository Structure

```text
README.md
planning.md
sustainable_ai_dataset.csv
evaluation_results.json
confusion_matrix.png
groq_baseline_prompt.md
ai_usage_transparency.md
```

---

# Future Improvements

- Increase the dataset to more than 1,000 examples.
- Add more borderline Medium vs High Quality examples.
- Include more misinformation examples.
- Experiment with larger encoder models such as RoBERTa and DeBERTa.
- Compare supervised learning with retrieval-augmented approaches.

---

# Conclusion

This project demonstrates that Sustainable AI discourse quality can be modeled using supervised fine-tuning. While DistilBERT achieved strong performance (81.6%), the Groq zero-shot baseline achieved higher accuracy, illustrating the advantages of large language models on complex reasoning tasks. The project highlights the importance of careful label design, balanced datasets, and detailed error analysis when building practical NLP classifiers.
