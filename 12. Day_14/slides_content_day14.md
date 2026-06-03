# Day 14: Model Selection & Introduction to RAG

## Slide 1: Title Slide
**Day 14: Selecting the Right LLM & Introduction to RAG**
- Topics: Model Selection, Benchmarks, Leaderboards, Vector Embeddings

---

## Slide 2: Course Progress Overview
**Where We Are**
- ✅ Day 11-12: Chat Completions API, Frontend Models, Tool Calling
- ✅ Day 13: Hugging Face Transformers
- 📍 Day 14: Model Selection & Code Generation
- 🎯 Day 14-15: RAG (Retrieval Augmented Generation)
- Ahead: Fine-tuning, Data Science, Final Project

---

## Slide 3: The Key Question
**Which is the Best LLM?**
- ❌ Wrong question: "What's the best model?"
- ✅ Right question: "What's the right model for my task?"

**Selection Strategy:**
1. Understand your requirements
2. Review basic model information
3. Analyze benchmarks
4. Test with your use case

---

## Slide 4: Model Basics - What to Look For
**Key Specifications:**
- Open source vs. Closed source
- Chat vs. Reasoning vs. Hybrid models
- Release date & knowledge cutoff
- Number of parameters (bigger ≈ smarter)
- Training tokens (more data = more knowledge)
- Context window size

**Find this info:** Model cards, provider documentation

---

## Slide 5: Cost & Performance Factors
**Beyond Intelligence:**
- **Cost:** API pricing vs. compute costs
- **Speed:** Response time (tokens/second)
- **Latency:** Time to first token (TTFT)
- **Rate Limits:** How many calls allowed
- **Time to Market:** Build complexity
- **License:** Usage restrictions, commercial limits

---

## Slide 6: Chinchilla Scaling Law
**Training at Scale**
- Relationship: Parameters ↔ Training Data
- Rule: 2x parameters → need 2x training tokens
- Why it matters less now:
  - Better compression techniques
  - Inference-time improvements (reasoning, RAG)
  - Smaller models getting smarter

---

## Slide 7: Hard Benchmarks - Part 1
**Testing Model Intelligence:**

**GPQA (Google-Proof Q&A)**
- 448 difficult physics, chemistry, biology questions
- PhD level: 65% | Non-PhD: 34%
- Top models now: ~88%

**MMLU Pro**
- Massive Multitask Language Understanding
- 10 answer choices (harder than original)
- Tests broad knowledge

---

## Slide 8: Hard Benchmarks - Part 2
**AIME**
- American Invitational Mathematics Examination
- Competitive high school math puzzles
- Tests mathematical reasoning

**Live Code Bench**
- Coding problems from LeetCode, Codeforces
- Constantly updated to prevent contamination

---

## Slide 9: Hard Benchmarks - Part 3
**MUSA (Multi-step Soft Reasoning)**
- Tests reasoning through complex scenarios
- Famous category: Murder mystery whodunits
- Model must identify means, motive, opportunity

**HLE (Humanity's Last Exam)**
- 2,500 superhuman-level questions
- Designed to be the hardest test possible
- Models went from 2-3% to 26%+ in months

---

## Slide 10: Benchmark Limitations
**Take with a Grain of Salt:**
- **Training Data Contamination:** Models see test questions during training
- **Inconsistent Application:** Different hardware, self-reporting
- **Narrow Scope:** Good at physics ≠ generally PhD-level
- **Limited Format:** Multiple choice misses nuance
- **Saturation:** Tests become too easy over time
- **Overfitting:** Selecting for specific benchmarks

Reference: Apple's paper on changing benchmark questions
https://arxiv.org/abs/2311.01964

---

## Slide 11: Top Leaderboards to Know
**Essential Resources:**

1. **Artificial Analysis** - artificialanalysis.ai
   - Intelligence, speed, price comparisons
   - Most comprehensive

2. **Vellum** - vellum.ai
   - API costs & context windows side-by-side

3. **Scale AI (SEAL)** - scale.com
   - Specialized expert leaderboards

4. **Hugging Face** - huggingface.co/spaces
   - Open source model focus

5. **Live Bench** - livebench.ai
   - Anti-contamination focus

---

## Slide 12: Current Top Models (As of Recording)
**Intelligence Leaders:**
1. GPT-5 Codex (coding optimized)
2. GPT-5 High (max reasoning)
3. Grok-4
4. Claude 4.5 Sonnet
5. Gemini 2.5 Pro
6. GPT-Opus-1 R20B (first open source!)

**Note:** Rankings change frequently - always check current leaderboards!

---

## Slide 13: Introduction to RAG
**The Problem:**
- LLMs have knowledge cutoff dates
- Can't access private/proprietary data
- Limited by context window size

**The Solution: RAG (Retrieval Augmented Generation)**
- Augment prompts with relevant context
- Retrieve information from external knowledge base
- Generate responses using both model knowledge + retrieved data

---

## Slide 14: Simple RAG Example
**Basic Approach (Dictionary Lookup):**

```python
knowledge = {
    "lancaster": "Avery Lancaster, CEO of InsureElm",
    "car": "CarElm - auto insurance product"
}
```

**Limitations:**
- Exact string matching only
- Brittle (fails on typos, synonyms)
- Can't handle "Who is Avery?" vs "Who is Lancaster?"

**We need something smarter...**

---

## Slide 15: Two Types of LLMs
**Autoregressive LLMs (What we've used so far)**
- Predict next token based on input
- Examples: GPT, Claude, Gemini
- Use: Text generation, chat, coding

**Encoder LLMs (New!)**
- Convert input → vector (numbers)
- Examples: BERT, OpenAI Embeddings, all-MiniLM
- Use: Classification, embeddings, semantic search

---

## Slide 16: What are Vector Embeddings?
**Concept:**
- Text → Numbers that represent meaning
- Similar meanings → Similar numbers (close in vector space)

**Example:**
- "Ticket prices to London" → [0.2, 0.8, 0.5, ...]
- "Flight costs to Heathrow" → [0.21, 0.79, 0.51, ...]
- Different words, same meaning, close vectors!

**Dimensions:** 100s to 1000s of numbers per vector

---

## Slide 17: Vector Math Magic
**Word2Vec Discovery:**

King - Man + Woman = Queen

Paris - France + England = London

**How it works:**
- Vectors capture semantic relationships
- Can add/subtract concepts
- Works for entire paragraphs with modern encoders!

---

## Slide 18: RAG with Vectors - The Big Idea
**Smart RAG Workflow:**

1. **User asks:** "Ticket prices to Heathrow?"
2. **Encode question** → vector
3. **Search vector database** for similar vectors
4. **Retrieve** matching text (even if different words!)
5. **Add to prompt:** Context + Question
6. **LLM generates** accurate answer

**Key:** Semantic search, not string matching!

---

## Slide 19: Vector Data Store
**Components:**
- **Encoder Model:** Converts text → vectors
- **Vector Database:** Stores vectors + original text
- **Similarity Search:** Finds closest vectors (cosine similarity)

**Popular Vector Stores:**
- Chroma
- Pinecone
- Weaviate
- FAISS

---

## Slide 20: Important Clarification
**Two Separate LLMs in RAG:**

**Encoder LLM** (Left side)
- Converts text to vectors
- Used for search only
- Example: text-embedding-3-large

**Autoregressive LLM** (Right side)
- Generates responses
- Receives natural language (not vectors!)
- Example: GPT-4, Claude

**They don't talk to each other directly!**

---

## Slide 21: RAG is a Hack (And That's OK!)
**Reality Check:**
- RAG is empirical (trial and error)
- Many tricks to improve retrieval
- Not perfect - just practical
- Constantly evolving techniques

**Why it works:**
- Better than nothing
- Cheaper than fine-tuning
- Flexible and updatable
- Good enough for most use cases

---

## Slide 22: Key Takeaways
**Model Selection:**
- No "best" model - only right model for your task
- Check basics, benchmarks, and leaderboards
- Test with your specific use case

**RAG Fundamentals:**
- Augment LLM knowledge with external data
- Vector embeddings enable semantic search
- Two LLMs: encoder for search, autoregressive for generation

**Next:** Hands-on RAG implementation with LangChain & Chroma

---

## Slide 23: Resources & Links
**Leaderboards:**
- Artificial Analysis: https://artificialanalysis.ai
- Vellum: https://www.vellum.ai/llm-leaderboard
- Live Bench: https://livebench.ai

**Documentation:**
- OpenAI Embeddings: https://platform.openai.com/docs/guides/embeddings
- Model Cards: Search "[Model Name] model card"

**Research:**
- Chinchilla Paper: https://arxiv.org/abs/2203.15556
- Apple Benchmark Paper: https://arxiv.org/abs/2311.01964

---

## Slide 24: Next Steps
**Coming Up:**
- Implement RAG with LangChain
- Use Chroma vector database
- Build production-ready RAG pipeline
- Advanced RAG techniques

**Practice:**
- Explore leaderboards
- Compare models for your use case
- Experiment with embeddings

**Questions?**

