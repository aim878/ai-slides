# Day 15: Building RAG with LangChain & Chroma

## Slide 1: Title Slide
**Day 15: Vectors, LangChain & Complete RAG Pipeline**
- Topics: LangChain, Document Chunking, Encoders, Vector Databases, RAG Implementation

---

## Slide 2: What We're Building Today
**The Complete RAG System**
- Part A: Divide documents into chunks
- Part B: Turn chunks into vectors with encoders
- Part C: Store vectors in Chroma database
- Part D: Visualize vectors in 2D and 3D
- Part E: Build complete RAG pipeline
- Part F: Deploy with Gradio UI

**This is the day you've been waiting for!**

---

## Slide 3: Quick RAG Recap
**The Big Idea (from Day 14)**

1. User asks a question
2. **Encode question** → vector (using encoder LLM)
3. **Search vector database** for similar vectors
4. **Retrieve** matching text chunks
5. **Augment prompt** with retrieved context
6. **Generate** response with LLM

**Today: We implement all of this!**

---

## Slide 4: Introducing LangChain
**What is LangChain?**
- Open source framework launched October 2022
- Created by Harrison Chase
- Abstraction layer for working with LLMs
- Helps build RAG pipelines, agents, and more
- Version 1.0 released October 2025 (major overhaul)

**Website**: https://python.langchain.com/

---

## Slide 5: LangChain Pros
**Why Use LangChain?**

✅ **Quick to Market**
- Rapid prototyping of LLM applications
- Pre-built components for common tasks
- Easy to glue together different LLMs

✅ **Robust Tooling**
- Document loaders, text splitters, retrievers
- Out-of-the-box integrations

✅ **Enterprise Adoption**
- Very popular in production
- Good for resume/career
- Widely used in industry

---

## Slide 6: LangChain Cons
**Considerations**

❌ **Less Needed Now**
- OpenAI-compatible endpoints everywhere
- Easy to switch models without abstraction
- Common patterns now standardized

❌ **Heavyweight**
- Steep learning curve
- Many packages and dependencies
- Custom terminology (HumanMessage, AIMessage, etc.)
- Own language (LCEL)

**Alternatives**: LiteLLM (lightweight), direct API calls

---

## Slide 7: LangChain Ecosystem
**The LangChain Family**

- **LangChain**: Core framework for LLM apps
- **LangGraph**: Agent workflow orchestration
- **LangSmith**: Observability platform
- **LangServe**: Deployment tools

**Multiple Packages**:
- `langchain-openai`
- `langchain-community`
- `langchain-chroma`
- `langchain-huggingface`

---

## Slide 8: Document Chunking - Why?
**The Problem**

Large documents contain many different topics:
- Employee HR records
- Product descriptions
- Contract terms
- Company policies

**User questions** usually pertain to ONE specific part

**Solution**: Break documents into smaller, focused chunks

**Better matching**: Question → Specific chunk (not entire document)

---

## Slide 9: Text Splitters in LangChain
**Chunking Strategies**

**CharacterTextSplitter**
- Simplest: Splits by character count
- No intelligence about content

**RecursiveCharacterTextSplitter** (Most Common)
- Tries to split at natural boundaries
- Priority: Double newlines → Single newlines → Sentences → Words
- Configurable chunk size and overlap

**Key Parameters**:
- `chunk_size`: Target size in characters (e.g., 1000)
- `chunk_overlap`: Overlap between chunks (e.g., 200)

---

## Slide 10: Why Chunk Overlap?
**The Overlap Strategy**

**Problem**: Answer might be split across chunk boundary

**Solution**: Overlapping chunks ensure complete context

```
Chunk 1: [--------OVERLAP]
Chunk 2:         [OVERLAP--------]
```

**Example**:
- Chunk size: 1000 characters
- Overlap: 200 characters
- Result: Better chance of capturing complete answers

**Trade-off**: More chunks = more storage, but better retrieval

---

## Slide 11: Encoder Models (Embeddings)
**Two Types of LLMs Revisited**

**Autoregressive** (Day 14)
- GPT, Claude, Gemini
- Generate text token by token

**Encoder** (Today's Focus)
- BERT, OpenAI Embeddings, Sentence Transformers
- Convert text → vector (numbers representing meaning)

**Critical**: Encoder creates vectors, NOT the vector database!

---

## Slide 12: Popular Encoder Models
**Common Choices**

**OpenAI**:
- `text-embedding-3-small` (1,536 dimensions)
- `text-embedding-3-large` (3,072 dimensions)
- Paid but very good

**Google**:
- `gemini-embedding-001`

**Hugging Face** (Open Source):
- `all-MiniLM-L6-v2` (384 dimensions)
- Most popular open source encoder
- Free and fast

**Historical**: Word2Vec (2013), BERT (2018)

---

## Slide 13: Vector Databases
**Storing and Searching Vectors**

**What They Do**:
- Store vectors efficiently
- Fast similarity search
- Find "closest" vectors to a query

**Popular Options**:

**Open Source**:
- **Chroma** (we'll use today) - Easy, SQLite-based
- **FAISS** (Facebook) - In-memory, very fast

**Paid/Managed**:
- **Pinecone**, **Weaviate** - Enterprise solutions

**Mainstream DBs**: Postgres, MongoDB, Elasticsearch (all support vectors now!)

---

## Slide 14: Encoder vs Vector Database
**Critical Distinction**

**Encoder Model**:
- Creates the vectors
- Determines dimensionality (384, 1536, 3072, etc.)
- Affects quality of semantic search

**Vector Database**:
- Stores the vectors
- Enables fast retrieval
- Infrastructure choice (cost vs performance)

**Key Point**: Focus on choosing the RIGHT encoder. Database is secondary.

---

## Slide 15: Creating Vectors with LangChain
**The Simple Code**

```python
from langchain_huggingface import HuggingFaceEmbeddings
from langchain_chroma import Chroma

# Create encoder
embeddings = HuggingFaceEmbeddings(
    model_name="all-MiniLM-L6-v2"
)

# Create vector store with chunks
vector_store = Chroma.from_documents(
    documents=chunks,
    embedding=embeddings,
    persist_directory="./vector_db"
)
```

**That's it!** Vectors created and stored.

---

## Slide 16: Visualizing Vectors
**Understanding High-Dimensional Space**

**Problem**: Humans can't visualize 384+ dimensions

**Solution**: Dimensionality reduction

**t-SNE** (t-distributed Stochastic Neighbor Embedding)
- Projects high-dimensional data to 2D or 3D
- Preserves similarity relationships
- Close in 2D/3D = Close in original space

**Why Visualize?**:
- Spot patterns and clusters
- Find outliers
- Understand encoder behavior
- Debug data quality issues

---

## Slide 17: Building the RAG Pipeline
**The Complete Flow**

```python
# 1. Create retriever from vector store
retriever = vector_store.as_retriever(k=5)

# 2. Fetch relevant context
context = retriever.invoke(question)

# 3. Build prompt with context
system_prompt = f"Context: {context}\n\nAnswer: {question}"

# 4. Call LLM
response = llm.invoke(system_prompt)
```

**With LangChain**: Even simpler with chains!

---

## Slide 18: RAG in 5 Lines of Code
**The Magic of LangChain**

```python
# Fetch context
context = retriever.invoke(question)

# Build messages
messages = [
    SystemMessage(content=f"Context: {context}"),
    HumanMessage(content=question)
]

# Get answer
response = llm.invoke(messages)
```

**That's a complete RAG system!**

---

## Slide 19: Handling Conversation History
**Stateless LLMs Need Context**

**Problem**: LLM doesn't remember previous messages

**Solution**: Pass full conversation history

```python
messages = [
    SystemMessage(content=system_prompt),
    HumanMessage(content="Who is Avery?"),
    AIMessage(content="Avery Lancaster, CEO..."),
    HumanMessage(content="What did she do before?")
]
```

**Challenge**: Should context retrieval use:
- Only latest question?
- All user questions combined?
- Each question separately?

**Answer**: Depends on use case!

---

## Slide 20: Production RAG Structure
**Modular Architecture**

**ingest.py**:
- Load documents
- Create chunks
- Generate vectors
- Store in Chroma
- Run periodically or on data changes

**answer.py**:
- `fetch_context(question)` - Retrieve relevant chunks
- `answer_question(question, history)` - Generate response

**app.py**:
- Gradio UI
- Calls answer.py functions
- Displays results

**Benefits**: Swappable implementations, clean separation

---

## Slide 21: Gradio RAG Application
**Interactive Demo**

**Features**:
- Chat interface
- Conversation history
- RAG-powered responses
- Shows source documents
- Handles typos and variations

**Example Queries**:
- "Who is Avery?" ✅
- "Who is Lancaster?" ✅
- "Who is Avry Lancster?" ✅ (fuzzy matching!)

---

## Slide 22: Comparing Encoders
**Experiment Results**

**all-MiniLM-L6-v2** (384 dims):
- Fast, free, decent separation
- Some overlap between categories

**text-embedding-3-small** (1,536 dims):
- Better separation
- Clearer clusters
- Low cost

**text-embedding-3-large** (3,072 dims):
- Best separation
- Distinct clusters by document type
- Higher cost but best quality

**Visualization shows the difference!**

---

## Slide 23: Key Insights from Visualization
**What We Learned**

1. **Encoder matters most** - Different encoders = completely different vector spaces
2. **Dimensionality helps** - More dimensions = better separation (usually)
3. **Semantic clustering** - Similar content clusters together naturally
4. **No magic** - Models weren't told document types, but separated them anyway!

**Practical Tip**: Always visualize your vectors to understand encoder behavior

---

## Slide 24: RAG Best Practices
**What We've Learned**

**Chunking**:
- Experiment with chunk size (500-2000 chars typical)
- Use overlap (10-20% of chunk size)
- Test different splitters

**Encoders**:
- Start with open source (all-MiniLM-L6-v2)
- Upgrade to OpenAI for better quality
- Test with your specific data

**Retrieval**:
- Retrieve multiple chunks (k=3-10)
- Consider conversation history
- Handle edge cases (typos, synonyms)

---

## Slide 25: Common Pitfalls
**Watch Out For**

❌ **Confusing encoder with database**
- Encoder creates vectors
- Database stores them

❌ **Ignoring conversation history**
- LLMs are stateless
- Must pass full history

❌ **Wrong context for follow-ups**
- "What did she do?" needs previous context
- Combine user messages for retrieval

❌ **Not testing different encoders**
- Huge impact on quality
- Easy to swap and compare

---

## Slide 26: Key Takeaways
**What You Can Now Do**

✅ Use LangChain for RAG pipelines
✅ Chunk documents with text splitters
✅ Create vectors with encoder models
✅ Store vectors in Chroma database
✅ Visualize vectors in 2D/3D with t-SNE
✅ Build complete RAG system in ~10 lines
✅ Deploy with Gradio UI
✅ Compare different encoders

**You've built a production-ready RAG system!**

---

## Slide 27: Resources & Links
**LangChain**:
- Documentation: https://python.langchain.com/
- GitHub: https://github.com/langchain-ai/langchain

**Vector Databases**:
- Chroma: https://www.trychroma.com/
- FAISS: https://github.com/facebookresearch/faiss
- Pinecone: https://www.pinecone.io/

**Encoders**:
- Sentence Transformers: https://www.sbert.net/
- OpenAI Embeddings: https://platform.openai.com/docs/guides/embeddings

**Visualization**:
- t-SNE: https://scikit-learn.org/stable/modules/generated/sklearn.manifold.TSNE.html

---

## Slide 28: Next Steps
**Coming Up**:
- Advanced RAG techniques
- Hybrid search (keyword + semantic)
- Re-ranking strategies
- Evaluation metrics
- Production optimization

**Practice**:
- Try different chunk sizes
- Compare encoders with your data
- Visualize your own documents
- Build custom RAG apps

**Experiment and iterate!**

---

## Slide 29: The RAG Journey
**Where We've Been**

Day 14: Understanding RAG concepts
- Vector embeddings theory
- Semantic search basics
- Simple dictionary lookup

Day 15: Building Real RAG
- LangChain framework
- Document chunking
- Vector databases
- Complete pipeline
- Production deployment

**Next**: Advanced RAG and optimization!

---

## Slide 30: Questions?

**You now have a working RAG system!**

Key achievements:
- 76 documents → 413 chunks → 413 vectors
- Semantic search working
- Fuzzy matching enabled
- Gradio UI deployed

**Go build amazing things with RAG!** 🚀

