# Agents & Tool Use: curriculum

*Course 6 of the library · Applied track · 18 chapters · about 22 hours · status: preview. Generated from `curricula/agents.txt` in the hub repository; edit it there.*

Building systems that take actions, and keeping them from taking bad ones. You will write an agent loop from nothing, design tools a model uses correctly, manage context and memory over long tasks, connect tools over MCP, build coding and computer-use agents, bound their cost and blast radius, and evaluate trajectories as well as outcomes. Every design decision is tested in an environment you build.

**For:** Engineers who can call a model and want to build something that plans, uses tools and recovers from its mistakes.

**By the end you can:**

- Write an agent harness from scratch and know what each part does
- Design tool interfaces that models use correctly, and measure misuse
- Manage context and memory across hundreds of steps
- Sandbox an agent and bound its cost, time and permissions
- Evaluate agents by end state and reliability, and improve them measurably

**Before you start:** Transformers From Scratch Ch 13 (Decoding and Sampling); Transformers From Scratch Ch 25 (DPO Variants, GRPO, and RLVR); Evaluation & Experimentation for LLM Systems Ch 13 (Evaluating Agents); Retrieval & Context Engineering Ch 15 (Context Engineering)

**Current to November 2026:** The agent harness as the unit of design (model + tools + context policy); MCP for tools and A2A for agent-to-agent delegation; context compaction and subagents for context isolation; skills as procedural memory loaded on demand; coding agents with test-driven loops; computer-use agents over screenshots and accessibility trees; state-checked agent benchmarks and pass^k; agentic RL training the models these harnesses run.

**Not covered:** Robotics, and agent frameworks taught as products (the course builds the parts instead).

**Reference package:** `agentkit/` · **Semantic hues:** dim-1 = the model; dim-2 = a tool; dim-3 = the environment; dim-4 = the context window; dim-5 = the human


## Part I · The loop

*Model, tools, environment, harness.*

### 01 · The Agent Loop from First Principles (55 min)

*Observe, decide, act — and when not to build an agent.*

Builds on: Transformers From Scratch Ch 13 (Decoding and Sampling)

1. **The loop** — a model, a tool call, an observation, repeated
2. **The harness** — everything around the model that decides what it sees and can do
3. **Workflows against agents** — when a fixed pipeline is the better design
4. **Stopping** — termination conditions and why agents stop too early or never

**Build:** a 150-line agent harness with no framework, used throughout the course.

### 02 · Function Calling Internals (55 min)

*How tool calls are trained, formatted, and parsed.*

Builds on: this course Ch 01 (The Agent Loop from First Principles)

Related: Inference Engineering at Scale Ch 11 (Structured Outputs)

1. **Tool calls in the chat template** — what the model actually sees
2. **Parsing** — streaming parsers and malformed calls
3. **Parallel tool calls and their ordering**
4. **Constrained decoding for arguments**

**Build:** a tool-call parser with fuzz tests against malformed model outputs.

### 03 · Designing Tools a Model Can Use (65 min)

*Schemas, names, errors, and interfaces.*

Builds on: this course Ch 02 (Function Calling Internals)

1. **Tool schemas** — names, descriptions and argument design, measured on misuse rates
2. **Errors as feedback** — messages that let the model recover
3. **Agent–computer interfaces** — tools shaped for models, not humans
4. **How models misuse tools** — a catalogue with fixes
5. **How many tools** — selection accuracy as the tool count grows

**Build:** a tool-misuse benchmark comparing two interface designs.

### 04 · Planning and Reasoning (60 min)

*ReAct, plan-and-execute, reflection, and search.*

Builds on: this course Ch 01 (The Agent Loop from First Principles); Transformers From Scratch Ch 25 (DPO Variants, GRPO, and RLVR)

1. **ReAct** — interleaving reasoning and actions
2. **Plan-and-execute** — and replanning on failure
3. **Reflection and self-critique** — when it helps and when it loops
4. **Tree search over actions**
5. **Reasoning models** — how built-in thinking changes the harness

**Build:** three planning strategies compared on one task suite, with tokens per success.


## Part II · Context and memory

*What the model sees, over a long task.*

### 05 · Context Management (60 min)

*Compaction, truncation, and cache-aware design.*

Builds on: Retrieval & Context Engineering Ch 15 (Context Engineering); this course Ch 01 (The Agent Loop from First Principles)

1. **The window fills** — tool outputs, history and instructions competing for tokens
2. **Truncating and summarising tool outputs**
3. **Compaction** — summarising history without losing the task
4. **Cache-aware context** — stable prefixes and append-only histories

**Build:** a compaction policy evaluated on long tasks for success and token cost.

### 06 · Memory Architectures (60 min)

*Scratchpads, files, retrieval, and skills.*

Builds on: this course Ch 05 (Context Management); Retrieval & Context Engineering Ch 03 (Dense Embeddings)

1. **Working memory** — scratchpads and plans in the context
2. **Files as memory** — notes the agent writes and reads
3. **Retrieval memory** — episodic and semantic stores
4. **Skills** — procedural memory loaded on demand
5. **Memory errors** — stale, wrong and poisoned memories

**Build:** file-based and retrieval memory compared across multi-session tasks.

### 07 · Protocols: MCP and A2A (55 min)

*Tools, resources, and agents across boundaries.*

Builds on: this course Ch 03 (Designing Tools a Model Can Use)

1. **MCP** — servers, tools, resources, prompts and transports
2. **Authorisation** — OAuth for remote MCP servers
3. **Tool discovery at scale** — many servers, many tools
4. **A2A** — delegating to other agents

**Build:** an MCP server and client, and a test suite for the server.


## Part III · Acting in the world

*Code, computers, and other agents.*

### 08 · Coding Agents (70 min)

*Navigation, edits, and test-driven loops.*

Builds on: this course Ch 03 (Designing Tools a Model Can Use); this course Ch 05 (Context Management)

1. **Repository navigation** — search, file views and symbol lookup
2. **Editing** — diffs, search-and-replace and whole-file writes compared
3. **Test-driven loops** — run, read the failure, fix
4. **The anatomy of a software-engineering benchmark task**

**Build:** a coding agent evaluated on a small set of real repository tasks.

### 09 · Computer-Use and Browser Agents (60 min)

*Screens, accessibility trees, and grounding.*

Builds on: this course Ch 03 (Designing Tools a Model Can Use)

Related: Multimodal & Generative Models Beyond Text Ch 04 (Vision-Language Models)

1. **Screenshots against accessibility trees against DOM** — what to show the model
2. **Grounding** — clicking the right element
3. **Latency and cost per step**
4. **Safety on real interfaces** — confirmations and irreversible actions

**Build:** a browser agent in a sandboxed browser, evaluated on scripted tasks.

### 10 · Multi-Agent Systems (55 min)

*Orchestrators, subagents, and when they hurt.*

Builds on: this course Ch 05 (Context Management)

1. **Orchestrator and workers** — dividing a task
2. **Subagents for context isolation**
3. **Where multi-agent designs measurably hurt** — coordination errors and cost multipliers
4. **Communication** — shared state against messages

**Build:** single-agent and orchestrator–worker designs compared on success and cost.

### 11 · Long-Horizon Autonomy (60 min)

*Budgets, checkpoints, and recovery.*

Builds on: this course Ch 05 (Context Management)

Related: Production AI Systems Ch 03 (Timeouts, Retries, and Streaming)

1. **Budgets** — steps, tokens, money and wall-clock time
2. **Checkpoints and durable execution** — resuming after a crash
3. **Human-in-the-loop** — approvals at the right moments
4. **Recovering from errors** — retries, backtracking and asking for help

**Build:** durable execution with budgets and approval checkpoints.


## Part IV · Safety and reliability

*Bounding the damage.*

### 12 · Sandboxing and Least Privilege (55 min)

*Containers, egress, secrets, and permissions.*

Builds on: this course Ch 07 (Protocols: MCP and A2A)

Related: AI Security & Red Teaming Ch 09 (Injection-Resistant Designs)

1. **Sandboxes** — containers and virtual machines, and what each isolates
2. **Network egress control** — allow-lists for the tools that need the network
3. **Secrets** — never in the context window
4. **Permission models** — read-only by default, scoped write, and confirmation

**Build:** a sandboxed tool runtime with an egress allow-list and audit log.

### 13 · A Taxonomy of Agent Failures (60 min)

*Worked diagnoses from real traces.*

Builds on: this course Ch 01 (The Agent Loop from First Principles); Evaluation & Experimentation for LLM Systems Ch 13 (Evaluating Agents)

1. **Loops and repetition**
2. **Hallucinated tools and arguments**
3. **Premature termination and false success claims**
4. **Context poisoning and drift from the goal**
5. **Reward hacking** — agents that game their own checks

**Build:** a trace-analysis tool that tags failures automatically, validated by hand.

### 14 · Reliability Engineering for Agents (55 min)

*pass^k, idempotency, and verification.*

Builds on: this course Ch 13 (A Taxonomy of Agent Failures)

1. **pass^k** — reliability over repeated attempts
2. **Idempotent tools and safe retries**
3. **Verification steps** — checking the work before reporting it
4. **Guardrails on actions**

**Build:** reliability improvements measured as pass^k on the task suite.


## Part V · Evaluate and improve

*Measure, then make it better.*

### 15 · Evaluating Agents (65 min)

*Trajectory and outcome, environments, and harness effects.*

Builds on: Evaluation & Experimentation for LLM Systems Ch 13 (Evaluating Agents)

1. **Outcome evaluation in state-checked environments**
2. **Trajectory evaluation** — efficiency, safety and process
3. **Building your own environment** — tasks, state, checkers and a simulated user
4. **Harness sensitivity** — and comparing models fairly

**Build:** an evaluation environment for the capstone agent.

### 16 · Improving Agents (60 min)

*Harness optimisation, trajectory fine-tuning, and RL.*

Builds on: this course Ch 15 (Evaluating Agents)

Related: Post-Training in Practice Ch 18 (Multi-Turn and Agentic RL)

1. **Harness and prompt optimisation** — systematic, measured
2. **Fine-tuning on successful trajectories**
3. **Agentic RL** — training the model in the environment
4. **What to change first** — the cheapest fix that moves the metric

**Build:** one harness change and one fine-tune, each evaluated against the baseline.

### 17 · Agent Economics (45 min)

*Tokens per task, caching, and model routing.*

Builds on: this course Ch 05 (Context Management)

Related: Production AI Systems Ch 12 (Cost Engineering)

1. **Cost per successful task** — the metric that matters
2. **Prompt caching for agents**
3. **Routing between small and large models**
4. **Latency per task**

**Build:** a cost-per-success report with routing and caching applied.

### 18 · Capstone: An Agent That Works, Measured (300 min)

*Build, sandbox, evaluate, improve.*

Builds on: this course Ch 15 (Evaluating Agents); this course Ch 16 (Improving Agents)

1. **The task domain and its environment**
2. **The agent** — harness, tools, memory and context policy
3. **The sandbox, budgets and approvals**
4. **The evaluation** — outcome, pass^k and cost
5. **One measured improvement**

**Build:** the agent, its environment, its evaluation and the improvement report.

## Capstone

A coding or research agent with a sandbox, budgets, a state-checked evaluation environment, and a measured improvement from one change to its harness.
