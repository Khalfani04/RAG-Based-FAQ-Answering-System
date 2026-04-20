# RAG-Based-FAQ-Answering-System
A Retrieval-Augmented Generation (RAG) system that answers questions about the **Honours Bachelor of Artificial Intelligence (HBAI) program at Durham College

# 🔍 RAG-Based FAQ Answering System
**Project 10 — NLP Course | Durham College HBAI Program**

A Retrieval-Augmented Generation (RAG) system that answers natural language questions about the Honours Bachelor of Artificial Intelligence (HBAI) program at Durham College. The system retrieves relevant context from a curated knowledge base and generates grounded answers using a fully local LLM — no API key required.

---

## 🧠 How It Works

```
User Question
     │
     ▼
[Embed with all-MiniLM-L6-v2]
     │
     ▼
[Query ChromaDB → Top-3 Chunks]
     │
     ▼
[Build Prompt: Context + Question]
     │
     ▼
[Flan-T5-Base → Grounded Answer]
     │
     ▼
Answer + Source References
```

1. **Embed** — The question is encoded into a 384-dim vector using `sentence-transformers/all-MiniLM-L6-v2`
2. **Retrieve** — ChromaDB returns the top-3 most semantically similar FAQ chunks
3. **Augment** — A prompt is built instructing the model to answer *only* from retrieved context
4. **Generate** — `google/flan-t5-base` generates the final answer

---

## 🛠️ Stack

| Component | Library / Model |
|---|---|
| Embeddings | `sentence-transformers/all-MiniLM-L6-v2` |
| Vector Store | `ChromaDB` (in-memory) |
| LLM | `google/flan-t5-base` |
| Runtime | Google Colab (CPU or GPU) |
| Language | Python 3.10+ |

No OpenAI API key or paid services required.

---

## 🚀 Setup & Run

### Option 1: Google Colab (Recommended)
1. Open `RAG_FAQ_Answering_System.ipynb` in Google Colab
2. Go to **Runtime > Run All**
3. First run takes ~3 minutes to download models

### Option 2: Local
```bash
git clone https://github.com/Khalfani04/rag-faq-answering-system
cd rag-faq-answering-system
pip install chromadb sentence-transformers transformers accelerate
jupyter notebook RAG_FAQ_Answering_System.ipynb
```

> **Note:** pip may show opentelemetry dependency warnings during install — these are unrelated to this project and can be safely ignored.

---

## 📁 Repository Structure

```
rag-faq-answering-system/
│
├── RAG_FAQ_Answering_System.ipynb   # Main notebook (all steps)
├── test_questions.csv               # 30 evaluation questions + scores
├── README.md                        # This file
└── evaluation_report.md             # Metrics, analysis, failure cases
```

---

## 📊 Sample Output

**In-scope question:**
```
Question: How does the co-op work term work?

Answer: Students complete one four-month paid work placement with an AI or
technology company in the third year.

Source Chunks: [doc_011] similarity=0.71, [doc_012] similarity=0.63, [doc_014] similarity=0.58
```

**Out-of-scope question:**
```
Question: Is the HBAI program available online?

Answer: I do not have enough information to answer that.

Source Chunks: [doc_002] similarity=0.29, [doc_007] similarity=0.29, [doc_011] similarity=0.25
```
The system correctly refuses to hallucinate when the answer isn't in the knowledge base.

---

## 📈 Evaluation Results

| Metric | Score |
|---|---|
| Retrieval Recall@3 | See `test_questions.csv` |
| Answer Faithfulness | See `test_questions.csv` |

Full evaluation methodology and failure analysis in `evaluation_report.md`.

---

## ⚙️ Extending the System

- **Larger LLM:** Swap `flan-t5-base` for `flan-t5-large` or a Mistral model via Ollama for better generation quality
- **Better retrieval:** Add a cross-encoder re-ranker (`cross-encoder/ms-marco-MiniLM-L-6-v2`)
- **Custom knowledge base:** Replace `faq_documents` with any domain-specific text corpus
- **Persistent storage:** Switch `chromadb.Client()` to `chromadb.PersistentClient(path="./db")` to save the vector store to disk

---

## 👤 Author
**Khalfani Norman** — Year 3, Honours Bachelor of Artificial Intelligence  
Durham College | [github.com/Khalfani04](https://github.com/Khalfani04)
