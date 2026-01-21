# 🚀 Progressive Summarization — Multi-Document

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

> ⚠️ **Scope Clarification:** 
> This project currently focuses on **inference and pipeline design** for multi-document summarization. 
> All reported results are generated using **pretrained PEGASUS and LED models** without any fine-tuning. 
> Any mention of fine-tuning (e.g., LoRA/adapters) in "Future Work" refers to **planned extensions**, not implemented features.

---

---

## 🔹 PEGASUS — Multi-Document News Summarization

### Dataset

* **MultiNews** — Each sample aggregates **2–10 related news articles** describing the same event
* **Evaluation Subset:** 1,000 validation + 1,000 test samples
* **Document Separation:** Articles within each sample are separated by `|||||` tokens
* Preprocessing includes **splitting documents**, **removing empty lines**, and **truncating inputs** to 1024 tokens for PEGASUS

### Why PEGASUS?

* Pretrained with **Gap Sentence Generation**, effective at identifying salient information
* Strong performance on news-style summarization
* Limited input length → requires **hierarchical/fusion pipeline**

---

### 🛠️ Pipeline Implementation — PEGASUS

**Overview:** Hierarchical summarization — first compress individual documents, then fuse salient info into a final summary.

```
[Input Sample: Multiple Articles] 
        │  (split by '|||||')
        ▼
┌─────────────────────────────┐
│ Document-Level Summaries     │  <- Each document summarized individually (max_len=256)
└─────────────────────────────┘
        │
        ▼
┌─────────────────────────────┐
│ Fusion Step                  │  <- Concatenate document summaries
└─────────────────────────────┘
        │
        ▼
┌─────────────────────────────┐
│ Final Abstractive Summary    │  <- Summarize fused text (max_len=150)
└─────────────────────────────┘
```

**Steps:**

1. Split each multi-document sample using `|||||` separators
2. Generate **document-level summaries** using PEGASUS
3. Concatenate summaries into a single **fused text**
4. Generate **final abstractive summary** from fused text

This approach **mitigates truncation**, handles redundancy, and preserves salient information.

### Results (Indicative)

| Metric             | Validation |  Test  |
| :----------------- | :--------: | :----: |
| **ROUGE-1**        |    20.8    |  20.65 |
| **ROUGE-2**        |     5.7    |  6.01  |
| **ROUGE-L**        |    12.19   |  12.46 |
| **Avg. Precision** |   44.77%   | 45.23% |

---

## 🔹 LED — Long Multi-Document Summarization

### Dataset

* **WikiSum** — Each sample aggregates content from **multiple documents and sources**
* **Evaluation Subset:** 1,000 validation + 1,000 test samples
* **Document Separation:** Documents concatenated with **special separators**, global attention applied to key tokens
* Supports ultra-long inputs (8k–16k+ tokens)

### Why LED?

* Longformer-based **sparse attention**, optimized for **very long concatenated contexts**
* Can handle multi-document inputs **without hierarchical preprocessing**

---

### 🛠️ Pipeline Implementation — LED

**Overview:** Direct long-context summarization

```
[Input Sample: Multiple Articles] 
        │  (concatenated with separators)
        ▼
┌─────────────────────────────┐
│ LED Model (Long-Context)     │  <- Sparse + global attention
└─────────────────────────────┘
        │
        ▼
┌─────────────────────────────┐
│ Final Abstractive Summary    │
└─────────────────────────────┘
```

**Steps:**

1. Concatenate all documents with special separators
2. Tokenize and assign **global attention** to key tokens
3. Generate final summary directly without hierarchical fusion

### Results (Indicative)

| Metric             | Validation | Test |
| :----------------- | :--------: | :--: |
| **ROUGE-1**        |    20.14   | 21.5 |
| **ROUGE-2**        |     8.9    |  9.1 |
| **ROUGE-L**        |    16.33   | 15.5 |
| **Avg. Precision** |   38.94%   |  38% |

---

## ⚠️ Challenges

* **Compute Constraints:** Limited Colab GPU memory restricted batch sizes, model input lengths, and runtime duration
* **Hierarchical Pipeline Design:** PEGASUS required document-level compression + fusion; LED required careful tokenization & global attention
* **Data Preprocessing:** Multi-document samples needed splitting, cleaning, truncation, and concatenation strategies
* **Evaluation & Resource Trade-offs:** Large models necessitated chunked evaluation and incremental ROUGE scoring to track progress

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
* Hierarchical pipelines are essential when input length is limited
* Long-context models reduce preprocessing needs but introduce efficiency trade-offs
* ROUGE becomes less reliable as abstraction and document count increase

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

