# Day 17: AI Agents Theory & Multi-Model Orchestration

## Slide 1: Introduction to AI Agents
**What Are AI Agents?**

**Definition (HuggingFace):**
> "AI agents are programs where LLM outputs control the workflow"

**What This Means:**
- The AI makes decisions about what happens next
- LLM outputs determine the sequence of tasks
- Dynamic process control rather than fixed scripts
- The system adapts based on AI decisions

**Key Point:** There's no single agreed definition - "agent" is used broadly in the industry, but this captures the core concept of AI-driven workflow control.

---

## Slide 2: Five Hallmarks of Agentic AI
**When Is Something Called "Agentic"?**

Any system with one or more of these characteristics:

**1. Multiple LLM Calls**
- Orchestrating several AI model calls in sequence
- Each call builds on previous results
- Example: Analyze document → Extract points → Generate summary

**2. Tool Use**
- LLMs can interact with external systems
- Call APIs, search databases, control devices
- Example: Agent searches web, reads results, decides if more info needed

**3. Coordination Environment**
- Multiple LLMs communicate with each other
- Share information and collaborate
- Example: Customer service system with specialized agents

---

## Slide 3: Five Hallmarks (Continued)
**More Characteristics of Agentic Systems**

**4. Planner LLM**
- An LLM coordinates and plans activities
- Decides what needs to happen and in what order
- Breaks down complex tasks into manageable pieces
- Example: Project manager agent assigning subtasks

**5. Autonomy**
- LLM makes decisions about the workflow
- Can "choose its own adventure" within boundaries
- Determines next steps based on observations
- Example: Trip planner that decides what information to gather

**Important:** Even one of these hallmarks qualifies a system as "agentic" - you don't need all five!

---

## Slide 4: Agentic Systems - Two Categories
**Anthropic's Framework for Understanding Agents**

**Category 1: Workflows**
- Models and tools follow predefined paths
- Sequence of steps is known ahead of time
- More predictable and controllable
- Like following a recipe - flexibility within structure
- Better for production systems needing reliability

**Category 2: Agents**
- Models dynamically direct their own processes
- No fixed path - decides steps as it goes
- Maintains control over task completion
- More flexible but less predictable
- Like cooking without a recipe - adapt as you go

**Key Insight:** Both are "agentic systems" - it's a spectrum, not binary. Workflows are constrained agents; agents are flexible workflows.

---

## Slide 5: Workflow Pattern 1 - Prompt Chaining
**Sequential Processing Through Multiple LLMs**

**Visual Flow:**
```
Input → LLM 1 → [Optional Code] → LLM 2 → [Optional Code] → LLM 3 → Output
```

**How It Works:**
- Output of one LLM becomes input to the next
- Each LLM handles one specific subtask
- Optional code can process/format between steps
- Like an assembly line - each station has one job

**Benefits:**
- **Focused Prompts:** Each LLM optimized for its specific task
- **Guardrails:** Fixed sequence keeps everything on track
- **Easy Debugging:** Know exactly which step failed
- **Optimization:** Use cheap models for simple tasks, powerful ones for complex

**Real Example:** Business analysis → Pick sector → Identify pain point → Propose AI solution

---

## Slide 6: Workflow Pattern 2 - Routing
**Intelligent Task Distribution to Specialists**

**Visual Flow:**
```
Input → Router LLM → Specialist LLM 1 (Task Type A)
                   → Specialist LLM 2 (Task Type B)
                   → Specialist LLM 3 (Task Type C) → Output
```

**How It Works:**
- Router LLM examines and classifies the input
- Decides which specialist should handle the task
- Routes to appropriate expert model
- Specialist processes and returns result

**Benefits:**
- **Separation of Concerns:** Each specialist excels in its domain
- **Efficiency:** Smaller, faster models for specific tasks
- **Cost Optimization:** Route simple tasks to cheap models, complex to expensive
- **Scalability:** Easy to add new specialists

**Real Example:** Customer service - Router directs technical questions to tech agent, billing to billing agent, general to support agent

---

## Slide 7: Workflow Pattern 3 - Parallelization
**Concurrent Processing for Speed and Multiple Perspectives**

**Visual Flow:**
```
Input → [Code Splits Task] → LLM 1 →
                            → LLM 2 → [Code Combines] → Output
                            → LLM 3 →
```

**How It Works:**
- Code (not LLM) breaks task into pieces
- Multiple LLMs process simultaneously
- Code aggregates results into final output
- All processing happens in parallel

**Benefits:**
- **Speed:** Much faster than sequential processing
- **Scalability:** Handle larger tasks by dividing them
- **Multiple Perspectives:** Different viewpoints on same problem
- **Redundancy:** Run same task multiple times, average for accuracy

**Real Examples:**
- Document analysis: Split chapters, summarize in parallel, combine
- Content moderation: Check hate speech, spam, misinformation simultaneously
- Idea evaluation: Business, technical, user perspectives in parallel

---

## Slide 8: Workflow Pattern 4 - Orchestrator-Worker
**LLM-Driven Dynamic Task Distribution**

**Visual Flow:**
```
Input → Orchestrator LLM → Worker LLM 1 →
                         → Worker LLM 2 → Orchestrator LLM → Output
                         → Worker LLM 3 →
```

**How It Works:**
- Orchestrator LLM (not code) breaks down the task
- Decides how many workers needed and what each does
- Workers execute their assigned subtasks
- Orchestrator synthesizes results intelligently

**Benefits:**
- **Dynamic Adaptation:** Strategy adapts to specific task
- **Intelligent Distribution:** Decides worker count and assignments
- **Smart Synthesis:** Understands context when combining results
- **Flexibility:** Handles varied, complex tasks hard to pre-code

**Key Difference from Parallelization:** An LLM makes the decisions, not fixed code - much more flexible and intelligent!

**Real Example:** Research project - Orchestrator divides into subtopics, assigns to workers, synthesizes comprehensive report

---

## Slide 9: Workflow Pattern 5 - Evaluator-Optimizer
**Quality Assurance Through Feedback Loops**

**Visual Flow:**
```
Input → Generator LLM → Evaluator LLM → Accept → Output
              ↑                        ↓ Reject + Feedback
              └─────────────────────────┘
```

**How It Works:**
- Generator LLM creates content/solution
- Evaluator LLM checks the work
- If approved: Output delivered
- If rejected: Feedback sent to generator, tries again
- Loop continues until accepted or max attempts reached

**Benefits:**
- **Quality Assurance:** Catches errors before reaching users
- **Improved Accuracy:** Feedback loop produces better results
- **Robustness:** More reliable for production systems
- **Specialization:** Generator focuses on creation, evaluator on validation

**Most commonly used pattern in production!**

**Real Example:** Code generation - Generator writes code, Evaluator checks syntax/security/best practices, provides feedback until code meets standards

---

## Slide 10: Agent Patterns - Open-Ended Systems
**Beyond Workflows: True Autonomous Agents**

**Key Characteristics:**
- **No Fixed Path:** Agent decides its own sequence of actions
- **Continuous Feedback Loops:** Information flows back and forth
- **Environment Interaction:** Agent acts on and observes external world
- **Open-Ended Process:** Can continue indefinitely
- **Agent Decides When Done:** No predetermined stopping point

**The Agent Loop:**
```
Human Request → LLM → Action → Environment Feedback → LLM → ...
                ↑                                          ↓
                └──────────────────────────────────────────┘
```

**What "Environment" Means:**
- Web browsing and interaction
- Database queries and updates
- File system operations
- API calls to external services
- IoT device control (lights, thermostats, etc.)

---

## Slide 11: Agent Patterns - Power and Challenges
**Why Agent Patterns Are Powerful**

**Flexibility:**
- Adapts strategy based on discoveries
- Tries different approaches if one fails
- No need to predefine all steps

**Complex Problem Solving:**
- Handles problems too complex to pre-script
- Figures out path dynamically
- Example: "Find best laptop deal" - agent searches, compares, checks reviews, finds coupons, evaluates shipping

**The Trade-offs:**
- **More Powerful:** Can solve much harder problems
- **Less Predictable:** Don't know exact path or time
- **Variable Quality:** Output not guaranteed
- **Cost Uncertainty:** Unknown number of API calls

**Critical Need:** Monitoring and guardrails to manage these challenges!

---

## Slide 12: Monitoring and Guardrails
**Essential Safety Mechanisms for Agent Systems**

**Monitoring - Visibility Into Agent Behavior:**
- What actions are being taken
- What decisions are being made
- How agents interact with each other
- Cost tracking (API calls)
- Time tracking (duration)
- Tools: OpenAI SDK tracing, LangSmith, custom logging

**Guardrails - Safety Boundaries:**

**Input Guardrails:**
- Validate user requests
- Block malicious inputs
- Ensure requests within scope

**Process Guardrails:**
- Limit iteration count (e.g., max 10 loops)
- Restrict tool access
- Require approvals for sensitive actions
- Set timeouts

**Output Guardrails:**
- Validate before returning to user
- Check for sensitive information
- Ensure quality standards met

---

## Slide 13: Working with Multiple LLMs
**Understanding the Model Landscape**

**Closed Source (Proprietary) Models:**
- **OpenAI:** GPT-4 (powerful, expensive), GPT-4-mini (fast, affordable), O1/O3-mini (reasoning)
- **Anthropic:** Claude 3.7 Sonnet (excellent all-around), Claude 3 Haiku (cheaper, faster)
- **Google:** Gemini 2.0 Flash (free tier!), Gemini Pro (more powerful)
- **DeepSeek:** V3 (671B params, very cheap), R1 (reasoning model)
  - Revolutionary: Near-GPT-4 performance at 1/30th training cost!
- **X.AI:** Grok (Elon Musk's model)

**Open Source Models (via Groq/Ollama):**
- **Llama:** 3.3 (70B - cloud only), 3.2 (3B - local friendly)
- **Qwen:** Alibaba's strong alternative
- **Gemma:** Google's open-source, optimized
- **Phi:** Microsoft's small but capable
- **DeepSeek Distilled:** Smaller versions for local use

---

## Slide 14: Model Selection Strategy
**Choosing the Right Model for Your Task**

**Consider These Factors:**

**1. Task Complexity:**
- Simple (classification, basic Q&A) → Small, cheap models
- Complex (reasoning, long-form) → Large, capable models

**2. Cost:**
- Development/testing → Free or cheap models
- Production → Balance cost vs. quality

**3. Speed:**
- Real-time applications → Fast models (even if less capable)
- Batch processing → Can use slower, more capable

**4. Privacy:**
- Sensitive data → Local models or private deployments
- Public data → Cloud models fine

**5. Context Length:**
- Short inputs/outputs → Any model
- Long documents → Need large context windows

**Resource:** Vellum Leaderboard (https://www.vellum.ai/llm-leaderboard) - Compare models, costs, performance benchmarks

---

## Slide 15: API Standardization - OpenAI Format
**One API Format for Multiple Providers**

**The Standard Format:**
```python
from openai import OpenAI

client = OpenAI(
    api_key="YOUR_KEY",
    base_url="PROVIDER_URL"  # Change this for different providers
)

response = client.chat.completions.create(
    model="model-name",
    messages=[{"role": "user", "content": "Your question"}]
)
answer = response.choices[0].message.content
```

**Who Uses OpenAI's Format:**
- Google Gemini (base_url ends with "/openai/")
- DeepSeek (compatible API)
- Groq (fast inference, OpenAI-compatible)
- Ollama (local, OpenAI-compatible)

**The Exception:** Anthropic has its own API (similar messages format, different structure)

**Why This Matters:** Learn one API, use everywhere! Dramatically simplifies multi-model development.

---

## Slide 16: Local Models with Ollama
**Running AI Models on Your Computer**

**What Is Ollama:**
- Runs open-source models locally
- Provides OpenAI-compatible API on localhost:11434
- Uses optimized C++ (llama.cpp) for efficiency
- Completely free, private, offline-capable

**Benefits:**
- **Free:** No API costs
- **Private:** Data never leaves your computer
- **Offline:** No internet needed
- **Learning:** Great for experimentation

**Limitations:**
- **Size:** Only small models (1-8B parameters)
- **Speed:** Slower than cloud (unless powerful computer)
- **Capability:** Smaller = less capable

**⚠️ Critical Warning:** Do NOT run Llama 3.3 (70B) locally! Requires ~60-100GB disk space and ~40GB RAM. Use Llama 3.2 (3B) instead.

**Setup:** Download from ollama.com, install, verify at localhost:11434

---

## Slide 17: Recommended Local Models
**Best Models for Running on Your Computer**

**For Good Balance (Recommended):**
- **Llama 3.2 (3B):** Good capability, runs well on most computers
- **Qwen 2.5 (3B):** Similar size, strong performance

**For Maximum Speed:**
- **Llama 3.2 (1B):** Very fast, limited capability
- **Phi-3 (mini):** Microsoft's efficient small model

**For Best Local Performance (if your computer can handle):**
- **Llama 3.2 (7B):** More capable, needs more resources
- **Gemma 2 (9B):** Google's model, good balance

**Specialized Models:**
- **DeepSeek-R1 distilled:** For reasoning tasks
- **CodeLlama:** Specialized for coding

**How to Use:**
```bash
ollama pull llama3.2          # Download model
ollama run llama3.2           # Run interactively
```

Browse all models: https://ollama.com/library

---

## Slide 18: Cost Considerations
**Understanding API Pricing and Free Options**

**Typical Costs for Learning:**
- Most exercises: Under $5 total
- Individual calls: Fractions of a cent
- Complex projects: Still very cheap

**Initial Deposits Required:**
- OpenAI: $5 minimum (US)
- Anthropic: $5 minimum
- DeepSeek: $2 minimum (lasts very long time)
- Groq: Pay-as-you-go, no minimum
- Gemini: Currently free tier available

**Completely Free Options:**
- **Ollama:** 100% free, runs locally
- **Gemini 2.0 Flash:** Free tier with usage limits
- **DeepSeek:** So cheap it's almost free

**The Reality:**
- Running these models requires massive compute
- Trillions of calculations per inference
- Huge electricity costs for providers
- Buying computer powerful enough would cost thousands
- **The value for a few dollars is incredible!**

---

## Slide 19: Lab Exercise - Model Competition
**Practical Multi-Model Orchestration**

**The Process:**

**Step 1: Question Generation**
- GPT-4-mini generates challenging question
- Question: "How would you analyze the ethical implications of using AI in predictive policing, considering bias, accountability, and societal impact?"

**Step 2: Multiple Models Answer (Parallel)**
- GPT-4-mini, Claude 3.7 Sonnet, Gemini 2.0 Flash
- DeepSeek V3, Llama 3.3 (70B via Groq), Llama 3.2 (3B via Ollama)
- All six provide their analysis

**Step 3: Evaluation**
- O3-mini (reasoning model) judges all responses
- Ranks them based on quality, comprehensiveness, structure

**Patterns Used:**
- **Prompt Chaining:** Question generation → Answering
- **Parallelization:** Six models answer simultaneously
- **Evaluator:** O3-mini judges and ranks
- **Hybrid approach** combining multiple patterns!

---

## Slide 20: Lab Results and Insights
**What We Learned from the Competition**

**Winner Rankings:**
1. **Gemini 2.0 Flash** (Winner!)
2. GPT-4-mini
3. Llama 3.3 (70B)
4. DeepSeek V3
5. Claude 3.7 Sonnet
6. Llama 3.2 (3B)

**Key Insights:**

**1. Model Differences Are Real:**
- Same question, very different responses
- Some comprehensive, others focused
- Different frameworks and approaches

**2. Size Isn't Everything:**
- Gemini 2.0 Flash beat larger models
- Architecture and training matter as much as size

**3. Small Models Struggle:**
- Llama 3.2 (3B) produced incomplete response
- Complex tasks need more capable models

**4. Evaluation Is Subjective:**
- Different evaluators might rank differently
- For objectivity: multiple evaluators, specific criteria, human validation

---

## Slide 21: Python Techniques from Lab
**Useful Coding Patterns Demonstrated**

**Zip Function - Iterate Two Lists Together:**
```python
for competitor, answer in zip(competitors, answers):
    print(f"{competitor}: {answer}")
```
Pairs up related data from two lists simultaneously

**Enumerate Function - Get Index and Item:**
```python
for index, answer in enumerate(answers):
    print(f"Response {index + 1}: {answer}")
```
Better than manually tracking a counter variable

**F-strings with Literal Braces:**
```python
text = f"JSON format: {{'key': 'value'}}"
```
Use `{{` and `}}` when you want actual braces in output

**Triple-Quoted Strings:**
```python
prompt = """
This is a multi-line string.
Perfect for long prompts.
No need for \n or concatenation.
"""
```
Clean way to handle long text blocks

---

## Slide 22: Development Environment
**Tools for Building AI Agents**

**Cursor IDE:**
- AI-powered code editor built on VSCode
- **Code Completion:** AI suggests code as you type
- **Code Explanation:** Ask it to explain any code
- **Code Generation:** Describe what you want, it writes it
- **Debugging Help:** Diagnose errors with AI assistance
- Dramatically speeds up development

**UV Package Manager:**
- Modern Python environment manager
- **Fast:** Written in Rust, incredibly quick
- **Simple:** Less confusing than Anaconda
- **Modern:** Current Python best practices
- **Integrated:** Many agent frameworks use UV

**Jupyter Notebooks:**
- Code in "cells" that run independently
- Perfect for learning, experimentation, documentation
- Run step-by-step, see results immediately
- Mix code, results, and explanations

---

## Slide 23: Commercial Applications
**Universal Applicability of Agent Patterns**

**Content Generation:**
- Prompt Chaining: Idea → Outline → Draft → Edit → Final
- Evaluator: Quality check before publishing
- Parallelization: Generate multiple versions, pick best

**Customer Service:**
- Routing: Direct queries to appropriate specialist
- Evaluator: Ensure responses appropriate and helpful
- Orchestrator-Worker: Break complex issues into subtasks

**Research and Analysis:**
- Parallelization: Analyze multiple sources simultaneously
- Orchestrator-Worker: Break research into subtopics
- Prompt Chaining: Gather → Analyze → Synthesize → Report

**Software Development:**
- Prompt Chaining: Requirements → Design → Code → Test
- Evaluator: Code review and validation
- Orchestrator-Worker: Divide project into components

**Business Intelligence:**
- Routing: Different analyses for different data types
- Parallelization: Analyze multiple datasets simultaneously
- Evaluator: Validate insights before presenting

---

## Slide 24: Improving Robustness and Accuracy
**Why Workflow Patterns Matter for Production**

**The Core Problem with Single LLM Calls:**
- Can make mistakes
- Miss important details
- Produce inconsistent results
- Hallucinate facts

**The Solution - Layered Quality Control:**

**1. Multiple Attempts:**
- Send same request to multiple models
- Compare results, vote, or average

**2. Evaluation Loops:**
- Have one model check another's work
- Iterate until quality standards met

**3. Decomposition:**
- Break complex tasks into simpler subtasks
- Each subtask easier to get right

**4. Specialization:**
- Use different models for different parts
- Each model optimized for its role

**Real Example - Legal Document Analysis:**
Routing → Parallelization → Orchestrator → Evaluator → Human Review
Each layer adds reliability!

---

## Slide 25: Best Practices
**Tips for Success with AI Agents**

**Learning Approach:**
1. **Watch First, Then Do:** Understand concepts, then implement yourself
2. **Experiment Freely:** Add print statements, try different models, break things
3. **Use AI to Learn:** Ask ChatGPT/Claude to explain concepts
4. **Debug Thoughtfully:** Read errors carefully, use troubleshooting guides

**Development Tips:**
- **Iterate and Experiment:** Test different models and prompts
- **Debugging Is Learning:** Embrace challenges as learning opportunities
- **Share Your Work:** GitHub repos, LinkedIn posts, community contributions

**Common Issues:**
- **Name Errors:** Forgot to run a cell - run cells in order
- **Import Errors:** Wrong environment - check kernel selection
- **API Errors:** Check API key, credit/quota, model name
- **Unexpected Results:** Print intermediate values, check prompts, try different models

---

## Slide 26: Key Takeaways
**Core Concepts to Remember**

**1. Agents vs Workflows:**
- Workflows: Predefined paths, more predictable
- Agents: Dynamic self-direction, more flexible
- Both are valuable, it's a spectrum

**2. Five Workflow Patterns:**
- **Prompt Chaining:** Sequential LLM calls
- **Routing:** Intelligent task distribution
- **Parallelization:** Concurrent processing
- **Orchestrator-Worker:** LLM-driven coordination
- **Evaluator-Optimizer:** Quality feedback loops

**3. Autonomy:**
- LLMs making decisions about workflow
- "Choosing their own adventure" within boundaries
- Core characteristic of agentic systems

**4. Multi-Model Orchestration:**
- Leverage different model strengths
- Combine models for better results
- OpenAI-compatible APIs simplify integration

**5. Monitoring & Guardrails:**
- Essential for production systems
- Visibility and safety boundaries
- Manage unpredictability and costs

---

## Slide 27: Resources and Links
**Essential Bookmarks for Your Journey**

**Key Resources:**
- **Vellum Leaderboard:** https://www.vellum.ai/llm-leaderboard
  - Compare models, costs, performance benchmarks
  
- **Ollama:** https://ollama.com
  - Run models locally for free
  
- **Anthropic Blog:** "Building Effective Agents"
  - Source of workflow patterns, excellent reading

**Learning Materials:**
- Setup guides and troubleshooting documentation
- Python tutorials (beginner & intermediate)
- Async programming guides
- Debugging techniques

**Community:**
- GitHub repositories for code sharing
- LinkedIn networking and project sharing
- Pull requests welcome for contributions

**Further Reading:**
- LLM fundamentals (transformers, attention, training)
- Agent framework documentation
- Production considerations (monitoring, cost optimization, security)

---

## Slide 28: Practical Exercises
**Apply What You've Learned**

**1. Add a Pattern to the Lab:**
- Take the model competition exercise
- Add another workflow pattern
- Try: Multiple evaluators voting, routing by question type, chained analysis

**2. Experiment with Models:**
- Same task, different models
- Compare: Quality, speed, cost, consistency
- Document your findings

**3. Build Something New:**
- Apply patterns to a problem you care about
- Automate a work task, create personal assistant, build research tool

**4. Improve Reliability:**
- Take a single LLM call
- Add: Evaluator to check output, multiple attempts with voting, decomposition into subtasks
- Measure improvement in quality

**5. Explore Local Models:**
- Set up Ollama
- Try different model sizes
- Compare local vs cloud performance

---

## Slide 29: Looking Forward
**What's Next in Your AI Agents Journey**

**Key Concepts Mastered Today:**
- What makes something "agentic"
- Difference between workflows and agents
- Five powerful workflow patterns
- Working with multiple LLMs
- Development environment and tools

**This Foundation Enables:**
- Building production-ready agent systems
- Designing robust, reliable AI applications
- Choosing appropriate patterns for problems
- Orchestrating multiple models effectively
- Managing costs and quality trade-offs

**Remember:**
- Theory guides practice
- Patterns solve problems
- Experimentation leads to learning
- Community makes you stronger

**The journey to mastering AI agents has begun!**

---

## Slide 30: Q&A and Discussion
**Questions to Consider**

**Reflection Questions:**
- Which workflow pattern best fits your current use case?
- What commercial applications can you envision?
- Which models will you experiment with first?
- What challenges do you anticipate?
- How can you apply these patterns to your work?

**Discussion Topics:**
- Trade-offs between workflows and agents
- When to use which pattern
- Cost vs. quality considerations
- Local vs. cloud model decisions
- Production deployment challenges

**Next Steps:**
- Experiment with the lab exercise
- Try different models and patterns
- Share your learnings with the community
- Start building your first agent!

**Keep building, keep learning, keep experimenting!**
