# Day 13: Building UIs, Tools & Multimodal AI

This notebook covers:
- Building user interfaces with Gradio
- Tool calling (function calling) with LLMs
- Multimodal AI: image generation and text-to-speech

## Prerequisites

- Completed Day 11 and Day 12 materials
- OpenAI API key
- Basic understanding of Python

## Setup Instructions

### 1. Create Conda Environment

```bash
conda create -n day13_env python=3.11 -y
conda activate day13_env
```

### 2. Install Requirements

```bash
pip install -r requirements.txt
```

### 3. Set Up Your API Key

Create a `.env` file in the Day_13 folder:

```
OPENAI_API_KEY=your_api_key_here
```

**Important:** Never commit your `.env` file to version control!

### 4. Run the Notebook

```bash
jupyter notebook day13_notebook.ipynb
```

Or open it in VS Code/Cursor with the Jupyter extension.

## What You'll Learn

1. **Gradio Basics** - Build web UIs with just Python
2. **Chat Interfaces** - Create chatbot UIs effortlessly
3. **Tool Calling** - Give LLMs the ability to call functions
4. **Image Generation** - Create images with DALL-E
5. **Text-to-Speech** - Generate audio from text
6. **Multimodal Assistant** - Combine everything together

## Cost Considerations

Most API calls in this notebook are very cheap (fractions of a cent). However:

- **Image generation** with DALL-E costs approximately **$0.04 per image**
- Be mindful when running image generation cells multiple times

## Folder Structure

```
Day_13/
├── README.md              # This file
├── requirements.txt       # Python dependencies
├── day13_notebook.ipynb   # Main notebook
├── lecture_notes_day13.md # Detailed lecture notes
├── slides_content_day13.md # Presentation slides content
└── .env                   # Your API key (create this yourself)
```

## Troubleshooting

### Gradio not launching?
- Make sure port 7860 is available (or try a different port)
- Try `demo.launch(server_port=7861)` if default port is busy

### Import errors?
- Ensure you've activated the conda environment
- Run `pip install -r requirements.txt` again

### API key errors?
- Check that `.env` file is in the correct folder
- Verify your API key is valid and has credits

## Next Steps

After completing this notebook:
1. Build a custom Gradio app for your use case
2. Add your own tools to the assistant
3. Experiment with different prompts for image generation
4. Try combining multiple modalities in creative ways

## Resources

- [Gradio Documentation](https://www.gradio.app/docs)
- [OpenAI Function Calling Guide](https://platform.openai.com/docs/guides/function-calling)
- [DALL-E API Reference](https://platform.openai.com/docs/api-reference/images)
- [Text-to-Speech API Reference](https://platform.openai.com/docs/api-reference/audio)

