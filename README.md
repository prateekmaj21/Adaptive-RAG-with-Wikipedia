# Adaptive-RAG-with-the-Wikipedia

This Python script implements an adaptive Retrieval Augmented Generation (RAG) system using LangChain.  \
**1. Setup and Dependencies:**

* **Imports:** Imports necessary libraries, including LangChain components for LLMs, embeddings, vectorstores, document loaders, and text splitters.  It also imports tools for interacting with Wikipedia and for visualizing the workflow.  Crucially, it includes LangSmith for experiment tracking and debugging.
* **Environment Setup:** Loads environment variables from a `.env` file, which should contain API keys for Groq (LLM), Tavily (potentially for another service, though not directly used in the current code), and LangSmith.  The script includes functions to prompt for the API keys if they are not already in the .env.
* **Package Installation:** Installs required Python packages using pip.

**2. Component Selection:**

* **LLM:** Initializes a `ChatGroq` LLM, using a specific model like `llama3-8b-8192`.
* **Embedding Model:** Sets up a sentence transformer embedding model.

**3. Workflow Definition (State Graph):**

* **`GraphState`:** Defines a Pydantic model representing the state of the RAG pipeline at various stages. This includes the user's query, retrieved documents, a potentially transformed query, graded documents, and the final response.
* **Nodes (Functions):** Defines functions for each step in the RAG workflow:
    * **`web_search`:** (Simulated) Performs a web search based on the query.
    * **`wikipedia_search`:** Uses the LangChain `WikipediaQueryRun` tool to query Wikipedia.  This is a key part of the adaptive logic.
    * **`retrieve`:** (Simulated) Retrieves documents from a vectorstore.
    * **`grade_documents`:** Filters retrieved documents, keeping the top two.  This is another adaptive point.
    * **`generate`:** Generates a response using the LLM and the graded documents.
    * **`transform_query`:** (Potentially) modifies the query to improve search results.

**4. Routing Logic:**

* **`route_question`:** Directs the query to either a web search, vectorstore retrieval, or *Wikipedia search* based on keywords in the question.  This is the core of the "adaptive" aspect.
* **`decide_to_generate`:** Decides whether to generate an answer directly or to transform the query first, based on the number of graded documents.
* **`grade_generation_v_documents_and_question`:** Decides whether the generated response is sufficient or if the workflow should continue or terminate.

**5. Workflow Compilation:**

* Creates a `StateGraph` from LangGraph, connecting the nodes with conditional edges based on the defined routing logic.  The graph defines the execution flow.  The visualization is created using the `get_graph` method.

**6. Execution and Visualization:**

* The script visualizes the compiled workflow graph as an image.
* It then runs the workflow several times with different example queries, demonstrating how the routing logic directs the flow.

**7. LangSmith Tracing:**

* Sets up LangSmith tracing for monitoring and debugging.  The API key and project name are set in environment variables and the code uses `tracing_v2_enabled` context manager for all subsequent executions.
* The code executes three different sample queries (gravity, AI advancements, machine learning) demonstrating the LangSmith tracing integration.

**In Summary:**

The script builds an adaptive RAG system that intelligently chooses between different information sources (Wikipedia, hypothetical web search and vector store) depending on the user query. The workflow is clearly defined by a state graph. The visualization helps understand how the system flows. Finally, LangSmith tracing helps track and debug the application, making it a more robust system.  The current code simulates parts of the flow and lacks an actual vector store.  Note that it uses the latest version of LangChain.
