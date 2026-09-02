[Home](../index.md) / [18 · AI in Salesforce](index.md) / **AI Fundamentals**

# AI Fundamentals

7 topics · Series 18: AI in Salesforce

**Topics on this page**

- [Supervised Learning](#supervised-learning)
- [Unsupervised Learning](#unsupervised-learning)
- [LLM](#llm)
- [Transformer](#transformer)
- [Tokens](#tokens)
- [Embeddings](#embeddings)
- [Generative AI](#generative-ai)

## Supervised Learning

*Learn from labeled examples to predict outcomes.*

### 🌱 Simple

*Beginner - plain language*

**Supervised learning** trains a model on **labeled data** (inputs with known correct answers) so it can **predict** the answer for new, unseen inputs — like predicting whether a lead will convert from past won/lost leads.

### 📏 Limits

*Governor & platform limits*

- Needs labeled data; classification vs regression; train/val/test.
- Metrics by error cost; watch overfitting, imbalance, leakage, drift.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Unsupervised Learning

*Find structure in unlabeled data.*

### 🌱 Simple

*Beginner - plain language*

**Unsupervised learning** finds **patterns and structure** in data that has **no labels** — like grouping customers into segments by behavior without being told the groups in advance.

### 📏 Limits

*Governor & platform limits*

- Clustering, dimensionality reduction, anomaly detection; no labels.
- Hard to evaluate (silhouette + business review); feeds supervised.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## LLM

*Large Language Models that generate text.*

### 🌱 Simple

*Beginner - plain language*

An **LLM (Large Language Model)** is a neural network trained on vast text to **predict the next token**, enabling it to generate human-like text, answer questions, summarize, and reason — the engine behind ChatGPT, Claude, and Salesforce's generative features.

### 📏 Limits

*Governor & platform limits*

- Transformer next-token predictor; instruction-tuned/aligned.
- Context window, temperature, hallucination, cost/latency; needs Trust Layer.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Transformer

*The neural architecture behind modern LLMs.*

### 🌱 Simple

*Beginner - plain language*

The **Transformer** is the neural-network architecture (2017 "Attention Is All You Need") that powers modern LLMs — using **self-attention** to weigh how each word relates to every other, processing sequences in parallel.

### 📏 Limits

*Governor & platform limits*

- Self-attention + multi-head + positional encoding; parallel processing.
- O(n²) attention → context-window limits; decoder-only for generation.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Tokens

*The units LLMs read, generate, and bill on.*

### 🌱 Simple

*Beginner - plain language*

**Tokens** are the chunks of text — roughly word-pieces (~4 characters / ¾ of a word in English) — that an LLM reads and generates. Models process and are **priced per token**, and their **context window** is measured in tokens.

### 📏 Limits

*Governor & platform limits*

- Subword units (~4 chars EN); prompt+completion fit context window.
- Per-token pricing; varies by language/code; manage via RAG/chunking.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Embeddings

*Numeric vectors capturing meaning of text.*

### 🌱 Simple

*Beginner - plain language*

**Embeddings** turn text (a word, sentence, or document) into a **vector of numbers** that captures its **meaning**, so similar meanings sit close together in vector space — the foundation of semantic search and RAG.

### 📏 Limits

*Governor & platform limits*

- Dense vectors; cosine similarity; semantic search/RAG foundation.
- Same model for query+docs; re-embed on change; vector DB storage.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Generative AI

*AI that creates new content from prompts.*

### 🌱 Simple

*Beginner - plain language*

**Generative AI** creates **new content** — text, images, code, summaries — from a prompt, rather than just classifying or predicting. In CRM it drafts emails, summarizes cases, and answers questions.

### 📏 Limits

*Governor & platform limits*

- Creates content (vs predictive scoring); conditioned on prompt+grounding.
- Prompt Builder/Agentforce/Model Builder; Trust Layer; human-in-the-loop.

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
