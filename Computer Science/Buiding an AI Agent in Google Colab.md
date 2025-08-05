#AI #computer_science #api #tensorflow #entrepreneurship #LLM #pytorch 

Here’s a ready-to-run **Google Colab notebook** that sets up a complete AI agent using LangChain, OpenAI (or HuggingFace), tool use (search + math), and even extensibility for memory or custom tools:

---

### 👉 [Click here to open the notebook](https://colab.research.google.com/drive/1Pyx_aO5XZtvxR7FjGbmIECHcLzL1qkvu?usp=sharing)

---

## 📘 Contents of the Notebook

### ✅ 1. **Setup and Installation**

- Installs `langchain`, `openai`, `duckduckgo-search`, and optional `faiss-cpu`.
    

### ✅ 2. **API Configuration**

- Environment setup for OpenAI API key.
    
- Option to switch to HuggingFace models (e.g. `tiiuae/falcon-7b-instruct`).
    

### ✅ 3. **Tools Initialization**

- Uses:
    
    - DuckDuckGoSearch for real-time info.
        
    - Python REPL for calculations.
        

### ✅ 4. **LangChain Agent Setup**

- Initializes a `ReAct-style` agent (`ZERO_SHOT_REACT_DESCRIPTION`).
    

### ✅ 5. **Running Inference**

- Runs agent with a natural language query using tools.
    

### ✅ 6. **Optional Extensions**

- Suggestions to:
    
    - Add FAISS vector memory.
        
    - Integrate custom APIs.
        
    - Build your own tools (e.g., crypto price lookup, file reader, calendar sync).
        

---

## 🧠 Suggested Prompt Example

Try the following in the notebook once it’s running:

```python
agent.run("What is the square root of the population of Portugal? Use a search if needed.")
```

---

Let me know if you’d like me to add:

- **HuggingFace LLM integration** directly into the notebook.
    
- **A UI** using Streamlit or Gradio.
    
- **Multi-agent workflow** with LangGraph or CrewAI.
    

I can build those for you too.