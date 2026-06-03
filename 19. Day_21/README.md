# Day 21: Introduction to LangGraph Framework

Welcome to Day 21! This folder contains materials for learning **LangGraph** - a framework for building stable, resilient, and repeatable agent workflows using graph structures.

## 📁 Folder Contents

- **Day_21_Presentation_Slides.md** - Slide content covering LangGraph concepts (35 slides)
- **Day_21_Lecture_Notes.md** - Comprehensive lecture notes with detailed explanations
- **Day_21_LangGraph.ipynb** - Step-by-step Jupyter notebook with code examples
- **requirements.txt** - Python dependencies
- **README.md** - This file (setup instructions)

## 🎯 What You'll Learn

### Core Concepts
1. **LangGraph Ecosystem** - Understanding LangChain, LangGraph, and LangSmith
2. **Graph Terminology** - Graphs, states, nodes, and edges
3. **Building Graphs** - The five-step process
4. **State Management** - Immutability and reducers
5. **Practical Examples** - Random response bot and OpenAI chatbot

### Key Topics
- Graph-based workflow design
- State immutability and why it matters
- The `add_messages` reducer
- Python type hints and annotations
- Visualizing graphs
- Integration with LangChain and OpenAI
- Current limitations (no memory between invocations)

## 🚀 Setup Instructions

### Step 1: Create a Conda Environment

```bash
# Create a new conda environment
conda create -n llm-env python=3.11 -y

# Activate the environment
conda activate llm-env
```

### Step 2: Install Dependencies

```bash
# Navigate to the Day_21 folder
cd Day_21

# Install required packages
pip install -r requirements.txt
```

### Step 3: Set Up API Keys

Create a `.env` file in the Day_21 folder:

```bash
# On Windows
type nul > .env

# On Mac/Linux
touch .env
```

Add your OpenAI API key to the `.env` file:

```
OPENAI_API_KEY=your_openai_api_key_here
```

**Get your API key**: https://platform.openai.com/api-keys

### Step 4: Run the Notebook

```bash
# Launch Jupyter
jupyter notebook Day_21_LangGraph.ipynb
```

Or use VS Code / Cursor with the Jupyter extension.

## 📊 What's in the Notebook

### Example 1: Random Response Graph
- Demonstrates core LangGraph concepts
- No LLM required - uses random word generation
- Shows that nodes are just Python functions
- Includes Gradio interface

### Example 2: OpenAI Chatbot
- Real chatbot using GPT-4o-mini
- Integration with LangChain's ChatOpenAI
- Graph visualization
- Demonstrates current limitation (no memory)

### Additional Sections
- Type hints and annotations tutorial
- State immutability demonstration
- Reducer explanation
- Summary and practice exercises

## 🔑 Key Takeaways

1. **LangGraph = Framework for robust agent workflows**
2. **Graphs = Tree structures of nodes and edges**
3. **State = Immutable snapshots of application state**
4. **Nodes = Python functions that do work**
5. **Edges = Functions that control flow**
6. **Five Steps**: Define State → Start Builder → Create Nodes → Create Edges → Compile
7. **Two Phases**: Build graph structure → Execute graph

## 🎨 Graph Visualization

LangGraph can visualize your workflows! The notebook includes examples of graph visualization using Mermaid diagrams.

**Optional**: For better visualization, install graphviz:

```bash
# On Windows (using conda)
conda install graphviz

# On Mac
brew install graphviz

# On Linux
sudo apt-get install graphviz
```

## ⚠️ Current Limitation

The basic graphs we build today **don't maintain memory** between invocations. Each `invoke()` call is independent.

**Example**:
```python
# First call
result1 = graph.invoke(State(messages=[{"role": "user", "content": "My name is Alice"}]))

# Second call (doesn't remember Alice)
result2 = graph.invoke(State(messages=[{"role": "user", "content": "What's my name?"}]))
```

**Solution**: Day 22 will cover memory, checkpointing, and persistent state!

## 🔧 Troubleshooting

### Import Errors

**Problem**: `ModuleNotFoundError: No module named 'langgraph'`

**Solution**:
```bash
conda activate llm-env
pip install langgraph langchain langchain-openai
```

### API Key Issues

**Problem**: `OpenAIError: The api_key client option must be set`

**Solution**:
1. Verify `.env` file exists in Day_21 folder
2. Check API key format: `OPENAI_API_KEY=sk-...`
3. Restart Jupyter kernel after adding `.env`

### Gradio Launch Issues

**Problem**: Gradio interface doesn't launch

**Solution**:
```bash
# Update gradio
pip install --upgrade gradio

# If port is busy, specify a different port
# In the notebook, change:
# interface.launch()
# to:
# interface.launch(server_port=7861)
```

### Graph Visualization Issues

**Problem**: `Exception: Graphviz not found`

**Solution**:
- Graph visualization is optional
- The notebook will still work, showing text representation
- To enable visualization, install graphviz (see above)

## 📚 Additional Resources

### Official Documentation
- **LangGraph Docs**: https://langchain-ai.github.io/langgraph/
- **LangChain Docs**: https://python.langchain.com/
- **LangSmith**: https://smith.langchain.com/

### GitHub Repositories
- **LangGraph**: https://github.com/langchain-ai/langgraph
- **LangChain**: https://github.com/langchain-ai/langchain

### Blog Posts
- **Anthropic - Building Effective Agents**: https://www.anthropic.com/research/building-effective-agents
- **LangChain Blog**: https://blog.langchain.dev/

### Community
- **LangChain Discord**: https://discord.gg/langchain
- **GitHub Discussions**: https://github.com/langchain-ai/langgraph/discussions

## 🎓 Practice Exercises

After completing the notebook, try these exercises:

1. **Modify Random Bot**: Change the word lists to create different silly sentences
2. **Multi-Node Graph**: Create a graph with 3+ nodes in sequence
3. **System Prompt**: Add a system prompt to give the chatbot a personality
4. **Temperature Experiments**: Test different temperature values (0.0 to 1.0)
5. **Counter Graph**: Build a graph that increments a counter

## 🔜 What's Next (Day 22)

Tomorrow we'll extend our LangGraph knowledge with:

- **Conversation Memory** - Maintaining history across invocations
- **Checkpointing** - Saving and restoring state
- **Tool Calling** - Adding tools to nodes
- **Conditional Edges** - Dynamic routing based on state
- **Complex Workflows** - Multi-node graphs with branching

## 💡 Tips for Success

1. **Run cells in order** - The notebook builds progressively
2. **Read the comments** - They explain what each section does
3. **Experiment** - Modify code and see what happens
4. **Visualize** - Use graph visualization to understand flow
5. **Check outputs** - Print statements show state evolution

## 📝 Notes

- **Python Version**: Requires Python 3.11+
- **API Costs**: Using GPT-4o-mini is very affordable (~$0.0001 per request)
- **Notebook Runtime**: Each example takes a few seconds to run
- **Memory Usage**: Minimal - suitable for any modern computer

## 🤝 Need Help?

If you encounter issues:

1. Check the Troubleshooting section above
2. Verify your conda environment is activated
3. Ensure all dependencies are installed
4. Check that your API key is valid
5. Review the lecture notes for concept clarification

## 📄 License

Educational materials for learning purposes.

---

**Happy Learning! 🚀**

See you in Day 22 where we'll add memory, tools, and advanced features to our LangGraph workflows!

