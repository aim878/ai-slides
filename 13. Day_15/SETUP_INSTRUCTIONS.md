# Day 15: Quick Start Guide

## 🚀 Quick Setup

### 1. Create Environment
```bash
conda create -n day15_rag python=3.11
conda activate day15_rag
```

### 2. Install Dependencies
```bash
pip install -r requirements.txt
```

### 3. Configure API Key
Create a `.env` file:
```
OPENAI_API_KEY=your_key_here
```

### 4. Run the Notebook
```bash
jupyter notebook day15_notebook.ipynb
```

## 📁 Files Created

- **slides_content_day15.md** - 30 slides covering all concepts
- **lecture_notes_day15.md** - Comprehensive educational notes
- **day15_notebook.ipynb** - Complete working implementation
- **requirements.txt** - All Python dependencies
- **README.md** - Detailed setup and usage guide

## 🎯 What You'll Learn

1. **LangChain Framework** - Pros, cons, and practical usage
2. **Document Chunking** - RecursiveCharacterTextSplitter with overlap
3. **Vector Embeddings** - all-MiniLM-L6-v2, OpenAI embeddings
4. **Chroma Database** - Vector storage and retrieval
5. **t-SNE Visualization** - 2D and 3D vector visualization
6. **RAG Pipeline** - Complete retrieval-augmented generation
7. **Gradio Interface** - User-friendly chat application

## 📊 What the Notebook Does

### Part 1: Setup
- Imports libraries
- Loads environment variables
- Verifies API key

### Part 2: Knowledge Base
- Creates sample InsureElm documents
- Employees, products, contracts, company info

### Part 3: Document Loading
- Uses LangChain DirectoryLoader
- Loads markdown files
- Adds metadata

### Part 4: Chunking
- RecursiveCharacterTextSplitter
- 1000 character chunks
- 200 character overlap

### Part 5: Embeddings
- Creates Hugging Face encoder (all-MiniLM-L6-v2)
- Generates 384-dimensional vectors
- Free and open source

### Part 6: Vector Database
- Creates Chroma vector store
- Stores chunks as vectors
- Enables similarity search

### Part 7: Visualization
- t-SNE dimensionality reduction
- 2D scatter plot
- 3D interactive visualization
- Shows document clustering

### Part 8: RAG Pipeline
- Simple RAG function
- RAG with conversation history
- Tests with multiple questions
- Handles typos and synonyms

### Part 9: Gradio Interface
- Chat interface
- Conversation history
- Example questions
- Production-ready UI

### Part 10: Encoder Comparison (Optional)
- Compares 3 encoders
- Visualizes differences
- Shows quality improvements

## 💡 Key Features

### Fuzzy Matching
- Handles typos: "Avry Lancster" → "Avery Lancaster"
- Understands synonyms: "CEO" = "Chief Executive Officer"
- Different phrasings work

### Conversation History
- Maintains context across questions
- Handles follow-ups: "What did she do before?"
- Combines user questions for better retrieval

### Visualization
- See how encoders cluster documents
- Understand vector space
- Compare encoder quality

## 🔧 Customization

### Use Your Own Documents
Replace the sample knowledge base:
```python
# Add your documents to knowledge_base/
knowledge_base/
  your_category/
    document1.md
    document2.md
```

### Change Chunk Size
```python
text_splitter = RecursiveCharacterTextSplitter(
    chunk_size=500,    # Smaller chunks
    chunk_overlap=100  # Less overlap
)
```

### Use Different Encoder
```python
# OpenAI Small
embeddings = OpenAIEmbeddings(model="text-embedding-3-small")

# OpenAI Large (best quality)
embeddings = OpenAIEmbeddings(model="text-embedding-3-large")
```

### Adjust Retrieval
```python
# Retrieve more chunks
retriever = vector_store.as_retriever(search_kwargs={"k": 10})
```

## 📈 Expected Results

### Document Statistics
- 4 sample documents
- ~10-15 chunks (depends on content length)
- 384 dimensions per vector (Hugging Face)

### Visualization
- Documents cluster by type
- Similar content groups together
- Some overlap is normal

### RAG Performance
- Accurate answers to factual questions
- Handles typos and variations
- Provides context-based responses

## 🐛 Troubleshooting

### Hugging Face Download
First run downloads ~100MB model. Requires internet connection.

### OpenAI Costs
- text-embedding-3-small: ~$0.004 for sample docs
- text-embedding-3-large: ~$0.027 for sample docs
- Very minimal cost for testing

### Gradio Port
If port 7860 is busy:
```python
demo.launch(server_port=7861)
```

### Memory Issues
If running all encoders causes memory issues, skip Part 9 or run encoders separately.

## 📚 Learning Path

1. **Run cells 1-8** - Basic RAG setup
2. **Examine visualizations** - Understand vector space
3. **Test RAG functions** - See retrieval in action
4. **Launch Gradio** - Interactive testing
5. **Compare encoders** - Understand quality differences

## 🎓 Key Takeaways

### Critical Distinction
- **Encoder** creates vectors (affects quality) ← Focus here!
- **Database** stores vectors (affects speed)

### Best Practices
- Start with free encoders for development
- Upgrade to OpenAI for production
- Experiment with chunk sizes
- Visualize to understand behavior
- Handle conversation history properly

### RAG Benefits
- Works with private data
- No model retraining needed
- Handles typos and synonyms
- Scalable to millions of documents
- Explainable (can show sources)

## 🚀 Next Steps

After completing this notebook:

1. **Add your documents** - Real knowledge base
2. **Optimize chunking** - Test different sizes
3. **Compare encoders** - Find best for your data
4. **Add features** - Citations, confidence scores
5. **Deploy** - Separate ingestion from querying
6. **Measure** - Track quality and performance

## 📖 Resources

- **LangChain Docs**: https://python.langchain.com/
- **Chroma Docs**: https://docs.trychroma.com/
- **Sentence Transformers**: https://www.sbert.net/
- **OpenAI Embeddings**: https://platform.openai.com/docs/guides/embeddings

---

**You're now ready to build production RAG systems!** 🎉

The notebook contains everything you need to understand and implement RAG from scratch. Work through it step by step, experiment with the code, and build something amazing!

