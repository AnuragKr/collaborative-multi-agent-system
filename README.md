## Collaborative Multi-Agent System
This project demonstrates how to build a collaborative multi-agent system using LangGraph to create a specialized "Data Analyst" capable of leveraging various tools to answer complex queries, particularly those involving data analysis and visualization. Inspired by the [AutoGen: Enabling Next-Gen LLM Applications via Multi-Agent Conversation, by Wu, et. al.](https://arxiv.org/pdf/2308.08155) paper, this system employs a "divide-and-conquer" approach, assigning specialized tasks to different agents.

## Project Overview
Traditional single-agent LLM systems can struggle with complex tasks requiring multiple tools or domains. This project addresses this limitation by orchestrating a team of specialized agents, each an "expert" in a specific area. The core workflow involves:

+ Researcher Agent: Utilizes web search tools to gather live information relevant to user queries.
+ Chart Generator Agent: Specializes in processing data and generating visualizations using Python.
+ Router: Dynamically directs tasks and calls the appropriate agent and tools based on the current state of the conversation.
This collaborative approach allows the system to handle more intricate requests by breaking them down into manageable sub-tasks for each expert agent.

## Workflow Visualization
The system's dynamic workflow is illustrated in the following diagram:
![](https://i.imgur.com/t4RGrJo.png)

## Features
+ Multi-Agent Collaboration: Employs a team of specialized agents (Researcher and Chart Generator) to tackle complex problems.
+ Dynamic Task Routing: A central router intelligently directs queries to the most suitable agent based on the task requirements.
+ Web Search Capabilities: Integrates the Tavily API for real-time information retrieval.
+ Python REPL for Data Analysis & Visualization: Utilizes a Python REPL tool, enabling the Chart Generator agent to execute Python code for data manipulation and chart creation.
+ LangGraph Integration: Built on LangGraph for robust and flexible state management and agent orchestration.
+ OpenAI Model Integration: Leverages OpenAI's gpt-4o model for intelligent decision-making and response generation.

## Key Components in the Notebook
+ Tools:
* ```TavilySearchResults```: For performing web searches with a specified ```max_results``` and ```search_depth```.
* ```python_repl```: A custom tool that executes Python code using ```langchain_experimental.utilities.PythonREPL```, returning the output or an error message.
+ LLM Setup: Uses ```ChatOpenAI``` with ```model="gpt-4o"```.
+ Agent Definition:
* ```AgentState```: A ```TypedDict``` to manage the state of the graph, including ```input```, ```chat_history```, and ```current_plan```.
* ```Agent```: A custom class to encapsulate an LLM and its bound tools, along with a function to run the agent with a given state.
+ Nodes and Edges (LangGraph):
* ```call_agent```: A generic node function to execute an agent and capture its output.
* ```route_agent```: A conditional router function that inspects the agent's output to decide the next step (continue, web search, generate chart, etc.).
* The ```StateGraph``` is used to define the flow between nodes, starting at ```agent_researcher``` and ending at ```__end__```.

## Example Usage
The notebook provides examples of how to query the multi-agent system. You can interact with it by invoking the compiled graph with an input dictionary containing your query.

```bash
Python

# Example from the notebook
# This will trigger the Researcher Agent and then the Chart Generator Agent
print(app.invoke({"input": "Plot the population of the 5 largest cities in the world", "chat_history": []}))
```
