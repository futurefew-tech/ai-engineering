# Assignment: Build & Evaluate a Prompt-First AI Assistant (No Code)

## Goal
Learn how AI engineers design, improve, and evaluate prompts to get reliable outputs from LLMs.

**Important:** No coding allowed. This is a prompt engineering + evaluation assignment.

---

## What You’ll Learn
After completing this assignment, you will understand:

- How to think like an AI engineer when designing prompts
- Why the same prompt can produce different outputs
- How prompt structure affects quality, consistency, and reliability
- How to improve prompts through iterative testing
- Role prompting, constraint prompting, and example-based prompting
- How to create structured JSON outputs from messy text
- How to design strong system prompts for AI assistants
- How to evaluate LLM outputs using clear scoring metrics
- How to identify prompt failures and debug them
- Trade-offs between simple vs complex prompts
- How to test prompt generalization on unseen inputs
- Professional prompt versioning and experiment tracking with LangSmith

---

## Task 0: Define Context (Mandatory)
Before starting, answer:

- Who is your target user?
- What does a good AI output look like?
- What makes an output unusable?

This helps you think like a product + AI engineer.

---

## Task 1: Prompt Iteration (v1 → v2 → v3)

Use **one article (400+ words)** for this entire section.

### v1 — Baseline Prompt
Create a simple prompt like:

```text
Summarize this article.
````

Run it **3 times** on the same article.

Observe:

* Does output change across runs?
* What tone does the model choose?
* Is the format consistent?
* Is the summary useful?

---

### v2 — Improved Structured Prompt

Improve the prompt using:

* Role definition (teacher / analyst / product manager)
* Clear output format
* Specific instructions
* Constraints (what to include / ignore)

Example ideas:

* Bullet points
* Fixed sections
* Maximum length
* Focus on business insights only

Run **3 times** again.

Compare with v1:

* What improved?
* What is still inconsistent?

---

### v3 — Advanced Prompt

Now improve further by adding:

* Step-by-step reasoning instructions
* One example input → output
* Hard constraints:

  * exact word limit
  * tone
  * audience level

Run:

* 3 times on the same article
* 1 time on a completely new article

Check:

* Does it stay consistent?
* Does it generalize?

---

## Task 2: Structured Information Extraction

Goal: Convert messy text into structured JSON.

Example messy input:

```text
Priya Sharma is a data analyst from Bangalore. She has 4 years of experience in fintech. Contact unknown.
```

### Prompt Version 1

Create a prompt that extracts:

```json
{
  "name": "",
  "location": "",
  "experience_years": "",
  "industry": "",
  "contact": ""
}
```

---

### Prompt Version 2 (Improved)

Improve your extraction prompt so it:

* Handles missing values with `null`
* Adds confidence score:

  * high
  * medium
  * low

Example:

```json
{
  "name": "Priya Sharma",
  "location": "Bangalore",
  "experience_years": 4,
  "industry": "fintech",
  "contact": null,
  "confidence": "high"
}
```

Test on:

* 1 clean example
* 1 messy/ambiguous example

---

## Task 3: System Prompt Design

Design a **system prompt** for an AI assistant called:

**LearnBridge (EdTech Support Agent)**

Your system prompt must define:

* Assistant persona
* Tone
* Allowed actions
* Restrictions
* Response format

---

### Test Cases

Test your assistant with 3 users:

1. Curious user
2. Frustrated user
3. Aggressive user

Check:

* Does tone stay consistent?
* Does the assistant follow rules?
* Does it avoid breaking character?

---

## Task 4: Evaluation Framework

Create a scoring table (1–5).

Example:

| Metric            | Meaning                               |
| ----------------- | ------------------------------------- |
| Accuracy          | Is the answer correct?                |
| Completeness      | Is all important information covered? |
| Format Compliance | Did output follow instructions?       |
| Consistency       | Same prompt → similar outputs         |
| Generalization    | Works on new inputs                   |

Use this to score:

* Prompt v1
* Prompt v2
* Prompt v3
* Extraction prompts
* System prompt

---

## Task 5: Failure Analysis

Pick your worst-performing prompt.

Answer:

* What failed?
* Why did it fail?
* What assumption was wrong?
* Fix one issue
* Test again

Show before vs after.

---

## Task 6: Version Comparison

Compare:

**v1 vs v2 vs v3**

Discuss:

* What improved?
* What got worse?
* Complexity vs reliability trade-offs
* Which version would you actually ship?

---

## Bonus (Optional): LangSmith Workflow

Use LangSmith like a professional AI engineer.

### What to Do

Save prompt versions:

* `summarizer_v1`
* `summarizer_v2`
* `summarizer_v3`

Track:

* Inputs
* Outputs
* Runs
* Failure cases

Evaluate:

* Which version performs best?
* Which version is most consistent?
* Which version fails most often?

---

## Deliverables

Submit:

✅ Context answers
✅ Article used
✅ Prompt versions (v1, v2, v3)
✅ Output screenshots / runs
✅ Extraction prompts + results
✅ System prompt + test conversations
✅ Evaluation score table
✅ Failure analysis
✅ Version comparison
✅ LangSmith screenshots / notes (if attempted)

---

## Evaluation Criteria

You will be evaluated on:

* Prompt quality
* Iteration thinking
* Evaluation depth
* Failure analysis
* System prompt design
* Structured extraction quality
* Clarity of reasoning

---

**Estimated Time: 3–6 hours**
