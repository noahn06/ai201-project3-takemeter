# TakeMeter

A fine-tuned text classifier that evaluates discourse quality in r/ArtificialInteligence by categorizing posts into `news`, `hot_take`, and `analysis`.

---

## Community Choice

I chose **r/ArtificialInteligence** on Reddit as my community. It is an active forum where discourse ranges from factual reporting on industry developments to bold speculative opinions and rigorous technical arguments. The community is a strong fit for a classification task because the same topic can generate all three types of post — a press release summary, a contrarian hot take, and a data-backed analysis — making the label boundaries meaningful and non-trivial. Domain knowledge ranges from first-time AI users to ML engineers, producing a wide spectrum of post quality and intent.

---

## Label Taxonomy

### `news`
New information relating to AI innovations, companies, usages, or research. The post primarily conveys verifiable facts with a clear source or attribution. If a post leads with a factual headline but adds a reaction or opinion, it is only labeled `news` if the factual content makes up the majority of the post.

**Example 1:**
> "Anthropic raises $2.5B Series E at $18.4B valuation, with Google leading the round. Funding will go toward scaling Claude's infrastructure and safety research according to the official press release."

**Example 2:**
> "OpenAI's o3 model scores 87.5% on ARC-AGI, up from GPT-4's 17%, according to results published on the ARC Prize leaderboard today."

---

### `hot_take`
A bold, confident opinion stated without cited evidence. The post asserts a claim rather than argues one. A hot take can include specific-sounding claims or numbers, but if there is no attribution to a source, report, or verifiable measurement, it is `hot_take`.

**Example 1:**
> "LLMs are fundamentally broken and no amount of RLHF will ever fix the hallucination problem. The whole paradigm is wrong."

**Example 2:**
> "The AI bubble is going to pop harder than crypto did. None of these companies have real revenue and the enterprise deals are all vaporware."

---

### `analysis`
A structured argument backed by statistics, reports, or observation. Evidence must come from a published report, official data, or reproducible measurement. Personal anecdotes and self-reported experiences do NOT count as reliable evidence.

**Example 1:**
> "I benchmarked GPT-4o, Claude 3.5, and Gemini 1.5 Pro on 500 real-world coding tasks using the HumanEval dataset. GPT-4o led at 72% pass@1, Claude at 68%, Gemini at 61%. Full methodology and results in the linked GitHub repo."

**Example 2:**
> "According to Sensor Tower's May 2026 report, ChatGPT has dropped below 50% of global assistant share for the first time (46.4%), with Gemini at 27.7% and Claude at 10.3%. This suggests the 'one model wins everything' thesis is failing in practice."

---

## Data Collection

**Source:** r/ArtificialInteligence on Reddit, collected manually from flair-filtered post listings.

**Process:** Posts were collected from three flair-filtered tabs (📰 News, 📊 Analysis/Opinion, 🔬 Research). Post title and body text were copied into a Google Sheet. Posts under 50 characters were skipped. Labels were assigned using the definitions above, with the flair as an initial signal. Analysis/Opinion-flair posts were reviewed individually to determine whether they were `hot_take` or `analysis` based on sourcing. Claude (claude-3-5-sonnet) was used to pre-label a batch of examples which were then reviewed and corrected. Pre-labeled examples are tracked in the `notes` column of the dataset.

**Label distribution:**

| Label | Count |
|---|---|
| `news` | [INSERT COUNT] |
| `hot_take` | [INSERT COUNT] |
| `analysis` | [INSERT COUNT] |
| **Total** | **[INSERT TOTAL]** |

**Difficult annotation cases:**

1. **Post:** "[INSERT POST TEXT]" — **Issue:** Led with a verifiable news fact but the majority of the post was the author's opinion on its implications. **Decision:** Labeled `hot_take` because the primary purpose was to share an opinion, not report information.

2. **Post:** "[INSERT POST TEXT]" — **Issue:** Contained a specific percentage figure but no source attribution. Sounded like `analysis` but violated the sourcing rule. **Decision:** Labeled `hot_take` because specificity without attribution = hot take.

3. **Post:** "[INSERT POST TEXT]" — **Issue:** Structured argument but all evidence was the author's personal experience over 3 months. No external data cited. **Decision:** Labeled `hot_take` because personal anecdote does not qualify as reliable evidence under my definitions.

---

## Fine-Tuning Approach

**Base model:** `distilbert-base-uncased` (HuggingFace)

**Training setup:** Fine-tuned on Google Colab T4 GPU using the `transformers` and `datasets` libraries. Dataset split: 70% train / 15% validation / 15% test. Training completed in approximately [INSERT TIME] minutes.

**Hyperparameter decisions:**
- **Epochs:** 3 (default). Did not increase because [INSERT REASONING, e.g., "validation loss plateaued after epoch 2"].
- **Learning rate:** 2e-5 (default). Kept standard as the dataset size (200 examples) did not warrant tuning.
- **Batch size:** 16 (default).

---

## Baseline

**Model:** `llama-3.3-70b-versatile` via Groq API (zero-shot, no fine-tuning)

**Prompt used:**
```
You are a text classifier. Classify the following Reddit post from r/ArtificialInteligence into exactly one of these three labels:

- `news` — The post primarily conveys new, verifiable information about AI...
- `hot_take` — The post makes a bold, confident opinion or prediction without citing sources...
- `analysis` — The post makes a structured argument backed by attributed statistics...

Respond with ONLY the label name. No explanation. No punctuation.

Post to classify:
{text}
```

**How results were collected:** The Colab notebook's Section 5 classified all test set examples using this prompt via the Groq API and computed accuracy and per-class metrics automatically.

---

## Evaluation Report

### Overall Accuracy

| Model | Accuracy |
|---|---|
| Zero-shot baseline (Llama 3.3 70B) | [INSERT]% |
| Fine-tuned DistilBERT | [INSERT]% |

---

### Per-Class Metrics

**Zero-shot baseline:**

| Label | Precision | Recall | F1 |
|---|---|---|---|
| `news` | [INSERT] | [INSERT] | [INSERT] |
| `hot_take` | [INSERT] | [INSERT] | [INSERT] |
| `analysis` | [INSERT] | [INSERT] | [INSERT] |

**Fine-tuned model:**

| Label | Precision | Recall | F1 |
|---|---|---|---|
| `news` | [INSERT] | [INSERT] | [INSERT] |
| `hot_take` | [INSERT] | [INSERT] | [INSERT] |
| `analysis` | [INSERT] | [INSERT] | [INSERT] |

---

### Confusion Matrix (Fine-tuned Model)

|  | Predicted: news | Predicted: hot_take | Predicted: analysis |
|---|---|---|---|
| **True: news** | [INSERT] | [INSERT] | [INSERT] |
| **True: hot_take** | [INSERT] | [INSERT] | [INSERT] |
| **True: analysis** | [INSERT] | [INSERT] | [INSERT] |

---

### Wrong Prediction Analysis

**Wrong prediction 1:**
> "Benchmarks show it well ahead in almost every category."
- **True label:** hot_take | **Predicted:** analysis (confidence: 0.40)
- **Analysis:** The word "benchmarks" is a strong signal the model associates with analysis — it sounds data-driven. But this post gives no actual benchmark numbers, no source, and no specifics. By my definition it's a hot take because specificity without attribution still counts as assertion. The model learned surface-level vocabulary ("benchmarks") rather than whether evidence was actually cited.

**Wrong prediction 2:**
> "Grok is linked to Twitter. Meta AI is linked to Facebook. The top results on Google are sponsored — companies pay to appear there. Google search often recommends Reddit posts for troubleshooting..."
- **True label:** hot_take | **Predicted:** analysis (confidence: 0.43)
- **Analysis:** This one is structured and lists multiple specific entities, which looks like a reasoned comparison. But it's all general knowledge stated as fact with no source — no study, no data, no link. The model was likely fooled by the list format and the named entities into thinking this was an analytical breakdown. It's actually just a hot take dressed up in enumeration.

**Wrong prediction 3:**
> "On June 11th Mark Warner, the vice-chair of the Senate Intelligence Committee, said that General Joshua Rudd, who leads the National Security Agency and the Pentagon's Cyber Command, had told him that..."
- **True label:** news | **Predicted:** analysis (confidence: 0.43)
- **Analysis:** This is the only news post the model got wrong. It contains a named official source, a date, and a specific quote — all news signals. The model likely predicted analysis because the post goes on to contextualize and interpret the claim (the rest of the post debates whether it's credible). This is a real edge case: the post starts as news but drifts into commentary, which confused the model. It's also worth noting news had only 1 example in the test set, so the model had almost no examples of news to learn from — data imbalance played a role here.

---

### Sample Classifications

| Post (truncated) | Predicted Label | Confidence | Notes |
|---|---|---|---|
| "[INSERT]" | [INSERT] | [INSERT]% | [Correct — explain why reasonable] |
| "[INSERT]" | [INSERT] | [INSERT]% | |
| "[INSERT]" | [INSERT] | [INSERT]% | |
| "[INSERT]" | [INSERT] | [INSERT]% | |
| "[INSERT]" | [INSERT] | [INSERT]% | |

---

### Reflection: What the Model Learned vs. What I Intended

[INSERT — e.g., "I intended the model to learn the distinction based on *sourcing* — whether a post cites evidence. What it appears to have learned instead is a surface-level association between certain words ('study shows', 'according to', 'researchers found') and the `analysis` label, while treating emotionally charged language as a signal for `hot_take`. This means the model likely fails on well-written, calm hot takes that don't use inflammatory language, and may misclassify unsourced posts that use academic-sounding phrasing."]

---

## Spec Reflection

**One way the spec helped:** [INSERT — e.g., "The requirement to write planning.md before collecting data forced me to define precise label boundaries early. When I encountered edge cases during annotation, I already had a written decision rule to refer to rather than making inconsistent judgment calls."]

**One way implementation diverged from the spec:** [INSERT — e.g., "My planning.md stated I would collect data from r/AI, but I switched to r/ArtificialInteligence because it had more active post volume and better flair categorization that aligned with my labels."]

---

## AI Usage

**Instance 1 — Bulk pre-labeling:**
I used Claude (claude-3-5-sonnet) to pre-label batches of 50 posts at a time using my label definitions from planning.md. I provided the full label definitions and decision rules in the prompt, and Claude returned a numbered list of labels. I then reviewed every pre-labeled example in the spreadsheet and corrected [INSERT NUMBER] labels where I disagreed with Claude's assignment. Pre-labeled examples are flagged in the `notes` column of the dataset CSV.

**Instance 2 — Failure pattern analysis:**
After running my model on the test set, I pasted all wrong predictions into Claude and asked it to identify patterns in the misclassifications. Claude identified [INSERT PATTERN, e.g., "a tendency to misclassify short posts as hot_take regardless of content"]. I verified this pattern by re-reading the actual posts and confirmed it was [accurate / partially accurate / inaccurate, and explain].

---

## Repository Contents

| File | Description |
|---|---|
| `planning.md` | Label design, data plan, evaluation metrics, AI tool plan |
| `data.csv` | Full labeled dataset (200 examples) |
| `README.md` | This file |
| `confusion_matrix.png` | Visual confusion matrix from fine-tuned model |
| `evaluation_results.json` | Raw metrics for both models |
| `prompts/claude_bulk_labeling_prompt.md` | Prompt used for AI pre-labeling |
| `prompts/groq_baseline_prompt.md` | Prompt used for zero-shot baseline |
| `prompts/failure_analysis_prompt.md` | Prompt used for failure pattern analysis |