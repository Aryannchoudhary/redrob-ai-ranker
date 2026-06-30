

# Production-Grade Two-Stage Candidate Ranking Pipeline

An intelligent, high-throughput candidate evaluation and ranking system designed to bypass keyword-stuffing vulnerabilities while operating under strict computational constraints.

## Key Features
* **Progressive Funnel Architecture:** 4-layer architecture optimizing compute efficiency.
* **Honeypot Shield:** Deterministic pre-filtering that removes corrupted or fraudulent profiles row-by-row.
* **Deep Semantic Context:** Dual-model approach (`all-MiniLM-L6-v2` + `ms-marco-MiniLM-L-6-v2`) prioritizing true engineering experience over buzzwords.
* **Behavioral Fusion:** Blends mathematical semantic scoring with real-world engagement indicators (recruiter response rates and activity recency).

---

## System Architecture & Workflow

The system funnels **100,000+ raw profiles** down to a validated **Top 100** list via four distinct operational layers:
[100,000+ Raw Candidates]
│
▼
(Layer 1: Deterministic Guardrails / The Shield)[92,504 Valid Candidates]
│
▼
(Layer 2: Dense Semantic Retrieval via FAISS)[Top 500 Candidates]
│
▼
(Layer 3: Contextual Cross-Encoder Reranking)[Top 500 Re-scored Contextually]
│
▼
(Layer 4: Behavioral Signal Fusion)[Top 100 Final Submissions]


1. **Layer 1 (The Shield):** A streaming JSONL parser that eliminates profiles with logical anomalies (e.g., negative experience values or inverted timestamps) keeping memory consumption near zero.
2. **Layer 2 (Dense Retrieval):** Encodes the distilled Job Description and candidate texts into 384-dimensional vectors using `all-MiniLM-L6-v2` and runs a low-latency $O(\log N)$ search via `faiss-cpu`.
3. **Layer 3 (Contextual Reranking):** Evaluates deep sentence-level dependencies of the Top 500 candidates simultaneously using `ms-marco-MiniLM-L-6-v2` to filter out keyword-stuffer bias.
4. **Layer 4 (Behavioral Fusion):** Integrates semantic relevance with platform engagement metrics using a deterministic heuristic formula:
$$\text{Final Score} = S_{\text{CE}} \times (1 + \alpha \cdot R_{\text{response}}) \times A_{\text{recency}}$$

---

## 📊 Performance & Results
* **Compute Budget:** Completes execution well under the 5-minute threshold on standard CPU hardware.
* **Memory Safety:** Streaming I/O architecture completely prevents RAM exhaustion crashes during ingestion.
* **Ranking Quality:** Successfully eliminates keyword spammers, promoting active, highly qualified engineers with rigorous, traceable audit trails appended to every row.
