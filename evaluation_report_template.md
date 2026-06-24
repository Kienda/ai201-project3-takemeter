# TakeMeter — Evaluation Report Template
## Sustainable AI Discussion Quality Classifier

---

## Report Metadata

| Field | Value |
|---|---|
| Project | TakeMeter — Sustainable AI Discussion Quality Classifier |
| Report Date | [YYYY-MM-DD] |
| Evaluator | [Name / Team] |
| Model Evaluated | [e.g., Groq llama-3.3-70b-versatile, zero-shot] |
| Prompt Version | [e.g., v1.0, v2.0-few-shot] |
| Dataset Version | [e.g., v1.0, 250 examples] |
| Test Set Size | [e.g., 38 examples] |
| Evaluation Environment | [e.g., Python 3.11, groq-sdk 0.11.0, temperature=0] |

---

## 1. Overview

[2-3 sentence summary of what was evaluated and the headline result.]

**Example:** *The zero-shot Groq llama-3.3-70b-versatile baseline was evaluated on a 38-example held-out test set from the TakeMeter Sustainable AI dataset (v1.0). The model achieved a macro F1 of [X.XX], [exceeding / falling short of] the minimum viable threshold of 0.75. The most common error type was [describe pattern].*

---

## 2. Dataset Statistics

### Full Dataset
| Split | Total | High Quality | Medium Quality | Low Quality |
|---|---|---|---|---|
| Train | [N] | [N] | [N] | [N] |
| Validation | [N] | [N] | [N] | [N] |
| Test | [N] | [N] | [N] | [N] |
| **Total** | **[N]** | **[N]** | **[N]** | **[N]** |

### Dataset Quality Notes
- **Known label ambiguities:** [e.g., "5 examples in the HQ/MQ boundary region were adjudicated by consensus; reviewer initially disagreed"]
- **Topic coverage:** [e.g., "High Quality posts cover 25+ distinct topics; Medium Quality posts show heavier concentration in transparency/disclosure themes"]
- **Notes field:** [e.g., "12 of 250 examples include explanatory notes indicating borderline cases"]

---

## 3. Model Performance

### 3.1 Overall Metrics

| Metric | Score | Target | Status |
|---|---|---|---|
| Accuracy | [X.XX] | ≥ 0.75 | [Pass / Fail] |
| Macro F1 | [X.XX] | ≥ 0.75 | [Pass / Fail] |
| Weighted F1 | [X.XX] | — | — |
| High↔Low Confusion Rate | [X%] | < 5% | [Pass / Fail] |

### 3.2 Per-Class Metrics

| Class | Precision | Recall | F1 | Support |
|---|---|---|---|---|
| High Quality (2) | [X.XX] | [X.XX] | [X.XX] | [N] |
| Medium Quality (1) | [X.XX] | [X.XX] | [X.XX] | [N] |
| Low Quality (0) | [X.XX] | [X.XX] | [X.XX] | [N] |
| **Macro Average** | **[X.XX]** | **[X.XX]** | **[X.XX]** | **[N]** |
| Weighted Average | [X.XX] | [X.XX] | [X.XX] | [N] |

### 3.3 Performance Against Targets

| Target | Required | Achieved | Met? |
|---|---|---|---|
| Macro F1 (minimum viable) | ≥ 0.75 | [X.XX] | [Yes / No] |
| Macro F1 (good) | ≥ 0.82 | [X.XX] | [Yes / No] |
| High Quality Recall | ≥ 0.80 | [X.XX] | [Yes / No] |
| Low Quality Precision | ≥ 0.85 | [X.XX] | [Yes / No] |
| High↔Low Confusion Rate | < 5% | [X%] | [Yes / No] |

---

## 4. Confusion Matrix Analysis

### 4.1 Confusion Matrix

```
                    Predicted HQ    Predicted MQ    Predicted LQ    Total
Actual HQ (2):          [N]             [N]             [N]          [N]
Actual MQ (1):          [N]             [N]             [N]          [N]
Actual LQ (0):          [N]             [N]             [N]          [N]
Total:                  [N]             [N]             [N]          [N]
```

### 4.2 Key Observations

**Observation 1 — Most common error type:**
[e.g., "The most frequent error is Medium Quality posts being classified as High Quality (N cases). These tend to be posts that cite a single statistic without context — the model treats the presence of a number as a High Quality signal."]

**Observation 2 — Adjacent class confusion:**
[e.g., "High↔Medium confusion (N cases) is more common than Low↔Medium confusion (N cases), suggesting the model has a better-calibrated boundary for identifying misinformation than for distinguishing evidence-based from opinion-based content."]

**Observation 3 — High↔Low confusion:**
[e.g., "N cases of High↔Low confusion were observed, representing X% of all errors. [Describe what these cases have in common.]"]

**Observation 4 — Systematic bias:**
[e.g., "The model shows a slight bias toward predicting Medium Quality (N of total test set), inflating recall for the middle class at the expense of precision."]

---

## 5. Failure Cases

### 5.1 High Quality Misclassified as Medium or Low

**Case 1:**
- **Post:** "[text]"
- **True Label:** High Quality
- **Predicted:** [Medium / Low] Quality
- **Confidence (if available):** [X.XX]
- **Hypothesis:** [Why the model may have failed — e.g., "Emotional opening sentence likely triggered Low Quality patterns before the analytical body could compensate"]

**Case 2:**
- **Post:** "[text]"
- **True Label:** High Quality
- **Predicted:** [Medium / Low] Quality
- **Hypothesis:** [...]

**Case 3:**
- **Post:** "[text]"
- **True Label:** High Quality
- **Predicted:** [Medium / Low] Quality
- **Hypothesis:** [...]

---

### 5.2 Low Quality Misclassified as Medium or High

**Case 1:**
- **Post:** "[text]"
- **True Label:** Low Quality
- **Predicted:** [Medium / High] Quality
- **Hypothesis:** [e.g., "Post cited a real paper name, causing the model to treat it as evidence-based despite the post drawing incorrect conclusions from it"]

**Case 2:**
- **Post:** "[text]"
- **True Label:** Low Quality
- **Predicted:** [Medium / High] Quality
- **Hypothesis:** [...]

**Case 3:**
- **Post:** "[text]"
- **True Label:** Low Quality
- **Predicted:** [Medium / High] Quality
- **Hypothesis:** [...]

---

### 5.3 Medium Quality Confusion

**Case 1 (predicted High):**
- **Post:** "[text]"
- **True Label:** Medium Quality
- **Predicted:** High Quality
- **Hypothesis:** [e.g., "Post contains a specific percentage claim that the model treated as a cited statistic, despite no source or context being provided"]

**Case 2 (predicted Low):**
- **Post:** "[text]"
- **True Label:** Medium Quality
- **Predicted:** Low Quality
- **Hypothesis:** [e.g., "Strongly-worded opinion with 'every' and 'always' language triggered Low Quality classification despite the underlying concern being legitimate"]

---

## 6. Error Pattern Analysis

[Identify 3-5 systematic patterns across all failure cases.]

**Pattern 1 — [Name]:**
[Description of the pattern and examples affected. What feature is confusing the model? How many failure cases show this pattern?]

*Example: "Hedging language → Medium Quality misclassification: Posts that open with 'I think' or 'probably' are often classified as Medium Quality even when they contain specific paper citations and technical details. This suggests the model weights discourse register signals too heavily relative to content signals."*

**Pattern 2 — [Name]:**
[...]

**Pattern 3 — [Name]:**
[...]

**Pattern 4 — [Name]:**
[...]

---

## 7. Lessons Learned

### About the Model
1. [e.g., "Zero-shot Groq llama-3.3-70b-versatile has strong Low Quality detection but weaker High/Medium discrimination"]
2. [e.g., "The model is sensitive to numerical content — any post with a number tends to get upgraded regardless of source or context"]

### About the Prompt
1. [e.g., "Rule 4 (authority bias) was effective — removing it during ablation reduced precision on Medium Quality posts by 0.06"]
2. [e.g., "The few-shot examples for Low Quality were more calibrating than those for High Quality; adding more Medium/High boundary examples would help"]

### About the Dataset
1. [e.g., "The High/Medium boundary is the hardest for human annotators, not just the model — 3 of the 5 disagreement cases were in this region"]
2. [e.g., "Posts with mixed signals (emotional opener + technical body) are systematically underrepresented in the dataset"]

### About the Evaluation
1. [e.g., "Test set of 38 examples is too small for stable macro F1 estimates — confidence intervals are wide"]
2. [e.g., "Per-class F1 variation between random test set seeds suggests evaluation needs a larger hold-out set"]

---

## 8. Future Improvements

### Immediate (before next evaluation)
- [ ] [e.g., "Add 3 few-shot examples targeting the most common failure pattern (statistic without context → High Quality)"]
- [ ] [e.g., "Revise Rule 1 in the prompt to explicitly address posts with self-reported numbers"]
- [ ] [e.g., "Generate 20 additional examples in the HQ/MQ boundary region to improve training signal"]

### Short-Term (next sprint)
- [ ] [e.g., "Fine-tune distilbert-base-uncased on full 250-example dataset and compare against Groq baseline"]
- [ ] [e.g., "Collect 50 real LinkedIn posts for domain validation evaluation"]
- [ ] [e.g., "Implement confidence scoring to support human-in-the-loop review for uncertain cases"]

### Long-Term
- [ ] [e.g., "Active learning pipeline to prioritize high-uncertainty examples for human labeling"]
- [ ] [e.g., "Extend label taxonomy to adjacent communities (AI safety, AI policy)"]
- [ ] [e.g., "Bias audit: evaluate whether accuracy differs across post length, persona type, or topic subtype"]

---

## 9. Next Evaluation Plan

| Item | Detail |
|---|---|
| Next evaluation date | [YYYY-MM-DD] |
| Model / prompt version | [What will change] |
| Target improvement | [e.g., "Macro F1 from 0.74 to 0.82 via few-shot augmentation"] |
| Key metric to improve | [e.g., "High Quality recall"] |
| Dataset changes | [e.g., "Add 20 boundary examples"] |

---

*Report template version 1.0 — TakeMeter Project*
