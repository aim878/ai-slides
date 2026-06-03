# Day 21: Introduction to LangGraph Framework

---

## Slide 1: Welcome to Day 21
- **Moving into LangGraph Territory**
- Building on previous agent frameworks (OpenAI Agents SDK, Crew AI)
- Focus: Stability, resiliency, and repeatability
- New way of thinking about agent workflows

**Key Point**: LangGraph offers a different approach - organizing workflows as graphs

---

## Slide 2: The LangChain Ecosystem
- **Three Main Products**:
  1. **LangChain** - Original framework for LLM abstractions
  2. **LangGraph** - Modern agent workflow orchestration
  3. **LangSmith** - Monitoring and debugging tool

**Important**: LangGraph is independent from LangChain (can use any framework)

**Website**: https://www.langchain.com/langgraph

---

## Slide 3: What is LangChain?
- **Original Purpose**: Simplify API integrations
- **Evolution**: Chaining LLM calls together
- **Features**:
  - RAG (Retrieval-Augmented Generation)
  - Prompt templates
  - Memory management
  - Declarative language (LCEL)
  - Tool abstractions

**Note**: More general-purpose, predates modern agent excitement

---

## Slide 4: What is LangGraph?
- **Core Purpose**: Platform for complex agent workflows
- **Focus Areas**:
  - Stability and fault tolerance
  - Resiliency at scale
  - Repeatability and monitoring
  - Human-in-the-loop capabilities

**Key Insight**: Organizes workflows as graph structures (tree-like)

---

## Slide 5: LangGraph Key Features
- **Agent-Driven Experiences**:
  - Multi-agent collaboration
  - Conversation history and memory
  - Time travel (checkpointing)
  - Human-in-the-loop interactions
  
- **Deployment**:
  - Fault-tolerant scalability
  - Ability to restore any point in time

---

## Slide 6: LangGraph Components
**Three Parts**:
1. **LangGraph Framework** - Core library (our focus)
2. **LangGraph Studio** - Visual builder tool
3. **LangGraph Platform** - Hosted deployment solution

**Analogy**: Similar to Crew AI (Framework, Studio, Enterprise)

---

## Slide 7: LangSmith Integration
- **Separate Monitoring Tool**
- Works with both LangChain and LangGraph
- **Capabilities**:
  - Visibility into LLM calls
  - Reasoning traces
  - Quick debugging of failures

**We'll use LangSmith** to monitor our graphs

**Website**: https://smith.langchain.com/

---

## Slide 8: Anthropic's Perspective
**From "Building Effective Agents" Blog Post**:
- Frameworks simplify low-level tasks
- BUT: Create abstraction layers that obscure prompts
- Can make debugging harder
- May add unnecessary complexity

**Anthropic's Recommendation**: Start with direct LLM API calls

**Source**: https://www.anthropic.com/research/building-effective-agents

---

## Slide 9: Core Terminology - Graph
**Graph**: A tree structure representing your agent workflow
- Nodes connected together
- Hierarchical dependencies
- Visual representation of logic flow

**Think**: Flowchart for agent operations

---

## Slide 10: Core Terminology - State
**State**: Current snapshot of your application
- Represents "state of the world" at any point
- Shared across entire application
- **Immutable** - never modified, only replaced

**Key Concept**: State is data, not a function

---

## Slide 11: Core Terminology - Nodes
**Nodes**: Python functions that do work
- Each node = one operation
- **Input**: Current state
- **Process**: Perform logic (call LLM, side effects, etc.)
- **Output**: Return updated state

**Remember**: Nodes receive state → do something → return new state

---

## Slide 12: Core Terminology - Edges
**Edges**: Python functions that determine flow
- Connect nodes together
- Decide what happens next based on state
- Two types:
  - **Simple**: Direct connection (A → B)
  - **Conditional**: Based on conditions (if X then B, else C)

**Summary**: Nodes do work, edges choose what's next

---

## Slide 13: Visual Example

```
     [Start]
        ↓
    [Node 1] ← Does work
        ↓
    [Node 2] ← Does more work
       / \
      /   \  ← Conditional edge
     ↓     ↓
[Node 3] [End]
```

**Nodes**: Orange circles (operations)
**Edges**: Arrows (flow control)

---

## Slide 14: Five Steps to Build a Graph
**Before your agents run, you must**:
1. Define State class
2. Start Graph Builder
3. Create Nodes (functions)
4. Create Edges (connections)
5. Compile the Graph

**Then**: Invoke the graph to run it

---

## Slide 15: Understanding the Two Phases
**Phase 1 - Graph Building** (Setup):
- Define state structure
- Create nodes and edges
- Compile the graph
- This happens at runtime, before execution

**Phase 2 - Graph Execution** (Running):
- Invoke the graph
- Agents perform work
- State flows through nodes

---

## Slide 16: State is Immutable
**What "Immutable" Means**:
- Never change the state object directly
- Receive old state → return new state
- Maintains snapshots for time travel

**Example Pattern**:
```
def my_node(old_state):
    # Don't modify old_state
    new_state = State(updated_value=...)
    return new_state
```

---

## Slide 17: Reducers Explained
**Reducer**: Special function for combining states
- Associated with specific state fields
- Combines new state with current state
- **Why needed?**: Enables parallel node execution

**Benefit**: Multiple nodes can update state simultaneously without conflicts

---

## Slide 18: Built-in Reducer - add_messages
**Most Common Reducer**: `add_messages`
- Concatenates message lists
- Packages messages (HumanMessage, AIMessage)
- Handles conversation history automatically

**Usage**: Annotate state fields with reducers

---

## Slide 19: Type Hints and Annotations
**Python Type Hints**:
```
def shout(text: str) -> str:
    return text.upper()
```

**Annotated Type Hints**:
```
from typing import Annotated

def shout(text: Annotated[str, "something to shout"]) -> str:
    return text.upper()
```

**LangGraph uses annotations** to specify reducers

---

## Slide 20: Defining State with Pydantic

**State Definition Example**:
```
from pydantic import BaseModel
from typing import Annotated
from langgraph.graph.message import add_messages

class State(BaseModel):
    messages: Annotated[list, add_messages]
```

**Breakdown**:
- `messages`: Field name
- `list`: Type
- `add_messages`: Reducer function

---

## Slide 21: Starting the Graph Builder

**Step 2 - Initialize Builder**:
```
from langgraph.graph import StateGraph

graph_builder = StateGraph(State)
```

**Note**: Pass the State **class**, not an instance

---

## Slide 22: Creating a Node

**Step 3 - Define Node Function**:
```
def my_node(old_state: State) -> State:
    # Do something
    result = some_operation()
    
    # Return new state
    return State(messages=[result])

# Add to graph
graph_builder.add_node("my_node", my_node)
```

---

## Slide 23: Creating Edges

**Step 4 - Connect Nodes**:
```
from langgraph.graph import START, END

# Simple edges
graph_builder.add_edge(START, "my_node")
graph_builder.add_edge("my_node", END)
```

**START** and **END** are special constants

---

## Slide 24: Compiling and Visualizing

**Step 5 - Compile**:
```
graph = graph_builder.compile()

# Visualize the graph
from IPython.display import Image, display
display(Image(graph.get_graph().draw_mermaid_png()))
```

**Benefit**: See your workflow visually before running

---

## Slide 25: Running the Graph

**Invoke to Execute**:
```
initial_state = State(messages=[{"role": "user", "content": "Hello"}])

result = graph.invoke(initial_state)

print(result)
```

**Key Method**: `graph.invoke(state)` executes the workflow

---

## Slide 26: Simple Example - Random Responses
**Demo Purpose**: Show graphs work without LLMs
- Node picks random noun + adjective
- Returns silly sentences
- Demonstrates state flow

**Takeaway**: Nodes are just Python functions (any logic works)

---

## Slide 27: Real Example - Chatbot Node

**Using LangChain's ChatOpenAI**:
```
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(model="gpt-4o-mini")

def chatbot_node(old_state: State) -> State:
    response = llm.invoke(old_state.messages)
    return State(messages=[response])
```

---

## Slide 28: Integration with LangChain
**Optional but Convenient**:
- LangChain provides `ChatOpenAI` wrapper
- Simplifies LLM calls
- Community examples use it
- **Alternative**: Use OpenAI SDK directly

**Flexibility**: LangGraph works with any framework

---

## Slide 29: Current Limitation - No Memory
**Problem**: Each invoke is independent
- No conversation history maintained
- Graph doesn't remember previous messages
- Each call starts fresh

**Solution**: Coming in Day 22 (tomorrow)!

---

## Slide 30: What's Next - Day 22
**Tomorrow We'll Add**:
- Conversation memory/history
- Tool calling capabilities
- Conditional edges
- More complex workflows

**Get Ready**: Building on today's foundation

---

## Slide 31: Key Takeaways
1. **LangGraph** = Framework for robust agent workflows
2. **Graph** = Tree structure of nodes and edges
3. **State** = Immutable snapshot of application
4. **Nodes** = Functions that do work
5. **Edges** = Functions that control flow
6. **Two Phases**: Build graph → Execute graph

---

## Slide 32: Resources
- **LangGraph Docs**: https://langchain-ai.github.io/langgraph/
- **LangChain Docs**: https://python.langchain.com/
- **LangSmith**: https://smith.langchain.com/
- **Anthropic Blog**: https://www.anthropic.com/research/building-effective-agents
- **GitHub**: https://github.com/langchain-ai/langgraph

---

## Slide 33: Practice Challenge
**Build Your First Graph**:
1. Define a State with messages
2. Create a simple node (can be silly!)
3. Add edges from START → node → END
4. Compile and visualize
5. Invoke with test input

**Goal**: Get comfortable with the five-step process

---

## Slide 34: Questions to Consider
- How does LangGraph compare to previous frameworks?
- When would you choose LangGraph over others?
- What problems does the graph structure solve?
- How does immutability help with reliability?

---

## Slide 35: Thank You!
**Day 21 Complete**
- Learned LangGraph fundamentals
- Understood graphs, states, nodes, edges
- Built first simple graph
- Ready for advanced features tomorrow

**See you in Day 22!**

