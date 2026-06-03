# Day 13: Building UIs, Tools & Multimodal AI
## Slide Content Outline

---

## Slide 1: Today's Objectives

**Building on Day 12, we will cover:**
- LLM Frameworks (LangChain, LiteLLM)
- Prompt Caching for cost optimization
- Building UIs with Gradio
- AI Assistants with smart prompting
- Tool Calling (Function Calling)
- Multimodal AI (Images & Audio)

---

## Slide 2: LLM Frameworks Overview

**Why use frameworks?**
- Simplify switching between different models
- Consistent interface across providers
- Cost tracking and monitoring

**Two main options:**
- **LangChain**: Heavyweight, powerful abstractions
- **LiteLLM**: Lightweight, simple model switching

---

## Slide 3: LiteLLM - Simple Model Switching

**Key benefits:**
- One interface for all providers
- Built-in cost tracking
- Easy model comparison

**Basic syntax:**
```python
from litellm import completion

response = completion(
    model="openai/gpt-4o-mini",
    messages=[{"role": "user", "content": "Hello"}]
)
```

---

## Slide 4: Cost Tracking with LiteLLM

**Track every API call:**
- Input tokens
- Output tokens
- Cost per call

**Use case:** Monitor spending per user/request in production

---

## Slide 5: Prompt Caching

**What is it?**
- Reuse processed prompts across calls
- Pay less for repeated context

**Provider differences:**
| Provider | Type | Benefit |
|----------|------|---------|
| OpenAI | Automatic | ~5x savings |
| Anthropic | Explicit | 10x savings (25% prime cost) |
| Gemini | Both modes | Varies |

---

## Slide 6: Prompt Caching Best Practice

**Important rule:** Static content FIRST, variable content LAST

**Wrong:** `[Date] + [Large Context] + [Question]`
**Right:** `[Large Context] + [Question] + [Date]`

Caching matches from the beginning of the prompt!

---

## Slide 7: LLM Conversations

**Two LLMs can talk to each other!**
- Assign different personas via system prompts
- Build conversation history programmatically
- Educational and entertaining use cases

**Example personas:**
- One argumentative, one polite
- One optimist, one pessimist

---

## Slide 8: Three-Way Conversations

**Challenge:** API only has user/assistant roles

**Solution:** Single user prompt with full context
```
System: "You are Alex in a conversation with Blake and Charlie..."
User: "The conversation so far is: [full history]. 
       Respond as Alex."
```

Useful for complex multi-agent scenarios!

---

## Slide 9: Introduction to Gradio

**What is Gradio?**
- Python library for building UIs
- Created by Hugging Face
- Perfect for data science demos

**Key benefit:** Build interfaces without frontend knowledge

---

## Slide 10: Gradio Basics

**Simple example:**
```python
import gradio as gr

def greet(name):
    return f"Hello, {name}!"

gr.Interface(fn=greet, inputs="text", outputs="text").launch()
```

That's it! A complete web UI in 4 lines.

---

## Slide 11: Gradio Interface Types

| Type | Use Case |
|------|----------|
| `gr.Interface` | Simple input → output |
| `gr.ChatInterface` | Chat/messaging style |
| `gr.Blocks` | Custom layouts |

---

## Slide 12: Gradio Chat Interface

**Perfect for AI chatbots:**
```python
def chat(message, history):
    # Call your LLM here
    return response

gr.ChatInterface(fn=chat, type="messages").launch()
```

Gradio handles all the UI complexity!

---

## Slide 13: Gradio Features

- **Streaming:** Show responses as they arrive
- **Markdown:** Render formatted text
- **Sharing:** `share=True` for public URL
- **Authentication:** `auth=("user", "pass")`
- **Dark/Light mode:** Respects user preference

---

## Slide 14: How Gradio Works

**Three steps under the hood:**
1. Generates Svelte frontend from Python description
2. Runs Starlette web server
3. Creates API routes for your callbacks

Result: Full web app from just Python!

---

## Slide 15: Building AI Assistants

**Key ingredients:**
- On-brand persona (system prompt)
- Memory illusion (conversation history)
- Domain expertise (context injection)

---

## Slide 16: Prompting Techniques

**One-shot prompting:**
> "If user asks X, respond like Y"

**Multi-shot prompting:**
> "Example 1: Q→A, Example 2: Q→A..."

More examples = more consistent behavior

---

## Slide 17: Dynamic Context Injection

**Add relevant info based on user input:**
```python
if "belt" in message.lower():
    system_message += "\nWe don't sell belts."
```

**This is the foundation of RAG!**
- Select relevant context dynamically
- Insert into system prompt
- Better answers without hallucination

---

## Slide 18: Introduction to Tools

**What are tools?**
- Give LLMs ability to call external functions
- Database lookups, calculations, bookings
- Foundation of Agentic AI

---

## Slide 19: How Tool Calling Really Works

**No magic! Just structured messaging:**

1. Tell LLM about available tools (JSON schema)
2. LLM responds: "Please run tool X"
3. YOU run the tool in your code
4. Call LLM again with tool results

LLMs only generate tokens - tools are your code!

---

## Slide 20: Tool Definition (JSON Schema)

```python
tool = {
    "type": "function",
    "function": {
        "name": "get_ticket_price",
        "description": "Get price for destination",
        "parameters": {
            "type": "object",
            "properties": {
                "city": {"type": "string"}
            },
            "required": ["city"]
        }
    }
}
```

---

## Slide 21: Using Tools in API Calls

```python
response = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=messages,
    tools=tools  # Pass tool definitions
)

# Check if tool call requested
if response.choices[0].finish_reason == "tool_calls":
    # Handle the tool call
```

---

## Slide 22: Handling Tool Calls

**Basic pattern:**
```python
while response.choices[0].finish_reason == "tool_calls":
    # Get tool request
    tool_call = response.choices[0].message.tool_calls[0]
    
    # Run your function
    result = my_function(tool_call.arguments)
    
    # Send result back to LLM
    messages.append({"role": "tool", "content": result})
    response = client.chat.completions.create(...)
```

---

## Slide 23: Common Tool Use Cases

- **Lookups:** Database queries, API calls
- **Actions:** Book tickets, send emails
- **Calculations:** Math operations
- **Code execution:** Run Python safely
- **UI updates:** Generate charts

---

## Slide 24: Agentic AI Concepts

**Definition 1:** LLM controls the workflow
**Definition 2:** LLM runs tools in a loop to achieve goals

**Key characteristics:**
- Memory persistence
- Planning capability
- Autonomous decisions
- Tool orchestration

---

## Slide 25: Multimodal AI

**Beyond text - multiple modalities:**
- Image generation
- Text-to-speech
- (Coming later: vision, transcription)

---

## Slide 26: Image Generation with DALL-E

```python
response = client.images.generate(
    model="dall-e-3",
    prompt="A vacation scene in Paris",
    size="1024x1024"
)
image_url = response.data[0].url
```

Cost: ~$0.04 per image

---

## Slide 27: Text-to-Speech

```python
response = client.audio.speech.create(
    model="gpt-4o-mini-tts",
    voice="onyx",  # or: alloy, echo, fable, nova, shimmer
    input="Hello, welcome to our service!"
)
audio_content = response.content
```

---

## Slide 28: Building Multimodal Assistants

**Combine everything:**
1. Chat interface (Gradio)
2. Tool calling (database lookup)
3. Image generation (destination photos)
4. Audio output (spoken responses)

Result: Rich, interactive AI assistant!

---

## Slide 29: Gradio Blocks for Custom UI

```python
with gr.Blocks() as ui:
    with gr.Row():
        chatbot = gr.Chatbot()
        image = gr.Image()
    with gr.Row():
        audio = gr.Audio()
    with gr.Row():
        textbox = gr.Textbox()
    
    textbox.submit(fn=chat, inputs=[...], outputs=[...])

ui.launch()
```

---

## Slide 30: Summary

**Today we covered:**
- LLM frameworks for easier model management
- Prompt caching for cost optimization
- Gradio for rapid UI development
- AI assistants with smart prompting
- Tool calling for extended capabilities
- Multimodal outputs (images, audio)

---

## Slide 31: Key Takeaways

1. **Frameworks** simplify multi-model workflows
2. **Gradio** = UIs in minutes, not hours
3. **Tools** extend LLM capabilities (no magic!)
4. **Prompting** techniques improve consistency
5. **Multimodal** AI creates rich experiences

---

## Slide 32: Next Steps

**Practice exercises:**
- Build a Gradio chatbot with your own persona
- Add a custom tool to your assistant
- Experiment with image generation prompts
- Create a multimodal demo for your use case

**Coming up:** Open-source models, Hugging Face, GPUs

