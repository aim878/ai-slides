# Day 14: Model Selection and Introduction to RAG

This folder contains materials for Day 14 of the LLM Engineering course, focusing on model selection strategies and an introduction to Retrieval Augmented Generation (RAG).

## Contents

- `slides_content_day14.md` - Presentation slides content with outline and key points
- `lecture_notes_day14.md` - Detailed lecture notes with explanations and examples
- `day14_notebook.ipynb` - Jupyter notebook with hands-on code examples
- `requirements.txt` - Python package dependencies
- `README.md` - This file
- `.env.example` - Example environment variables file

## Topics Covered

### 1. Model Selection
- Understanding model specifications (parameters, context window, cost)
- Evaluating models based on benchmarks
- Using leaderboards for comparison
- Chinchilla Scaling Law

### 2. Benchmarks and Leaderboards
- Hard benchmarks: GPQA, MMLU Pro, AIME, Live Code Bench, MUSA, HLE
- Benchmark limitations and considerations
- Top leaderboards: Artificial Analysis, Vellum, Scale, Hugging Face, Live Bench

### 3. Introduction to RAG
- What is Retrieval Augmented Generation
- Simple RAG implementation with dictionary lookup
- Limitations of string matching

### 4. Vector Embeddings
- Understanding encoder vs autoregressive LLMs
- What are vector embeddings
- Semantic similarity and cosine similarity
- Improved RAG with embeddings

## Setup Instructions

### Prerequisites
- Python 3.11 or higher
- Conda (Anaconda or Miniconda)
- OpenAI API key

### Step 1: Create a Conda Environment

```bash
# Create a new conda environment
conda create -n day14_env python=3.11 -y

# Activate the environment
conda activate day14_env
```

### Step 2: Install Dependencies

```bash
# Navigate to the Day_14 folder
cd Day_14

# Install required packages
pip install -r requirements.txt
```

### Step 3: Set Up Environment Variables

Create a `.env` file in the Day_14 directory:

```bash
# On Windows
copy .env.example .env

# On macOS/Linux
cp .env.example .env
```

Edit the `.env` file and add your OpenAI API key:

```
OPENAI_API_KEY=your-api-key-here
```

**Important:** Never commit your `.env` file to version control!

### Step 4: Launch Jupyter Notebook

```bash
# Start Jupyter Notebook
jupyter notebook day14_notebook.ipynb
```

Or use Jupyter Lab:

```bash
# Start Jupyter Lab
jupyter lab day14_notebook.ipynb
```

## Running the Notebook

The notebook is organized into sections:

1. **Setup** - Import libraries and configure API access
2. **Simple RAG** - Build a basic RAG system with dictionary lookup
3. **Vector Embeddings** - Learn about embeddings and semantic search
4. **Improved RAG** - Implement semantic RAG with OpenAI embeddings
5. **Interactive Demo** - Launch Gradio interfaces to test both approaches

### Running Individual Cells

- Execute cells in order from top to bottom
- Use `Shift + Enter` to run a cell and move to the next one
- Use `Ctrl + Enter` (or `Cmd + Enter` on Mac) to run a cell without moving

### Testing the Gradio Interface

When you run the Gradio cells, a web interface will open in your browser. You can:
- Ask questions about InsureElm employees and products
- Compare simple string matching vs semantic search
- See how vector embeddings improve retrieval

## Key Concepts

### Simple RAG
- Uses exact string matching
- Fast but brittle
- Fails with typos, synonyms, or variations

### Semantic RAG with Embeddings
- Uses vector embeddings to capture meaning
- More robust and flexible
- Handles synonyms, typos, and related concepts
- Requires API calls to generate embeddings

### Vector Embeddings
- Convert text to numerical vectors (lists of numbers)
- Similar meanings → similar vectors
- Enable semantic search using cosine similarity

## Useful Resources

### Leaderboards
- [Artificial Analysis](https://artificialanalysis.ai) - Comprehensive model comparisons
- [Vellum](https://www.vellum.ai/llm-leaderboard) - API costs and context windows
- [Live Bench](https://livebench.ai) - Anti-contamination benchmarks

### Documentation
- [OpenAI Embeddings Guide](https://platform.openai.com/docs/guides/embeddings)
- [Gradio Documentation](https://gradio.app/docs/)
- [Python dotenv](https://pypi.org/project/python-dotenv/)

### Research Papers
- [Chinchilla Scaling Law](https://arxiv.org/abs/2203.15556)
- [Apple Benchmark Paper](https://arxiv.org/abs/2311.01964)
- [GPQA Paper](https://arxiv.org/abs/2311.12022)

## Troubleshooting

### OpenAI API Key Not Found
- Make sure your `.env` file is in the same directory as the notebook
- Check that the API key is correctly formatted (starts with `sk-`)
- Verify the `.env` file is not named `.env.txt` (Windows may hide the extension)

### Module Not Found Errors
- Ensure you've activated the conda environment: `conda activate day14_env`
- Reinstall requirements: `pip install -r requirements.txt`
- Restart the Jupyter kernel: Kernel → Restart

### Gradio Interface Not Loading
- Check your internet connection
- Try setting `share=False` in the `launch()` call
- Make sure port 7860 is not blocked by firewall

### Rate Limit Errors
- OpenAI has rate limits based on your account tier
- Wait a few seconds between requests
- Consider upgrading your OpenAI account for higher limits

## Cost Considerations

This notebook uses OpenAI's API which incurs costs:

- **GPT-4o-mini**: ~$0.15 per 1M input tokens, ~$0.60 per 1M output tokens
- **text-embedding-3-small**: ~$0.02 per 1M tokens

Estimated cost for running the entire notebook: **< $0.10**

The notebook is designed to minimize costs by:
- Using the cheapest models (gpt-4o-mini, text-embedding-3-small)
- Limiting the number of API calls
- Using a small knowledge base

## Next Steps

After completing this notebook, you'll be ready for:
- Advanced RAG techniques
- Using vector databases (Chroma, Pinecone, Weaviate)
- LangChain framework for production RAG
- Chunking strategies for large documents
- Hybrid search (combining keyword and semantic search)

## Additional Notes

### Knowledge Base
The notebook uses a fictional insurance company "InsureElm" as an example. The knowledge base includes:
- Employee information (CEO, CTO, Data Engineer)
- Product descriptions (CarElm, HealthElm, LifeElm, ClaimElm)

Feel free to modify the knowledge base to test with your own data!

### Extending the Code
You can extend the notebook by:
- Adding more entries to the knowledge base
- Experimenting with different embedding models
- Adjusting the similarity threshold
- Implementing hybrid search (combining string matching and embeddings)
- Adding metadata filtering

## Support

If you encounter any issues or have questions:
1. Check the troubleshooting section above
2. Review the lecture notes for detailed explanations
3. Consult the official documentation links
4. Experiment with the code - it's designed to be educational!

## License

This educational material is provided for learning purposes. Please respect OpenAI's terms of service when using their API.

---

**Happy Learning! 🚀**

Remember: RAG is a practical hack that works remarkably well. Don't worry if it seems imperfect - that's the nature of working with LLMs!

