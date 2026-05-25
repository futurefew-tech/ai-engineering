# LLM & RAG — Interview Question Bank

> **Scope:** AI Engineering · MLOps · LLM Systems · GenAI · Production ML  
> **Level:** Intermediate → Advanced  
> **Format:** Each question includes answer bullets, follow-up probes, common mistakes, and study pointers.

---

## 1. What is LoRA and how does it solve the catastrophic forgetting problem in full fine-tuning?

**Answer bullets**
- LoRA injects small trainable matrices into frozen pre-trained weight layers: `ΔW = A × B` where rank `r << n`, so only A and B are trained.
- The original weight matrix W remains unchanged — the base model is fully preserved, eliminating catastrophic forgetting.
- At inference, LoRA adapters can be merged into W or swapped per task, enabling multi-persona or multi-domain deployment from a single base model.
- Trade-offs: still requires GPU compute; rank `r` and learning rate are sensitive hyperparameters; does not eliminate hallucination.

**Follow-up probes**
- How do you choose rank `r`, and what happens if it is too large or too small?
- What is QLoRA and what additional problem does it solve?
- Can LoRA be stacked with RAG? When would you combine both?

**Common mistakes**
- Saying LoRA trains no parameters — it does train the adapter matrices A and B.
- Not knowing that merged LoRA weights are mathematically identical to a fine-tuned model at inference time.
- Ignoring that LoRA does not prevent hallucination on facts outside the adapter's training data.

**Study deeper**
`LoRA paper (Hu et al. 2021)` · `QLoRA / bitsandbytes` · `PEFT library (HuggingFace)` · `Adapter layers` · `Continual learning in NLP`

---

## 2. When would you choose fine-tuning over RAG, and when would you combine both?

**Answer bullets**
- Fine-tune when the model needs to internalize style, structure, or terminology that cannot be expressed via retrieval (e.g., brand voice, code formatting conventions).
- Use RAG when facts change frequently — knowledge updates are just index refreshes, no retraining required.
- Fine-tuning risks domain lock-in: a model fine-tuned on medical coding degrades at creative writing. RAG avoids this by keeping knowledge outside weights.
- Common production pattern: fine-tune for style + RAG for facts. The model speaks the right language while grounding answers in current data.
- Fine-tuning on hallucinated or low-quality data compounds errors (e.g., a model inventing a fake company founder when fine-tuned on sparse internal docs).

**Follow-up probes**
- How does fine-tuning interact with a model's ability to follow instructions from a system prompt?
- At what dataset size does fine-tuning start yielding reliable gains?
- How would you detect that a fine-tuned model is overfitting to its training domain?

**Common mistakes**
- Claiming fine-tuning is always better because it modifies the model directly — misses update cost, domain lock-in, and hallucination risk.
- Not mentioning that RAG and fine-tuning are complementary, not mutually exclusive.
- Forgetting that fine-tuning requires curated, clean data — garbage in, garbage out at a higher cost.

**Study deeper**
`PEFT / LoRA` · `RLHF` · `Instruction tuning` · `Dataset curation for LLMs` · `Catastrophic forgetting`

---

## 3. Long context windows can now hold thousands of pages. Why would you still use RAG instead of stuffing everything into context?

**Answer bullets**
- **Cost:** token cost scales linearly (or super-linearly with attention); a 3,000-page context is expensive on every single query.
- **Lost-in-the-middle problem:** LLMs attend poorly to information buried in the middle of very long contexts — empirically measured recall degrades significantly.
- **Latency:** time-to-first-token grows with context length; user experience and SLA compliance suffer.
- **Privacy and access control:** full-context means every user query sees all documents regardless of clearance level. RAG enables per-query metadata filtering.
- **Grounding and attribution:** RAG can return the specific chunk ID that produced each claim; full-context attribution is opaque.
- **Freshness:** RAG indexes update incrementally; full-context must be rebuilt on every call.

**Follow-up probes**
- At what context length does the "lost in the middle" effect become measurable?
- How would you combine RAG with a long-context model for maximum accuracy on complex documents?
- What is prefix caching / context caching and how does it partially mitigate long-context cost?

**Common mistakes**
- Concluding that long context windows eliminate RAG entirely — they do not address cost, privacy, or lost-in-the-middle.
- Not knowing the "Lost in the Middle" paper (Liu et al. 2023) — frequently cited in technical interviews.
- Treating context window size and effective context utilization as equivalent.

**Study deeper**
`Lost in the Middle (Liu et al. 2023)` · `Attention patterns in long-context models` · `KV cache management` · `Prefix caching (Anthropic, OpenAI)` · `Needle-in-a-haystack benchmarks`

---

## 4. Walk me through how you would evaluate a RAG pipeline end-to-end. What metrics would you track?

**Answer bullets**
- **Retrieval:** recall@k (did the relevant chunk appear in top-k?), MRR, NDCG, context precision (what fraction of retrieved chunks were actually useful?).
- **Generation:** faithfulness (does the answer only use retrieved context?), answer relevance (is the response on-topic?), hallucination rate.
- **End-to-end:** latency breakdown (retrieval vs. LLM generation), P50/P95, cost per query.
- Automate with Ragas, TruLens, or DeepEval on a golden test set; add human eval for safety-critical domains.
- Separate retrieval failures from generation failures — this is the most common debugging insight teams miss.

**Follow-up probes**
- How do you build a golden test set when you have no labeled data?
- What does the Ragas faithfulness metric actually measure under the hood?
- How would you integrate RAG evals into a CI/CD pipeline to catch regressions on deploy?

**Common mistakes**
- Using BLEU or ROUGE — they measure surface similarity, not factual accuracy or groundedness.
- Only measuring final answer quality without decomposing into retrieval vs. generation errors.
- No latency monitoring in production — retrieval bottlenecks are invisible without tracing.

**Study deeper**
`Ragas framework` · `TruLens` · `ARES evaluation` · `Synthetic test set generation (GPT-4 as judge)` · `A/B testing LLM pipelines`

---

## 5. Your RAG chatbot returns inconsistent answers to the same question across requests. How do you debug it?

**Answer bullets**
1. **Check retrieval consistency first:** log retrieved chunk IDs per query — are the same chunks returned each time? Non-determinism here points to embedding model drift, ANN index non-determinism, or a recently updated index.
2. **Check LLM temperature:** high temperature causes stochastic generation. Set `temperature=0` for reproducibility in factual applications.
3. **Check prompt construction:** verify the retrieved context is injected identically — a formatting bug can silently drop chunks.
4. **Check chunk boundaries:** if a sentence spans two chunks and retrieval alternates which chunk it returns, the answer changes.
5. **Check embedding model versioning:** if the model was updated, old and new embeddings exist in different vector spaces — re-embed all documents.
6. **Add tracing:** capture the full trace (query → retrieved chunks → constructed prompt → response) on every call using LangSmith, Langfuse, or Arize.
7. **Isolate variables:** change one parameter at a time — chunk size, top-k, temperature — and measure faithfulness before/after.

**Follow-up probes**
- How do you detect embedding space drift in a production index?
- What is the impact of chunk overlap on retrieval consistency?
- How would you version-control a vector index?

**Common mistakes**
- Blaming the LLM first — retrieval inconsistency is at least as likely a root cause.
- Not logging retrieved chunks — you cannot debug a RAG system without this.
- Changing multiple parameters simultaneously, making root cause identification impossible.

**Study deeper**
`Vector index versioning` · `Embedding model versioning` · `LangSmith / Arize observability` · `Chunk overlap sensitivity` · `Deterministic vs. stochastic LLM settings`

---

## 6. Users report your RAG chatbot "makes up quotes" that don't exist in source documents. How do you fix it permanently?

**Answer bullets**
- **Root cause:** the LLM is generating plausible-sounding text not present in retrieved chunks — hallucination triggered by missing context.
- **Prompt-level fix (short-term):** explicit system instruction — only return verbatim quotes if they exist in the provided context; otherwise paraphrase with attribution.
- **Structural fix:** require citation IDs — force the model to return the source chunk ID alongside each claim; validate in post-processing whether the claim exists in that chunk.
- **Faithfulness guardrail:** run a second pass with an NLI model or a lightweight LLM to verify each response claim exists in the retrieved context before returning to the user.
- **Retrieval fix (root cause):** if relevant chunks are simply missing, the LLM fills gaps with confabulation — improve recall@k with hybrid retrieval or re-ranking.
- **Regression prevention:** add hallucination rate to your eval suite; fail deploys that exceed a threshold.

**Follow-up probes**
- How do you implement faithfulness checking without the cost of a second full LLM call?
- What is the difference between hallucination and confabulation? Does it matter operationally?
- How does retrieval recall correlate with observed hallucination rate in your experience?

**Common mistakes**
- Fixing only the prompt — without improving retrieval recall, the model still hallucinates when relevant context is missing.
- Not adding automated faithfulness regression tests — the bug reappears silently after a model upgrade.
- Confusing citation formatting issues (wrong source number) with actual hallucination (invented content).

**Study deeper**
`Ragas faithfulness metric` · `SelfCheckGPT` · `NLI-based fact verification` · `Chain-of-verification prompting` · `Hallucination taxonomy`

---

## 7. Design a secure, on-premise RAG system for a government agency handling classified documents.

**Answer bullets**
- **No external APIs:** all components run on-premise — open-source LLM (Llama 3, Mistral), local embedding model, self-hosted vector DB (Weaviate, Qdrant, pgvector).
- **Air-gap option:** for fully classified environments — no internet egress; all model weights and dependencies loaded offline at provisioning time.
- **Clearance-level enforcement:** every chunk stores a metadata tag reflecting its classification level; retrieval pipeline filters by the querying user's clearance before returning results.
- **Audit logging:** immutable, append-only logs of every query, retrieved chunk ID, and generated response — stored on a separate write-once system, not the query path.
- **Encryption at rest and in transit:** AES-256 for stored vectors and documents; TLS 1.3 for all internal service communication.
- **Red-team testing:** adversarial queries designed to extract above-clearance content, prompt injection via tool results, and cross-user data leakage.
- Reference: ChatGPT Gov launch confirms the industry trend toward compliance-ready isolated LLM environments.

**Follow-up probes**
- How do you prevent a low-clearance user from extracting high-clearance content through carefully crafted queries?
- What open-source embedding models would you select, and what benchmark would you use to evaluate them?
- How do you perform model weight updates in an air-gapped environment at scale?

**Common mistakes**
- Suggesting a hosted API (OpenAI, Anthropic) for classified data — an immediate disqualifier in this context.
- Relying on vector similarity alone without metadata-level clearance filtering — embeddings do not enforce access control.
- Ignoring audit and non-repudiation requirements, which are mandatory in government contexts.

**Study deeper**
`FedRAMP authorization` · `On-premise LLM deployment (Ollama, vLLM)` · `Clearance-aware RBAC in vector DBs` · `Air-gap model distribution` · `Zero-trust network architecture`

---

## 8. Design a multi-agent RAG system for a law firm where different agents handle contracts, case law, and billing.

**Answer bullets**
- **Separate indexes per domain:** contracts, case law, and billing each have their own index with independent refresh schedules, chunk sizes, and access controls.
- **Router agent:** classifies the incoming query using intent detection and dispatches to the appropriate specialist agent; handles ambiguous multi-domain queries by spawning parallel agents.
- **Specialist agents:** each has retrieval config tuned for its domain (e.g., case law benefits from hybrid BM25 + dense retrieval for precise statutory language).
- **Orchestrator:** coordinates cross-domain queries (e.g., "does this contract clause conflict with recent case law?") by aggregating and synthesizing specialist outputs.
- **RBAC:** paralegals access billing; associates access case law; only partners access sealed case files — enforced at the retrieval layer, not just the UI.
- **Observability:** trace every agent hop with LangSmith or Langfuse; log retrieved source IDs for auditability and privilege review.
- **Graceful degradation:** if a specialist agent times out, return a partial answer with explicit uncertainty rather than silently hallucinating.

**Follow-up probes**
- How do you prevent the router from misclassifying ambiguous queries, and what is your fallback strategy?
- How would you handle a query requiring synthesis across all three domains simultaneously?
- What is your strategy for index freshness — how quickly does newly filed case law appear in the index?

**Common mistakes**
- A single monolithic agent and index — no separation of concerns, no granular access control, impossible to audit.
- No graceful degradation — agents must fail explicitly, not silently.
- Ignoring attorney-client privilege and data isolation requirements between client matters.

**Study deeper**
`Multi-agent frameworks (LlamaIndex, AutoGen, CrewAI)` · `Router agents and intent classification` · `Hybrid retrieval (BM25 + dense)` · `LangSmith / Langfuse tracing` · `Legal AI compliance`

---

## 9. A company claims their LlamaIndex integration reduced campaign creation time by 70% and sped up compliance processing 3x. How would you validate these claims before presenting them to your CTO?

**Answer bullets**
- **Establish a baseline:** pre-integration metrics — wall-clock time per campaign, human-hours per compliance review, error rates. Without this, the percentage claim is meaningless.
- **Define the metric precisely:** "time reduction" could mean wall-clock, human-hours, or AI-assisted time. Human oversight time often remains constant even when LLM generation is fast.
- **Run a controlled A/B experiment:** matched cohorts of campaigns processed with and without the RAG pipeline. Control for campaign complexity and team experience.
- **Validate compliance quality, not just speed:** 3x faster compliance with higher error rate is strictly worse. Measure false-negative rate on compliance rules.
- **Check statistical significance:** marketing automation has high variance by campaign type; a small pilot can easily produce misleading averages.
- **Account for total cost of ownership:** subtract infra, embedding, and LLM API costs from the claimed time savings. A 70% time reduction that doubles compute cost may not be worthwhile.
- **Evaluate on held-out data:** pilot metrics often overfit to the development distribution; test on unseen campaign types.

**Follow-up probes**
- How would you structure a randomized experiment for an LLM-assisted creative workflow?
- What confounds exist in time-to-completion studies for tasks with significant human judgment?
- How would you present this analysis to a non-technical CFO?

**Common mistakes**
- Accepting vendor case study metrics at face value without replication.
- Not separating AI generation time from human review time — the bottleneck often shifts, not disappears.
- Ignoring cost — a favorable time reduction metric that increases total spend is not a business win.

**Study deeper**
`LLM evaluation methodology` · `Causal inference for AI product metrics` · `Offline vs. online evaluation` · `Metric reliability and validity in NLP` · `DORA metrics for AI engineering`

---

## 10. What are the cost tradeoffs between different LlamaIndex index types, and how do you choose the right one?

**Answer bullets**
- **VectorStoreIndex:** no LLM calls during build; uses embedding model only. Cheapest to build, scales well. Best default choice.
- **SummaryIndex / SimpleKeywordTableIndex:** no LLM calls during build. SummaryIndex creates a sequential summary chain; good for document-level Q&A.
- **TreeIndex:** makes LLM calls at build time to construct a tree of summaries — expensive on large corpora. Useful for hierarchical queries but budget must be planned.
- **KeywordTableIndex:** uses LLM to extract keywords during indexing — also incurs build-time LLM cost.
- **Decision rule:** prefer VectorStoreIndex for most production RAG. Use TreeIndex or KeywordTableIndex only if query patterns require it, and cache the index to avoid rebuilding.
- **Caching strategy:** persist indexes to disk or a vector DB; load on startup rather than rebuilding. Rebuilding on every deployment is the most common cost-waste pattern.
- **Chunking cost:** embedding 1M tokens is cheap compared to LLM summarization of the same corpus — choose embedding-only indexes unless the query type demands otherwise.

**Follow-up probes**
- How does chunk size affect both retrieval quality and embedding cost?
- What is your strategy for incrementally updating a VectorStoreIndex as new documents arrive?
- How would you estimate the monthly cost of a production RAG system serving 10,000 queries per day?

**Common mistakes**
- Using TreeIndex without realizing it makes LLM calls during build — cost surprise in production.
- Rebuilding indexes on every deployment — turns a one-time indexing cost into a recurring one.
- Conflating index type choice with retrieval algorithm choice — they are independent decisions.

**Study deeper**
`LlamaIndex index type documentation` · `LlamaParse for complex document ingestion` · `Embedding model cost benchmarks (MTEB)` · `Incremental index updates` · `Token cost estimation for LLM pipelines`

---

*End of question bank — 10 questions, intermediate to advanced.*
