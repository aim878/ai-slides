# Day 18: Tools, Resources & Building Your Career Agent

## Slide 1: Introduction to Day 18
**From Theory to Practice: Building Real Agents**

**Today's Focus:**
- Understanding AI Agent Frameworks landscape
- Resources: Enhancing LLM capabilities with context
- Tools: Giving LLMs the power to take actions
- Building a practical Career Conversation Agent

**Key Shift:**
- Moving from theoretical patterns to hands-on implementation
- No frameworks this week - direct LLM interaction
- Understanding what happens "under the hood"
- Building foundational knowledge for future frameworks

**What You'll Build:**
A deployable career chatbot that answers questions about your professional background, uses tools to record user interest, and can be hosted on your website.

---

## Slide 2: AI Agent Frameworks Landscape
**Understanding Your Options**

**Complexity Hierarchy (Bottom to Top):**

**Level 1: No Framework**
- Direct API calls to LLMs
- Full control over prompts and logic
- What we're doing this week
- Anthropic's "Building Effective Agents" advocates this approach

**Level 2: Lightweight Frameworks**
- **OpenAI Agents SDK:** Simple, elegant, flexible (Week 2)
- **CrewAI:** Low-code, configuration-driven (Week 3)

**Level 3: Heavyweight Frameworks**
- **LangGraph:** Complex, powerful, computational graphs (Week 4)
- **Autogen:** Microsoft's distributed agent environment (Week 5)

**Special Mention: MCP (Model Context Protocol)**
- Not a framework, but a protocol
- Open-source way to connect models to data and tools
- Anthropic's approach to standardization (Week 6)

---

## Slide 3: Framework Trade-offs
**Choosing the Right Approach**

**No Framework (Direct APIs):**
- ✅ Complete control and transparency
- ✅ See exactly what's happening
- ✅ No abstractions to learn
- ❌ More boilerplate code
- ❌ Manual handling of common patterns

**Lightweight Frameworks:**
- ✅ Reduced boilerplate
- ✅ Still feel like working with LLMs
- ✅ Easier to get started
- ❌ Some abstraction overhead
- ❌ Framework-specific patterns

**Heavyweight Frameworks:**
- ✅ Very powerful capabilities
- ✅ Handle complex scenarios
- ✅ Rich ecosystems
- ❌ Steep learning curve
- ❌ Project becomes "framework-centric"
- ❌ Heavy terminology and concepts

**Recommendation:** Start simple, add complexity as needed

---

## Slide 4: What Are Resources?
**Enhancing LLM Capabilities with Context**

**Definition:**
Resources are additional information you provide to LLMs to improve their expertise and effectiveness on specific tasks.

**How It Works:**
- Grab relevant data for the question
- Insert it into the prompt
- LLM can reference this information in responses

**Simple Example:**

You provide the LLM with a system prompt identifying it as a customer support agent, then include resources like ticket prices, company policies, and FAQs. When a user asks about flight costs to Paris, the LLM can reference the ticket price information you provided to give an accurate answer.

**The LLM now has context to answer accurately!**

**Beyond Simple Insertion:**
- Can use techniques to find most relevant context
- RAG (Retrieval Augmented Generation)
- Use other LLMs to help select relevant information
- Dynamically choose what context to include

---

## Slide 5: Resources in Practice
**Real-World Applications**

**Customer Support:**
- Product documentation
- Company policies
- Previous ticket resolutions
- FAQ databases

**Career Chatbot (Our Project):**
- LinkedIn profile (PDF)
- Resume/CV
- Personal summary
- Project descriptions
- Skills and achievements

**Legal Assistant:**
- Case law documents
- Relevant statutes
- Previous case outcomes
- Client-specific information

**Research Assistant:**
- Academic papers
- Research notes
- Domain-specific knowledge
- Citation databases

**Key Insight:** Resources turn generic LLMs into domain experts!

---

## Slide 6: What Are Tools?
**Giving LLMs the Power to Act**

**Definition:**
Tools allow LLMs to perform actions and interact with the real world, not just generate text.

**Core Concept:**
- LLM can decide to use a tool at its discretion
- This is key to giving LLMs autonomy
- Transforms chatbots into agents

**Examples of Tools:**
- Query a database (SQL)
- Send messages/notifications
- Search the web
- Read/write files
- Call external APIs
- Control IoT devices
- Make calculations
- Book appointments

**The Magic:**
LLM decides WHEN and HOW to use tools based on the conversation context.

---

## Slide 7: How Tools Actually Work
**Demystifying the "Magic"**

**What It Seems Like:**
LLM on the cloud can directly execute functions on your computer - sounds magical!

**What Actually Happens:**
1. You describe available tools in JSON format
2. You send this JSON to the LLM with your prompt
3. LLM responds: "I want to use tool X with parameters Y"
4. Your code checks the response (if statement!)
5. Your code runs the tool
6. You send results back to LLM
7. LLM generates final response

**The Reality:**
- It's JSON and if statements
- No actual magic
- LLM just generates likely next tokens
- Those tokens happen to be tool call requests
- You interpret and execute them

**Key Takeaway:** Understanding this removes the mystery and empowers you to build!

---

## Slide 8: Tool Calling Example
**Seeing It in Action**

**Prompt to ChatGPT:**

You tell the LLM it's a support agent for an airline with the ability to query ticket prices. You instruct it to respond with a specific format when it wants to use the tool. When a user asks about flight costs to Paris, the LLM responds by indicating it wants to use the tool to fetch the ticket price for Paris.

**ChatGPT Response:**

The LLM generates text saying it wants to use the tool to fetch the ticket price for Paris. Your code then parses this response, calls your actual ticket price function, and sends the result back to the LLM for the final answer.

**That's It!**
- LLM generates text indicating tool use
- Your code parses this response
- Your code calls the actual function
- You send result back to LLM
- LLM incorporates result into final answer

**The "Autonomy":**
LLM decided to use the tool - you gave it the option, it chose to take it.

---

## Slide 9: Building the Career Agent - Overview
**Project Architecture**

**Components:**

**1. Resources:**
- LinkedIn profile (PDF)
- Personal summary text
- Career highlights

**2. Tools:**
- `record_user_details()` - Capture interested users
- `record_unknown_question()` - Track unanswered questions

**3. System Prompt:**
- Instructions for the agent's behavior
- Tone and style guidelines
- When to use tools

**4. Evaluation (Optional):**
- Second LLM checks response quality
- Provides feedback for improvement
- Ensures professional output

**5. User Interface:**
- Gradio for simple web interface
- Chat-style interaction
- Easy to deploy

---

## Slide 10: Setting Up Resources
**Loading Career Information**

**LinkedIn Profile:**

Use a PDF reading library to open your LinkedIn PDF file, then loop through each page extracting the text content. Combine all the text from every page into a single string variable that contains your complete LinkedIn profile information.

**Personal Summary:**

Open your summary text file and read its entire contents into a variable. This gives you a brief, personalized summary that complements the LinkedIn profile with additional context and personality.

**System Prompt with Resources:**

Create a comprehensive system prompt that identifies the LLM as representing you personally. Include instructions about answering career-related questions professionally. Then embed both your personal summary and complete LinkedIn profile text directly into this prompt. Add guidelines about tone and behavior, including instructions to admit when it doesn't know something. This entire prompt with embedded resources is sent to the LLM with every conversation.

---

## Slide 11: Defining Tools - JSON Format
**Describing Tools to the LLM**

**Tool Definition Structure:**

Each tool is described using JSON format with several key components: the tool type (function), a unique name matching your Python function, a clear description explaining what the tool does and when to use it, and a parameters section detailing each input the tool needs. For each parameter, specify its data type and provide a description. Mark which parameters are required versus optional. This JSON structure is what gets sent to the LLM so it understands what tools are available.

**What This Does:**
- Tells LLM a tool exists
- Describes what it does
- Specifies required parameters
- LLM uses this to decide if/when to call it

**Note:** Frameworks automate this JSON generation!

---

## Slide 12: Implementing Tool Functions
**The Actual Python Functions**

**Record User Details:**

This function accepts an email address (required) along with optional name and notes parameters. It formats a notification message containing all the provided information, sends it to your phone via Pushover, and returns a success confirmation message that the LLM will see.

**Record Unknown Question:**

This function takes a question as input, formats it into a notification message indicating the agent couldn't answer it, sends the notification to your phone, and returns a confirmation that the question was recorded for future improvement.

**Pushover:**
- Simple push notification service
- Free for first month
- Easy alternative to SMS
- Install app on phone
- Get API key and user token

---

## Slide 13: Handling Tool Calls
**The Critical Function**

**Core Logic:**

The handler function loops through each tool call request from the LLM. For each request, it extracts the tool name and arguments from the LLM's response. Then comes the "glorified if statement" - it checks which tool was requested and calls the corresponding Python function with the provided arguments. The function result is formatted into a message with the proper role and ID, then added to a list. This list of tool results is returned to be sent back to the LLM.

**Pythonic Alternative:**

Instead of multiple if statements, you can use Python's globals dictionary to look up functions by their string name and call them dynamically. This is more elegant and scales better when you have many tools.

---

## Slide 14: The Agent Loop
**Putting It All Together**

**The Main Chat Function:**

The chat function starts by building a messages list containing the system prompt, all previous conversation history, and the current user message. It then enters a loop that continues until the LLM indicates it's finished. 

In each iteration, it calls the OpenAI API with the messages and tool definitions. It checks the finish reason from the response - if the LLM wants to use tools, the function executes those tools using the handler, adds the results to the messages, and loops back to call the LLM again. If the LLM doesn't request tools, it's done, so the function exits the loop and returns the final response to the user.

This loop structure is what makes it an "agent" - it can iterate multiple times, using tools as needed, until it achieves the goal.

---

## Slide 15: Understanding the Loop
**Step-by-Step Execution**

**Iteration 1:**
1. User asks: "What's your greatest accomplishment?"
2. Call LLM with message + tools
3. LLM responds with answer (no tools needed)
4. Return answer to user

**Iteration 2:**
1. User asks: "Who's your favorite musician?"
2. Call LLM with message + tools
3. LLM: "I don't know" + calls `record_unknown_question`
4. We execute the tool (send notification)
5. Call LLM again with tool result
6. LLM generates final response to user

**Iteration 3:**
1. User: "I'd like to get in touch"
2. LLM asks for email
3. User provides email
4. LLM calls `record_user_details` tool
5. We execute (send notification)
6. LLM confirms with user

**The loop continues until LLM decides it's done!**

---

## Slide 16: System vs User Prompts
**Separating Concerns**

**System Prompt:**
- Overall instructions and context
- Sets the tone and behavior
- Defines the agent's role
- Includes resources (LinkedIn, summary)
- Describes when to use tools
- Remains constant across conversation

**User Prompt:**
- The actual question from the user
- Changes with each message
- What the user wants to know
- Drives the conversation forward

**Why Separate?**
- Clearer prompt engineering
- Easier to maintain
- Better model performance
- Standard practice in LLM applications

**Example:**
- System: "You are a helpful career assistant..."
- User: "What's your educational background?"

---

## Slide 17: Structured Outputs
**Getting Structured Data from LLMs**

**What Are Structured Outputs?**
- LLMs respond with specific data structures
- Not just free-form text
- JSON that maps to Python objects
- Guaranteed format

**Using Pydantic:**

Pydantic is a Python library for data validation. You define a class that describes the structure you want - for example, an Evaluation class with two fields: is_acceptable as a boolean and feedback as a string. This class serves as a schema for the LLM's response.

**Calling with Structured Output:**

When calling the LLM, you specify your Pydantic class as the response format. The LLM generates JSON matching this structure, and the client library automatically parses it into a Python object. You can then access the fields directly as object attributes - the is_acceptable field will be a proper boolean, and feedback will be a string. This ensures type safety and predictable data structures.

**Connection to Tools:**
Tool calling IS structured outputs! Same underlying mechanism.

---

## Slide 18: Adding an Evaluator
**Quality Control Pattern**

**The Evaluator-Optimizer Pattern:**
1. Generator LLM creates response
2. Evaluator LLM checks quality
3. If acceptable: return to user
4. If not: provide feedback and regenerate

**Evaluator System Prompt:**

Create a system prompt for the evaluator LLM that clearly defines its role and criteria. Instruct it to check whether responses are professional, engaging, accurate based on the provided context, have appropriate tone, and actually answer the question. Tell it to respond using the structured evaluation object format you've defined.

**Benefits:**
- Improved response quality
- Catches errors before user sees them
- Production-ready reliability
- Can use different models (e.g., Gemini for evaluation)

**In Our Project:**
We built this in Day 17 lab - can integrate it here!

---

## Slide 19: Gradio User Interface
**Simple Web Interface**

**What Is Gradio?**
- Python library for building data science UIs
- Incredibly simple to use
- Great for demos and prototypes
- Can be deployed to production

**Basic Chat Interface:**

Import the Gradio library and define your chat function that takes a message and history as parameters. Then create a ChatInterface by calling Gradio's ChatInterface constructor, passing in your chat function and a title. Finally, call the launch method to start the web server. 

**That's It!**
- Automatic chat UI
- Message history handled
- Clean, professional look
- Runs locally or can be deployed

**Why Gradio?**
Even if you're terrible at frontend (like the instructor!), you can build beautiful interfaces.

---

## Slide 20: Deployment with Hugging Face Spaces
**Making It Live**

**Hugging Face Spaces:**
- Free hosting for Gradio apps
- Simple deployment process
- Can embed in your website
- Professional URLs

**Deployment Steps:**

Navigate to your project directory in the terminal and run the gradio deploy command. This starts an interactive deployment process.

**Questions You'll Answer:**
1. App title: "Career Conversation"
2. App file: app.py
3. Hardware: cpu-basic (free tier!)
4. Secrets: OpenAI API key, Pushover tokens
5. GitHub action: No

**Secrets (Environment Variables):**

You'll need to provide your OpenAI API key, Pushover user key, and Pushover token as secrets. These are stored securely on Hugging Face and made available to your app as environment variables.

**Result:**

Your app will be deployed and accessible at a Hugging Face Spaces URL with your username and app name in the path.

---

## Slide 21: Embedding in Your Website
**Professional Integration**

**Hugging Face Provides:**
- Embed code for your space
- iframe that loads your app
- Looks native to your site

**Example:**

Use an HTML iframe element with the source pointing to your Hugging Face Space URL. Set the width to 100% to make it responsive and choose an appropriate height like 600 pixels. This embeds your chat interface directly into your website, making it appear as if it's part of your site while actually being hosted on Hugging Face.

**Benefits:**
- No backend infrastructure needed
- Free hosting
- Automatic updates when you push changes
- Professional appearance

**Use Cases:**
- Personal website portfolio
- Professional profile
- Replace static resume
- Interactive career showcase

**The Future of Resumes:**
Instead of PDF, people chat with your AI to learn about you!

---

## Slide 22: The Agent Loop Definition (2026)
**Modern Understanding of Agents**

**Evolution of Definitions:**

**Early 2025 (Sam Altman/OpenAI):**
"AI systems that can do work for you independently"

**Mid 2025 (Anthropic/HuggingFace):**
"AI systems where an LLM controls the workflow"

**2026 Consensus:**
"An LLM equipped with tools, running in a loop to achieve a goal"

**Key Components:**
1. **LLM** - The intelligence
2. **Tools** - The capabilities
3. **Loop** - Iterative execution
4. **Goal** - Specific objective

**What It Feels Like:**
You call the agent → It's off doing its thing → Returns with result

**What Actually Happens:**
Your code repeatedly calls LLM → Interprets responses → Executes tools → Loops until done

---

## Slide 23: Building a Simple Agent Loop
**Concrete Example: To-Do List Agent**

**The Tools:**

Two simple functions manage a to-do list. The first function accepts a list of task descriptions and adds each one to the todos list while marking them as not completed. The second function takes an index and marks that specific to-do item as completed. Both functions return a formatted report showing all current to-dos and their completion status.

**The Problem:**

A classic math problem: A train leaves Boston at 2pm traveling 60mph, while another train leaves New York at 3pm traveling 80mph toward Boston. When do they meet?

**What Happens:**
1. Agent creates to-dos: "Interpret problem", "Set up variables", "Compute", "Format answer"
2. Agent marks each complete as it works through them
3. Agent provides final answer

**The Magic:**

The agent autonomously plans its own approach and executes each step systematically. You see it progressively check off items as it works, then deliver the final answer. This demonstrates how LLM plus tools plus loop equals intelligent, goal-oriented behavior!

---

## Slide 24: Why Agent Loops Work
**The Unreasonable Effectiveness**

**Better Performance:**
- Breaking problems into steps improves accuracy
- Iterative refinement produces better results
- Tool use enables capabilities beyond text generation

**Real Computation:**
- Can actually calculate, not just estimate
- Can access real-time data
- Can verify its own work

**Emergent Behavior:**
- LLM decides its own strategy
- Adapts based on intermediate results
- Shows "reasoning" through process

**Key Insight:**
Simply generating tokens as an answer doesn't perform as well as using agentic techniques that cause the LLM to work through problems step-by-step.

**Remember:**
It's still just next-token prediction! But structured in a way that produces remarkably intelligent behavior.

---

## Slide 25: Practical Considerations
**Building Production Agents**

**Error Handling:**
- Tools can fail
- LLMs can make mistakes
- Network issues
- Handle gracefully!

**Cost Management:**
- Each loop iteration costs tokens
- Set maximum iterations
- Monitor spending
- Use cheaper models where appropriate

**Latency:**
- Multiple LLM calls take time
- Consider streaming responses
- Show progress to users
- Set timeouts

**Security:**
- Validate tool inputs
- Limit tool capabilities
- Don't expose sensitive functions
- Audit tool usage

**Testing:**
- Test with various inputs
- Check edge cases
- Verify tool calls work correctly
- Monitor in production

---

## Slide 26: Commercial Applications
**Real-World Use Cases**

**Customer Support:**
- Answer questions using knowledge base (resources)
- Book appointments (tools)
- Check order status (tools)
- Escalate to human when needed (tools)

**Personal Assistants:**
- Manage calendar (tools)
- Send emails (tools)
- Research topics (tools + resources)
- Track tasks (tools)

**Business Automation:**
- Process documents (resources)
- Update databases (tools)
- Generate reports (resources + tools)
- Notify stakeholders (tools)

**Career Applications:**
- Our career chatbot!
- Recruitment screening
- Interview preparation
- Professional networking

**Key Differentiator:**
Tools transform chatbots from informational to actionable!

---

## Slide 27: Best Practices
**Building Effective Agents**

**Start Simple:**
- Begin with no framework
- Understand the fundamentals
- Add complexity as needed

**Clear Prompts:**
- Be explicit about tool usage
- Describe when to use each tool
- Repetition in prompts helps!

**Tool Design:**
- Keep tools focused and simple
- Clear descriptions
- Validate inputs
- Return useful information

**Testing Strategy:**
- Test each tool independently
- Test with various prompts
- Check edge cases
- Monitor token usage

**Iterate:**
- Start with basic functionality
- Add tools incrementally
- Refine prompts based on behavior
- Gather user feedback

---

## Slide 28: Key Takeaways
**What You've Learned**

**1. Framework Landscape:**
- Spectrum from no framework to heavyweight
- Trade-offs between control and convenience
- Start simple, add complexity as needed

**2. Resources:**
- Enhance LLM capabilities with context
- Turn generic models into domain experts
- Simple but powerful technique

**3. Tools:**
- Give LLMs ability to act
- "Just" JSON and if statements
- Enables true agent behavior

**4. Agent Loop:**
- LLM + Tools + Loop = Agent
- Iterative execution until goal achieved
- Remarkably effective approach

**5. Practical Skills:**
- Built deployable career agent
- Understood tool calling mechanism
- Ready for frameworks next week

---

## Slide 29: Next Steps and Exercises
**Continuing Your Journey**

**Improve Your Career Agent:**
1. Add more resources (projects, achievements, fun facts)
2. Implement RAG for better context retrieval
3. Add more tools (SQL database, calendar integration)
4. Integrate the evaluator from Day 17
5. Improve UI with Gradio customization
6. Add streaming responses

**Build Your Own Agent Loop:**
- Start fresh notebook
- Create simple tools
- Implement the loop from scratch
- Test with different problems
- Add your own creative tools

**Explore:**
- Try different models
- Experiment with prompt engineering
- Test various tool combinations
- Deploy your own version

**Share:**
- Post your agent on LinkedIn
- Get feedback from community
- Iterate based on responses

---

## Slide 30: Resources and Links
**Essential References**

**Key Resources:**
- **Anthropic "Building Effective Agents":**
  https://www.anthropic.com/research/building-effective-agents
  
- **Simon Willison's Blog:**
  https://simonwillison.net
  Excellent insights on agents and LLMs
  
- **Hugging Face Spaces:**
  https://huggingface.co/spaces
  Free deployment platform
  
- **Gradio Documentation:**
  https://gradio.app
  UI framework for ML apps
  
- **Pushover:**
  https://pushover.net
  Simple push notifications

**Tools:**
- PyPDF2: PDF parsing
- Pydantic: Data validation
- python-dotenv: Environment variables

**Next Week:**
OpenAI Agents SDK - Our first framework!

**Keep building, keep learning!**

