# Employee Handbook RAG Assistant

An end-to-end Retrieval-Augmented Generation (RAG) system for answering questions from an employee handbook using semantic retrieval, lexical retrieval, hybrid ranking, neural reranking, and grounded answer generation.

## Project Overview

This project builds a document-grounded question-answering system over an employee handbook.

Instead of relying only on an LLM to generate answers, the system first retrieves relevant sections from the handbook and then provides those sections as context to the language model.

The system is designed to reduce unsupported answers by instructing the generator to answer using only the retrieved handbook context.

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
Page-aware chunking for retrieval
Sentence Transformer embeddings
ChromaDB vector search
BM25 lexical retrieval
Hybrid semantic + keyword retrieval
Cross-Encoder neural reranking
FLAN-T5 answer generation
Source-page tracking
Evaluation of answer and page accuracy
Interactive question-answering interface
| Component           | Technology                |
| ------------------- | ------------------------- |
| Language            | Python                    |
| Environment         | Google Colab              |
| Document Processing | PyPDF                     |
| Embeddings          | Sentence Transformers     |
| Vector Database     | ChromaDB                  |
| Lexical Retrieval   | BM25                      |
| Reranking           | Cross-Encoder             |
| LLM                 | FLAN-T5                   |
| ML/NLP              | Hugging Face Transformers |

Retrieval Pipeline

The retrieval system uses a multi-stage approach.

1. Semantic Retrieval

The handbook is divided into chunks and converted into dense vector embeddings using a Sentence Transformer model.

ChromaDB is used to retrieve semantically similar chunks for a given question.

This allows the system to retrieve relevant content even when the wording of the question differs from the wording used in the handbook.

2. BM25 Retrieval

BM25 provides lexical retrieval based on keyword overlap.

This is useful when a question contains specific terminology that appears directly in the handbook.

3. Hybrid Retrieval

The semantic and lexical retrieval signals are combined into a hybrid score:

Hybrid Score =
α × Semantic Similarity
+
(1 − α) × BM25 Similarity
This combines the strengths of semantic search and traditional keyword-based retrieval.

4. Cross-Encoder Reranking

The initial retrieval stage produces a set of candidate passages.

These candidates are then passed through a Cross-Encoder reranker, which evaluates the relevance of each passage against the original question.

The highest-ranked passages are selected as the final context.

5. Grounded Answer Generation

The retrieved context is passed to FLAN-T5 along with a constrained prompt.

The model is instructed to answer using only the provided employee handbook context.

If the required information cannot be found in the retrieved context, the system is instructed not to invent an answer.

Evaluation

The system includes an evaluation dataset covering topics such as:

Annual leave
Maternity leave
Working from home
Rest periods
Attendance
Compassionate leave
Working hours
Home-working equipment and safety

Two evaluation metrics are used.

Answer Accuracy

Checks whether the generated answer contains the expected answer.

Page Accuracy

Checks whether the source page returned by the system matches the expected handbook page.

Results

On the evaluation dataset used for this project:

Answer Accuracy: 100%
Page Accuracy:   100%

These results reflect performance on the project's evaluation set and are not intended to represent production-scale benchmark performance.

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
├── README.md
├── requirements.txt
└── .gitignore
How to Run

The project was developed in Google Colab.

1. Clone the repository
git clone https://github.com/khalidshaikhh006-cpu/employee-handbook-rag.git
cd employee-handbook-rag
2. Install dependencies
pip install -r requirements.txt
3. Open the notebook

Open:

employee_handbook_rag.ipynb

The notebook contains the complete pipeline including:

Package installation
PDF loading and text extraction
Text cleaning and page-aware chunking
Embedding generation
ChromaDB vector indexing
BM25 indexing
Hybrid retrieval
Cross-Encoder reranking
FLAN-T5 answer generation
Evaluation
Interactive question answering
4. Run the notebook

Execute the notebook cells sequentially and provide the employee handbook PDF when prompted.

Limitations
The current implementation is designed around a single employee handbook.
Answer quality depends on retrieval quality.
The evaluation dataset is relatively small.
The current implementation is notebook-based rather than deployed as a production API or web application.
The current evaluation focuses on answer containment and source-page accuracy rather than broader generative evaluation metrics.
Future Improvements
Build a Streamlit web interface
Support multiple policy documents
Add document upload functionality
Expand the evaluation benchmark
Add citation-aware answer generation
Experiment with stronger embedding and reranking models
Add conversation history and follow-up question support
Deploy the RAG pipeline as an API
Disclaimer

This project is a demonstration of a RAG-based document question-answering system.

The employee handbook is used as the source document for the project.
