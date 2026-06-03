# Day 18: Building a Career Conversation Agent

## Overview

This project builds a career chatbot that can answer questions about your professional background using AI. The agent uses:
- **Resources**: Your LinkedIn profile and personal summary to answer questions
- **Tools**: Functions to record user interest and unknown questions
- **Agent Loop**: Iterative execution with LLM + tools until goal achieved

## Project Structure

```
Day_18/
├── Day_18_Career_Agent.ipynb    # Main Jupyter notebook with code
├── Day_18_Presentation_Slides.md # Presentation slides
├── Day_18_Lecture_Notes.md       # Detailed lecture notes
├── requirements.txt               # Python dependencies
├── README.md                      # This file
├── .env.example                   # Example environment variables
└── me/                            # Your personal files (create this!)
    ├── linkedin.pdf               # Your LinkedIn profile PDF
    └── summary.txt                # Your personal summary
```

## Prerequisites

- Python 3.10 or higher
- OpenAI API key
- (Optional) Pushover account for notifications

## Setup Instructions

### Option 1: Using Conda (Recommended)

#### Step 1: Create Conda Environment

```bash
# Create a new conda environment
conda create -n career-agent python=3.10 -y

# Activate the environment
conda activate career-agent
```

#### Step 2: Install Dependencies

```bash
# Install required packages
pip install -r requirements.txt
```

#### Step 3: Set Up Environment Variables

Create a `.env` file in the Day_18 directory:

```bash
# Copy the example file
cp .env.example .env

# Edit .env and add your keys
# Use any text editor (notepad, nano, vim, etc.)
```

Your `.env` file should contain:

```
OPENAI_API_KEY=your_openai_api_key_here
PUSHOVER_USER=your_pushover_user_key_here
PUSHOVER_TOKEN=your_pushover_api_token_here
```

**Getting Your Keys:**

**OpenAI API Key:**
1. Go to https://platform.openai.com/api-keys
2. Create a new API key
3. Copy and paste into .env file

**Pushover (Optional):**
1. Go to https://pushover.net
2. Create an account
3. Install Pushover app on your phone
4. Get your User Key and create an API Token
5. Add to .env file

#### Step 4: Prepare Your Personal Files

Create a `me/` folder and add your files:

```bash
# Create the directory
mkdir me

# Add your files:
# - me/linkedin.pdf: Download your LinkedIn profile as PDF
# - me/summary.txt: Write a brief summary about yourself
```

**Getting Your LinkedIn PDF:**
1. Go to your LinkedIn profile
2. Click "More" (three dots)
3. Select "Save to PDF"
4. Save as `me/linkedin.pdf`

**Creating Your Summary:**
Create `me/summary.txt` with content like:

```
I'm currently a Software Engineer at TechCorp, specializing in AI and machine learning.
I have 5 years of experience building scalable systems and leading technical teams.
Fun fact: I love hiking and have climbed 15 mountains!
```

#### Step 5: Run the Notebook

```bash
# Start Jupyter
jupyter notebook Day_18_Career_Agent.ipynb
```

Or use Jupyter Lab:

```bash
jupyter lab Day_18_Career_Agent.ipynb
```

### Option 2: Using venv (Alternative)

If you prefer using Python's built-in virtual environment:

```bash
# Create virtual environment
python -m venv venv

# Activate it
# On Windows:
venv\Scripts\activate
# On Mac/Linux:
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Follow steps 3-5 from Option 1
```

## Running the Agent

### In Jupyter Notebook

1. Open `Day_18_Career_Agent.ipynb`
2. Run all cells in order
3. The Gradio interface will launch in your browser
4. Start chatting with your career agent!

### As a Python Script

If you want to run it as a standalone script:

```bash
# Convert notebook to Python script
jupyter nbconvert --to script Day_18_Career_Agent.ipynb

# Run the script
python Day_18_Career_Agent.py
```

## Testing the Agent

Try these questions to test different features:

**Testing Resources (Career Information):**
- "What's your current job?"
- "What's your educational background?"
- "Tell me about your experience"
- "What are your key skills?"

**Testing Unknown Question Tool:**
- "What's your favorite color?"
- "Who's your favorite musician?"
- Any question not covered in your resources

**Testing User Details Tool:**
- "I'd like to get in touch"
- Provide your email when asked
- Check your phone for notification (if Pushover configured)

## Deploying to Production

### Deploy to Hugging Face Spaces

1. Convert notebook to app.py (or use the code directly)
2. Run deployment command:

```bash
gradio deploy
```

3. Answer the prompts:
   - App title: "Career Conversation"
   - App file: "app.py"
   - Hardware: "cpu-basic" (free tier)
   - Secrets: Add your API keys
   - GitHub action: No

4. Your app will be live at:
   `https://huggingface.co/spaces/YOUR_USERNAME/career-conversation`

### Embed in Your Website

After deploying to Hugging Face, you can embed it in your website:

```html
<iframe
  src="https://huggingface.co/spaces/YOUR_USERNAME/career-conversation"
  width="100%"
  height="600px"
  frameborder="0"
></iframe>
```

## Troubleshooting

### Common Issues

**Issue: "Module not found" errors**
```bash
# Make sure environment is activated
conda activate career-agent

# Reinstall requirements
pip install -r requirements.txt
```

**Issue: "OpenAI API key not found"**
```bash
# Check .env file exists
ls .env

# Check it contains OPENAI_API_KEY
cat .env

# Make sure no spaces around = sign
# Correct: OPENAI_API_KEY=sk-...
# Wrong: OPENAI_API_KEY = sk-...
```

**Issue: "File not found: me/linkedin.pdf"**
```bash
# Create the me directory
mkdir me

# Add your files
# Make sure files are named exactly:
# - linkedin.pdf (not LinkedIn.pdf or linkedin.PDF)
# - summary.txt (not summary.TXT)
```

**Issue: Gradio interface doesn't launch**
```bash
# Try specifying a different port
# In the notebook, change:
interface.launch()
# To:
interface.launch(server_port=7861)
```

**Issue: Pushover notifications not working**
```bash
# This is optional! The agent works without it
# Check your keys are correct in .env
# Test with: push("Test message")
```

## Customization

### Change the Model

In the notebook, find this line:

```python
model="gpt-4-mini"
```

Change to any OpenAI model:
- `gpt-4-mini` - Fast and cheap (recommended)
- `gpt-4` - More capable but expensive
- `gpt-3.5-turbo` - Cheaper alternative

### Add More Tools

Create a new function:

```python
def my_new_tool(parameter):
    """Description of what this tool does"""
    # Your code here
    return "Result"
```

Add its JSON description to the `tools` list.

### Improve the UI

Customize Gradio interface:

```python
interface = gr.ChatInterface(
    fn=chat,
    title="My Custom Title",
    theme=gr.themes.Soft(),  # Try different themes
    examples=[...],  # Add custom examples
)
```

## Learning Resources

- **Day 18 Lecture Notes**: Detailed explanations of all concepts
- **Day 18 Slides**: Quick reference for key points
- **OpenAI Documentation**: https://platform.openai.com/docs
- **Gradio Documentation**: https://gradio.app/docs
- **Pushover Documentation**: https://pushover.net/api

## Next Steps

1. **Improve Resources**: Add more information about yourself
2. **Add More Tools**: Database queries, calendar integration, etc.
3. **Implement RAG**: Better context retrieval for large documents
4. **Add Evaluator**: Quality control for responses
5. **Deploy**: Put it on your website!
6. **Share**: Post on LinkedIn and get feedback

## Support

If you encounter issues:
1. Check the troubleshooting section above
2. Review the lecture notes for detailed explanations
3. Check that all files are in the correct locations
4. Verify your API keys are correct
5. Make sure your environment is activated

## License

This project is for educational purposes. Feel free to use and modify for your own career agent!

## Acknowledgments

Based on the AI Agents Engineering course. This project demonstrates:
- Direct LLM API usage (no frameworks)
- Tool calling implementation
- Agent loop pattern
- Production deployment

Happy building! 🚀

