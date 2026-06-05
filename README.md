# Hierarchical Multi-Agent Planning & Execution Framework

A research-oriented multi-agent AI system built with LangGraph that decomposes complex user goals into task DAGs, executes them through specialized agents, evaluates output quality, and adaptively replans when necessary.

The framework is designed to study whether structured multi-agent workflows outperform traditional single-agent systems on complex reasoning and planning tasks.

---

## Architecture

```text
User Goal
    │
    ▼
┌──────────┐
│  Router  │
└────┬─────┘
     │
     ▼
┌──────────────┐
│ DAG Planner  │
└────┬─────────┘
     │
     ▼
┌───────────────────────┐
│ Execution Orchestrator│
└────┬──────────────────┘
     │
     ├──────────────┐
     ▼              ▼
┌─────────┐    ┌─────────┐
│Research │    │Executor │
│ Worker  │    │ Worker  │
└────┬────┘    └────┬────┘
     └──────┬───────┘
            ▼
      ┌──────────┐
      │ Critic   │
      └────┬─────┘
           │
           ▼
   ┌──────────────┐
   │ Replanner    │
   └────┬─────────┘
        │
        ▼
┌─────────────────┐
│Output Assembler │
└─────────────────┘
```

---

## Key Features

### DAG-Based Planning

- Converts high-level goals into a Directed Acyclic Graph (DAG) of tasks.
- Supports dependency-aware execution.
- Validates task ordering using topological sorting.

### Specialized Agent Roles

- **Router Agent** – Classifies and routes user requests.
- **Planner Agent** – Generates executable task DAGs.
- **Research Worker** – Retrieves external information using Tavily.
- **Executor Worker** – Performs reasoning and task execution.
- **Critic Agent** – Evaluates solution quality.
- **Adaptive Replanner** – Revises plans based on critic feedback.
- **Output Assembler** – Produces the final response.

### Adaptive Self-Correction

- Maintains error memory across iterations.
- Automatically replans when quality thresholds are not met.
- Supports iterative improvement loops.

### Evaluation Framework

Includes:

- Single-agent baseline
- Multi-agent system
- Ablation experiments
- Runtime analysis
- Token usage tracking
- Convergence measurements
- Quality scoring via LLM-as-a-Judge

---

## Tech Stack

- LangGraph
- LangChain
- Anthropic Claude
- Pydantic
- Tavily Search
- MemorySaver
- Matplotlib
- Python

---

## Workflow

### 1. Routing

The router determines whether a request requires planning or direct execution.

### 2. Planning

The planner generates a dependency-aware task graph.

### 3. Execution

Tasks are executed in topological order by specialized workers.

### 4. Evaluation

The critic scores the generated solution using:

- Rule-based checks
- LLM-based evaluation

### 5. Replanning

If the score falls below a threshold, the replanner modifies the DAG and retries.

### 6. Final Synthesis

Outputs from all completed tasks are combined into a coherent final answer.

---

## Components

| Component | Purpose |
|------------|----------|
| `PipelineState` | Shared state schema |
| `LLMClient` | Provider abstraction with token tracking |
| `router_agent` | Request classification |
| `dag_planner_agent` | DAG generation and validation |
| `research_worker_agent` | External information gathering |
| `executor_worker_agent` | Task execution |
| `critic_agent` | Output evaluation |
| `adaptive_replanner_agent` | Plan revision |
| `execution_orchestrator` | Dependency-aware execution |
| `output_assembler` | Final response synthesis |

---

## Experimental Setup

The notebook contains three major experimental configurations:

### Baseline

A traditional single-agent system using a single LLM call.

### Hierarchical Multi-Agent System

Full workflow with:

- Planning
- Research
- Execution
- Critique
- Replanning

### Ablation Studies

Component-wise removal experiments to quantify the contribution of:

- Planning
- Critic feedback
- Replanning
- External research

Measured across quality, runtime, and token efficiency.

---

## Metrics Collected

- Final quality score
- Runtime
- Token usage
- Number of iterations
- Convergence rate
- Success rate

---

## Installation

```bash
pip install \
    langgraph \
    langchain \
    langchain-core \
    langchain-anthropic \
    pydantic \
    tavily-python \
    matplotlib
```

Set API keys:

```bash
export ANTHROPIC_API_KEY=YOUR_KEY
export TAVILY_API_KEY=YOUR_KEY
```

---

## Running the Project

Launch Jupyter Notebook:

```bash
jupyter notebook planner.ipynb
```

Run the notebook sequentially to:

1. Initialize the LLM and agent stack.
2. Construct the LangGraph workflow.
3. Execute benchmark tasks.
4. Run ablation studies.
5. Generate evaluation metrics and visualizations.

---

## Research Question

**Can hierarchical multi-agent systems with planning, critique, and adaptive replanning consistently outperform traditional single-agent LLM pipelines on complex tasks while maintaining reasonable computational cost?**

This project serves as an experimental framework for investigating that question.

---

## Future Improvements

- Parallel task execution
- Dynamic agent spawning
- Long-term memory integration
- Tool-augmented reasoning
- Human-in-the-loop feedback
- Multi-modal planning agents
- Distributed execution support

---

## Results

The framework is intended to compare:

| System | Quality | Cost | Runtime |
|----------|---------|---------|---------|
| Single Agent | Baseline | Low | Fast |
| Multi-Agent | Higher | Moderate | Slower |
| Ablated Variants | Variable | Variable | Variable |

Actual results depend on the benchmark suite and evaluation settings used.

---

## License

MIT License.
