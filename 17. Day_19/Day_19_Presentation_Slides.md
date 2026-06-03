# Day 19: Asynchronous Python & OpenAI Agents SDK

## Presentation Slides Content

---

### Slide 1: Welcome to Day 19
**Title:** Day 19 - OpenAI Agents SDK

**Content:**
- Transitioning from basic agent concepts to framework implementation
- Focus: OpenAI Agents SDK (formerly Swarm)
- Prerequisites: Understanding async/await patterns in Python
- Building practical multi-agent systems

**Key Point:** The newest and most lightweight agent framework from OpenAI

---

### Slide 2: Why Async Python Matters
**Title:** The Foundation - Asynchronous Python

**Content:**
- All major agent frameworks use async IO
- Critical for understanding agent orchestration
- Not optional - fundamental to modern agent development
- Takes 30 minutes to master, saves hours of confusion

**Analogy:** Like learning to drive before road tripping

---

### Slide 3: What is Async IO?
**Title:** Async IO - Lightweight Concurrency

**Content:**
- Alternative to multithreading and multiprocessing
- Introduced in Python 3.5
- Doesn't use OS-level threads or multiple processes
- Allows thousands of concurrent operations with minimal resources
- Perfect for I/O-bound operations (API calls, network requests)

**Key Benefit:** Ideal for LLM API calls where most time is waiting

---

### Slide 4: The Short Version
**Title:** Async IO in Two Keywords

**Content:**
Two special keywords make async Python work:

**`async`** - Declares a function as asynchronous (coroutine)
- Use: Place before function definition
- Example syntax: `async def process_data()`

**`await`** - Schedules and waits for coroutine execution
- Use: Place before calling async function
- Example syntax: `result = await process_data()`

**Rule:** async functions must be called with await

---

### Slide 5: Functions vs Coroutines
**Title:** Understanding Coroutines

**Content:**
When you use `async def`, you're not creating a function:

**Regular Function:**
- Executes immediately when called
- Runs until completion or return

**Coroutine:**
- Returns a coroutine object when called
- Doesn't execute until awaited or scheduled
- Can be paused and resumed
- Managed by event loop

**Critical Distinction:** Calling a coroutine doesn't run it!

---

### Slide 6: The Event Loop
**Title:** How Async Python Actually Works

**Content:**
The event loop is the heart of async IO:

**What it does:**
- Maintains a queue of coroutines to execute
- Executes one coroutine at a time (not true multithreading)
- When a coroutine waits for I/O, switches to another
- Returns to the first when I/O completes

**Why it's lightweight:**
- No OS threads - all managed in Python
- Manual approach to concurrency
- Minimal resource consumption
- Can handle 10,000+ concurrent operations

---

### Slide 7: Running Multiple Coroutines
**Title:** Concurrent Execution with asyncio.gather

**Content:**
To run multiple coroutines concurrently, use `asyncio.gather()`:

**Syntax pattern:**
- `results = await asyncio.gather(coroutine1(), coroutine2(), coroutine3())`

**What happens:**
- All three coroutines are scheduled
- Event loop runs them concurrently
- When one blocks on I/O, others continue
- Returns list of all results

**Use case:** Making multiple LLM calls in parallel

---

### Slide 8: Why Agents Use Async
**Title:** Async IO and Agent Frameworks

**Content:**
Every major agent framework uses async IO because:

**Multiple API Calls:**
- Agents often call LLMs repeatedly
- Each call involves network waiting
- Async allows other work during waits

**Multi-Agent Systems:**
- Different agents can work simultaneously
- Coordinate without blocking each other
- Efficient resource utilization

**Scalability:**
- Handle hundreds of agent interactions
- Minimal memory overhead
- Fast context switching

---

### Slide 9: Introducing OpenAI Agents SDK
**Title:** OpenAI Agents SDK Overview

**Content:**
**Philosophy:**
- Lightweight and flexible
- Not opinionated - gives you control
- Handles boilerplate (JSON, tool definitions)
- Makes common tasks simple

**Key Features:**
- Simplified tool definition
- Agent coordination
- Built-in tracing and monitoring
- Works with multiple LLM providers

**Best For:** Developers who want flexibility with convenience

---

### Slide 10: Core Terminology - Agent
**Title:** Understanding Agents

**Content:**
**Agent Definition:**
- A package around LLM calls
- Has a specific role and purpose
- Defined by name and instructions (system prompt)
- Can have tools and capabilities

**Think of it as:**
- A specialized AI assistant
- Focused on one particular task
- Part of a larger system
- Can collaborate with other agents

**Example:** Sales agent, email formatter, joke teller

---

### Slide 11: Core Terminology - Handoffs
**Title:** Agent Handoffs

**Content:**
**Handoff Definition:**
- Interaction between agents
- Delegation of control from one agent to another
- Control doesn't return to original agent

**How it works:**
- Agent completes its task
- Passes control to specialized agent
- Next agent continues workflow

**Use case:** Sales manager hands off to email formatter

**Key Difference:** Unlike tools, handoffs transfer ownership

---

### Slide 12: Core Terminology - Guardrails
**Title:** Agent Guardrails

**Content:**
**Guardrails Definition:**
- Checks and controls on agent behavior
- Ensure agents stay on task
- Prevent agents from "going off the rails"

**Types:**
- Input guardrails: Validate incoming data
- Output guardrails: Check generated responses
- Behavioral constraints: Limit agent actions

**Purpose:** Reliable, predictable agent behavior

---

### Slide 13: Three Steps to Run an Agent
**Title:** Basic Agent Execution

**Content:**
**Step 1: Create an Agent**
- Instantiate the Agent class
- Provide name, instructions, model

**Step 2: Use Trace (optional but recommended)**
- Wrap execution in trace context
- Logs all interactions
- Available in OpenAI monitoring tools

**Step 3: Call runner.run**
- Actually executes the agent
- It's an async function (coroutine)
- Must use await to run it

**Result:** Agent processes input and returns output

---

### Slide 14: The Agents Package
**Title:** Key Imports from OpenAI

**Content:**
Import from the `agents` package:

**Agent** - Main class for creating agents

**Runner** - Executes agents

**Trace** - Monitors and logs agent interactions

**Note on naming:** The package name "agents" is generic, may conflict with your own code

**Installation:** Package name is `openai` (includes agents module)

---

### Slide 15: Creating Your First Agent
**Title:** Agent Creation Pattern

**Content:**
To create an agent, provide:

**Name:** Identifier for the agent
- Example: "jokester", "sales_agent_1"

**Instructions:** System prompt defining agent's role
- Example: "You are a joke teller"
- Sets tone, context, and behavior

**Model:** Which LLM to use
- Example: "gpt-4o-mini"
- Defaults to OpenAI but supports others

**Optional:** Tools, handoffs, guardrails

---

### Slide 16: Runner.run Method
**Title:** Executing Agents

**Content:**
**Basic usage pattern:**
- Call `runner.run()` with agent and message

**Parameters:**
- Agent: The agent instance to run
- Message: User prompt/input to the agent

**Important:** It's a coroutine!
- Returns coroutine object if not awaited
- Must use `await runner.run()` to execute

**Result access:**
- Response object with `final_output` attribute
- Contains agent's response

---

### Slide 17: Trace Context Manager
**Title:** Monitoring with Traces

**Content:**
**Using traces:**
- Wrap agent execution in `with trace("name"):` block
- Records all agent interactions
- Groups related calls under one heading

**Benefits:**
- See entire workflow in one view
- Debug complex multi-agent systems
- Track timing and token usage
- Available at platform.openai.com/traces

**Best Practice:** Always use traces for complex workflows

---

### Slide 18: Streaming Responses
**Title:** Real-Time Agent Output

**Content:**
**Alternative to runner.run:**
- Use `runner.run_streamed()` for streaming

**Implementation:**
- Returns coroutine without await
- Use `async for` to iterate through chunks
- Print chunks as they arrive

**Why stream:**
- Better user experience
- See progress in real-time
- Start processing partial results
- Feels more responsive

---

### Slide 19: Async IO Gather for Parallel Agents
**Title:** Running Multiple Agents Concurrently

**Content:**
**Pattern for parallel execution:**
- Use `asyncio.gather()` with multiple `runner.run()` calls
- All agents execute concurrently
- Collect results as a list

**Example scenario:**
- Three sales agents write different email styles
- All run simultaneously
- Gather results to compare

**Efficiency gain:** 3x faster than sequential execution

---

### Slide 20: Vibe Coding Best Practices
**Title:** Coding with LLM Assistance

**Content:**
**Vibe Coding:** Term coined by Andrej Karpathy for LLM-assisted development

**Five Key Tips:**

1. **Good Vibes:** Craft reusable prompts, ask for concise code, mention current date
2. **Vibe but Verify:** Ask multiple LLMs the same question
3. **Step Up the Vibe:** Generate code in small 10-line chunks, not 200 lines at once
4. **Vibe and Validate:** Have second LLM review first's answer
5. **Vibe with Variety:** Ask for 3 different approaches

---

### Slide 21: Vibe Coding - Key Principles
**Title:** Avoiding LLM-Assisted Pitfalls

**Content:**
**Don't generate 200 lines at once**
- Break problems into small steps
- Ask LLM to create the step-by-step plan first
- Generate and test each piece independently
- Build verified working solution incrementally

**Always understand the code**
- Ask LLM to explain anything unclear
- Don't blindly copy-paste
- When bugs happen, you'll be stuck without understanding

**Verify with multiple sources**
- Use ChatGPT AND Claude
- Cross-reference answers
- One often spots issues the other missed

---

### Slide 22: Building an Agent Workflow
**Title:** Sales Development Rep Example

**Content:**
**Project: Automated SDR**

Three layers of architecture:

**Layer 1: Workflow**
- Simple agent calls orchestrated in Python
- Manual control flow

**Layer 2: Tools**
- Agents that can use tools (functions)
- Wrapping functions for agent use

**Layer 3: Collaboration**
- Agents calling other agents
- Tools vs. handoffs patterns

---

### Slide 23: Agent Workflows - Layer 1
**Title:** Manual Agent Orchestration

**Content:**
**Approach:**
- Create multiple agents with different instructions
- Call them manually in Python code
- Use asyncio.gather for parallel execution
- Combine results with another agent

**Example:**
- Three sales agents (professional, humorous, concise)
- Each writes a cold email
- Fourth agent picks the best one

**Pattern Recognition:** This is a variation of evaluator-optimizer pattern

---

### Slide 24: Tools in OpenAI Agents SDK
**Title:** Simplified Tool Definition

**Content:**
**Remember the old way:**
- Manual JSON schemas for each tool
- Complex parameter definitions
- Custom handler functions with if statements

**The new way:**
- Decorate function with `@function_tool`
- SDK generates JSON automatically
- Reads function signature and docstring
- Creates parameter schema

**Result:** Write normal Python functions, get tools automatically

---

### Slide 25: Function Tool Decorator
**Title:** Creating Tools from Functions

**Content:**
**How it works:**
- Add `@function_tool` above function definition
- Function becomes a FunctionTool object
- Docstring becomes tool description
- Type hints become parameter schema

**What SDK generates:**
- Tool name
- Tool description
- JSON schema for parameters
- Execution wrapper

**No more manual JSON!** Just write clean Python with good docstrings

---

### Slide 26: Agents as Tools
**Title:** Wrapping Agents for Reuse

**Content:**
**Concept:**
- Any agent can become a tool
- Call `.as_tool()` on an agent instance

**What this creates:**
- FunctionTool wrapper around the agent
- When "tool" is called, it runs the agent
- Returns agent's output as tool result

**Use case:**
- Sales agent becomes a tool
- Manager agent uses three sales agents as tools
- Lets manager choose which to call and when

---

### Slide 27: Tools vs Handoffs
**Title:** Two Ways for Agents to Collaborate

**Content:**
**Tools (Request-Response):**
- Agent calls tool and gets result back
- Control returns to calling agent
- Agent continues with tool result
- Like using a feature or capability

**Handoffs (Delegation):**
- Agent passes control to another agent
- Control does NOT return
- Second agent completes the workflow
- Like delegating a task

**When to use:** Tools for capabilities, handoffs for delegation

---

### Slide 28: SendGrid for Email Automation
**Title:** External Tool Integration

**Content:**
**SendGrid Setup:**
- Free transactional email service (owned by Twilio)
- Create API key in settings
- Verify sender email address
- Add key to .env file

**Why useful:**
- Agents can actually send emails
- Real-world tool integration example
- Demonstrates external API calls from agents

**Alternatives:** Sparkpost or any email API

**URL:** sendgrid.com

---

### Slide 29: Building Sales Manager Agent
**Title:** Multi-Agent System with Tools

**Content:**
**Architecture:**
- Three sales agents wrapped as tools
- Send_email function as tool
- Sales manager coordinates everything

**Manager instructions:**
- Use all three tools to generate emails
- Never generate emails yourself
- Pick the best one
- Send the best email only

**Result:** Agent makes decisions autonomously about which tools to use and when

---

### Slide 30: Complete SDR with Handoffs
**Title:** Advanced Multi-Agent Architecture

**Content:**
**Final architecture:**

**Sales Manager:**
- Has 3 sales agents as tools
- Has email manager as handoff

**Email Manager (handoff target):**
- Has subject writer tool (agent wrapped)
- Has HTML converter tool (agent wrapped)
- Has send_html_email tool (function)

**Flow:** Manager generates emails → picks best → hands off to email manager → formats and sends

**View in traces:** See entire workflow visualized

---

### Slide 31: Commercial Applications
**Title:** Real-World Use Cases

**Content:**
**Sales Automation:**
- Cold email generation and follow-ups
- Email-based conversation agents
- Lead qualification and nurturing

**Process Automation:**
- Recruitment workflows
- Customer support routing
- Document processing pipelines

**Key Insight:** This pattern applies to ANY business process requiring:
- Multiple specialized tasks
- Decision-making between options
- Tool integration
- Autonomous operation

---

### Slide 32: Exercises and Next Steps
**Title:** Practice and Extend

**Content:**
**Exercises:**

1. **Identify Patterns:** Find agentic design patterns in the examples
2. **Add More Tools:** Extend with new agents and capabilities
3. **Hard Challenge:** Make agent respond to email replies (webhooks)

**Key Learning:** Hands-on practice is essential

**Next Session:** More advanced OpenAI Agents SDK features, multi-agent coordination patterns

---

### Slide 33: Key Takeaways
**Title:** Day 19 Summary

**Content:**
**Async Python:**
- Foundation for all agent frameworks
- `async def` creates coroutines
- `await` executes them
- `asyncio.gather()` for parallel execution

**OpenAI Agents SDK:**
- Lightweight and flexible
- Simplified tool definitions
- Agent, runner, trace pattern
- Tools vs handoffs

**Practical Skills:**
- Multi-agent orchestration
- Tool integration
- Workflow design

**Remember:** Vibe code responsibly!

---

### Slide 34: Resources
**Title:** Learn More

**Content:**
**Documentation:**
- OpenAI Agents SDK Docs: platform.openai.com/docs/guides/agents
- Python asyncio: docs.python.org/3/library/asyncio.html

**Tools:**
- SendGrid: sendgrid.com
- OpenAI Platform Traces: platform.openai.com/traces

**Practice:**
- Build your own multi-agent system
- Experiment with different tool combinations
- Try various handoff patterns

**Community:**
- Share your agent projects
- Learn from others' implementations

---

### Slide 35: Thank You
**Title:** Questions?

**Content:**
- Continue to Day 20 for more advanced agent patterns
- Practice with the code examples
- Experiment with your own use cases
- Remember: Understanding async is the key to mastering agent frameworks

**Happy Coding!**

