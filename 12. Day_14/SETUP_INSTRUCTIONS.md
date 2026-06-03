# Day 14 - Quick Setup Guide

## ✅ What's Been Created

All Day 14 materials have been successfully created:

1. **slides_content_day14.md** - 24 slides with presentation content
2. **lecture_notes_day14.md** - Comprehensive lecture notes (~15,000 words)
3. **day14_notebook.ipynb** - Interactive Jupyter notebook with code examples
4. **requirements.txt** - Python dependencies
5. **README.md** - Detailed setup and usage instructions

## 🚀 Quick Start (5 Minutes)

### Step 1: Create Environment
```bash
conda create -n day14_env python=3.11 -y
conda activate day14_env
```

### Step 2: Install Packages
```bash
cd Day_14
pip install -r requirements.txt
```

### Step 3: Add Your API Key
Create a file named `.env` in the Day_14 folder:
```
OPENAI_API_KEY=sk-your-key-here
```

### Step 4: Run the Notebook
```bash
jupyter notebook day14_notebook.ipynb
```

## 📚 What You'll Learn

### Part 1: Model Selection
- How to choose the right LLM for your task
- Understanding benchmarks (GPQA, MMLU Pro, AIME, HLE, etc.)
- Using leaderboards (Artificial Analysis, Vellum, Live Bench)
- Chinchilla Scaling Law

### Part 2: Simple RAG
- What is Retrieval Augmented Generation
- Building a basic RAG system with dictionary lookup
- Understanding limitations of exact string matching

### Part 3: Vector Embeddings
- Two types of LLMs: Autoregressive vs Encoder
- What are vector embeddings
- Cosine similarity for semantic search
- Creating embeddings with OpenAI API

### Part 4: Semantic RAG
- Improved RAG using vector embeddings
- Semantic search that handles synonyms and variations
- Interactive Gradio interfaces to compare approaches

## 📝 Files Overview

### slides_content_day14.md
- 24 presentation slides
- Brief, concise content perfect for teaching
- Includes URLs and references
- Covers all key concepts

### lecture_notes_day14.md
- Detailed educational content
- Simple, easy-to-understand English
- Real-world examples with sources
- Comprehensive explanations of all topics

### day14_notebook.ipynb
- Step-by-step code implementation
- 32 cells (markdown + code)
- Follows the transcript closely
- Comments and explanations throughout
- Two complete RAG implementations:
  1. Simple dictionary-based RAG
  2. Advanced semantic RAG with embeddings

### requirements.txt
```
openai==1.54.0
python-dotenv==1.0.0
gradio==4.44.0
numpy==1.26.4
```

## 🎯 Key Concepts Demonstrated

### Simple RAG (Cells 1-12)
```python
# Dictionary-based lookup
knowledge = {"lancaster": "Avery Lancaster is CEO..."}
context = get_relevant_context("Who is Lancaster?")
```

**Limitations:**
- ❌ "Who is Avery?" - Fails (only last names)
- ❌ "Who is the CEO?" - Fails (no "CEO" key)
- ❌ Typos break everything

### Semantic RAG (Cells 13-31)
```python
# Embedding-based semantic search
embedding = get_embedding("Who is the CEO?")
results = semantic_search(query, top_k=2)
```

**Advantages:**
- ✅ "Who is Avery?" - Works!
- ✅ "Who is the CEO?" - Works!
- ✅ "Tell me about auto insurance" - Finds "CarElm"!
- ✅ Handles synonyms and variations

## 💡 Interactive Features

The notebook includes two Gradio interfaces:

1. **Simple RAG Interface**
   - Test exact string matching
   - See where it fails

2. **Semantic RAG Interface**
   - Test semantic search with embeddings
   - Compare with simple approach
   - See the improvement!

## 📊 Cost Estimate

Running the entire notebook costs approximately **$0.05-0.10**:
- GPT-4o-mini: ~$0.15 per 1M input tokens
- text-embedding-3-small: ~$0.02 per 1M tokens
- Small knowledge base keeps costs minimal

## 🔗 Important Resources

### Leaderboards
- [Artificial Analysis](https://artificialanalysis.ai) - Best overall
- [Vellum](https://www.vellum.ai/llm-leaderboard) - API costs
- [Live Bench](https://livebench.ai) - Anti-contamination

### Documentation
- [OpenAI Embeddings](https://platform.openai.com/docs/guides/embeddings)
- [Gradio Docs](https://gradio.app/docs/)

### Research Papers
- [Chinchilla Scaling Law](https://arxiv.org/abs/2203.15556)
- [Apple Benchmark Paper](https://arxiv.org/abs/2311.01964)
- [GPQA Paper](https://arxiv.org/abs/2311.12022)

## 🐛 Troubleshooting

### API Key Not Found
```bash
# Make sure .env is in Day_14 folder
# Check it's not named .env.txt
# Verify key starts with sk-
```

### Module Not Found
```bash
conda activate day14_env
pip install -r requirements.txt
# Restart Jupyter kernel
```

### Gradio Not Loading
```python
# In notebook, use:
demo.launch(share=False, inbrowser=True)
```

## 📖 How to Use for Teaching

### For Slides Presentation
1. Open `slides_content_day14.md`
2. Convert to PowerPoint/Google Slides
3. Add visuals and diagrams
4. Present to audience

### For Lecture
1. Use `lecture_notes_day14.md` as teaching guide
2. Follow the detailed explanations
3. Reference examples and sources
4. Answer questions using the comprehensive content

### For Hands-On Workshop
1. Have students set up environment
2. Walk through `day14_notebook.ipynb` together
3. Run cells step-by-step
4. Let students experiment with Gradio interfaces
5. Encourage modifications to knowledge base

## 🎓 Learning Path

1. **Read** lecture notes to understand concepts
2. **Review** slides for key points
3. **Run** notebook cells in order
4. **Experiment** with Gradio interfaces
5. **Modify** knowledge base with your own data
6. **Test** different queries and thresholds

## ✨ What Makes This Special

### Follows Transcript Exactly
- All concepts from the transcript included
- Same examples and explanations
- Same teaching progression

### Production-Ready Code
- Proper error handling
- Clean, commented code
- Best practices followed
- Ready to extend

### Educational Focus
- Step-by-step progression
- Clear explanations
- Visual feedback with print statements
- Interactive demos

## 🚀 Next Steps After Day 14

After completing this material, you'll be ready for:
- LangChain framework
- Vector databases (Chroma, Pinecone, Weaviate)
- Advanced RAG techniques
- Chunking strategies
- Hybrid search
- Production RAG pipelines

## 📞 Support

If you encounter issues:
1. Check README.md for detailed troubleshooting
2. Review lecture notes for concept clarification
3. Verify environment setup
4. Check OpenAI API key and credits

## 🎉 Success Criteria

You've successfully completed Day 14 when you can:
- ✅ Explain the difference between autoregressive and encoder LLMs
- ✅ Describe how vector embeddings work
- ✅ Implement both simple and semantic RAG
- ✅ Use OpenAI's embedding API
- ✅ Calculate cosine similarity
- ✅ Build a Gradio interface for RAG
- ✅ Understand benchmark limitations
- ✅ Navigate model selection leaderboards

---

**Created for Day 14 of LLM Engineering Course**
**Topic: Model Selection & Introduction to RAG**

Happy Learning! 🚀

