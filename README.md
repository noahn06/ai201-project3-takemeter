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
| `news` | 10 |
| `hot_take` | 82 |
| `analysis` | 116 |
| **Total** | **208** |

**Difficult annotation cases:**

1. **Post:** "For the task I challenged it with — a problem in Diophantine analysis — it cost me 10x more than Opus 4.8 on max effort. It did the job better, but we're talking about Opus 4.8 after it had been degraded from normal performance..." — **Issue:** Starts as a personal anecdote but includes a specific 10x cost metric and an industry observation about model degradation before launches. Could be `analysis` because of the comparative data. **Decision:** Labeled `analysis` because the 10x figure and the causal reasoning about degraded model versions constitute a structured cost-benefit argument, not just a story.

2. **Post:** "It's not even a knowledge gap, it's a willingness gap. The brand already knows every failure case better than anyone — their support team answers those questions all day. They just won't publish it because legal and marketing both flinch..." — **Issue:** Sounds like an opinion rant but makes a specific distinction and reasons through the cause. No external source. **Decision:** Labeled `analysis` because it builds a causal chain (brands have the knowledge → institutional resistance stops publishing → it's a choice not a limitation) rather than just asserting a complaint.

3. **Post:** "Because there's more text to read at Reddit... if you feed it nine pages of Shakespeare and one page of James Joyce, it's going to mostly sound like Shakespeare. Reddit has a really large amount of text, larger than any ten other websites..." — **Issue:** Written casually, no citations, reads like a hot take. But it explains the mechanism behind why LLMs sound like Reddit. **Decision:** Labeled `analysis` because it explains a causal mechanism (training data volume → probability weighting) with a concrete comparison, not just an assertion.

---

## Fine-Tuning Approach

**Base model:** `distilbert-base-uncased` (HuggingFace)

**Training setup:** Fine-tuned on Google Colab T4 GPU using the `transformers` and `datasets` libraries. Dataset split: 70% train / 15% validation / 15% test. Training completed in approximately 10 minutes.

**Hyperparameter decisions:**
- **Epochs:** 3 (default). Did not increase because the dataset is small (208 examples) and additional epochs risk overfitting.
- **Learning rate:** 2e-5 (default). Kept standard as the dataset size did not warrant tuning.
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
| Zero-shot baseline (Llama 3.3 70B) | 43.8% |
| Fine-tuned DistilBERT | 65.6% |

---

### Per-Class Metrics

**Zero-shot baseline:**

| Label | Precision | Recall | F1 |
|---|---|---|---|
| `news` | 0.00 | 0.00 | 0.00 |
| `hot_take` | 0.43 | 0.92 | 0.59 |
| `analysis` | 0.50 | 0.11 | 0.18 |

**Fine-tuned model:**

| Label | Precision | Recall | F1 |
|---|---|---|---|
| `news` | 0.00 | 0.00 | 0.00 |
| `hot_take` | 1.00 | 0.23 | 0.38 |
| `analysis` | 0.62 | 1.00 | 0.77 |

---

### Confusion Matrix (Fine-tuned Model)

|  | Predicted: news | Predicted: hot_take | Predicted: analysis |
|---|---|---|---|
| **True: news** | 0 | 0 | 1 |
| **True: hot_take** | 0 | 3 | 10 |
| **True: analysis** | 0 | 0 | 18 |

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
| "According to Sensor Tower's May 2026 report, ChatGPT has dropped below 50% of global assistant share..." | `analysis` | 81% | Correct — cites a named report with specific percentages, exactly what `analysis` requires |
| "The unit distance conjecture was disproved by GPT-5.5, and I don't recall the NSA freaking out..." | `analysis` | 42% | Wrong — this is a `hot_take`; low confidence suggests the model was uncertain |
| "Watch the movie Wargames. That kid was average." | `analysis` | 42% | Wrong — extremely short hot take, no structure or evidence at all |
| "Every email touches AI now. Teams chats as well depending on the length." | `analysis` | 42% | Wrong — unsourced assertion predicted as analysis; model appears to default to `analysis` at low confidence |
| "Some historians of technology think it always is, and bubbles like this always accompany new technologies." | `analysis` | 45% | Wrong — vague appeal to unnamed historians; no citation, no data, true label is `hot_take` |

---

### Reflection: What the Model Learned vs. What I Intended

I intended the model to learn the distinction based on sourcing — whether a post actually cites evidence or just asserts a claim. What it appears to have learned instead is a surface-level association between post length and structure and the `analysis` label. Almost every wrong prediction followed the same pattern: a `hot_take` that was more than one sentence long, structured in a list or enumeration, or used words like "benchmarks" or named specific entities — and the model called it `analysis`. Meanwhile, genuine analysis posts that were well-sourced were predicted correctly with high confidence.

The bigger issue is the `news` class. With only 10 examples in the full dataset and 1 in the test set, the model effectively never learned what `news` looks like. It scored 0 across precision, recall, and F1 for both the baseline and the fine-tuned model. This is a data problem, not a model problem — the label boundary is clear, but there simply wasn't enough training signal. The `hot_take` → `analysis` confusion is the dominant failure mode: 10 out of 11 wrong predictions were `hot_take` posts predicted as `analysis`. The model learned to predict `analysis` as its safe default when uncertain, which is visible in the confidence scores — most wrong predictions had confidence around 0.42–0.45.

---

## Spec Reflection

**One way the spec helped:** Writing `planning.md` before collecting data forced me to define label boundaries precisely before I touched any examples. When I ran into hard annotation cases — like a post that uses benchmark language but cites nothing — I had a written decision rule to refer back to instead of making inconsistent calls in the moment. The hard edge cases section in particular was useful as a running log that kept my labeling consistent across 208 examples.

**One way implementation diverged from the spec:** My original `planning.md` said I would do annotation entirely by hand. In practice I used Claude to pre-label batches of posts and then reviewed and corrected them. The manual review was genuine — I corrected a meaningful number of labels — but the collection process was faster than the spec's estimated 1–2 hours because of the pre-labeling step.

---

## AI Usage

**Instance 1 — Bulk pre-labeling:**
I used Claude to pre-label batches of posts using my label definitions from `planning.md`. I provided the full label definitions and decision rules in the prompt, and Claude returned a label per post. I then reviewed every pre-labeled example and corrected any labels I disagreed with. This sped up annotation but the review was genuine — I caught cases where Claude labeled structured-but-unsourced posts as `analysis` when they should have been `hot_take`.

**Instance 2 — CSV format troubleshooting:**
When my CSV kept breaking the Colab notebook with a `KeyError: 'label'`, I asked an AI assistant to help diagnose the issue. It identified that my file was being read as a single column named `text,label` instead of two separate columns — meaning the entire CSV was wrapped in quotes and the comma delimiter was not being recognized. The fix was to re-export the file from Google Sheets, which stripped the bad quoting and produced a properly formatted two-column CSV.

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