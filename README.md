# video-game
## Amazon Video Game Product Reviews RAG System
A Retrieval-Augmented Generation (RAG) pipeline designed to process, store, and analyze thousands of customer reviews from the Amazon Product Reviews dataset (McAuley Lab). The system uses a localized, resource-efficient vectorization fallback combined with Google Gemini to generate highly grounded answers directly linked to user evidence chunks.

## Overview
Deep evaluation done into customer reviews allowing users to ask video game related questions regarding game experiences and receive synthesized, cited summaries bound tightly to raw localized data structures.
The scope of the project is to analyze any customer reviews based on video games. The system should not answer any question outside the related topic. To enable this a prompt is engineered in a way that guardrails are set to limit what the LLM can answer. About 50,000 reviews were used to train the model. The process starts by data crawling from McAuley lab, it is then preprocessed(removing punctuation, html tags), chunking using recursive character splitting, embedding and finally prompting to see whether the pipeline can now handle user questions.

## Key Technical Specifications
| Component	| Selection |
|Topic Boundary |	Customer experiences, ratings, and product feedback for video games|
|Source Data |	Amazon Product Reviews Dataset / UCSD McAuley Lab|
|Vectorization	| Local TfidfVectorizer fallback (384 dimensional feature space) |
|Vector Database	| ChromaDB (Localized persistent directory)|
|Distance Metric |	Cosine Space (hnsw:space = cosine) |
|Chunking Strategy |	Recursive Character Text Splitting (1,000 char size / 200 char overlap)|
|LLM Model Target |	Google Gemini 3.5 Flash-Lite (models/gemini-3.5-flash-lite)|
|Generation Hyperparameters	| Temperature: 0.2 | Top-P: 1.0 | Top-K Retrieval: 5 |



## Directory Structure
The data pipeline relies on a clean partition between raw, normalized, intermediate chunk segments, and persistent database folders:
text
```
├── data/
│   ├── raw/        # unmodified source review TXT records
│   ├── clean/      # Deduplicated, normalized Unicode / text files
│   ├── processed/  # Intermediate extracted data arrays and chunks (.csv, .npy)
│   ├── logs/       # Dataset logs and entry points
│   └── vector_db/  # ChromaDB collection
├── .env            # Environment configurations (API keys, model tags)
└── videogame.ipynb  # Primary notebook containing tasks 0 through 8

```

## Sample Retrieval Output
```
You: What did reviewers say about the gameplay?

Assistant:
Answer:
Based on the provided reviews, gameplay feedback varied by game:
- One reviewer described a game as a "constant struggle" due to severe controller recognition flaws [document_038.txt].
- Another highlighted stealth gameplay, one-hit kills, and ninja gadgets like shurikens as what makes the game awesome [document_009.txt].

Sources Used:
- Filename: document_009.txt
  Chunk ID: document_009_chunk_0560
  Similarity: 0.3799
  Evidence: "Stealth gameplay and one hit kills are what makes this game awesome Did I forget ninja gadgets..."
```
