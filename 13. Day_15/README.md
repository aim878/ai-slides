# Day 15: Building RAG with LangChain & Chroma

## Overview

This project demonstrates how to build a complete Retrieval Augmented Generation (RAG) system using LangChain and Chroma vector database. You'll learn to:

- Use LangChain for document processing
- Chunk documents with text splitters
- Create vector embeddings with encoder models
- Store vectors in Chroma database
- Visualize vectors in 2D and 3D
- Build a complete RAG pipeline
- Deploy with Gradio interface

## Prerequisites

- Python 3.8 or higher
- OpenAI API key
- Conda (recommended) or pip

## Setup Instructions

### Option 1: Using Conda (Recommended)

1. **Create a new conda environment**:
```bash
conda create -n day15_rag python=3.11
```

2. **Activate the environment**:
```bash
conda activate day15_rag
```

3. **Install the required packages**:
```bash
pip install -r requirements.txt
```

### Option 2: Using pip with virtual environment

1. **Create a virtual environment**:
```bash
python -m venv venv
```

2. **Activate the environment**:
   - On Windows:
   ```bash
   venv\Scripts\activate
   ```
   - On macOS/Linux:
   ```bash
   source venv/bin/activate
   ```

3. **Install the required packages**:
```bash
pip install -r requirements.txt
```

## Environment Configuration

1. **Create a `.env` file** in the same directory as the notebook:
```bash
OPENAI_API_KEY=your_openai_api_key_here
```

2. **Get your OpenAI API key**:
   - Go to https://platform.openai.com/api-keys
   - Create a new API key
   - Copy and paste it into your `.env` file

## Project Structure

```
Day_15/
├── day15_notebook.ipynb          # Main Jupyter notebook
├── requirements.txt              # Python dependencies
├── README.md                     # This file
├── .env                          # Your API keys (create this)
├── knowledge_base/               # Sample documents (create this)
│   ├── employees/
│   ├── products/
│   ├── contracts/
│   └── company/
└── vector_db/                    # Created automatically by Chroma
```

## Running the Notebook

1. **Start Jupyter**:
```bash
jupyter notebook
```

2. **Open** `day15_notebook.ipynb`

3. **Run the cells** in order from top to bottom

## What's Covered

### Part 1: LangChain Introduction
- Understanding LangChain pros and cons
- Setting up LangChain packages
- Working with document loaders

### Part 2: Document Chunking
- Loading documents from directories
- Using RecursiveCharacterTextSplitter
- Experimenting with chunk sizes and overlap

### Part 3: Vector Embeddings
- Understanding encoder models
- Using Hugging Face embeddings (all-MiniLM-L6-v2)
- Comparing with OpenAI embeddings

### Part 4: Vector Database
- Creating Chroma vector store
- Storing document chunks as vectors
- Querying the vector database

### Part 5: Visualization
- Using t-SNE for dimensionality reduction
- Creating 2D scatter plots
- Creating interactive 3D visualizations
- Comparing different encoders

### Part 6: RAG Pipeline
- Building a retriever
- Creating RAG functions
- Handling conversation history
- Testing with queries

### Part 7: Gradio Application
- Creating a chat interface
- Deploying the RAG system
- Testing with real queries

## Sample Knowledge Base

The notebook expects a `knowledge_base` directory with markdown files. You can create sample files or use your own documents.

**Example structure**:
```
knowledge_base/
├── employees/
│   └── avery_lancaster.md
├── products/
│   └── car_insurance.md
├── contracts/
│   └── service_agreement.md
└── company/
    └── about.md
```

## Key Concepts

### Document Chunking
Breaking large documents into smaller, focused pieces for better retrieval:
- **Chunk size**: 1000 characters (typical)
- **Chunk overlap**: 200 characters (20%)
- **Strategy**: RecursiveCharacterTextSplitter

### Encoder Models
Models that convert text to vector embeddings:
- **all-MiniLM-L6-v2**: 384 dimensions, free, open source
- **text-embedding-3-small**: 1,536 dimensions, OpenAI, paid
- **text-embedding-3-large**: 3,072 dimensions, OpenAI, best quality

### Vector Databases
Specialized databases for storing and searching vectors:
- **Chroma**: Easy to use, SQLite-based, perfect for development
- **FAISS**: Fast, in-memory, great for prototyping
- **Pinecone**: Managed, scalable, production-ready

### RAG Pipeline
The complete flow:
1. User asks a question
2. Question → vector (encoder)
3. Search vector database
4. Retrieve relevant chunks
5. Augment prompt with context
6. LLM generates response

## Common Issues and Solutions

### Issue: Import errors for LangChain packages
**Solution**: Make sure you've installed all packages from requirements.txt. LangChain has multiple packages that need to be installed separately.

### Issue: Hugging Face model download fails
**Solution**: The first time you run the notebook, it will download the model (~100MB). Ensure you have a stable internet connection.

### Issue: OpenAI API errors
**Solution**: 
- Check your API key is correct in `.env`
- Ensure you have credits in your OpenAI account
- Verify the `.env` file is in the same directory as the notebook

### Issue: Chroma database errors
**Solution**: If you get database errors, delete the `vector_db` directory and run the ingestion cells again.

### Issue: Gradio interface doesn't launch
**Solution**: 
- Check if port 7860 is already in use
- Try `demo.launch(server_port=7861)` to use a different port
- Use `share=False` for local-only access

## Performance Tips

1. **Start with small encoder**: Use all-MiniLM-L6-v2 for development (free and fast)
2. **Upgrade for production**: Switch to OpenAI embeddings for better quality
3. **Optimize chunk size**: Experiment with different sizes for your data
4. **Cache embeddings**: Chroma persists vectors, so you don't need to recreate them
5. **Use appropriate k**: Retrieve 3-10 chunks depending on your use case

## Cost Considerations

**Hugging Face (all-MiniLM-L6-v2)**:
- Cost: Free
- Speed: Fast
- Quality: Good for development

**OpenAI text-embedding-3-small**:
- Cost: ~$0.02 per 1M tokens
- Speed: Fast
- Quality: Very good

**OpenAI text-embedding-3-large**:
- Cost: ~$0.13 per 1M tokens
- Speed: Moderate
- Quality: Best

**Example**: 400 chunks × 500 tokens = 200K tokens
- Small: $0.004 (less than a penny)
- Large: $0.026 (3 cents)

## Next Steps

After completing this notebook:

1. **Experiment with your own documents**
   - Add your own knowledge base
   - Test different document types
   - Measure retrieval quality

2. **Try different encoders**
   - Compare all three encoders
   - Visualize the differences
   - Choose the best for your use case

3. **Optimize chunking**
   - Test different chunk sizes
   - Experiment with overlap
   - Measure impact on retrieval

4. **Build production system**
   - Separate ingestion from querying
   - Add error handling
   - Deploy with proper UI

5. **Advanced techniques**
   - Hybrid search (keyword + semantic)
   - Re-ranking strategies
   - Query expansion
   - Evaluation metrics

## Resources

**LangChain**:
- Documentation: https://python.langchain.com/
- GitHub: https://github.com/langchain-ai/langchain

**Chroma**:
- Documentation: https://docs.trychroma.com/
- GitHub: https://github.com/chroma-core/chroma

**Sentence Transformers**:
- Documentation: https://www.sbert.net/
- Models: https://huggingface.co/sentence-transformers

**OpenAI**:
- Embeddings Guide: https://platform.openai.com/docs/guides/embeddings
- API Reference: https://platform.openai.com/docs/api-reference

## Troubleshooting

If you encounter any issues:

1. **Check Python version**: `python --version` (should be 3.8+)
2. **Verify packages**: `pip list | grep langchain`
3. **Test API key**: Run a simple OpenAI API call
4. **Check logs**: Look for error messages in notebook output
5. **Clear cache**: Delete `vector_db` and restart kernel

## Support

For questions or issues:
- Review the lecture notes
- Check the LangChain documentation
- Examine the code comments in the notebook
- Test with smaller datasets first

## License

This project is for educational purposes. Please ensure you comply with:
- OpenAI's usage policies
- LangChain's license (MIT)
- Chroma's license (Apache 2.0)

## Acknowledgments

- LangChain team for the excellent framework
- Chroma team for the easy-to-use vector database
- Sentence Transformers for open source embeddings
- OpenAI for powerful embedding models

---

**Happy RAG Building!** 🚀

Remember: RAG is one of the most important techniques in modern LLM applications. Master it, and you'll be able to build powerful, knowledge-grounded AI systems!

