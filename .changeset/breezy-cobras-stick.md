---
'@mastra/mongodb': minor
---

Added `MongoDBVector.hybridSearch()` to combine vector and full-text retrieval with optional Atlas native reranking. Configure independent retrieval queries, candidate limits, fusion weights, and reranking fields without changing existing query APIs.

Previously, applications called `hybridQuery()` and passed its results to a separately configured reranker. On a supporting Atlas deployment, retrieval and reranking can now run in one call without an application-side reranking client or separate Voyage API key:

```typescript
const results = await store.hybridSearch({
  indexName: 'articles',
  vector: { queryVector: embedding, limit: 50, numCandidates: 1000 },
  text: { query: question, paths: ['metadata.text'], limit: 50 },
  rerank: { query: question, paths: ['metadata.text'], numDocsToRerank: 50 },
  topK: 5,
});
```

Omit `rerank` for hybrid retrieval only, or use `vector.query` on an automatic-embedding index. Native reranking defaults to `rerank-3` and requires MongoDB 9.0+ on an Atlas M10-or-higher cluster with Native Reranking enabled.
