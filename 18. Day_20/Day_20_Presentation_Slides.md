# Day 20: Multi-Model Orchestration, Structured Outputs & Deep Research Agent

## Slide 1: Welcome to Day 20
**Advanced OpenAI Agents SDK**

Today's Topics:
- Multi-model orchestration with OpenAI Agents SDK
- Structured outputs using Pydantic models
- Input and output guardrails for safety
- Building a production-ready Deep Research Agent
- Converting notebooks to Python modules with Gradio UI

---

## Slide 2: Recap from Day 19
**What We Learned Previously**

- Tools: Using function decorators to wrap Python functions as agent tools
- Agents as Tools: Converting agents into callable tools for other agents
- Handoffs: Transferring control between agents (vs. calling and returning)
- Async IO: Running multiple operations in parallel for efficiency

Key Distinction: Tools = call and return | Handoffs = transfer control

---

## Slide 3: Today's Journey
**Three Major Extensions**

1. **Multi-Model Orchestration**: Using models beyond OpenAI (Gemini, DeepSeek, Grok)
2. **Structured Outputs**: Requiring agents to return specific data structures
3. **Guardrails**: Protecting inputs and outputs with validation agents

Plus: Building a complete Deep Research Agent with proper Python modules!

---

## Slide 4: Multi-Model Orchestration
**Why Use Different Models?**

Different models have different strengths:
- Cost efficiency: Some models are cheaper for simple tasks
- Specialized capabilities: Some excel at specific domains
- Redundancy: Multiple perspectives on the same problem
- Avoiding vendor lock-in: Flexibility to switch providers

The OpenAI Agents SDK makes this seamless through OpenAI-compatible endpoints

---

## Slide 5: Setting Up Alternative Models
**OpenAI-Compatible Endpoints**

Many providers offer OpenAI-compatible APIs:
- **Google Gemini**, **DeepSeek**, **Grok (xAI)**, **Open Router**

Configuration needed:
1. Base URL: Provider's API endpoint
2. API Key: Your authentication credential
3. Model Name: Specific model to use

Process:
- Create AsyncOpenAI client with base URL and API key
- Pass model object to Agent (string = OpenAI, object = custom endpoint)

---

## Slide 6: Multi-Model Example & Best Practices
**Practical Implementation**

Example: Three sales agents with different models
- DeepSeek (cost-effective), Gemini (creative), Llama 3.3 (fast)
- Run in parallel using async/await
- Total time ≈ slowest agent (not sum of all)

**Cautionary Tale**: Autonomous agents can loop infinitely
- Lesson: Always add explicit constraints, timeouts, and iteration limits
- Monitor costs and execution time

---

## Slide 7: Structured Outputs Overview
**Beyond Text Responses**

Problem: LLMs naturally generate unstructured text
Solution: Structured outputs force responses into specific schemas

Benefits:
- Predictable data structures for downstream processing
- Type safety and validation
- Easier integration with databases and APIs
- Reduced parsing errors

---

## Slide 8: Pydantic Models & Structured Outputs
**Defining Data Schemas**

Pydantic: Python's data validation library using type hints
- Define classes inheriting from `BaseModel`
- Add type-annotated fields with docstrings (LLM sees these!)
- Automatic validation and serialization

Behind the scenes: Pydantic → JSON schema → LLM → JSON → Pydantic object

**Pro Tip**: Include a "reason" field before main output
- Forces model to articulate reasoning first
- Improves quality of subsequent fields (Chain of Thought effect)

---

## Slide 9: Guardrails for System Protection
**Input & Output Validation**

Guardrails are LLM-powered validation checkpoints:
- **Input Guardrails**: Validate incoming requests (before processing)
- **Output Guardrails**: Validate final responses (before showing users)

Why LLM-based? Semantic understanding beyond simple patterns
- Detect PII, inappropriate content, outdated information
- Validate tone, completeness, business logic

**Important**: Guardrails only at system boundaries (input/output), not mid-workflow

---

## Slide 10: Implementing Guardrails
**The Decorator Pattern**

Use `@input_guardrail` or `@output_guardrail` decorators

Guardrail function returns `GuardrailFunctionOutput`:
- `output_info`: Metadata for tracing
- `tripwire_triggered`: Boolean (True = validation failed)

When tripwire triggers:
- Exception raised immediately
- Workflow stops
- No further processing

Example: Name check agent detecting PII in messages

---

## Slide 11: Deep Research Agent Overview
**The Classic Agentic Use Case**

What it does:
- Takes research query → Plans searches → Executes in parallel
- Synthesizes findings → Emails comprehensive report

Why build your own?
- Customize for your domain/business
- Control costs and search depth
- Add proprietary data sources
- Universal applicability across industries

---

## Slide 12: OpenAI Web Search Tool
**Hosted Tool & Costs**

OpenAI provides hosted tools (they run on their infrastructure):
- **Web Search Tool** (what we'll use)
- File Search Tool, Computer Tool

**Important Pricing** (early 2025):
- GPT-4o-mini + low context: $0.025/search
- 3 searches ≈ $0.08, 20 searches ≈ $0.50
- Monitor usage! Sessions can cost $1-3

Use "low" search context size to minimize costs

---

## Slide 13: Deep Research Architecture
**Five Key Components**

1. **Planner Agent**: Query → Structured search plan (WebSearchPlan)
2. **Search Agent**: Executes searches using Web Search Tool (parallel with asyncio)
3. **Writer Agent**: Synthesizes results → Structured report (1000+ words)
4. **Email Agent**: Converts markdown → HTML → Sends via SendGrid
5. **Research Manager**: Orchestrates workflow with live progress updates

Each component: Single responsibility, structured I/O, configurable depth

---

## Slide 14: From Notebook to Production
**Building a Web Interface**

Transition: Notebooks → Python Modules → Gradio UI

Gradio benefits:
- No frontend knowledge needed
- Components: Blocks, Textbox, Button, Markdown
- Generator callbacks for live progress updates
- Deploy to Hugging Face Spaces (free hosting)

Your Deep Research Agent becomes a shareable portfolio piece!

---

## Slide 15: Enhancements & Applications
**Taking It Further**

Enhancement opportunities:
1. Clarifying questions before searching
2. Autonomous manager (decides when to search more)
3. Evaluator-optimizer pattern for quality control
4. Query refinement based on findings
5. Custom proprietary data sources

Real-world applications:
- Market research, competitive intelligence
- Technical documentation, legal research
- Medical literature review
- Universal architecture across domains

---

## Slide 16: Key Takeaways & Resources
**What We Learned Today**

✓ Multi-model orchestration with OpenAI-compatible endpoints
✓ Structured outputs using Pydantic for predictable responses
✓ Input/output guardrails for system protection
✓ Complete Deep Research Agent (production-ready!)
✓ Gradio UI and deployment strategies

**Resources**:
- OpenAI: https://platform.openai.com/docs/agents
- Gradio: https://gradio.app/
- Pydantic: https://docs.pydantic.dev/

---

## Slide 17: Challenge & Next Steps
**Take It to the Next Level**

Your challenge:
1. Add clarifying questions and autonomous decision-making
2. Implement evaluator-optimizer pattern
3. Deploy to Hugging Face Spaces and share!

**Coming Next**: Crew AI - Different agent framework with role-based design

You've built portfolio-worthy projects:
- Multi-agent SDR system
- Career conversation agent
- Deep Research Agent

Keep experimenting and building! 🚀

