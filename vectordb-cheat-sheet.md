# Vector DB 
1. What is a Vector DB?
Imagine you have thousands of photos or text docs. A computer can't "read" them like we do, so it turns them into
Vectors (long lists of numbers) using an Embedding Model.
 * Input: Text, Images, Audio.
 * Embedding: The process of turning input into a vector.
 * Vector DB: A specialized database that stores these numbers and lets you search for "similar" things (Nearest Neighbor Search).

## Tools
* [CLI for Weaviate, Milvus, Chroma, Qdrant, and other vector DBs](https://github.com/maximilien/weave-cli)

## Links
* [vector DB comparison](https://superlinked.com/vector-db-comparison)
  * [cosine simularity](https://en.wikipedia.org/wiki/Cosine_similarity)

##  pgvector vs. Dedicated Vector DB
┌─────────────┬─────────────────────────────────────────────────────┬────────────────────────────────────┐
│ Feature     │ pgvector (Postgres)                                 │ Dedicated Vector DB (Pinecone,     │
│             │                                                     │ Milvus)                            │
├─────────────┼─────────────────────────────────────────────────────┼────────────────────────────────────┤
│ Best For    │ Small to Medium scale (~10M vectors).               │ Massive scale (Billions of         │
│             │                                                     │ vectors).                          │
│ Ease of Use │ Great if you already use Postgres. No new tech.     │ New system to learn and maintain.  │
│ Hybrid      │ Best: Combine SQL (where id=123) + Vector.          │ Harder: Must sync data between two │
│ Search      │                                                     │ DBs.                               │
│ Speed       │ Good, but can slow down under heavy load.           │ Extremely fast (optimized for just │
│             │                                                     │ vectors).                          │
│ ClickHouse  │ Note from image: High-speed analytical DB that also │                                    │
│             │ supports vectors.                                   │                                    │
└─────────────┴─────────────────────────────────────────────────────┴────────────────────────────────────┘

## Search Algorithms: How the "Magic" Happens
When searching, you don't want to check every single vector (that's too slow). You use an index:

* HNSW (Hierarchical Navigable Small World)
   * The "Luxury" choice.
   * How it works: Creates a "web" of points. It jumps between far-apart points to get close, then zooms in.
   * Pros: Super fast search, high accuracy, handles new data easily.
   * Cons: Uses a lot of RAM (Memory).
* IVFFlat (Inverted File Flat)
   * The "Budget" choice.
   * How it works: Groups vectors into "buckets" (clusters). It finds the right bucket and only searches inside it.
   * Pros: Low memory usage, fast to set up.
   * Cons: Accuracy can drop if you add too much new data without rebuilding the index.

## Metadata Filtering: Pre vs. Post
If you want to search for "Shoes" (Vector) that are "Red" (Metadata):
* Pre-filtering (The Right Way):
   1. Find all Red items first.
   2. Search for the most similar Shoes within that red list.
   * Result: You always get exactly what you asked for.
* Post-filtering (The Fast but Risky Way):
   1. Find the 10 most similar Shoes first.
   2. Remove any that aren't Red.
   * Result: You might end up with only 1 or 2 results (or zero!) because the best matches weren't red.

## The Golden Rule: Dimensions, Memory, & Speed
 * Dimension: The "length" of the vector (e.g., 1536 numbers).
 * Memory: Higher dimensions = More RAM needed.
 * Speed: Higher dimensions = More math = Slower queries.
 * Pro Tip: Finding the balance between "high detail" (high dimensions) and "low cost" (low dimensions) is the
   key to a good Vector DB setup.
