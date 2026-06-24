# TakeMeter — Sustainable AI Discussion Quality Classifier
## Project Planning Document

---

## 1. Community

### Description
The target community is LinkedIn professionals posting about **Sustainable AI** and closely related topics including:
- Green AI and energy-efficient model design
- Responsible AI governance with environmental dimensions
- AI energy consumption and infrastructure carbon footprint
- Data center efficiency (PUE, cooling, water usage)
- AI hardware sustainability (chip manufacturing, e-waste, hardware lifecycle)
- Environmental impact of large-scale model training and inference
- AI governance frameworks that include environmental criteria
- AI applications that address climate and energy challenges

These posts come from a broad mix of professionals: ML researchers, data scientists, infrastructure engineers, AI policy advocates, sustainability consultants, corporate executives, venture investors, and students.

### Why This Community Is a Good Classification Target

The Sustainable AI LinkedIn community is an ideal classification target for five reasons:

1. **High quality variance.** Posts range from precise citations of peer-reviewed research to fabricated statistics and apocalyptic misinformation, spanning the full spectrum of discourse quality.

2. **Stakes are real.** Misinformation in this space — overclaiming AI's environmental harm or falsely claiming sustainability is "solved" — can distort public policy, corporate decisions, and research priorities.

3. **Labels are assessable.** Domain experts can reach reasonable agreement on quality classifications. The label boundaries are principled, not purely subjective.

4. **Scale justifies automation.** Thousands of posts are published daily on these topics. Manual quality curation is not feasible; a classifier enables scalable surfacing of evidence-based content.

5. **Natural difficulty gradient.** The task includes genuinely ambiguous cases (a real statistic paired with a wrong conclusion; an emotional post with a legitimate core concern) that make it a non-trivial classification problem — appropriate for an advanced AI course project.

---

## 2. Labels

### High Quality
**Definition:** Evidence-based discussion containing specific reasoning, examples, tradeoffs, statistics, research findings, technical details, or nuanced analysis.

**Key indicators:**
- Cites specific studies, papers, or data sources by name
- Includes specific statistics with sufficient context (model type, hardware, task)
- Discusses tradeoffs between competing technical or policy approaches
- Uses technical vocabulary accurately and specifically
- Acknowledges nuance, uncertainty, or limitations in claims
- References specific technologies, frameworks, methodologies, or standards

**Example 1 (High Quality):**
> "Strubell et al.'s 2019 analysis estimated that training a large transformer with neural architecture search could emit over 280 tonnes of CO2e — roughly 60x a standard training run. Subsequent work has significantly nuanced these figures based on hardware generation and grid location, but the core finding stands: without standardized energy reporting norms, the ML community cannot measure progress toward sustainability."

*Why High Quality:* Cites a specific paper with author and year, provides a specific quantitative claim, acknowledges subsequent nuancing, and identifies the lasting methodological implication.

**Example 2 (High Quality):**
> "Flash Attention reduces self-attention memory complexity from O(n²) to O(n) by using tiling to avoid materializing the full attention matrix. On A100 GPUs this yields roughly 3x speedup for sequences over 4,000 tokens, directly reducing GPU-hours and energy for training and inference at long context lengths."

*Why High Quality:* Explains the mechanism, specifies the hardware context, gives a concrete performance figure, and connects to a sustainability implication.

---

### Medium Quality
**Definition:** Reasonable opinion or observation related to Sustainable AI that lacks strong evidence, data, or detailed reasoning.

**Key indicators:**
- Expresses a plausible professional opinion without supporting data
- Makes observations about industry trends without substantiation
- Proposes policy ideas without feasibility analysis or evidence
- Shares personal experience without measurement or methodology
- Uses hedging language ("I think," "probably," "seems like," "I suspect")
- Directionally plausible but unverifiable as stated

**Example 1 (Medium Quality):**
> "AI companies should be more transparent about the energy their models consume. As a user, I have no way to know the environmental cost of the tools I rely on every day, and I think that needs to change."

*Why Medium Quality:* The position is reasonable and the concern is legitimate, but no evidence, data, or specific policy mechanism is provided. Not misinformation — just an unsubstantiated opinion.

**Example 2 (Medium Quality):**
> "I feel like inference efficiency matters more than training efficiency for long-term AI sustainability, but I haven't seen good data on deployed model energy consumption from any major provider to confirm this."

*Why Medium Quality:* The intuition is directionally correct and the author explicitly acknowledges lack of evidence. Plausible but unsubstantiated.

---

### Low Quality
**Definition:** Contains hype, misinformation, unsupported claims, exaggeration, clickbait, or extreme conclusions without evidence.

**Key indicators:**
- Makes extreme claims without evidence ("AI will destroy the planet")
- Cites specific statistics that appear fabricated or wildly exaggerated
- Makes absolute claims about all companies, all people, or all outcomes
- Uses conspiracy framing ("hiding the truth," "follow the money," "they don't want you to know")
- Calls for immediate extreme action without reasoned justification
- Makes personal accusations or guilt assignments without evidence
- Presents opinion as established, unchallengeable fact
- Contains obvious clickbait, engagement-bait, or share-bait language

**Example 1 (Low Quality):**
> "AI data centers now consume more electricity than the entire continent of Africa combined. This is a planetary emergency and the mainstream media refuses to cover it."

*Why Low Quality:* The specific statistic is false (Africa consumes approximately 700 TWh/year, far exceeding current global data center consumption estimates). Stated as fact without source, paired with alarmist framing and a media conspiracy claim.

**Example 2 (Low Quality):**
> "Sustainable AI is 100% capitalist greenwashing propaganda. Every single company claiming to be green is lying. No exceptions. Wake up."

*Why Low Quality:* Absolute claim without evidence, dismisses the possibility of any genuine progress, uses conspiracy-adjacent framing, provides no supporting information.

---

## 3. Hard Edge Cases

### Edge Case 1: Accurate fact, incorrect conclusion
**Post:** "Microsoft announced they match 100% of their energy use with renewable energy certificates. That means Azure AI workloads are now 100% carbon free with zero environmental impact."

**Possible labels:** High Quality, Low Quality

**Decision: Low Quality**

**Reasoning:** The first sentence is factually accurate — Microsoft does purchase renewable energy certificates at scale. However, the conclusion is incorrect: annual renewable energy matching via certificates does not mean real-time carbon-free operations. The claim that workloads are "100% carbon free with zero environmental impact" is misinformation despite being grounded in a true premise. Correct facts leading to incorrect conclusions qualify as misinformation.

---

### Edge Case 2: Legitimate concern, no evidence
**Post:** "I worry that 'Green AI' is becoming a marketing label without enough substance behind it. Without standardized emissions metrics, organizations can make almost any claim they want and nobody can verify it."

**Possible labels:** High Quality, Medium Quality

**Decision: Medium Quality**

**Reasoning:** The greenwashing concern is well-documented and the observation about measurement gaps is accurate. However, the post provides no specific examples, no data, no citation of existing measurement efforts. The substance of the concern qualifies it as Medium Quality rather than Low Quality. The lack of evidence qualifies it as Medium Quality rather than High Quality. It is an informed opinion, not evidence-based analysis.

---

### Edge Case 3: Self-reported efficiency gain
**Post:** "Our team switched from a 70B parameter model to a 13B model for our production classification pipeline and cut inference costs by 80% with essentially no accuracy loss. Probably one of the most impactful sustainability decisions we made this year."

**Possible labels:** High Quality, Medium Quality

**Decision: Medium Quality (borderline)**

**Reasoning:** The 80% cost reduction is a specific, quantitative claim, which pulls toward High Quality. However, it is self-reported with no methodology, no energy measurement (only cost), and a "probably" qualifier that weakens the sustainability connection. Cost reduction and energy reduction are correlated but not identical. High Quality would require actual energy measurements with specified hardware and context. The claim is specific enough to be interesting but not rigorous enough for High Quality.

---

### Edge Case 4: Named economic concept, no citation
**Post:** "The Jevons Paradox predicts that efficiency improvements in AI compute will increase total energy consumption rather than decrease it. This has happened in every prior shift in computing history."

**Possible labels:** High Quality, Medium Quality

**Decision: High Quality (borderline)**

**Reasoning:** The post correctly names and applies a specific, well-established economic concept to AI. The historical claim — that efficiency gains increase total consumption — is directionally accurate across computing history (Moore's Law era), though not formally cited. This is the kind of applied conceptual reasoning that characterizes High Quality: using an established framework with historical context to make a non-obvious point. A formal citation would strengthen it, but the reasoning is specific enough to qualify.

---

### Edge Case 5: Extreme claim with legitimate core
**Post:** "Every technology company claiming sustainable AI credentials while simultaneously scaling model sizes by 10x per year is engaged in a fundamental contradiction."

**Possible labels:** Medium Quality, Low Quality

**Decision: Medium Quality (borderline)**

**Reasoning:** "Every technology company" is an overstatement — some companies are genuinely investing in efficiency alongside scale. However, the core observation (a tension between sustainability claims and scaling trajectories) reflects a real and documented industry dynamic. The post is not misinformation — it is an overstatement of a legitimate concern. This keeps it in Medium Quality. If the post included a fabricated statistic or called for extreme action, it would move to Low Quality.

---

## 4. Data Collection Plan

### Sources
**Primary source — Synthetic generation:**
LinkedIn-style posts generated by Claude claude-sonnet-4-6 using structured prompts specifying community, label definitions, professional register, and diversity requirements. All examples reviewed and approved by human annotators before inclusion.

**Supplementary sources (for future expansion):**
- Manual collection of public LinkedIn posts using keyword search (requires compliance with LinkedIn Terms of Service and privacy review)
- Academic commentary sections from ML paper discussion forums
- Expert-authored examples from domain practitioners in sustainable AI

### Target Counts
| Label | Count | Percentage |
|---|---|---|
| High Quality | 80 | 32% |
| Medium Quality | 90 | 36% |
| Low Quality | 80 | 32% |
| **Total** | **250** | **100%** |

**Rationale for distribution:** The slight plurality of Medium Quality examples reflects the realistic distribution of LinkedIn discourse on technical topics — most posts represent uninformed but genuine opinions rather than systematic misinformation or rigorous analysis. Near-equal High and Low Quality examples provide balanced training signal at both quality extremes.

### Handling Imbalance
The 80/90/80 distribution is near-balanced, making class imbalance less acute than in many NLP classification tasks. Mitigation strategies if evaluation reveals imbalance issues:

1. **Class weights in loss function** (preferred): Weight each class inversely proportional to its frequency in the training set.
2. **Targeted data augmentation**: Generate additional examples for underperforming classes using the same generation pipeline, with human review.
3. **Avoid oversampling via duplication**: Duplicating minority class examples introduces memorization bias. Prefer genuine new examples.
4. **Per-class recall monitoring**: Track recall separately per class during validation to detect degradation before it affects overall F1.

---

## 5. Evaluation Metrics

### Accuracy
**Formula:** (Correctly classified examples) / (Total examples)
**Interpretation:** Overall performance summary. Can be misleading in imbalanced settings, but useful as a sanity check when the dataset is near-balanced (as this one is).

### Precision (per class)
**Formula:** TP / (TP + FP) for each class
**Interpretation:** Of all examples the model predicted as class X, what fraction are actually class X. High precision for High Quality means the model rarely promotes Low or Medium content as High Quality.

### Recall (per class)
**Formula:** TP / (TP + FN) for each class
**Interpretation:** Of all actual examples in class X, what fraction did the model correctly identify. High recall for Low Quality means the model rarely misses misinformation.

### Macro F1 Score
**Formula:** Arithmetic mean of per-class F1 scores (F1 = 2 × precision × recall / (precision + recall))
**Interpretation:** Primary evaluation metric. Treats all three classes as equally important. Balances precision and recall. More informative than accuracy for this task.

### Confusion Matrix
**Structure:** 3×3 matrix with actual labels as rows, predicted labels as columns.
**Interpretation:** Reveals systematic error patterns. The most important cell to watch is the High Quality ↔ Low Quality confusion — these are the most consequential classification errors (surfacing misinformation as high-quality content, or burying genuine analysis as low-quality).

---

## 6. Definition of Success

### Target Performance Tiers
| Tier | Macro F1 | Interpretation |
|---|---|---|
| Minimum viable | ≥ 0.75 | Performs above chance; suitable for research evaluation |
| Good | ≥ 0.82 | Suitable for assistive quality filtering |
| Strong | ≥ 0.88 | Suitable for semi-automated moderation at scale |

### Specific Class-Level Targets
- High Quality recall ≥ 0.80: Most evidence-based content should surface correctly
- Low Quality precision ≥ 0.85: Limit false flagging of legitimate content
- High Quality ↔ Low Quality confusion rate < 5%: These are the most serious error type

### Deployment Readiness Criteria
A model is considered deployment-ready when ALL of the following are met:
1. Test set macro F1 ≥ 0.82 on held-out data not used in prompt engineering
2. High↔Low confusion rate < 5% across the test set
3. Human audit of 50 random predictions agrees with model ≥ 85% of the time
4. Model produces stable, consistent predictions at temperature = 0
5. Inference latency < 2 seconds per post on target hardware
6. Manual review of edge cases shows no systematic bias by professional role or topic subtype

---

## 7. AI Tool Plan

### Label Stress Testing
**Process:** Use Claude claude-sonnet-4-6 to generate adversarial edge cases that probe label boundaries:
- "High Quality-looking posts" containing subtle factual errors or unsupported conclusions
- "Low Quality-looking posts" with a legitimate technical core buried in emotional framing
- Posts that mix registers: emotional opener, technical body, exaggerated conclusion
- Posts citing real papers but drawing incorrect conclusions
- Posts from authoritative-sounding personas (CEO, researcher) without actual evidence

**Human role:** All stress test examples are adjudicated by a human reviewer. AI generates candidates; humans assign final labels with written justifications. Disagreements between AI-suggested and human-assigned labels are documented.

### Annotation Assistance
**Process:** For each dataset example, Claude generates a candidate label prediction with a brief explanation. A human annotator reviews the prediction and either accepts it, rejects it with a correction, or flags it for discussion.

This is annotation *assistance*, not automation:
- Human label is always ground truth
- AI explanations are starting points for human reflection, not conclusions
- Agreement rates between AI suggestions and human labels are tracked as a measure of task difficulty

### Failure Analysis
After model training and evaluation:
1. All misclassified test examples are collected
2. Claude is prompted to analyze each failure: "What characteristics of this post might cause a classifier to assign [wrong label] instead of [correct label]?"
3. Patterns across failures are identified collaboratively: "What do these 10 failure cases have in common?"
4. Human reviewer validates which patterns are genuine vs. artifacts
5. Targeted additional training examples are generated to address identified weak spots
6. A follow-up evaluation measures improvement after targeted augmentation

**Boundaries of AI Use:**
- AI generates training data; humans approve each example
- AI generates candidate labels; humans make final label decisions
- AI identifies potential failure patterns; humans decide what to do about them
- Model architecture, hyperparameter choices, and evaluation criteria are human decisions
- Deployment readiness is a human judgment call, not delegated to any automated system
