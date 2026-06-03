# Day 16: RAG Evaluations & Advanced Techniques

## Overview

This module builds upon Day 15's RAG implementation by adding:
- **Scientific evaluation frameworks** for measuring RAG performance
- **Advanced RAG techniques** including semantic chunking, reranking, and query expansion
- **Production-ready implementation** without LangChain dependencies
- **Quantitative optimization** using metrics like MRR, NDCG, and LLM-as-a-Judge

## What You'll Learn

1. **RAG Evaluation Fundamentals**
   - Building golden test datasets
   - Retrieval metrics (MRR, NDCG, Precision, Recall)
   - Answer quality metrics (LLM-as-a-Judge)
   - Scientific iteration methodology

2. **10 Advanced RAG Techniques**
   - Chunking R&D & Semantic Chunking
   - Encoder Selection & Optimization
   - Prompt Engineering
   - Document Pre-processing
   - Query Rewriting & Expansion
   - Reranking
   - Hierarchical RAG
   - Graph RAG
   - Agentic RAG

3. **Production Implementation**
   - Direct Chroma API usage (no LangChain)
   - Structured outputs with Pydantic
   - Multiprocessing for speed
   - Cost and latency optimization

## Prerequisites

- Completion of Day 15 (Basic RAG with LangChain)
- OpenAI API key
- Python 3.8+
- Basic understanding of:
  - Vector embeddings
  - LLM prompting
  - Python async/multiprocessing (helpful but not required)

## Setup Instructions

### Option 1: Using Conda (Recommended)

```bash
# Create a new conda environment
conda create -n day16-rag python=3.11 -y

# Activate the environment
conda activate day16-rag

# Install requirements
pip install -r requirements.txt
```

### Option 2: Using venv

```bash
# Create virtual environment
python -m venv venv

# Activate the environment
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activate

# Install requirements
pip install -r requirements.txt
```

### Environment Variables

Create a `.env` file in the Day_16 directory:

```env
OPENAI_API_KEY=your_openai_api_key_here
```

**Important:** Never commit your `.env` file to version control!

## Project Structure

```
Day_16/
├── day16_notebook.ipynb          # Main tutorial notebook
├── requirements.txt               # Python dependencies
├── README.md                      # This file
├── slides_content_day16.md       # Presentation slides content
├── lecture_notes_day16.md        # Detailed lecture notes
├── .env                          # Your API keys (create this)
├── knowledge_base/               # Sample documents (auto-generated)
│   ├── employees/
│   ├── products/
│   └── contracts/
├── vector_db/                    # Chroma vector database (auto-generated)
└── tests.jsonl                   # Evaluation test dataset (auto-generated)
```

## Quick Start

1. **Set up your environment** (see Setup Instructions above)

2. **Create your `.env` file** with your OpenAI API key

3. **Open the Jupyter notebook:**
   ```bash
   jupyter notebook day16_notebook.ipynb
   ```

4. **Run cells sequentially** - The notebook will:
   - Generate a sample knowledge base (InsureElm company)
   - Create a test dataset with 150 questions
   - Build evaluation framework
   - Implement baseline RAG
   - Run experiments with different techniques
   - Show quantitative improvements

## Key Features

### 1. Evaluation Framework

The notebook includes a complete evaluation system:

**Retrieval Metrics:**
- **MRR (Mean Reciprocal Rank)**: Measures position of first relevant chunk
- **NDCG (Normalized Discounted Cumulative Gain)**: Measures distribution of relevant chunks
- **Keyword Coverage**: Percentage of required keywords found

**Answer Quality Metrics:**
- **Accuracy**: Is the information correct?
- **Completeness**: Does it include all necessary information?
- **Relevance**: Is there unnecessary information?

### 2. Advanced Techniques Implemented

**Semantic Chunking:**
- LLM-based intelligent document splitting
- Generates headlines and summaries for each chunk
- Preserves semantic coherence

**Query Rewriting:**
- Clarifies ambiguous questions
- Incorporates conversation history
- Optimizes for retrieval

**Query Expansion:**
- Uses both original and rewritten queries
- Merges results intelligently
- Increases recall without sacrificing precision

**Reranking:**
- LLM reorders chunks by relevance
- Dramatically improves answer quality
- Allows efficient context window usage

### 3. Performance Improvements

**Baseline (Day 15):**
- MRR: 0.7298 🔴
- Accuracy: 3.99 🔴

**After Optimization (Day 16):**
- MRR: 0.9116 🟢 (+25%)
- Accuracy: 4.62 🟢 (+16%)

## Cost Estimates

**One-time Setup:**
- Semantic chunking (76 documents): ~$0.30
- Embeddings (534 chunks): ~$0.05
- **Total: ~$0.35**

**Per Query:**
- Query rewriting: $0.0001
- Reranking: $0.0003
- Answer generation: $0.0005
- **Total: ~$0.001 per question**

**Evaluation (150 tests):**
- Retrieval evaluation: Free
- Answer evaluation (LLM judge): ~$0.02
- **Total: ~$0.02**

## Notebook Contents

### Part 1: Setup & Knowledge Base Creation
- Environment setup and imports
- Generate sample InsureElm company data
- Create 76 documents across 3 categories

### Part 2: Building the Test Dataset
- Generate 150 evaluation questions
- Include keywords and reference answers
- Categorize by question type

### Part 3: Baseline RAG Implementation
- Load documents
- Create embeddings
- Store in Chroma
- Implement basic retrieval

### Part 4: Evaluation Framework
- Calculate MRR, NDCG, Keyword Coverage
- Implement LLM-as-a-Judge
- Create evaluation dashboard

### Part 5: Experiments
- Experiment 1: Chunk size optimization
- Experiment 2: Encoder selection
- Measure impact of each change

### Part 6: Advanced Techniques
- Semantic chunking with LLMs
- Query rewriting and expansion
- Reranking implementation
- Multiprocessing for speed

### Part 7: Final Evaluation
- Compare baseline vs advanced
- Analyze improvements
- Visualize results

## Tips for Success

### 1. Start with Small Experiments
Don't try to implement everything at once. The notebook guides you through incremental improvements.

### 2. Monitor Costs
Each LLM call costs money. The notebook uses GPT-4o-mini by default (affordable), but you can switch to open-source models if preferred.

### 3. Adjust for Your Use Case
The InsureElm example is generic. Think about how to adapt these techniques to your specific domain.

### 4. Save Your Experiments
The notebook includes code to save evaluation results. Keep track of what works!

### 5. Use Multiprocessing Carefully
Start with 3-5 workers. Increase gradually to avoid rate limits.

## Troubleshooting

### Issue: "OpenAI API key not found"
**Solution:** Make sure your `.env` file is in the Day_16 directory and contains:
```
OPENAI_API_KEY=your_actual_key_here
```

### Issue: Rate limit errors
**Solution:** Reduce the number of multiprocessing workers in the semantic chunking cell:
```python
workers = 3  # Start with 3, increase gradually
```

### Issue: Chroma database locked
**Solution:** Restart the Jupyter kernel and delete the `vector_db` folder:
```bash
rm -rf vector_db  # macOS/Linux
rmdir /s vector_db  # Windows
```

### Issue: Out of memory
**Solution:** Process documents in smaller batches:
```python
batch_size = 10
for i in range(0, len(documents), batch_size):
    batch = documents[i:i+batch_size]
    process_batch(batch)
```

### Issue: Slow evaluation
**Solution:** Reduce test set size for faster iteration:
```python
test_questions = test_questions[:50]  # Use 50 instead of 150
```

## Advanced Challenges

### Challenge 1: Beat the Benchmark
**Goal:** Achieve MRR > 0.92 and Accuracy > 4.7

**Strategies:**
- Experiment with different chunk sizes
- Try other embedding models
- Improve prompts
- Implement hierarchical RAG

### Challenge 2: Personal Knowledge Worker
**Goal:** Build a RAG system for your own documents

**Steps:**
1. Collect your documents (Google Drive, notes, etc.)
2. Adapt the ingestion pipeline
3. Create relevant test questions
4. Optimize for your use case

### Challenge 3: Agentic RAG
**Goal:** Implement self-evaluating agentic RAG

**Approach:**
1. Give LLM tools for retrieval
2. Let it decide how to search
3. Evaluate its own answers
4. Iterate until achieving 5/5 scores

## Additional Resources

### Documentation
- **Chroma**: https://docs.trychroma.com/
- **OpenAI Embeddings**: https://platform.openai.com/docs/guides/embeddings
- **Pydantic**: https://docs.pydantic.dev/

### Related Tutorials
- Day 14: Prompt Engineering & Context Management
- Day 15: RAG with LangChain & Chroma
- Week 6-8: Fine-tuning & Production Deployment

### Community
- Share your results on LinkedIn with #RAGOptimization
- Join discussions in the course forum
- Contribute improvements via pull requests

## Key Takeaways

1. **Evaluation is Critical**
   - Can't improve what you don't measure
   - Build test datasets early
   - Use multiple metrics

2. **RAG is Empirical**
   - No one-size-fits-all solution
   - Must experiment with your data
   - Iterate scientifically

3. **Start Simple, Add Complexity**
   - Basic RAG first
   - Add techniques based on failures
   - Measure each addition

4. **LLMs Are Versatile**
   - Use for chunking, rewriting, reranking, judging
   - Structured outputs ensure consistency
   - Cost-effective for most operations

5. **Production Readiness**
   - Consider cost, latency, and reliability
   - Implement error handling
   - Monitor and log everything

## Next Steps

After completing this module:

1. **Apply to Your Domain**
   - Build a RAG system for your company/project
   - Create domain-specific test datasets
   - Optimize for your use case

2. **Explore Advanced Techniques**
   - Implement hierarchical RAG
   - Try graph-based retrieval
   - Build agentic RAG systems

3. **Continue Learning**
   - Week 6: Data Science Foundations
   - Week 7-8: Fine-tuning & Capstone Project
   - Agentic AI Course (optional)

## Support

If you encounter issues or have questions:
1. Check the Troubleshooting section above
2. Review the lecture notes for detailed explanations
3. Consult the course forum
4. Reach out to the instructor

## License

This educational material is provided for learning purposes. Feel free to adapt and use for your projects.

---

**Happy Learning! Build amazing RAG systems! 🚀**

