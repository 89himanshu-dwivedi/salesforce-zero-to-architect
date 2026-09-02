[Home](../index.md) / [18 · AI in Salesforce](index.md) / **RAG**

# RAG

5 topics · Series 18: AI in Salesforce

**Topics on this page**

- [Retrieval Augmented Generation](#retrieval-augmented-generation)
- [Chunking](#chunking)
- [RAG Embeddings](#rag-embeddings)
- [Vector Search](#vector-search)
- [Semantic Search](#semantic-search)

## Retrieval Augmented Generation

*Ground LLMs with retrieved knowledge at query time.*

### 🌱 Simple

*Beginner - plain language*

**RAG (Retrieval-Augmented Generation)** improves LLM answers by **retrieving relevant documents** at query time and feeding them into the prompt — so the model answers from **real, current knowledge** instead of just its training.

### 📏 Limits

*Governor & platform limits*

- Chunk→embed→retrieve→augment→generate→cite; vector store.
- Current/cheap to update vs fine-tuning; access-controlled retrieval; Data Cloud.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Chunking

*Split documents into retrievable pieces.*

### 🌱 Simple

*Beginner - plain language*

**Chunking** splits documents into **smaller pieces** (paragraphs, sections) so each can be embedded and retrieved independently — letting RAG fetch just the relevant part instead of whole documents.

### 📏 Limits

*Governor & platform limits*

- Size/overlap balance precision vs context; fixed/structural/semantic.
- Attach metadata (source/permissions); special tables/code; re-chunk on change.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## RAG Embeddings

*Vectorize chunks and queries for retrieval.*

### 🌱 Simple

*Beginner - plain language*

In RAG, **embeddings** convert each document chunk and the user's query into **vectors**, so retrieval can find chunks whose meaning is closest to the question — the matching engine of RAG.

### 📏 Limits

*Governor & platform limits*

- Embed chunks→vector index; embed query→cosine top-k; same model.
- Re-embed on change; hybrid (semantic+keyword); Data Cloud managed.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Vector Search

*Find nearest vectors in high-dimensional space.*

### 🌱 Simple

*Beginner - plain language*

**Vector search** finds the stored vectors **closest** to a query vector in high-dimensional space — the retrieval step that powers semantic search and RAG, returning the most similar chunks.

### 📏 Limits

*Governor & platform limits*

- Cosine/dot similarity; ANN (HNSW/IVF) for scale; top-k + re-rank.
- Metadata/permission filtering; vector DB; hybrid; recall/latency tuning.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Semantic Search

*Search by meaning, not keywords.*

### 🌱 Simple

*Beginner - plain language*

**Semantic search** finds results by **meaning** rather than exact keywords — so "how do I reset my password" matches an article titled "Account Recovery Steps" even with no shared words.

### 📏 Limits

*Governor & platform limits*

- Meaning-based via embeddings+vector search; synonyms/intent/context.
- Hybrid with keyword for exact terms; re-ranking; access-controlled.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

---

## Connect

These pages carry the **definitions and limits** only. The advanced depth, real-world
scenarios, error playbooks, best-option reasoning and interview questions are kept aside.

If you would like them, or you want to talk about anything on this page:

- **LinkedIn** - [in/himanshukumar-sf](https://www.linkedin.com/in/himanshukumar-sf/)
- **X** - [@kum60094](https://x.com/kum60094)
- **GitHub** - [89himanshu-dwivedi](https://github.com/89himanshu-dwivedi)
- **Email** - [himanshu.jee.1996@gmail.com](mailto:himanshu.jee.1996@gmail.com)

*- Himanshu Kumar*
