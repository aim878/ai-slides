# LLM Engineering - Getting Started

This guide will help you set up your environment and run the notebook.

## Prerequisites

- Anaconda or Miniconda installed
- OpenAI API key (get one at https://platform.openai.com)

## Setup Instructions

### Step 1: Create a Conda Environment

Open your terminal (or Anaconda Prompt on Windows) and run:

```bash
conda create -n llm-env python=3.11 -y
```

### Step 2: Activate the Environment

```bash
conda activate llm-env
```

### Step 3: Install Required Packages

Navigate to this folder and install the requirements:

```bash
pip install -r requirements.txt
```

### Step 4: Set Up Your API Key

Create a file named `.env` in this folder (same location as the notebook).

Add your OpenAI API key to the file:

```
OPENAI_API_KEY=sk-your-api-key-here
```

**Important:** Replace `sk-your-api-key-here` with your actual API key.

### Step 5: Run the Notebook

Start Jupyter:

```bash
jupyter notebook
```

Then open `llm_intro.ipynb` and run the cells!

## Troubleshooting

### "API key not found" error
- Make sure your `.env` file is in the same folder as the notebook
- Check that the key starts with `sk-`
- Make sure you saved the `.env` file after adding your key

### "Module not found" error
- Make sure you activated the conda environment: `conda activate llm-env`
- Reinstall requirements: `pip install -r requirements.txt`

### Kernel not showing in Jupyter
Run this command:
```bash
python -m ipykernel install --user --name=llm-env
```

## Files in This Folder

- `llm_intro.ipynb` - The main notebook with code examples
- `requirements.txt` - Python packages needed
- `.env` - Your API key (you create this)
- `README.md` - This file

## Next Steps

After running the notebook successfully:
1. Try changing the system prompt to get different responses
2. Experiment with different models (gpt-4.1-mini, gpt-4.1-nano)
3. Build your own application using the patterns learned!

