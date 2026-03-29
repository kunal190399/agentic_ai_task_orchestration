# Agentic AI for Autonomous Task Management

A multi-agent AI system demonstrating autonomous task decomposition, dependency-aware scheduling, distributed execution, and explainable decision tracing — built as research preparation for doctoral work in agentic AI frameworks for real-world software systems.

---

## Overview

Modern software platforms require increasingly complex, multi-step workflows that consume significant human coordination effort. This project explores how agentic AI architectures can autonomously manage these workflows — decomposing high-level goals into executable task graphs, scheduling tasks while respecting dependencies, dispatching specialised agents in parallel where possible, and maintaining a complete audit trail so human operators can understand and override any decision.

The system demonstrates three core research capabilities:

- **Multi-agent architecture** — specialised agents (Planner, Scheduler, Executor, Critic) coordinate autonomously to complete complex tasks
- **Planning and dynamic task allocation** — topological sorting produces dependency-aware execution waves that maximise parallelism
- **Controllable and explainable AI** — every agent decision is logged with its reasoning, confidence score, and input/output, enabling full human audit and intervention

---

## Motivation

Agentic AI represents a fundamental shift from AI as a tool to AI as an autonomous actor. The key open challenge is not capability — current LLMs can perform complex reasoning — but **controllability**: how do we allow AI agents to act semi-independently while ensuring human operators can understand, audit, and override their decisions in real-world deployments?

This project is built as preparation for doctoral research on agentic AI frameworks at the intersection of AI, software engineering, and multi-agent systems, in collaboration with MCD Systems and Teesside University as part of the Tees Valley Investment Zone PhD programme.

---

## System Architecture

```
User Goal
    |
    v
PlannerAgent
  - Decomposes goal into task dependency graph
  - Assigns agent types and priorities to each task
  - Logs reasoning for every decomposition decision
    |
    v
SchedulerAgent
  - Runs topological sort on dependency graph
  - Groups tasks into parallel execution waves
  - Prioritises within waves by task priority score
    |
    v
ExecutorAgents (wave by wave)
  - AnalysisAgent   → requirements and objective analysis
  - DataAgent       → data retrieval and processing
  - ProcessingAgent → synthesis across multiple inputs
  - OutputAgent     → structured result generation
  - CriticAgent     → quality validation and gap identification
    |
    v
DecisionTrace (continuous throughout)
  - Every agent action logged with timestamp
  - Reasoning and confidence score recorded
  - Full audit trail for human review and override
```

---

## Key Components

### PlannerAgent
Decomposes a high-level user goal into a directed acyclic graph (DAG) of tasks. Each task specifies its agent type, dependencies, and priority. In production this would use an LLM to generate the task graph dynamically. The planner logs its reasoning for every decomposition decision.

### SchedulerAgent
Implements Kahn's algorithm for topological sorting of the task DAG. Produces execution waves — groups of tasks whose dependencies are all satisfied and can therefore run in parallel. Tasks within each wave are ordered by priority. The scheduler enables maximum throughput without violating dependencies.

### ExecutorAgents
Five specialised executor types, each with a distinct capability domain. Executors operate autonomously within their scope, logging every action. They share a context dictionary that passes results between dependent tasks, simulating the inter-agent communication required in real distributed systems.

### DecisionTrace
The explainability core of the system. Every agent action — planning decisions, scheduling choices, task executions — is logged with a timestamp, action type, task identifier, input and output data, human-readable reasoning, and a confidence score. This trace is the mechanism by which the system remains controllable: human operators can review any decision, understand why it was made, and intervene if needed.

---

## Results

Running the pipeline on a realistic enterprise task produces:

- **5 tasks** across 3 execution waves
- **Wave 0:** T1 (Requirements Analysis) and T2 (Data Retrieval) execute in parallel — no dependencies
- **Wave 1:** T3 (Core Processing) executes after both T1 and T2 complete
- **Wave 2:** T4 (Output Generation) then T5 (Quality Validation) execute sequentially
- **Full audit trail** of all agent actions with reasoning and confidence scores
- **Critic Agent validation** flags quality issues and recommendations before final output is returned

---

## Visualisations

### Task Dependency Graph
Shows the DAG structure with nodes coloured by agent type. Directed edges show execution dependencies. The parallel structure of Wave 0 is clearly visible.

### Agent Decision Trace Timeline
Shows all agent actions in chronological order, coloured by agent. Demonstrates the interleaving of Planner, Scheduler, and Executor actions across the pipeline lifetime.

![Agentic Pipeline](agentic_pipeline.png)

---

## Requirements

```bash
pip install langchain langchain-openai langchain-community langgraph openai python-dotenv networkx matplotlib
```

| Library | Purpose |
|---|---|
| LangChain / LangGraph | Agent orchestration framework (production extension) |
| NetworkX | Task dependency graph construction and topological sort |
| Matplotlib | Visualisation of task graph and decision trace |
| Python stdlib | dataclasses, typing, time, datetime, json |

---

## Usage

Run the notebook cell by cell:

```bash
jupyter notebook agentic_ai_task_orchestration.ipynb
```

Output saved automatically:
- `agentic_pipeline.png` — two-panel visualisation figure

To run with a different goal, change the `goal` string in Cell 5:

```python
result = orchestrator.run(
    goal="Your custom goal here"
)
```

---

## Connection to PhD Research

This project is built as preparation for doctoral research into agentic AI for autonomous task management at Teesside University, supervised by Prof. Annalisa Occhipinti, in partnership with MCD Systems.

**Aim 1 — Multi-agent AI architectures:**
The Planner, Scheduler, Executor, and Critic agents form a complete multi-agent architecture. Each agent operates within a defined scope with clear interfaces. The DecisionTrace provides the communication layer that connects agents while maintaining transparency.

**Aim 2 — Planning, scheduling, and dynamic task allocation:**
Kahn's topological sort algorithm produces provably valid execution orders. Priority-based ordering within waves enables dynamic allocation based on task importance. The dependency graph structure is designed to be extensible to runtime task insertion and re-scheduling.

**Aim 3 — Real-world integration, robustness, and explainability:**
The shared context dictionary simulates the inter-agent state sharing required for real software platform integration. The confidence scoring and reasoning logging provide the explainability layer needed for enterprise deployment. The architecture is designed to plug into real LLM APIs (LangChain) and external tool integrations with minimal modification.

**Next steps toward the PhD:**
- Replace simulated executors with real LLM-powered agents using LangChain tool use
- Implement reinforcement learning-based policy for dynamic task re-prioritisation at runtime
- Add formal verification of task graph validity (deadlock detection, cycle prevention)
- Integrate with MCD Systems' real software platforms for empirical evaluation
- Develop formal metrics for controllability, robustness, and deployment efficiency

---

## Project Structure

```
agentic-ai-task-orchestration/
├── agentic_ai_task_orchestration.ipynb   # Full pipeline notebook
├── agentic_pipeline.png                  # Output visualisation
└── README.md                             # This file
```

---

## References

Weng, L. (2023). LLM Powered Autonomous Agents. *Lil'Log*. https://lilianweng.github.io/posts/2023-06-23-agent/

Park, J.S., et al. (2023). Generative Agents: Interactive Simulacra of Human Behavior. *Proceedings of UIST 2023*.

Kahn, A.B. (1962). Topological sorting of large networks. *Communications of the ACM*, 5(11), 558-562.

Chase, H. (2022). LangChain: Building applications with LLMs through composability. https://github.com/langchain-ai/langchain

Teesside University (2026). Agentic AI for Autonomous Task Management — PhD Studentship, Tees Valley Investment Zone. School of Computing, Engineering and Digital Technologies.

---

## Author

**Kunal Kamble**
MSc Advanced Computer Science, University of Liverpool
[LinkedIn](https://linkedin.com/in/kunal-kamble19) | [Email](mailto:kamblekunal165@gmail.com)

*Built in preparation for the PhD studentship in Agentic AI for Autonomous Task Management at Teesside University, supervised by Prof. Annalisa Occhipinti, in partnership with MCD Systems.*
