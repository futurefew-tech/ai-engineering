# Assignment: Build an AI Article API

## Goal
Build a FastAPI application that uses an **open-source LLM** to analyze article text.

## What You’ll Learn
After completing this assignment, you will understand:

- How to build REST APIs using FastAPI
- How to connect an application with an open-source LLM
- How to design effective prompts for summarization and classification
- How to structure AI outputs into clean JSON responses
- How to validate API request inputs using Pydantic
- How to handle API errors gracefully
- How to separate application logic into clean modules (routes, schemas, services)
- How to integrate LLM inference into backend applications
- How to improve AI output consistency with prompt engineering
- Basic techniques for handling long inputs (chunking/splitting)
- Logging and debugging AI-powered APIs
- Writing production-style README documentation

---

## Tasks

### 1) Summarization API
Create this endpoint:

```http
POST /summarize
````

**Input**

```json
{
  "article_text": "Full article content..."
}
```

**Output**

```json
{
  "summary": "50-word summary of the article",
  "key_insights": [
    "Insight 1",
    "Insight 2",
    "Insight 3"
  ]
}
```

---

### 2) Classification API

Create this endpoint:

```http
POST /classify
```

**Input**

```json
{
  "article_text": "Full article content..."
}
```

**Output**

```json
{
  "category": "Technology"
}
```

Example categories:

* Technology
* Business
* Politics
* Sports
* Entertainment
* Health
* Science
* Other

---

## Rules

* Use **FastAPI**
* Use an **open-source LLM** (Llama, Mistral, Gemma, Qwen, Phi, etc.)
* Design your own prompts
* Return clean JSON responses
* Use proper request validation
* Add basic error handling

---

## Bonus (Optional)

Choose any:

* Handle long articles (chunking/splitting)
* Improve output consistency
* Add logging
* Retry if model gives bad output

---

## Deliverables

Submit:

✅ GitHub repository
✅ README with:

* setup steps
* how to run
* sample API requests
* model used

---

## Suggested Project Structure

```bash
ai-article-api/
├── app/
│   ├── main.py
│   ├── schemas.py
│   ├── llm_service.py
│   └── routes.py
├── requirements.txt
└── README.md
```

---

## Evaluation

You will be evaluated on:

* Working API
* Clean code
* Prompt design
* Error handling
* JSON output quality

---

**Estimated Time: 2–4 hours**

```
```
