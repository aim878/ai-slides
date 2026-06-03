# Day 19: Asynchronous Python & OpenAI Agents SDK

## Overview

This folder contains materials for Day 19, focusing on:
- Asynchronous Python fundamentals (async/await)
- OpenAI Agents SDK introduction
- Multi-agent workflows
- Tools and handoffs
- Building an automated Sales Development Rep

## What's Included

### 📊 Day_19_Presentation_Slides.md
35 slides covering:
- Async IO fundamentals and event loops
- OpenAI Agents SDK core concepts
- Agent creation and execution
- Tools and function decorators
- Handoffs vs tools
- Vibe coding best practices
- Multi-agent system architecture

### 📝 Day_19_Lecture_Notes.md
Comprehensive educational content including:
- Deep dive into asynchronous Python
- Coroutines vs functions explained
- Event loop mechanics
- Complete OpenAI Agents SDK tutorial
- Tools and handoffs detailed explanations
- Commercial applications and use cases
- Practical exercises

### 💻 Day_19_OpenAI_Agents.ipynb
Step-by-step Jupyter notebook with:
- Async IO examples
- First agent creation
- Multi-agent workflows
- Tool definition with decorators
- Agent-as-tool pattern
- Handoff implementation
- Complete automated SDR system

### 📄 Additional Files
- `requirements.txt` - Python dependencies
- `README.md` - This file
- `.env.example` - Environment variable template

## Setup Instructions

### Prerequisites
- Python 3.8 or higher (for async/await support)
- Conda or virtualenv
- OpenAI API key
- SendGrid account (free tier)

### Step 1: Create a Conda Environment

```bash
# Create a new conda environment
conda create -n agents-env python=3.11 -y

# Activate the environment
conda activate agents-env
```

### Step 2: Install Dependencies

```bash
# Navigate to the Day_19 folder
cd Day_19

# Install required packages
pip install -r requirements.txt
```

### Step 3: Set Up Environment Variables

Create a `.env` file in the Day_19 directory:

```env
# OpenAI API Key (required)
OPENAI_API_KEY=your_openai_api_key_here

# SendGrid API Key (required for email sending)
SENDGRID_API_KEY=your_sendgrid_api_key_here
```

**Getting your keys:**

**OpenAI API Key:**
1. Go to https://platform.openai.com/api-keys
2. Click "Create new secret key"
3. Copy the key and paste it into your `.env` file

**SendGrid API Key:**
1. Sign up at https://sendgrid.com (free tier available)
2. Go to Settings → API Keys
3. Click "Create API Key"
4. Give it full access and copy the key
5. Go to Settings → Sender Authentication
6. Click "Verify a Single Sender"
7. Verify your email address (you'll use this as the "from" address)

### Step 4: Update Email Addresses in Notebook

In the notebook, update these variables with your actual email addresses:

```python
from_email = "your-verified-sender@email.com"  # Must be verified in SendGrid
to_email = "recipient@email.com"  # Where you want to receive test emails
```

### Step 5: Run the Notebook

```bash
# Start Jupyter
jupyter notebook Day_19_OpenAI_Agents.ipynb
```

Or use Jupyter Lab:

```bash
jupyter lab Day_19_OpenAI_Agents.ipynb
```

## Understanding Async Python

### Key Concepts

**async def** - Creates a coroutine (not a regular function)
- Example: `async def my_function():`

**await** - Executes a coroutine and waits for its result
- Example: `result = await my_function()`

**asyncio.gather()** - Runs multiple coroutines concurrently
- Example: `results = await asyncio.gather(func1(), func2(), func3())`

### Why Async Matters for Agents

- LLM API calls spend most time waiting for network responses
- Async allows other work to happen during waits
- Can run multiple agent interactions concurrently
- Scales to thousands of operations with minimal resources

## OpenAI Agents SDK Basics

### Three Core Concepts

1. **Agent** - A package around LLM calls with specific role and instructions
2. **Handoffs** - Delegation of control from one agent to another
3. **Guardrails** - Checks and controls to ensure proper agent behavior

### Basic Usage Pattern

```python
from agents import Agent, runner, trace

# 1. Create an agent
agent = Agent(
    name="my_agent",
    instructions="You are a helpful assistant",
    model="gpt-4o-mini"
)

# 2. Run with tracing
with trace("my_task"):
    result = await runner.run(agent, "Your message here")
    print(result.final_output)
```

## Tools vs Handoffs

### Tools
- **Purpose**: Give agents capabilities
- **Control Flow**: Agent → Tool → Back to Agent
- **Use When**: Agent needs a feature to complete its task
- **Example**: Sales agent using an email sender

### Handoffs
- **Purpose**: Delegate complete control
- **Control Flow**: Agent A → Agent B (A is done)
- **Use When**: Agent's role is complete, another should take over
- **Example**: Sales manager hands off to email formatter

## Project: Automated Sales Development Rep

The notebook builds a complete multi-agent system in three layers:

### Layer 1: Simple Workflow
- Three sales agents with different styles
- Manual orchestration with Python
- Parallel execution with asyncio.gather()
- Picker agent to select best email

### Layer 2: Agents with Tools
- `@function_tool` decorator for simple tool creation
- Agents wrapped as tools with `.as_tool()`
- Sales manager coordinates tool usage
- SendGrid integration for actual email sending

### Layer 3: Handoffs
- Email formatter agents (subject writer, HTML converter)
- Handoff pattern for delegation
- Complete workflow: generate → pick → format → send

## Vibe Coding Best Practices

From Andrej Karpathy's approach to LLM-assisted development:

1. **Good Vibes** - Craft quality prompts, mention current date
2. **Vibe but Verify** - Ask multiple LLMs the same question
3. **Step Up the Vibe** - Generate 10-15 lines at a time, not 200
4. **Vibe and Validate** - Have one LLM review another's code
5. **Vibe with Variety** - Request 3 different approaches

## Viewing Traces

After running agents with the trace context manager:

1. Go to https://platform.openai.com/traces
2. Find your trace by name
3. Explore:
   - All LLM calls made
   - Tools used
   - Handoffs executed
   - Timing and token usage

This is invaluable for debugging multi-agent systems!

## Exercises

### Beginner
1. Create a new agent with a different personality
2. Run agents in parallel with asyncio.gather()
3. Create a simple tool with @function_tool

### Intermediate
4. Wrap an agent as a tool
5. Create a manager agent that uses 3+ tools
6. Implement a handoff between two agents

### Advanced
7. Build a multi-layer agent system
8. Add fact-checking tools
9. Implement mail merge functionality
10. Create webhook handlers for email replies

## Commercial Applications

This pattern applies to many business processes:

- **Sales**: Cold email generation, follow-ups, lead qualification
- **Recruitment**: Candidate outreach, screening, scheduling
- **Customer Support**: Ticket triage, routing, automated responses
- **Content**: Blog generation, social media, newsletters
- **Operations**: Document processing, data entry, workflow automation

Any process with multiple specialized steps and decision-making can benefit from multi-agent systems!

## Troubleshooting

### Import Error: "No module named 'agents'"

Make sure you have the latest openai package:
```bash
pip install --upgrade openai
```

The `agents` module is part of the openai package.

### Async Errors: "coroutine was never awaited"

Remember to use `await` when calling async functions:
```python
# Wrong
result = runner.run(agent, message)

# Correct
result = await runner.run(agent, message)
```

### SendGrid Errors

- Verify you've created an API key
- Confirm your sender email is verified
- Check that SENDGRID_API_KEY is in your .env file
- Make sure from_email matches your verified sender

### Trace Not Showing Up

- Check you're logged into the correct OpenAI account
- Verify your API key is valid
- Traces may take a few seconds to appear
- Refresh the traces page

## Additional Resources

### Documentation
- OpenAI Agents SDK: https://platform.openai.com/docs/guides/agents
- Python asyncio: https://docs.python.org/3/library/asyncio.html
- SendGrid Python: https://github.com/sendgrid/sendgrid-python

### Tools
- OpenAI Platform: https://platform.openai.com
- Trace Dashboard: https://platform.openai.com/traces
- SendGrid Dashboard: https://app.sendgrid.com

### Learning
- Anthropic's Agentic Design Patterns
- Andrej Karpathy on vibe coding
- Python async/await tutorials

## Next Steps

After completing Day 19:

1. **Practice**: Build your own multi-agent system
2. **Experiment**: Try different tool combinations
3. **Explore**: Test various handoff patterns
4. **Apply**: Think about automating processes in your domain
5. **Continue**: Move on to Day 20 for more advanced patterns

## Support

If you encounter issues:

1. Check the troubleshooting section above
2. Review the lecture notes for detailed explanations
3. Examine the notebook comments and markdown cells
4. Consult the official OpenAI Agents SDK documentation

Remember: The key to mastering agents is practice. Don't just read the code—run it, modify it, break it, and fix it!

## License

This educational material is provided for learning purposes. Please respect API usage limits and SendGrid's terms of service.

---

**Happy Building! 🚀**

