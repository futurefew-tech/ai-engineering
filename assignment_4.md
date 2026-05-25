# Assignment: Build a Resume Intelligence System (RAG)

## Goal
Build an AI-powered hiring assistant that can understand resumes, answer recruiter questions, compare candidates, and match candidates to job descriptions.

You will build this step by step—from a simple single-resume assistant to a multi-candidate ranking system.

---

## What You’ll Learn
After completing this assignment, you will understand:

- How RAG (Retrieval-Augmented Generation) works
- Resume parsing and text extraction challenges
- Chunking strategies for document retrieval
- Embeddings and vector databases
- Semantic search and retrieval pipelines
- Prompting LLMs with retrieved context
- Hallucination detection and evaluation
- Metadata-based filtering
- Candidate matching and ranking logic
- Multi-document AI system design
- Real-world AI engineering trade-offs (latency, cost, reliability)

---

## Task 1: Talk to One Resume
Start with **1 resume PDF**.

Build a system that can:

- Extract text from the PDF
- Clean obvious noise
- Answer recruiter questions

Example questions:

```text
What skills does this candidate have?
What projects has this candidate worked on?
How many years of experience do they have?
````

**Goal:** Build a basic working prototype.

---

## Task 2: Build Proper RAG

Improve your system using retrieval.

Build:

* Resume chunking
* Embedding generation
* Vector storage (FAISS / Chroma / similar)
* Top-k retrieval
* LLM answer generation using retrieved chunks

Experiment with:

* Different chunk sizes
* Fixed vs section-based chunking
* Different top-k values

Compare:

* Full resume context vs RAG approach

**Goal:** Understand retrieval quality.

---

## Task 3: Evaluate Your System

Create **10 recruiter-style test questions**.

Evaluate:

* Accuracy
* Completeness
* Hallucination
* Retrieval quality

Questions to explore:

* What queries fail?
* When does retrieval miss important context?
* Does increasing top-k always help?

---

## Task 4: Multi-Resume Search

Scale to **10 resumes**.

Build a pipeline to:

* Parse resumes
* Chunk content
* Create embeddings
* Store candidate metadata

Suggested metadata:

* Candidate name
* Skills
* Experience years
* Current role (if available)

Support recruiter queries like:

```text
Find candidates with Python + NLP experience
Show resumes with production ML experience
```

---

## Task 5: Candidate Comparison

Add comparison features.

Examples:

```text
Compare top 3 candidates for an ML Engineer role
Who is strongest in deep learning?
```

Your system should explain *why* candidates were selected.

**Goal:** Move from Q&A → decision support.

---

## Task 6: JD Matching & Ranking

Input: Job Description

Output: Ranked candidate list

Example:

```text
JD: Looking for an ML Engineer with Python, NLP, FastAPI, deployment experience
```

Expected output:

* Ranked candidates
* Match reasoning
* Skill gaps (optional)

Think about:

* synonym handling
* long resume bias
* missing skills
* explainability

---

## Task 7: System Design Thinking

Design your full production architecture.

Cover:

* Resume ingestion pipeline
* Parsing layer
* Vector database
* Retrieval layer
* LLM layer
* API / UI layer
* Monitoring
* Failure handling

Questions:

* What if PDF parsing fails?
* What if retrieval finds nothing?
* Where will hallucinations happen?
* What will you log?

---

## Bonus (Optional)

Choose any:

* Build a simple UI
* Resume upload feature
* JD upload + matching dashboard
* OCR support for scanned resumes
* Better reranking logic
* Resume scorecards

---

## Deliverables

Submit:

✅ GitHub repository
✅ README with setup + architecture
✅ Sample resumes used
✅ Test questions + evaluation results
✅ System design diagram
✅ Demo screenshots / video

---

## Evaluation

You will be evaluated on:

* RAG implementation quality
* Retrieval accuracy
* Ranking logic
* Evaluation depth
* System design thinking
* Code quality

---

**Estimated Time: 6–12 hours**

```
```
