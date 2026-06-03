# Introduction to LLM Engineering
## Presentation Slides

---

## Slide 1: Title

# Introduction to Large Language Models (LLMs)

**Your Journey into AI Engineering**

From Understanding the Basics to Building Real Applications

---

## Slide 2: What We'll Cover Today

### Learning Objectives

1. Understanding what LLMs are and how they work
2. Running open source models locally with Ollama
3. Navigating the landscape of AI models
4. Making API calls to GPT
5. Mastering system prompts vs user prompts
6. Building a practical website summarizer

---

## Slide 3: What is a Large Language Model?

### Definition

An AI system trained on massive text data that can understand and generate human-like text.

### How It Works

- Predicts the next word based on context
- Trained on trillions of words from the internet
- Uses billions of parameters (learned patterns)

### Simple Analogy

Like autocomplete on your phone, but 1000x smarter.

---

## Slide 4: The Scale of LLMs

### Training Data
- Trillions of words from books, websites, articles

### Model Size  
- 270 million to 1.7 trillion parameters

### Compute Power
- Thousands of GPUs training for weeks/months

### Result
- Systems that can write, code, analyze, and reason

---

## Slide 5: What is Ollama?

### The Tool

A free application that runs AI models on YOUR computer.

### Key Features

- Works on Windows, Mac, Linux
- Simple command-line interface
- Downloads and manages models automatically
- No internet needed after download

### Why It Matters

**Privacy** + **Free** + **Fast** = Local AI

**Website:** ollama.com

---

## Slide 6: Installing Ollama

### Steps

1. Go to **ollama.com**
2. Click **Download**
3. Choose your OS (Windows/Mac/Linux)
4. Install (just like any application)
5. Open Terminal or PowerShell

### Verify Installation

```bash
ollama
```

*Should show help menu with commands*

---

## Slide 7: Running Your First Model

### The Command

```bash
ollama run gemma3:270m
```

### What This Does

- `ollama run` → Start a model
- `gemma3` → Google's Gemma 3 model
- `:270m` → 270 million parameter version

### Then Just Chat!

```
>>> Hello!
Hi there! How can I help you today?
```

*Press Ctrl+D to exit*

---

## Slide 8: Understanding Model Sizes

### Parameters = Model's "Brain Cells"

| Size | Parameters | RAM Needed | Speed |
|------|------------|------------|-------|
| Tiny | 270M-1B | 1-2 GB | Very Fast |
| Small | 1-3B | 2-4 GB | Fast |
| Medium | 7-8B | 8-12 GB | Medium |
| Large | 70B+ | 40GB+ | Slow |

### Rule of Thumb

~1 GB RAM per 1 billion parameters

---

## Slide 9: Models Available in Ollama

| Model | Creator | Sizes | Best For |
|-------|---------|-------|----------|
| Gemma | Google | 270M - 27B | General use |
| Llama | Meta | 1B - 405B | All-around |
| Phi | Microsoft | 3.8B | Efficiency |
| Mistral | Mistral AI | 7B | Performance |
| Qwen | Alibaba | 0.5B - 110B | Versatility |
| DeepSeek | DeepSeek | Various | Reasoning |

**Full list:** ollama.com/library

---

## Slide 10: Frontier Models (Closed Source)

### The Big Four

| Company | Model | Known For |
|---------|-------|-----------|
| **OpenAI** | GPT-4, GPT-5 | Industry standard, versatile |
| **Anthropic** | Claude | Nuanced, thoughtful |
| **Google** | Gemini | Multimodal, integrated |
| **xAI** | Grok | Real-time info |

### Characteristics

- Most powerful models available
- Accessed via paid APIs or subscriptions
- Training cost: $100M+

---

## Slide 11: Open Source Models

### Major Players

| Model | Company | Why Notable |
|-------|---------|-------------|
| **Llama** | Meta | Most popular open source |
| **Mistral** | Mistral AI | Excellent efficiency |
| **Qwen** | Alibaba | Underrated powerhouse |
| **Gemma** | Google | Small but capable |
| **DeepSeek** | DeepSeek AI | Near-GPT performance, low cost |

### Advantages

Free, private, customizable, no internet required

---

## Slide 12: Closed vs Open Source Comparison

| Aspect | Closed Source | Open Source |
|--------|--------------|-------------|
| **Cost** | Pay per use | Free |
| **Privacy** | Data sent to servers | Stays on your machine |
| **Capability** | Generally higher | Rapidly improving |
| **Customization** | Limited | Full control |
| **Access** | API/subscription | Download freely |
| **Internet** | Required | Optional |

---

## Slide 13: Three Ways to Use LLMs

### 1. Consumer Products

ChatGPT, Claude.ai, Gemini app

*Easy to use, subscription model*

### 2. Cloud APIs

OpenAI API, Anthropic API, Vertex AI

*For developers, pay per token*

### 3. Local Inference

Ollama, Hugging Face, LM Studio

*Free, private, hardware dependent*

---

## Slide 14: Setting Up OpenAI API

### Step 1: Get Your Key

1. Go to platform.openai.com
2. Create account → API Keys section
3. Create new secret key
4. Copy immediately (shown only once!)

### Step 2: Store Securely

Create `.env` file:
```
OPENAI_API_KEY=sk-your-key-here
```

**Never put API keys directly in code!**

---

## Slide 15: Loading Your API Key

```python
from dotenv import load_dotenv
import os

# Load the .env file
load_dotenv()

# Get the key
api_key = os.getenv("OPENAI_API_KEY")

# Verify
if api_key:
    print("Key loaded!")
```

### Why This Matters

- Security: Key not visible in code
- Sharing: Safe to share code without key
- Professional: Industry standard practice

---

## Slide 16: The Chat Completions API

### Basic Pattern

```python
from openai import OpenAI

client = OpenAI()

response = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[
        {"role": "user", "content": "Hello!"}
    ]
)

print(response.choices[0].message.content)
```

### Key Components

- **model**: Which GPT version to use
- **messages**: List of conversation messages
- **response**: Contains the AI's reply

---

## Slide 17: Understanding the Messages Format

### Structure

```python
messages = [
    {"role": "system", "content": "Instructions"},
    {"role": "user", "content": "Question"},
    {"role": "assistant", "content": "Response"}
]
```

### The Three Roles

| Role | Purpose | Example |
|------|---------|---------|
| **system** | Set AI behavior | "You are helpful" |
| **user** | Human input | "What is 2+2?" |
| **assistant** | AI response | "4" |

---

## Slide 18: System Prompt vs User Prompt

### System Prompt

- Sets the personality/behavior
- Defines the task and constraints
- Used once at conversation start
- Controls HOW the AI responds

### User Prompt

- The actual question/request
- What to respond TO
- Changes with each interaction
- Controls WHAT to respond about

---

## Slide 19: System Prompt Examples

### Helpful Assistant
```python
"You are a helpful assistant."
```
*Response: "2 + 2 equals 4."*

### Snarky Assistant
```python
"You are a snarky assistant with attitude."
```
*Response: "Oh wow, it's 4. Groundbreaking."*

### Teacher
```python
"You are a patient kindergarten teacher."
```
*Response: "Let's count together! 1, 2... and 1, 2 more... that's 4!"*

---

## Slide 20: The Power of System Prompts

### Small Changes → Big Differences

| Prompt | Output Style |
|--------|--------------|
| "Be brief" | One sentence answers |
| "Be thorough" | Detailed explanations |
| "Speak like a pirate" | "Arrr matey!" |
| "Respond in Spanish" | Full Spanish output |
| "Format as JSON" | Structured data |

### Key Insight

System prompts are your primary control mechanism.

---

## Slide 21: Building a Website Summarizer

### The Concept

URL → Fetch Content → Send to GPT → Summary

### Components Needed

1. **Web scraper** - Gets page content
2. **System prompt** - Defines summarization task
3. **User prompt** - Contains the content
4. **API call** - Sends to GPT
5. **Display** - Shows formatted result

---

## Slide 22: Summarizer System Prompt

```python
system_prompt = """You are an assistant that 
analyzes website content and provides concise 
summaries.

Guidelines:
- Focus on main content, ignore navigation
- Highlight key points
- Use bullet points
- Keep under 200 words
- Respond in markdown format"""
```

---

## Slide 23: Summarizer Code Flow

```
┌─────────────────┐
│  Input URL      │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Fetch Content   │  (Web scraping)
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Build Messages  │  (System + User prompts)
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Call OpenAI API │  (GPT processes)
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Display Summary │  (Formatted output)
└─────────────────┘
```

---

## Slide 24: Customizing Output with Prompts

### Different System Prompts = Different Results

**Professional Summary:**
"Provide an executive summary for business leaders."

**Snarky Review:**
"Give a humorous, snarky take on the website."

**Translation:**
"Summarize in French."

**Format Change:**
"List only the 5 most important points."

---

## Slide 25: Practical Business Applications

### Email Management
- Generate subject lines
- Summarize threads
- Draft responses

### Content Creation
- Social media posts
- Article summaries
- Tone adjustment

### Data Analysis
- Report summaries
- Key point extraction
- Trend identification

### Customer Service
- Ticket categorization
- Response templates
- Sentiment analysis

---

## Slide 26: Real-World Application Ideas

| Application | System Prompt Focus |
|------------|---------------------|
| Meeting notes processor | Extract action items |
| Code reviewer | Find bugs, suggest fixes |
| Legal document summarizer | Plain language summary |
| Product review analyzer | Sentiment + key themes |
| Research assistant | Academic summaries |

---

## Slide 27: Key Terminology

| Term | Meaning |
|------|---------|
| **LLM** | Large Language Model |
| **Token** | ~4 characters of text |
| **Prompt** | Input to the model |
| **Inference** | Running the model |
| **Parameters** | Model's learned values |
| **API** | Programming interface |
| **Fine-tuning** | Training on custom data |
| **RAG** | Retrieval + Generation |

---

## Slide 28: Model Selection Guide

### When to Use What

| Need | Use This |
|------|----------|
| Best quality | GPT-4o, Claude Sonnet |
| Budget conscious | GPT-4o-mini, Gemini Flash |
| Privacy critical | Ollama (local) |
| Coding tasks | GPT-4o, Claude |
| Quick experiments | Ollama with small model |
| Production apps | Cloud APIs |

---

## Slide 29: Summary

### Key Takeaways

1. **LLMs predict next tokens** - Simple concept, powerful results

2. **Multiple options exist** - Cloud APIs, local models, products

3. **System prompts control behavior** - Small changes, big impact

4. **APIs are straightforward** - Messages format is easy to learn

5. **Build practical tools** - Summarizers, assistants, analyzers

---

## Slide 30: Next Steps

### Practice Exercises

1. Try 3 different Ollama models
2. Create 5 different system prompts
3. Build your own tool (email, code, etc.)

### Resources

- OpenAI Docs: platform.openai.com/docs
- Ollama Library: ollama.com/library
- Anthropic Docs: docs.anthropic.com

### Remember

**The best way to learn is by building!**

---

## Slide 31: Questions?

### Contact & Resources

**Documentation:**
- OpenAI: platform.openai.com
- Ollama: ollama.com
- Anthropic: anthropic.com

**Practice:**
- Start simple
- Experiment constantly  
- Build real projects

---

**End of Presentation**

