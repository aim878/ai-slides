# Day 16: RAG Evaluations & Advanced Techniques

## Slide 1: Title
**Day 16: RAG Evaluations & Advanced Techniques**
- Building on Day 15: From Basic RAG to Production-Ready Systems
- Focus: Scientific Evaluation & Optimization

---

## Slide 2: Recap - What We've Built So Far
**Your RAG Journey**
- ✅ Day 14: Prompt Engineering & Context Management
- ✅ Day 15: RAG Pipeline with LangChain & Chroma
- 🎯 Today: Making RAG Production-Ready

---

## Slide 3: The RAG Reality Check
**What's Great About RAG:**
- Quick to build (minutes to working prototype)
- Highly scalable (100x more data? No problem!)
- Saves context length
- Focuses on relevant information only

**What's Not So Great:**
- It's basically a hack (performance trick)
- Very empirical (trial and error)
- Unexpected failures
- "Whack-a-mole" debugging

---

## Slide 4: Why Evaluations Matter
**The Problem:**
- How do you know if your RAG is actually working?
- Intuition isn't enough
- Need quantitative metrics

**The Solution:**
- Build evaluation framework
- Measure retrieval quality
- Measure answer quality
- Iterate scientifically

---

## Slide 5: The Three Steps of RAG Evaluation
**Step 1: Build Golden Dataset**
- Curate test questions & reference answers
- Include keywords for context validation
- Get from real users (best source!)
- Living, breathing test set

**Step 2: Measure Retrieval**
- MRR, NDCG, Precision, Recall

**Step 3: Measure Answers**
- LLM-as-a-Judge approach
- Accuracy, Completeness, Relevance

---

## Slide 6: Retrieval Metrics Explained
**Mean Reciprocal Rank (MRR):**
- Where is the first relevant chunk?
- 1st position = 1.0, 2nd = 0.5, 3rd = 0.33
- Higher is better

**NDCG (Normalized Discounted Cumulative Gain):**
- How well distributed are relevant chunks?
- Penalizes relevant content appearing late
- Perfect score = 1.0

**Keyword Coverage:**
- % of required keywords found in retrieved chunks
- Similar to recall metric

---

## Slide 7: Precision vs Recall
**Recall (More Important for RAG):**
- "Did we find the relevant content?"
- % of tests where top-k chunks contain hits
- Focus: Don't miss important information

**Precision:**
- "How much junk did we retrieve?"
- % of retrieved chunks that are relevant
- Less critical for RAG (LLMs handle noise)

---

## Slide 8: LLM-as-a-Judge
**Why Use LLMs for Evaluation?**
- Better than word overlap metrics
- Understands semantic similarity
- Can evaluate multiple dimensions

**Evaluation Dimensions:**
- **Accuracy**: Is the answer correct?
- **Completeness**: Does it include all needed info?
- **Relevance**: Is there unnecessary information?

**Best Practice:**
- Use stronger model as judge
- Structured outputs for consistency
- Score 1-5 on each dimension

---

## Slide 9: Experimental Results - Baseline
**Initial Performance (Day 15):**
- MRR: 0.7298 🔴
- NDCG: 0.7387
- Keyword Coverage: 83.8%
- Answer Accuracy: 3.99 🔴

**What This Means:**
- Relevant chunk typically in 2nd-3rd position
- Missing ~16% of keywords
- Answers incomplete/inaccurate

---

## Slide 10: Experiment 1 - Chunk Size
**Testing Different Chunk Sizes:**
- Original: 1000 chars → MRR 0.7298
- Smaller: 500 chars → MRR 0.7604 ✅
- Larger: 1667 chars → MRR 0.7475

**Key Insight:**
- Smaller chunks = better matching
- But risk losing context
- Need to balance with retrieval count (k)

---

## Slide 11: Experiment 2 - Encoder Models
**Comparing Embedding Models:**
- HuggingFace (all-MiniLM-L6-v2): Baseline
- OpenAI text-embedding-3-small: MRR 0.7849 ✅
- OpenAI text-embedding-3-large: MRR 0.7903 ✅✅

**Result After Optimization:**
- MRR: 0.7903 🟡 (from 0.7298)
- Accuracy: 4.21 🟡 (from 3.99)
- Significant improvement!

---

## Slide 12: The Zoo of Advanced RAG Techniques
**10 Pro Techniques:**
1. Chunking R&D
2. Encoder Selection
3. Prompt Engineering
4. Document Pre-processing
5. Query Rewriting
6. Query Expansion
7. Reranking
8. Hierarchical RAG
9. Graph RAG
10. Agentic RAG

---

## Slide 13: Technique 1 & 2 - Chunking & Encoders
**Chunking R&D:**
- Experiment with different strategies
- RecursiveCharacterTextSplitter
- MarkdownHeaderTextSplitter
- Semantic Chunking (LLM-based)

**Encoder Selection:**
- Test multiple embedding models
- Consider domain-specific encoders
- Balance quality vs cost/speed
- For images: Use caption generation first

---

## Slide 14: Technique 3 & 4 - Prompts & Document Pre-processing
**Prompt Engineering:**
- Add static context (date, company info)
- Include conversation history
- Be specific about evaluation criteria
- Often overlooked but high impact!

**Document Pre-processing:**
- Rewrite documents for better retrieval
- Convert tables to natural language
- Use LLM to restructure content
- Semantic chunking with summaries

---

## Slide 15: Technique 5 & 6 - Query Rewriting & Expansion
**Query Rewriting:**
- User question → Optimized search query
- Incorporate conversation history
- Clarify ambiguous questions
- LLM rewrites before retrieval

**Query Expansion:**
- Generate multiple query variations
- Retrieve chunks for each variation
- Merge results (remove duplicates)
- Increases recall significantly

---

## Slide 16: Technique 7 - Reranking
**What is Reranking?**
- Retrieved many chunks (k × n queries)
- Use LLM to reorder by relevance
- Most relevant → top
- Least relevant → bottom

**Benefits:**
- Better than vector similarity alone
- Can truncate after reranking
- Saves context window space
- Improves answer quality

**Implementation:**
```
1. Retrieve 20 chunks
2. LLM ranks by relevance
3. Keep top 10 for final answer
```

---

## Slide 17: Technique 8 - Hierarchical RAG
**The Problem:**
- RAG struggles with spanning questions
- "How many employees earn > $60k?"
- Needs info across many documents

**The Solution:**
- Create summary documents at different levels
- All employees → salary summary
- All products → feature comparison
- Query summaries first, then drill down

**Caveat:**
- Still hacky (whack-a-mole)
- Need to anticipate question types

---

## Slide 18: Technique 9 - Graph RAG
**When Documents Have Relationships:**
- Employee → Manager (another employee)
- Product → Related Products
- Document → Source Documents

**Approach:**
- Store relationships in metadata
- Or use graph database (Neo4j)
- Retrieve chunk + related chunks (1-2 hops)

**Best For:**
- Highly connected data
- Relationship-based queries
- Not always necessary (metadata often enough)

---

## Slide 19: Technique 10 - Agentic RAG
**The Modern Approach:**
- Don't hardcode retrieval pipeline
- Give LLM tools for retrieval
- Let it decide: vector search, SQL, file search
- Can iterate until satisfied

**Comparison to Traditional RAG:**
- More flexible
- Can handle unexpected questions
- Less predictable
- Higher cost/latency

**Similar to:**
- Query expansion + rewriting + reranking
- But LLM orchestrates everything

---

## Slide 20: Is RAG Dead?
**Argument 1: "Context windows are huge now!"**
- ❌ Knowledge bases grow larger too
- ❌ Inefficient to process everything
- ✅ RAG still valuable for filtering

**Argument 2: "Agentic RAG replaced it!"**
- ❌ Still retrieval-augmented generation
- ❌ Just different orchestration
- ✅ Long live RAG (by any name)!

---

## Slide 21: Advanced Implementation - No LangChain
**Why Go Native?**
- Full control over each step
- Clearer separation of concerns
- No framework lock-in
- Educational value

**Key Components:**
- Direct Chroma API usage
- OpenAI embeddings API
- Custom document classes
- Pydantic for structured outputs

---

## Slide 22: Semantic Chunking with LLMs
**Traditional Chunking:**
- Split at character count
- Or at markdown headers
- Mechanical, not semantic

**LLM-Based Semantic Chunking:**
```
For each document:
1. LLM analyzes content
2. Divides into meaningful sections
3. Creates headline + summary
4. Preserves original text
```

**Benefits:**
- Chunks align with meaning
- Better retrieval accuracy
- Built-in summaries for context

---

## Slide 23: Structured Outputs for Consistency
**The Problem:**
- LLMs return free-form text
- Hard to parse programmatically
- Inconsistent formats

**The Solution: Structured Outputs**
```python
class Chunk(BaseModel):
    headline: str
    summary: str
    original_text: str

# LLM forced to return this schema
response_format=Chunk
```

**Use Cases:**
- Chunking
- Reranking
- Answer evaluation

---

## Slide 24: Reranking Implementation
**Process:**
1. Retrieve 20 chunks (from multiple queries)
2. Prompt LLM: "Rank these by relevance"
3. LLM returns: [4, 1, 7, 2, ...] (chunk IDs)
4. Reorder chunks accordingly
5. Keep top 10 for final answer

**Example Result:**
- Manchester University query
- Relevant chunk was 5th → moved to 1st
- Answer: Jessica Liu ✅

---

## Slide 25: Query Expansion Strategy
**The Challenge:**
- Query rewriting sometimes helps, sometimes hurts
- Rewritten query may add noise ("InsureElm")

**The Solution:**
- Don't replace original query
- Use BOTH queries
- Retrieve chunks for each
- Merge (remove duplicates)
- Rerank combined results

**Result:**
- Best of both worlds
- Increased recall
- Reranking filters noise

---

## Slide 26: Multiprocessing for Speed
**The Problem:**
- Processing 76 documents serially = 10 minutes
- Each document = separate LLM call

**The Solution:**
```python
from multiprocessing import Pool

with Pool(processes=5) as pool:
    chunks = pool.map(process_document, documents)
```

**Result:**
- 5-10x faster
- Watch for rate limits!
- Adjust worker count as needed

---

## Slide 27: Final Results - Advanced RAG
**Retrieval Metrics:**
- MRR: 0.9116 🟢 (was 0.7298)
- NDCG: 0.9025 🟢 (was 0.7387)
- Keyword Coverage: 96% 🟢 (was 83.8%)

**Answer Quality:**
- Accuracy: 4.62 🟢 (was 3.99)
- Completeness: 4.35 🟡 (was 3.85)
- Relevance: 4.84 🟢 (was 4.57)

**Improvement:**
- From RED → GREEN across the board!
- Production-ready performance

---

## Slide 28: What Made the Difference?
**Key Improvements:**
1. **Semantic Chunking**: LLM-based meaningful splits
2. **Better Encoder**: text-embedding-3-large
3. **Query Expansion**: Original + rewritten queries
4. **Reranking**: LLM reorders by relevance
5. **Improved Prompts**: Explicit evaluation criteria

**Cost:**
- ~$0.50 for full pipeline
- Mostly from semantic chunking (76 docs × LLM calls)

---

## Slide 29: Best Practices Summary
**1. Always Evaluate:**
- Build golden test set first
- Measure before optimizing
- Track multiple metrics

**2. Iterate Scientifically:**
- Change one thing at a time
- Measure impact
- Keep what works

**3. Don't Guess:**
- No "best" technique for all cases
- Your data is unique
- Experiment and measure

**4. Focus on Business Impact:**
- Retrieval metrics guide optimization
- Answer quality matters most
- Align with user needs

---

## Slide 30: Common Pitfalls
**1. Over-optimizing Retrieval:**
- High MRR doesn't guarantee good answers
- Balance retrieval and answer metrics

**2. Ignoring Simple Solutions:**
- Prompt engineering often overlooked
- Can have huge impact with minimal effort

**3. Premature Complexity:**
- Start simple (basic RAG)
- Add techniques only when needed
- Measure impact of each addition

**4. Forgetting the User:**
- Metrics are proxies
- Real user feedback is gold
- Continuously update test set

---

## Slide 31: When Each Technique Helps
**Query Rewriting**: Follow-up questions, ambiguous queries
**Query Expansion**: Low recall, missing relevant docs
**Reranking**: High k retrieval, multiple query sources
**Semantic Chunking**: Complex documents, poor chunk boundaries
**Hierarchical RAG**: Spanning questions, aggregation queries
**Graph RAG**: Relationship-heavy data, connected entities
**Agentic RAG**: Unpredictable questions, multi-step reasoning

---

## Slide 32: The Evaluation Framework
**Components:**
1. **Test Data**: tests.jsonl (150 questions)
2. **Eval Module**: Calculates MRR, NDCG, coverage
3. **LLM Judge**: Scores accuracy, completeness, relevance
4. **UI**: Gradio dashboard for visualization

**Workflow:**
```
Change code → Run evaluation → Check metrics → Iterate
```

---

## Slide 33: Beyond This Course
**Further Optimizations:**
- Try different LLM judges (GPT-4, Claude)
- Experiment with more encoders
- Implement hierarchical RAG
- Add graph-based retrieval
- Build agentic RAG with tools

**Production Considerations:**
- Caching for common queries
- Monitoring and logging
- A/B testing different approaches
- Cost optimization
- Latency optimization

---

## Slide 34: The Agentic Challenge
**For Advanced Learners:**

**Level 1**: Add keyword search tool
- Simple string matching across documents

**Level 2**: Full agentic retrieval
- Vector search tool
- File search tool
- SQL query tool (if applicable)
- Let LLM orchestrate

**Level 3**: Self-evaluating agent
- Generate answer
- Evaluate with LLM judge
- If score < threshold, iterate
- Loop until 5/5 or max attempts

---

## Slide 35: Real-World Applications
**Business Use Cases:**
- Customer support knowledge base
- Internal documentation search
- Legal document analysis
- Medical record querying
- Code repository search

**Personal Use Cases:**
- Personal knowledge base (Google Drive, notes)
- Email search and summarization
- Research paper organization
- Learning resource aggregation

---

## Slide 36: Key Takeaways
**1. RAG is Empirical:**
- No one-size-fits-all solution
- Must experiment with your data

**2. Evaluation is Critical:**
- Can't improve what you don't measure
- Build test set early

**3. Start Simple, Add Complexity:**
- Basic RAG first
- Add techniques based on failures
- Measure each addition

**4. LLMs Are Versatile:**
- Chunking, rewriting, reranking, judging
- Use them throughout pipeline

**5. It's Still RAG:**
- Whether traditional or agentic
- Core concept remains valuable

---

## Slide 37: Resources & References
**Key Concepts:**
- MRR, NDCG, Precision, Recall
- LLM-as-a-Judge
- Structured Outputs
- Semantic Chunking

**Tools & Libraries:**
- Chroma: https://www.trychroma.com/
- LiteLLM: https://litellm.ai/
- Pydantic: https://docs.pydantic.dev/
- TQDM: https://tqdm.github.io/

**Further Reading:**
- RAG evaluation papers
- Embedding model benchmarks (MTEB)
- Advanced retrieval techniques

---

## Slide 38: Assignment - Beat the Benchmark
**Mandatory Challenge:**
- Take the provided code
- Improve evaluation metrics
- Target: MRR > 0.92, Accuracy > 4.7
- Document your experiments

**Stretch Challenge:**
- Build personal knowledge worker
- Use your own documents
- Implement 3+ advanced techniques
- Share results on LinkedIn

**Agentic Challenge:**
- Implement self-evaluating agentic RAG
- Achieve 4.9+ across all metrics
- Share your approach!

---

## Slide 39: What's Next?
**Week 6-8: Capstone Project**
- Traditional data science foundations
- Fine-tuning LLMs
- Production deployment
- Real-world problem solving

**Don't Stop Here!**
- RAG is just one application
- More exciting topics ahead
- Keep building, keep learning

---

## Slide 40: Thank You!
**You've Learned:**
- ✅ RAG evaluation frameworks
- ✅ 10 advanced RAG techniques
- ✅ Scientific optimization approach
- ✅ Production-ready implementation

**You Can Now:**
- Build and evaluate RAG systems
- Optimize systematically
- Implement advanced techniques
- Deploy with confidence

**Keep Experimenting! 🚀**

