# LLM Engineering - Day 12
## Slides Content (Continuation from Day 11)

---

## Slide 1: Title

# Deep Dive into LLM APIs & Building Applications

**Day 12 - From Understanding to Building**

Building on our foundations to create real applications

---

## Slide 2: What We'll Cover Today

### Building on Day 11
- Understanding the Chat Completions API
- Types of LLMs: Base, Chat, Reasoning
- The Transformer Architecture Story
- Tokens and Tokenization
- The Illusion of Memory
- Context Windows & API Costs
- Building a Multi-Step Application

---

## Slide 3: The Chat Completions API

### What is it?
- Standard way to call LLMs
- Invented by OpenAI, adopted by everyone
- Named because: Give chat history → Get completion

### The Core Idea
You give it a conversation, it predicts the most likely response.

---

## Slide 4: Behind the Scenes - Endpoints

### What Really Happens
```
Your Code → HTTP Request → OpenAI Server → Response
```

### The Endpoint
```
POST https://api.openai.com/v1/chat/completions
```

### What You Send
- Headers (API key)
- Payload (model, messages)

---

## Slide 5: The OpenAI Python Library

### What It Actually Is
- Just a lightweight wrapper around HTTP calls
- Makes API calls easier to write
- Not running any AI locally!

### Why Use It?
- Cleaner code
- No manual JSON handling
- Built-in error handling

---

## Slide 6: One Library, Multiple Providers

### The Secret
Everyone adopted OpenAI's format!

### How to Switch Providers
```python
# Change base_url to call different models
client = OpenAI(
    base_url="https://generativelanguage.googleapis.com/v1beta/openai/",
    api_key=google_api_key
)
```

Same code, different AI provider!

---

## Slide 7: Three Types of LLMs

| Type | What It Does | Example |
|------|-------------|---------|
| **Base Model** | Predicts next token only | Like predictive text |
| **Chat Model** | Trained for conversations | ChatGPT, Claude |
| **Reasoning Model** | Thinks before answering | GPT-5, o1 |

---

## Slide 8: Base Models

### Definition
- Just predicts what comes next
- No conversation training
- Like advanced autocomplete

### Example
Input: "The capital of France is"
Output: "Paris, which is known for..."

### When to Use
- Fine-tuning for custom tasks
- Building specialized models

---

## Slide 9: Chat Models (Instruct Models)

### Definition
- Trained to follow instructions
- Understands system/user/assistant format
- Optimized for conversation

### Training Method
- Reinforcement Learning from Human Feedback (RLHF)
- Human ratings improve responses

### Characteristics
- Fast responses
- Good for creative content
- Interactive conversations

---

## Slide 10: Reasoning Models

### Definition
- Thinks step-by-step before answering
- Shows (or hides) thinking process
- Better at complex problems

### The "Wait" Trick (Budget Forcing)
Simply adding "Wait" to the thinking trace makes it reconsider!

### When to Use
- Math problems
- Logic puzzles
- Complex reasoning tasks

---

## Slide 11: Hybrid Models

### The Best of Both Worlds
- Can reason when needed
- Fast chat when not needed
- Model decides how much thinking

### Examples
- GPT-5
- Gemini 2.5 Pro
- Claude 4.5 Sonnet

---

## Slide 12: The Transformer Story

### 2017: "Attention Is All You Need"
- Google researchers publish paper
- New neural network architecture
- Key innovation: Attention mechanism

### Why It Worked
- Processes sequences efficiently
- Can run in parallel (unlike LSTM)
- Scales with more data and compute

---

## Slide 13: The Rise of GPT

| Year | Model | Parameters |
|------|-------|------------|
| 2018 | GPT-1 | 117 million |
| 2019 | GPT-2 | 1.5 billion |
| 2020 | GPT-3 | 175 billion |
| 2023 | GPT-4 | 1.76 trillion |
| 2024+ | GPT-5 | Unknown (likely 10T+) |

---

## Slide 14: What Are Parameters?

### Simple Definition
The "knowledge" stored in the model

### Think of It Like
- More parameters = More "brain cells"
- More capacity to learn patterns
- Generally smarter but slower

### Key Insight
We've gotten better at efficiency - smaller models doing more!

---

## Slide 15: Training Time vs Inference Time Scaling

### Training Time Scaling
- Bigger model
- More parameters
- More training data
- Expensive upfront

### Inference Time Scaling
- Same model
- Better prompts (RAG)
- More reasoning (thinking tokens)
- Expensive per-use

Both approaches improve results!

---

## Slide 16: What Are Tokens?

### Definition
Chunks of text the model processes

### Not Characters, Not Words
- "Hello" = 1 token
- "Unbelievable" = 3 tokens (Un-believ-able)
- Numbers split into 3-digit chunks

### Rule of Thumb
~1 token ≈ 4 characters ≈ 0.75 words

---

## Slide 17: Tokenization Examples

| Text | Tokens |
|------|--------|
| "Hello" | 1 |
| "AI" | 1 |
| "Artificial Intelligence" | 2 |
| "3.14159" | 3 (3, .141, 59) |
| "Supercalifragilistic" | 5+ |

### Why It Matters
- Affects costs
- Affects context limits
- Affects how model "sees" text

---

## Slide 18: The Illusion of Memory

### Critical Understanding
**Every API call is stateless!**

### What This Means
- Model doesn't remember previous calls
- No persistent conversation memory
- Each request is independent

### How We Create "Memory"
Pass the entire conversation history with every call!

---

## Slide 19: Memory in Practice

### First Call
```python
messages = [
    {"role": "user", "content": "My name is John"}
]
# Response: "Nice to meet you, John!"
```

### Second Call (Without History)
```python
messages = [
    {"role": "user", "content": "What's my name?"}
]
# Response: "I don't know your name"
```

---

## Slide 20: Creating the Illusion

### The Trick
Include previous messages!

```python
messages = [
    {"role": "user", "content": "My name is John"},
    {"role": "assistant", "content": "Nice to meet you!"},
    {"role": "user", "content": "What's my name?"}
]
# Response: "Your name is John"
```

This is how ChatGPT "remembers"!

---

## Slide 21: Context Window

### Definition
Maximum tokens the model can process at once

### Includes
- System prompt
- All conversation history
- Your new message
- The generated response

### Model Comparison
| Model | Context Window |
|-------|---------------|
| GPT-5 | 400,000 tokens |
| Claude | 200,000 tokens |
| Gemini | 1,000,000 tokens |

---

## Slide 22: API Costs

### Pricing Structure
- Input tokens: Cheaper
- Output tokens: More expensive
- Reasoning tokens: Count as output

### Example: GPT-5
- Input: $1.25 per million tokens
- Output: $10 per million tokens

### GPT-5 Nano (Budget Option)
- Input: $0.05 per million tokens
- Output: $0.40 per million tokens

---

## Slide 23: Understanding Costs

### Perspective
- 1 million tokens ≈ Complete Works of Shakespeare
- Simple chat: Fractions of a cent
- Agents/loops: Can add up quickly

### Cost Traps
1. Long conversation histories
2. Reasoning tokens (hidden cost)
3. Agent loops running many calls

---

## Slide 24: One-Shot Prompting

### Definition
Provide one example of desired output

### Why It Works
Model learns the pattern from your example

### Example
```
"Respond in this JSON format:
{
  "type": "about page",
  "url": "https://example.com/about"
}"
```

---

## Slide 25: JSON Response Format

### Forcing JSON Output
```python
response = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=messages,
    response_format={"type": "json_object"}
)
```

### Why It Works
Constrains model to only output valid JSON at inference time

---

## Slide 26: Streaming Responses

### What Is Streaming?
Get tokens as they're generated (typewriter effect)

### How to Enable
```python
response = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=messages,
    stream=True
)
```

### Benefits
- Better user experience
- See progress immediately
- Can cancel early if needed

---

## Slide 27: Building Multi-Step Applications

### The Pattern
1. First LLM call: Analyze/Extract
2. Process results
3. Second LLM call: Generate/Transform

### Example: Brochure Generator
1. Get website links
2. LLM: Select relevant links (JSON)
3. Fetch linked pages
4. LLM: Generate brochure (Markdown)

---

## Slide 28: Practical Application Flow

```
URL Input
    ↓
Fetch Website Links
    ↓
LLM Call #1: Select Relevant Links (JSON)
    ↓
Fetch Each Relevant Page
    ↓
LLM Call #2: Generate Brochure (Markdown)
    ↓
Display/Stream Result
```

---

## Slide 29: Key Takeaways

1. **API Calls are HTTP requests** - The library just makes it easier

2. **Memory is an illusion** - Pass full history every time

3. **Tokens affect everything** - Costs, limits, understanding

4. **Multiple LLM calls** - Chain them for complex tasks

5. **Experiment constantly** - Iterate on prompts for better results

---

## Slide 30: Next Steps

### Practice Exercises
1. Build a multi-step application
2. Experiment with JSON outputs
3. Try streaming responses
4. Compare costs across models

### Coming Up
- Deeper into Agents
- Tool Calling
- Building Chat UIs
- Multimodal Applications

---

**End of Day 12 Slides**

