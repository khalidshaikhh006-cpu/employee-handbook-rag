# Employee Handbook RAG Assistant

An end-to-end Retrieval-Augmented Generation (RAG) system that answers questions from an employee handbook using semantic search, lexical search, hybrid retrieval, and neural reranking.

## Project Overview

This project builds a document-grounded question-answering assistant over an employee handbook.

Instead of relying only on an LLM's internal knowledge, the system retrieves relevant sections from the handbook and uses them as context for answer generation. This helps keep responses grounded in the source document and provides page-level traceability.

## Architecture

```text
Employee Handbook PDF
        ↓
Document Extraction & Cleaning
        ↓
Page-Aware Chunking
        ↓
Sentence Transformer Embeddings
        ↓
        ┌───────────────────┐
        │  Hybrid Retrieval │
        └───────────────────┘
           ↙             ↘
     Vector Search      BM25
           ↘             ↙
        Hybrid Ranking
              ↓
      Cross-Encoder Reranking
              ↓
        Relevant Context
              ↓
        FLAN-T5 Generator
              ↓
      Grounded Answer

Key Features
PDF document processing
Page-aware text extraction
Text chunking for retrieval
Sentence Transformer embeddings
ChromaDB vector search
BM25 lexical retrieval
Hybrid retrieval combining semantic and keyword search
Cross-Encoder reranking
FLAN-T5 answer generation
Source-page tracking
Evaluation of answer and page accuracy
Interactive question-answering interface

| Component           | Technology                |
| ------------------- | ------------------------- |
| Language            | Python                    |
| Environment         | Google Colab              |
| Embeddings          | Sentence Transformers     |
| Vector Database     | ChromaDB                  |
| Lexical Retrieval   | BM25                      |
| Reranking           | Cross-Encoder             |
| LLM                 | FLAN-T5                   |
| Document Processing | PyPDF                     |
| ML/NLP              | Hugging Face Transformers |

Retrieval Pipeline

The retrieval system uses a multi-stage approach:

1. Semantic Retrieval

The handbook chunks are converted into dense vector embeddings using a Sentence Transformer model.

ChromaDB is then used to retrieve semantically similar chunks for a given question.

2. BM25 Retrieval

BM25 provides lexical retrieval based on keyword overlap.

This helps retrieve relevant passages when the question contains specific terminology appearing in the document.

3. Hybrid Retrieval

Semantic similarity and BM25 scores are combined to improve retrieval robustness.
Hybrid Score =
α × Semantic Similarity
+
(1 − α) × BM25 Similarity

4. Cross-Encoder Reranking

The initial candidate passages are reranked using a Cross-Encoder to identify the passages most relevant to the user's question.

5. Grounded Answer Generation

The top-ranked context is passed to FLAN-T5 along with a constrained prompt.

The model is instructed to answer using only the retrieved handbook context rather than inventing information.

Evaluation

The system includes an evaluation set covering areas such as:

Annual leave
Maternity leave
Working from home
Rest periods
Attendance
Compassionate leave
Working hours

Two metrics are evaluated:

Answer Accuracy
Checks whether the generated answer contains the expected answer.

Page Accuracy
Checks whether the retrieved source page matches the expected handbook page.

The final evaluation achieved:
Answer Accuracy: 100%
Page Accuracy:   100%

Example

Question

How many days of annual leave can be carried over?

Answer

A maximum of 5 days' annual leave may be carried over from one year's entitlement to the next.

Source

Employee Handbook — Page 20

Project Structure
employee-handbook-rag/
│
├── employee_handbook_rag.ipynb
└── README.md
How to Run

The project was developed in Google Colab.

Open employee_handbook_rag.ipynb.
Upload the required employee handbook PDF when prompted.
Run the notebook cells sequentially.
Install the required Python packages when prompted.
Execute the retrieval and model initialization cells.
Run the evaluation section.
Use the final interactive interface to ask questions about the handbook.
Limitations
The system is currently designed around a single employee handbook.
Answer quality depends on retrieval quality.
The evaluation dataset is relatively small.
The current implementation is notebook-based rather than deployed as a production API or web application.
Future Improvements
Build a Streamlit web interface
Support multiple policy documents
Add document upload functionality
Improve evaluation using larger benchmark datasets
Add citation-aware answer generation
Experiment with stronger embedding and reranking models
Deploy the RAG pipeline as an API
Disclaimer
This project is a demonstration of a RAG-based document question-answering system. The employee handbook is used as the source document for the project.
