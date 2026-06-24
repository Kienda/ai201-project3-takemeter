# TakeMeter — Groq Baseline Classification Prompt
## Model: llama-3.3-70b-versatile

This document contains the production-ready classification prompt for the TakeMeter Sustainable AI quality classifier. The prompt is designed for zero-shot use with Groq's `llama-3.3-70b-versatile` model at `temperature=0`.

---

## Prompt Template

```
You are a text quality classifier for a Sustainable AI discourse dataset.

Your task is to classify LinkedIn posts about Sustainable AI, Green AI, Responsible AI, AI energy consumption, data center efficiency, AI infrastructure, environmental impact of AI, and AI governance.

CLASSIFICATION LABELS:
- 0 = Low Quality
- 1 = Medium Quality
- 2 = High Quality

LABEL DEFINITIONS:

High Quality (2):
Evidence-based discussion containing specific reasoning, examples, tradeoffs, statistics, research findings, technical details, or nuanced analysis. The post must do more than state a plausible opinion — it must support the claim with evidence, mechanism, or specific context.

Medium Quality (1):
Reasonable opinion or observation related to Sustainable AI that lacks strong evidence, data, or detailed reasoning. The post is not misinformation, but it does not provide the evidence needed to be High Quality.

Low Quality (0):
Contains hype, misinformation, unsupported claims, exaggeration, clickbait, or extreme conclusions without evidence. This includes false statistics, conspiracy framing, absolute claims without support, and calls for extreme action without justification.

CLASSIFICATION RULES (apply these in order):

Rule 1: A specific statistic without source, context (model type, hardware, task), or methodology is Medium Quality, not High Quality.

Rule 2: A true fact used to support a false conclusion is Low Quality, not High Quality. Evaluate the conclusion, not just the premise.

Rule 3: Emotional language alone does not make a post Low Quality. If the underlying claim is specific and evidenced, classify by the evidence quality.

Rule 4: Professional framing ("as a researcher," "in my 20 years of experience") does not substitute for evidence. Judge the content, not the claimed authority.

Rule 5: Absolute claims ("every company," "always," "impossible," "never") without evidence are Low Quality indicators, especially when contradicted by documented counterexamples.

Rule 6: A vague call to action ("we need to do better," "act now," "wake up") appended to any post does not change its label — classify based on the analytical content.

EXAMPLES:

Post: "Strubell et al.'s 2019 analysis found that training a large transformer with neural architecture search can emit over 280 tonnes of CO2e — roughly 60x a standard training run. While later work has refined these estimates, the core finding remains: without standardized energy reporting norms in ML research, meaningful progress comparison is impossible."
Label: 2

Post: "Flash Attention reduces self-attention memory complexity from O(n²) to O(n) using tiling, delivering roughly 3x training speedup on A100 GPUs for sequences over 4,000 tokens and proportionally reducing per-step energy consumption."
Label: 2

Post: "The Jevons Paradox predicts that AI hardware efficiency gains will increase total energy consumption rather than decrease it, by lowering the cost of compute and enabling new applications. Every prior computing efficiency transition has followed this pattern."
Label: 2

Post: "AI companies should be more transparent about how much energy their models consume. As a user, I have no idea what the environmental cost of my daily AI use is, and I think that needs to change."
Label: 1

Post: "I think inference efficiency matters more than training efficiency for long-term AI sustainability, but I haven't seen good deployed-system energy data to confirm this."
Label: 1

Post: "Building AI data centers near renewable energy generation facilities, not just buying certificates, seems like the most straightforward path to clean compute. I'm not sure why this isn't already the default."
Label: 1

Post: "AI data centers now consume more electricity than the entire continent of Africa combined. This is a planetary emergency and the media refuses to cover it."
Label: 0

Post: "If you use ChatGPT daily, you are personally responsible for accelerating climate collapse. Delete your AI apps now before it's too late."
Label: 0

Post: "Microsoft announced 100% renewable energy matching, which means Azure AI workloads are now completely carbon free with zero environmental impact. Problem solved."
Label: 0

Now classify the following post. Return ONLY the numeric label (0, 1, or 2). Do not include any explanation, punctuation, or additional text.

Post: {post_text}
Label:
```

---

## Usage Instructions

### Python (Groq SDK)

```python
from groq import Groq

client = Groq(api_key="YOUR_GROQ_API_KEY")

PROMPT_TEMPLATE = """You are a text quality classifier for a Sustainable AI discourse dataset.
[... full prompt text above ...]
Post: {post_text}
Label:"""

def classify_post(post_text: str) -> int:
    prompt = PROMPT_TEMPLATE.format(post_text=post_text)
    response = client.chat.completions.create(
        model="llama-3.3-70b-versatile",
        messages=[{"role": "user", "content": prompt}],
        temperature=0,
        max_tokens=5,
    )
    raw = response.choices[0].message.content.strip()
    label = int(raw[0])  # Extract first character as integer
    assert label in (0, 1, 2), f"Unexpected label: {raw}"
    return label

# Label mapping for display
LABEL_NAMES = {0: "Low Quality", 1: "Medium Quality", 2: "High Quality"}
```

### Key Parameters
| Parameter | Value | Reason |
|---|---|---|
| `temperature` | 0 | Deterministic, reproducible outputs |
| `max_tokens` | 5 | Only a single digit is needed |
| `model` | llama-3.3-70b-versatile | Strong instruction-following, fast via Groq |

---

## Numeric Label Mapping

| Numeric | String Label |
|---|---|
| 0 | Low Quality |
| 1 | Medium Quality |
| 2 | High Quality |

---

## Calibration Notes

This prompt was designed to address the following known failure modes in zero-shot LLM classifiers for this task:

1. **True-premise/false-conclusion confusion** — Rule 2 explicitly addresses posts that cite real facts but draw incorrect conclusions. Without this rule, models tend to classify these as High Quality because of the factual premise.

2. **Authority bias** — Rule 4 prevents the model from upgrading posts to High Quality because they claim expert credentials. Professional framing is a common feature of Medium Quality posts.

3. **Emotional language → Low Quality bias** — Rule 3 prevents the model from downgrading genuinely substantive posts that happen to use emotional framing.

4. **Statistic without context → High Quality bias** — Rule 1 prevents the model from classifying "AI uses 40% more energy than X" as High Quality just because it contains a number.

---

## Extending to Few-Shot

If zero-shot performance is insufficient (macro F1 < 0.75), extend the prompt with additional examples targeting the specific failure mode:

```
# Add to the EXAMPLES section:

Post: "Training a single large language model emits as much CO2 as five cars over their entire lifetimes, including manufacturing."
Label: 2
# Note: This is a commonly cited statistic from Strubell et al. and is High Quality when stated with appropriate context.

Post: "Our team reduced inference costs by 60% by switching to a smaller model. Probably our best sustainability decision this quarter."
Label: 1
# Note: Specific cost claim without energy measurement or methodology = Medium Quality.

Post: "AI is going to destroy the planet. We have 5 years to ban it or we are all done."
Label: 0
```
