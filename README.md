# Progressive-Summarization--Multi-Doc-

> **From Single-Document to Multi-Document Abstractive Summarization**
> Internship baseline extended into a system-level NLP project (personal extension)

This repository presents a **continuous progression of an abstractive summarization system**, evolving from **single-document summarization** to **multi-document and multi-source summarization**. The project focuses on how **model architecture, attention mechanisms, and pipeline design** must adapt as task complexity increases.

The original work (BART & LongT5) serves as a **baseline foundation** ([Internship Repo](https://github.com/dss-28/Suvidha_Internship/blob/main/README.md)), while the current extension (PEGASUS & LED) explores **multi-document summarization at scale**.

---

## 🧭 Project Evolution

Single-Document (Baseline) ──▶ Multi-Document (Extension)
Short News / Long Docs        News Aggregation / Long Multi-Source
BART, LongT5                 PEGASUS, LED

This progression reflects **real-world NLP system development**, where increasing input size and document cardinality demand changes not only in models, but also in **pipeline strategy**.

---

## 🧩 Phase I — Single-Document Summarization (Baseline)

> *Summary only — full details available in the [Internship Repo](https://github.com/dss-28/Suvidha_Internship/blob/main/README.md)*

### Models & Scope

* **BART (facebook/bart-large-cnn)** — short news articles (CNN/DailyMail)
* **LongT5 (google/long-t5-tglobal-base)** — long-form documents (ArxivSum)
* **Setting:** Inference-only (pretrained models)

### Key Takeaways

* BART excels at concise, structured news summarization
* LongT5 handles long-context inputs using sparse + global attention
* Input length and attention design strongly affect summary quality

---

## 🧩 Phase II — Multi-Document Summarization (System Extension)

This phase extends the pipeline to **multi-document and multi-source summarization**, where redundancy, ordering bias, and context length become dominant challenges.

---

## 🔹 PEGASUS — Multi-Document News Summarization

### Dataset

* **MultiNews**
* **Evaluation Subset:** 1,000 validation + 1,000 test samples
* Each sample contains **multiple related news articles** describing the same event

### Why PEGASUS?

* Pretrained with **Gap Sentence Generation**, effective at identifying salient information
* Strong performance on news-style summarization
* Limited input length → requires **pipeline adaptation**

### Pipeline Design

* Hierarchical / fusion-based pipeline:

  1. Individual documents summarized or compressed
  2. Salient information fused into a compact representation
  3. Final abstractive summary generated

### Results (Indicative)

| Metric             | Validation |  Test   |
| :----------------- | :--------: | :----:  |
| **ROUGE-1**        |    20.8    |  20.65  |
| **ROUGE-2**        |    5.7     |  6.01   |
| **ROUGE-L**        |    12.19   |  12.46  |
| **Avg. Precision** |    44.77%  |  45.23% |

> 📌 *Values are placeholders.*

---

## 🔹 LED — Long Multi-Document Summarization

### Dataset

* **WikiSum**
* **Evaluation Subset:** 1,000 validation + 1,000 test samples
* Each sample aggregates content from **multiple documents and sources**

### Why LED?

* Longformer-based **sparse attention**
* Supports very long inputs (8k–16k+ tokens)
* Designed to handle **concatenated multi-document contexts directly**

### Pipeline Design

* Documents concatenated with separators
* Global attention applied to key tokens
* No hierarchical preprocessing required for baseline evaluation

### Results (Indicative)

| Metric             | Validation |  Test  |
| :----------------- | :--------: | :----: |
| **ROUGE-1**        |    20.14   |  21.5  |
| **ROUGE-2**        |    8.9     |  9.1   |
| **ROUGE-L**        |    16.33   |  15.5  |
| **Avg. Precision** |    38.94%  |  38%   |

---

Perfect — we can add a **Challenges** section that’s concise, honest, and highlights what’s new in your personal extension. Here’s a professional way to phrase it for your **personal repo README**:

---

## ⚠️ Challenges

* **Compute Constraints:** Free Google Colab GPUs limited batch sizes, model input lengths, and runtime duration — strategies like **chunked summaries, batch tuning, and incremental evaluation** were reused from the baseline.
* **Pipeline Adaptation for Multi-Document Summarization:** Unlike the single-document phase, handling multiple documents required **hierarchical and fusion strategies** (for PEGASUS) and careful **input concatenation with separators** (for LED).

> 💡 Overall, the **basic challenges** from the internship baseline remain, but the **multi-document handling and pipeline design** introduced the main new engineering effort.

---

## 🔬 Comparative Insights

| Aspect            | PEGASUS                        | LED                         |
| ----------------- | ------------------------------ | --------------------------- |
| Input Length      | Limited (~1k tokens)           | Very long (8k–16k+)         |
| Pipeline Need     | Hierarchical / fusion required | Direct ingestion sufficient |
| Strength          | Salient news aggregation       | Long-context reasoning      |
| Engineering Focus | Pipeline design                | Attention configuration     |

---

## 🧠 Key Learnings

* Multi-document summarization is a **systems problem**
* Hierarchical pipelines essential when context length is limited
* Long-context models reduce preprocessing needs but introduce efficiency trade-offs
* ROUGE less reliable as abstraction and document count increase

---

## 🔮 Future Work

* Lightweight fine-tuning (LoRA / adapters) on MultiNews or WikiSum
* Hybrid extractive → abstractive pipelines
* Factuality and coherence evaluation beyond ROUGE
* Hierarchical extensions for LED for efficiency and consistency

---

## 🧰 Tech Stack

* **Language:** Python
* **Frameworks:** PyTorch, Hugging Face Transformers
* **Datasets:** CNN/DailyMail, ArxivSum, MultiNews, WikiSum
* **Evaluation:** ROUGE, Precision-based metrics
* **Environment:** Google Colab (GPU)

---

## 👤 Author

**Darshan Shirsat**
MTech AI & Data Science

---

⭐ *Personal extension of single-document summarization to multi-document systems. If you find it useful, consider giving it a star.*

---


If you want, I can also **suggest GitHub repo description + topics** optimized for recruiter visibility so it gets maximum attention. Do you want me to do that?
