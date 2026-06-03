# Day 20: Deep Research Agent with Multi-Model Orchestration

## Overview

This folder contains materials for Day 20 of the AI Agents course, focusing on building a production-ready Deep Research Agent with multi-model orchestration, structured outputs, and guardrails.

## What You'll Learn

- **Multi-Model Orchestration**: Using different AI models (OpenAI, Gemini, DeepSeek, Grok) in one workflow
- **Structured Outputs**: Enforcing specific response schemas using Pydantic models
- **Guardrails**: Protecting your system with input/output validation
- **Deep Research Agent**: Building a complete research automation system
- **Production Practices**: Converting notebooks to Python modules with Gradio UI

## Files in This Folder

- **`Day_20_Presentation_Slides.md`**: Presentation slides (46 slides, no code blocks)
- **`Day_20_Lecture_Notes.md`**: Detailed lecture notes with explanations and examples
- **`Day_20_transcript.txt`**: Original lecture transcript
- **`requirements.txt`**: Python dependencies
- **`README.md`**: This file

## Setup Instructions

### 1. Create a Conda Environment

```bash
# Create a new conda environment
conda create -n llm-env python=3.11 -y

# Activate the environment
conda activate llm-env
```

### 2. Install Requirements

```bash
# Navigate to the Day_20 folder
cd Day_20

# Install all dependencies
pip install -r requirements.txt
```

### 3. Set Up Environment Variables

Create a `.env` file in the `Day_20` folder with your API keys:

```env
# Required
OPENAI_API_KEY=your_openai_api_key_here

# Optional: For multi-model orchestration
GOOGLE_API_KEY=your_google_api_key_here
DEEPSEEK_API_KEY=your_deepseek_api_key_here
GROK_API_KEY=your_grok_api_key_here

# Optional: For email functionality
SENDGRID_API_KEY=your_sendgrid_api_key_here
```

### 4. Get API Keys

#### OpenAI (Required)
1. Go to https://platform.openai.com/api-keys
2. Create a new API key
3. Add credits to your account

#### SendGrid (For Email Features)
1. Sign up at https://sendgrid.com/
2. Create an API key with "Mail Send" permissions
3. Verify your sender email address

#### Optional Model Providers
- **Google Gemini**: https://ai.google.dev/
- **DeepSeek**: https://www.deepseek.com/
- **Grok (xAI)**: https://x.ai/

## Key Concepts

### 1. Multi-Model Orchestration

Use different AI models in the same workflow by configuring OpenAI-compatible endpoints:

```python
from openai import AsyncOpenAI

# Gemini
gemini_client = AsyncOpenAI(
    base_url="https://generativelanguage.googleapis.com/v1beta/openai/",
    api_key=os.getenv("GOOGLE_API_KEY")
)

# DeepSeek
deepseek_client = AsyncOpenAI(
    base_url="https://api.deepseek.com",
    api_key=os.getenv("DEEPSEEK_API_KEY")
)
```

### 2. Structured Outputs

Define Pydantic models to enforce response schemas:

```python
from pydantic import BaseModel

class WebSearchItem(BaseModel):
    reason: str
    """Why this search is important"""
    query: str
    """The search query to execute"""

class WebSearchPlan(BaseModel):
    searches: list[WebSearchItem]
    """List of searches to perform"""
```

### 3. Guardrails

Validate inputs and outputs to protect your system:

```python
class NameCheckOutput(BaseModel):
    is_name_in_message: bool
    name: str

# Use an LLM to check for PII
def check_for_names(message: str) -> NameCheckOutput:
    # Returns structured validation result
    pass
```

### 4. Deep Research Workflow

The complete research agent follows these steps:

1. **Plan**: Convert query into specific search queries
2. **Search**: Execute searches in parallel (using OpenAI's WebSearchTool)
3. **Synthesize**: Combine results into comprehensive report
4. **Deliver**: Send formatted report via email

## Important Cost Information

### OpenAI Web Search Tool Pricing

As of early 2025, OpenAI's hosted Web Search Tool costs:

- **GPT-4o-mini with low search context**: ~$0.025 per search
- **3 searches**: ~$0.08
- **10 searches**: ~$0.25
- **20 searches**: ~$0.50

**Tips to Control Costs:**
- Start with 3 searches to test
- Use "low" search context size
- Use GPT-4o-mini model (cheapest)
- Monitor usage on OpenAI dashboard

Check current pricing: https://openai.com/api/pricing/

## Architecture

### Deep Research Agent Components

1. **Planner Agent**
   - Input: Research query
   - Output: List of search queries (structured)
   - Model: GPT-4o-mini

2. **Search Agent**
   - Input: Search query
   - Output: 2-3 paragraph summary
   - Tool: OpenAI WebSearchTool
   - Model: GPT-4o-mini

3. **Writer Agent**
   - Input: Query + search results
   - Output: Comprehensive report (structured)
   - Model: GPT-4o-mini

4. **Email Agent**
   - Input: Report
   - Output: HTML email
   - Tool: SendGrid API
   - Model: GPT-4o-mini

5. **Research Manager**
   - Orchestrates all agents
   - Handles parallel execution
   - Provides progress updates

## Implementation Notes

### Simplified Version

The materials include a simplified implementation that:
- Uses standard OpenAI API (not Agents SDK)
- Simulates web searches (to avoid costs during learning)
- Demonstrates all key concepts
- Is fully functional and extensible

### Production Version

For production use, you would:
- Use the OpenAI Agents SDK (currently in beta)
- Use real WebSearchTool for actual web searches
- Add proper error handling and retries
- Implement rate limiting
- Add monitoring and logging
- Deploy with Gradio or FastAPI

## Running the Code

### Option 1: Jupyter Notebook (Recommended for Learning)

```bash
# Activate environment
conda activate llm-env

# Start Jupyter
jupyter notebook

# Open the notebook and run cells
```

### Option 2: Python Script

If you create a Python script version:

```bash
# Activate environment
conda activate llm-env

# Run the script
python deep_research.py
```

### Option 3: Gradio Web Interface

For a user-friendly web interface:

```bash
# Activate environment
conda activate llm-env

# Run Gradio app
python deepresearch.py

# Access at http://localhost:7860
```

## Enhancements

### 1. Add Clarifying Questions

Ask the user for clarification before starting research:

```python
class ClarifyingQuestions(BaseModel):
    questions: list[str]

def generate_clarifying_questions(query: str) -> ClarifyingQuestions:
    # Generate 3 questions to better understand the query
    pass
```

### 2. Query Refinement

Update searches based on initial findings:

```python
def refine_search_plan(original_query, initial_results) -> WebSearchPlan:
    # Generate refined searches based on what we've learned
    pass
```

### 3. Evaluator-Optimizer Pattern

Add an agent to review and improve results:

```python
class EvaluationResult(BaseModel):
    quality_score: int
    strengths: list[str]
    gaps: list[str]
    improvement_suggestions: list[str]

def evaluate_report(query, report) -> EvaluationResult:
    # Assess report quality and suggest improvements
    pass
```

### 4. Autonomous Manager

Let the agent decide when to do more searches:

```python
# Instead of fixed workflow, give the manager autonomy
manager_agent = Agent(
    name="Research Manager",
    instructions="Coordinate research. Decide if more searches are needed.",
    tools=[plan_searches_tool, perform_search_tool, write_report_tool]
)
```

## Deployment

### Deploy to Hugging Face Spaces

If you create a Gradio interface:

```bash
# Install Gradio
pip install gradio

# Deploy to Hugging Face Spaces
gradio deploy
```

This gives you a free public URL to share your Deep Research Agent!

## Troubleshooting

### Common Issues

1. **"OpenAI API key not found"**
   - Make sure `.env` file is in the correct directory
   - Check that `OPENAI_API_KEY` is set correctly
   - Try `load_dotenv(override=True)`

2. **"SendGrid API key not found"**
   - Email features are optional
   - Comment out email-related code if not using
   - Verify your SendGrid API key has "Mail Send" permissions

3. **"Module not found" errors**
   - Make sure you're in the correct conda environment
   - Run `pip install -r requirements.txt` again
   - Check Python version (should be 3.11+)

4. **High costs**
   - Start with `NUM_SEARCHES = 3`
   - Use simulated searches during development
   - Monitor usage on OpenAI dashboard
   - Set spending limits in OpenAI account settings

5. **Slow execution**
   - This is normal for web searches (each takes 5-10 seconds)
   - Parallel execution helps (all searches run simultaneously)
   - Consider caching results for repeated queries

## Resources

### Documentation
- **OpenAI API**: https://platform.openai.com/docs
- **OpenAI Agents SDK**: https://platform.openai.com/docs/agents
- **Pydantic**: https://docs.pydantic.dev/
- **Gradio**: https://gradio.app/
- **SendGrid**: https://docs.sendgrid.com/

### Alternative Models
- **Google Gemini**: https://ai.google.dev/
- **DeepSeek**: https://www.deepseek.com/
- **Grok (xAI)**: https://x.ai/
- **Open Router**: https://openrouter.ai/

### Pricing
- **OpenAI Pricing**: https://openai.com/api/pricing/
- **OpenAI Trace Viewer**: https://platform.openai.com/traces

## Next Steps

1. **Complete the notebook**: Work through all cells and understand each component
2. **Experiment**: Try different queries and search counts
3. **Enhance**: Add clarifying questions, query refinement, or evaluation
4. **Deploy**: Create a Gradio interface and deploy to Hugging Face Spaces
5. **Share**: Post your Deep Research Agent as a portfolio piece!

## Week 2 Complete!

Congratulations on completing Week 2! You've learned:
- Day 17: Agent fundamentals and design patterns
- Day 18: Multi-model orchestration and career agent
- Day 19: Async IO and OpenAI Swarm framework
- Day 20: Structured outputs, guardrails, and Deep Research

**Next Week**: Crew AI - A different agent framework with role-based design!

## Support

If you have questions or run into issues:
1. Check the troubleshooting section above
2. Review the lecture notes for detailed explanations
3. Consult the official documentation links
4. Experiment with simpler versions first

Happy building! 🚀

