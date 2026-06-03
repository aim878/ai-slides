# Day 12: Deep Dive into LLM APIs & Building Applications

This is the continuation from Day 11. Make sure you have completed Day 11 setup first.

## What You'll Learn

- Understanding how the Chat Completions API really works
- Tokens and tokenization
- The illusion of memory in LLMs
- Building multi-step applications
- JSON responses and streaming

## Prerequisites

- Day 11 completed
- OpenAI API key (or you can modify for other providers)
- Python 3.9+ installed

## Setup Instructions

### Option 1: Use Existing Day 11 Environment

If you already have the environment from Day 11:

```bash
conda activate llm-env
pip install tiktoken beautifulsoup4 requests
```

### Option 2: Create Fresh Environment

```bash
# Create conda environment
conda create -n llm-day12 python=3.11 -y

# Activate it
conda activate llm-day12

# Install requirements
pip install -r requirements.txt
```

## Setting Up Your API Key

Create a `.env` file in this folder:

```
OPENAI_API_KEY=sk-your-api-key-here
```

**Important:** Never commit your `.env` file to version control!

## Running the Notebook

```bash
# Make sure environment is activated
conda activate llm-day12  # or llm-env

# Start Jupyter
jupyter notebook
```

Then open `day12_notebook.ipynb` and run the cells.

## Files in This Folder

- `day12_notebook.ipynb` - Main notebook with code examples
- `requirements.txt` - Python packages needed
- `README.md` - This file
- `.env` - Your API key (create this yourself)

## Topics Covered

1. **Chat Completions API Deep Dive**
   - Understanding endpoints
   - What the OpenAI library actually does

2. **Tokens and Tokenization**
   - Using tiktoken library
   - Counting tokens
   - Understanding costs

3. **The Illusion of Memory**
   - Demonstrating stateless API calls
   - Building conversation history

4. **Multi-Step Applications**
   - Chaining LLM calls
   - JSON response format
   - Streaming responses

## Troubleshooting

### "Module not found" Error
Make sure you activated the conda environment and installed requirements.

### API Key Issues
Check that your `.env` file is in the same folder as the notebook.

### Kernel Not Found
Run: `python -m ipykernel install --user --name=llm-day12`

## Next Steps

After completing this notebook:
- Experiment with different prompts
- Try building your own multi-step application
- Compare responses from different models

