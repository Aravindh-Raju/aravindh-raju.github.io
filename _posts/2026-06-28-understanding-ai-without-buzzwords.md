---
title: "Understanding AI Without The Buzzwords"
categories:
  - Blog
tags:
  - AI
  - LLM
  - GenAI
  - RAG
  - Machine Learning
---

As AI started becoming a bigger part of my work and learning, I kept running into terms like:

- Tokens
- Embeddings
- Vectors
- Context Window
- Chunking
- Semantic Search
- RAG
- Reranking

Every article, video, or tutorial seemed to use these terms as if everyone already knew what they meant.

The problem was that every explanation seemed to assume I understood the other concepts already.

To understand embeddings, I needed to know vectors.

To understand vectors, I needed to know embeddings.

To understand RAG, I needed to understand chunking, embeddings, semantic search, vector databases, and a dozen other things.

It felt like one of those interview questions where every answer creates three new questions.

After spending some time learning, experimenting, and building things, the concepts finally started making sense.

So I thought I'd share the explanations that helped everything click for me.

If you're someone who keeps hearing these terms but never quite understood how they fit together, this post is for you.

---

## First: How AI Actually Sees Text

Humans see words.

AI doesn't.

To an LLM (Large Language Model), everything eventually becomes numbers.

Before understanding anything else, this is probably the most important thing to know.

When we see:

```text
Kubernetes
```

we immediately associate it with containers, deployments, pods, and cloud-native applications.

An AI model doesn't think like that.

It converts text into numerical representations that it can process mathematically.

Everything else in this blog builds on that idea.

---

## Tokens

One of the first terms you'll hear is **tokens**.

A token is simply a small piece of text.

For example:

```text
I love AWS
```

might become:

```text
"I"
"love"
"AWS"
```

Three tokens.

But tokenization isn't always based on words.

For example:

```text
unbelievable
```

might become:

```text
"un"
"believ"
"able"
```

depending on the tokenizer being used.

This matters because AI models process information using tokens.

They also limit conversations, context windows, and often pricing based on tokens.

Not words.

---

## Context Window

The context window is the amount of information a model can remember while generating a response.

Think of it like working memory.

Imagine reading a book.

If you could only remember the last two pages, you'd constantly lose track of the story.

If you could remember the last hundred pages, understanding the story becomes much easier.

LLMs work similarly.

A larger context window means the model can consider more information before responding.

That's why newer models can analyze larger documents, longer conversations, and bigger codebases than older ones.

---

## Embeddings

This was probably the concept that confused me the most.

The explanation that finally clicked was:

> An embedding is a numerical representation of meaning.

Let's take three words:

```text
Dog
Puppy
Golden Retriever
```

Humans instantly know they're related.

Embeddings help AI understand those relationships too.

The model converts text into a list of numbers.

Something like:

```text
Dog -> [0.24, -0.18, 0.67, ...]
Puppy -> [0.21, -0.15, 0.63, ...]
```

The actual lists are much larger, but the idea is the same.

Words and sentences with similar meanings end up with similar numerical representations.

That's what makes embeddings so powerful.

---

## Vectors

Once text is converted into an embedding, it becomes a vector.

Simply put:

> A vector is the list of numbers that represents the meaning of the text.

You can think of it as:

```text
Text
  ↓
Embedding
  ↓
Vector
```

For practical purposes, people often use the terms embeddings and vectors interchangeably.

When someone says:

> "Store the vectors in a vector database"

they're essentially talking about storing embeddings.

---

## Why Do We Need Embeddings?

Imagine searching for:

```text
How do I deploy an application on Kubernetes?
```

But the document contains:

```text
Deploying workloads to a Kubernetes cluster
```

Traditional keyword search might struggle because the wording is different.

The meaning is similar, but the words aren't identical.

Embeddings solve this problem because they focus on meaning instead of exact word matches.

And that's where semantic search comes in.

---

## Semantic Search

Traditional search asks:

> Do the words match?

Semantic search asks:

> Does the meaning match?

For example:

```text
How do I scale pods?
```

might return:

```text
Horizontal Pod Autoscaler Guide
```

even though the wording is different.

The system understands that both are talking about similar concepts.

The first time I learned this, it felt like magic.

It turns out the magic is mostly vectors, similarity calculations, and a lot of math.

Still pretty impressive.

---

## Chunking

LLMs have limits.

You can't always send an entire book, wiki, or large PDF to a model.

So documents are split into smaller sections called chunks.

For example:

```text
100-page document
        ↓
Chunk 1
Chunk 2
Chunk 3
Chunk 4
...
```

Each chunk is converted into embeddings and stored separately.

This allows the system to retrieve only the most relevant pieces later.

Choosing chunk size is important.

Too small:

- Context gets lost

Too large:

- Retrieval becomes less accurate

Finding the right chunk size often involves experimentation.

As is tradition in software engineering.

---

## Vector Databases

Once embeddings are generated, they need to be stored somewhere.

That's where vector databases come in.

Popular examples include:

- Pinecone
- Weaviate
- Chroma
- OpenSearch Vector Search

Unlike traditional databases that search using exact matches, vector databases search using similarity.

You can ask:

> "Find content similar to this question"

and the database returns the closest matches based on meaning.

---

## RAG (Retrieval-Augmented Generation)

This is where many modern AI applications bring everything together.

RAG stands for:

**Retrieval-Augmented Generation**

The flow looks like this:

```text
User Question
       ↓
Convert Question to Embedding
       ↓
Search Vector Database
       ↓
Retrieve Relevant Chunks
       ↓
Send Chunks + Question to LLM
       ↓
Generate Response
```

Instead of relying only on what it learned during training, the model receives additional context from your documents.

That's why modern AI assistants can answer questions about:

- Company documentation
- Internal knowledge bases
- PDFs
- Wikis
- Product manuals

without retraining the model.

Once I understood RAG, a lot of enterprise AI use cases suddenly started making sense.

---

## Reranking

This is another concept I discovered recently.

Imagine semantic search returns:

```text
Document A
Document B
Document C
Document D
```

All of them are somewhat relevant.

But which ones are the best matches?

That's where reranking comes in.

A reranking model takes the retrieved results and reorders them based on relevance.

Think of semantic search as creating a shortlist.

Think of reranking as choosing the best candidates from that shortlist.

This helps ensure that the most useful information reaches the LLM.

And better information usually means better answers.

---

## How Everything Connects

When I finally understood the entire flow, everything started clicking.

```text
Document
   ↓
Chunking
   ↓
Embeddings
   ↓
Vectors
   ↓
Vector Database
   ↓
Semantic Search
   ↓
Reranking
   ↓
Relevant Chunks
   ↓
LLM Context Window
   ↓
Response
```

Seeing it as one pipeline made it much easier to understand.

Instead of treating these as separate buzzwords, I could finally see how each piece contributed to the overall system.

---

## There's Still More to Learn

Of course, this only scratches the surface.

As I continued learning, I realized there are many more concepts behind modern AI systems, such as:

- Model Parameters
- Fine-Tuning and Model Customization
- Training vs Inference
- Quantization
- AI Quality Evaluation
- Prompt Engineering
- Agentic Workflows
- Model Training Pipelines

Each of these topics could easily deserve a blog of its own.

For now, my goal was simply to understand the building blocks that appear most often when people talk about LLMs, RAG applications, and modern AI systems.

The good news is that once concepts like embeddings, vectors, chunking, and semantic search start making sense, many of the advanced topics become much easier to follow.

The rest is a journey for another day.

---

## Final Thoughts

When I first started reading about AI systems, all these terms felt overwhelming.

Every article seemed to assume I already knew what embeddings, vectors, chunking, and RAG meant.

What helped me most was realizing that they're all pieces of the same puzzle.

At a high level:

- Tokens help the model process text
- Context windows determine how much information it can remember
- Embeddings convert meaning into numbers
- Vectors store those numerical representations
- Chunking breaks large documents into manageable pieces
- Semantic search finds relevant content
- Reranking improves search quality
- RAG combines everything to generate better answers

Once I saw how they connected, the concepts became much less intimidating.

Hopefully this explanation helps someone else avoid the same confusion I had.

---

## Over to You

What was the AI concept that took you the longest to understand?

For me, embeddings and vectors were easily the biggest source of confusion. Once those clicked, everything else started making a lot more sense.