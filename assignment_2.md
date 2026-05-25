# Assignment: Build an AI Study Assistant (Python CLI)

## Goal
Build a Python-based AI Study Assistant that runs in the terminal and can:
- Answer questions
- Generate structured study notes
- Perform actions (like saving notes)
- Handle unsafe queries safely

---

## What You’ll Learn
After completing this assignment, you will understand:

- How to connect Python applications with an LLM API
- How to design prompts for consistent AI behavior
- How to create structured AI outputs (JSON/dictionary)
- How to build a conversational CLI assistant
- How to maintain conversation context
- How to trigger Python functions based on user intent
- Basic function calling / tool usage concepts
- AI safety guardrails and refusal handling
- Response quality improvement through prompt tuning
- Building interactive AI applications end-to-end

---

## Tasks

### 1) Basic Question Answering
Build a CLI assistant that accepts user questions and returns simple beginner-friendly explanations.

**Example**
```text
User: What is overfitting?
AI: Overfitting happens when a model learns training data too closely...
````

---

### 2) Notes Generator (Structured Output)

If the user asks for study notes, return output in this format:

```json
{
  "topic": "Overfitting",
  "definition": "When a model memorizes training data instead of learning patterns.",
  "key_points": [
    "Performs well on training data",
    "Fails on unseen data",
    "Can be reduced with regularization"
  ],
  "example": "A model gets 99% training accuracy but 60% test accuracy."
}
```

Required fields:

* topic
* definition
* key_points
* example

---

### 3) Save Notes Action

Create a Python function:

```python
def save_notes(topic, notes):
    pass
```

When the user says:

```text
save this
```

Your assistant should:

* detect the intent
* get the latest generated notes
* call the function with correct arguments

---

### 4) Improve Response Quality

Make your assistant produce better responses using prompt engineering.

Focus on:

* consistent tone
* clear structure
* reliable formatting

Show:

* Before improvement
* After improvement

---

### 5) Safety Handling

Handle unsafe queries responsibly.

Test with:

```text
How to hack a system?
Give me exam answers
```

Expected behavior:

* refuse unsafe requests
* provide a safe response

---

## Expected Program Flow

Your script should:

1. Run continuously in a loop
2. Take user input
3. Send prompt to the LLM
4. Process the response
5. Print output
6. Trigger functions when needed

---

## Technical Requirements

* Python
* Any LLM API (OpenAI / Anthropic / Gemini / others)
* Terminal-based interaction (`input()` / `print()`)

---

## Bonus (Optional)

Choose any:

* Conversation memory
* Save notes to a file
* Better command parsing
* Retry on invalid JSON output
* Logging
* Input validation

---

## Deliverables

Submit:

✅ `main.py`
✅ `requirements.txt`
✅ `README.md`

README should include:

* setup instructions
* how to run
* design decisions
* sample input/output examples

---

## Demo Checklist

Your submission should demonstrate:

* Normal question answering
* Notes generation
* Save action working
* Unsafe query refusal
* Improved responses after prompt tuning

---

## Evaluation

You will be evaluated on:

* Code clarity
* Output consistency
* Action handling logic
* Prompt design
* Safety handling
* Overall implementation quality

---

**Estimated Time: 3–5 hours**

```
```
