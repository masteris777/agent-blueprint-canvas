# Agent Blueprint Canvas Manifest

## Purpose

The Agent Blueprint Canvas is a visual design language for describing agents and multi-agent systems before implementation.

Its purpose is to give Business Analysts, Architects, and Engineers a shared artifact that is:

- simple enough to use in live workshops
- precise enough to guide implementation
- independent of any specific SDK or vendor

The canvas captures the design intent of an agent, not the final implementation details.

---

## Design Layer

The Agent Blueprint operates at the design intent layer.

It sits above implementation and below detailed architecture and workflow notation.
It is the shared visual artifact used before engineering begins.

After the canvas is complete, downstream artifacts may define workflow logic, technical design, protocol bindings, runtime architecture, and operational implementation details.

It is not:

- a deployment specification
- a prompt template
- a runtime contract
- a workflow diagram

It is a structured design sketch that a business stakeholder can review and an engineer can use as the basis for detailed technical design.

---

## Scope

This manifest defines:

- the canonical building blocks of an Agent Blueprint Canvas
- the meaning of each canvas section
- how a canvas maps to implementation concepts
- how canvases participate in a larger system view

This manifest does not define:

- vendor-specific runtime APIs
- final prompt wording
- middleware implementation details
- deployment architecture

---

## Core Principle

The canvas is a design artifact.

Each visible section must represent a concept that stakeholders can reason about directly.
Implementation-specific details should only appear when they materially affect the design.

As a result:

- model or LLM choice is not a required canvas element
- prompt instructions are represented in condensed form through Mission and Skills
- technical enforcement outside the prompt is represented through Controls

---

## System View

The larger system view is composed of Agent Blueprint Canvases and their relationships.

An Agent Blueprint Canvas is the primary building block.
Multiple canvases can be placed inside a broader system view to represent:

- collaboration between agents
- invocation paths
- shared or separate context boundaries
- external systems accessed through tools

The single-agent canvas is the base unit from which larger designs are composed.

---

## Context Boundary

A Context Boundary is the structural container for agents that operate over the same shared session context.

Within the canvas, the following terms are treated as equivalent at the conceptual level:

- context
- session context
- session memory
- thread context
- shared state for a run or session

If multiple agents are enclosed by the same Context Boundary, they share the same working context for that run or session.
This means they operate over the same conceptual memory space, even if they only use different parts of it.

If an agent is shown without an explicit boundary, the default assumption is that it operates in its own implicit context boundary.

The default visual representation of a Context Boundary is a dashed rectangle surrounding the canvases that share the same context.

### What a Context Boundary Means

A Context Boundary means:

- shared session context
- shared artifacts produced during the run
- shared intermediate outputs and working memory
- handoff inside the same contextual space

The shared context may contain, for example:

- user inputs
- conversation history
- intermediate artifacts
- documents
- notes and summaries
- tool results
- analysis outputs

### What a Context Boundary Does Not Mean

A Context Boundary does not automatically imply:

- the same permissions
- the same tools
- the same knowledge access
- the same context projection for each agent

Those remain agent-specific.

### Cross-Boundary Rule

Crossing a Context Boundary is always explicit.

- agents inside the same boundary may hand off directly
- agents outside the boundary do not share context automatically
- external agents are invoked as tools and communicate through tool inputs and outputs

This rule prevents accidental assumptions of hidden state sharing across independent systems.

### Handoff Inside a Boundary

When an agent performs a handoff to another agent inside the same Context Boundary:

- control moves to the next agent
- the next agent continues within the same shared context
- artifacts produced earlier remain available in that context
- no explicit state transfer is required at the canvas level

---

## Context View

Although agents inside a Context Boundary share the same context, each agent may attend to only a subset of that shared context.

This subset is the agent's Context View.

Conceptually, this can be described as a projection function:

`f(context)`

where the full shared context exists at the boundary level, and each agent consumes only the part relevant to its role.

This means:

- the shared context is the source of truth
- each agent may load only the parts it needs
- agents do not need identical awareness of the full context in order to share it structurally

Context View may be declared on an Agent Blueprint Canvas to show which subset of the shared context that specific agent reads.
The Context Boundary remains the main structural signal for showing where context is shared.

---

## Workflow Integration

The Agent Blueprint Canvas defines the structure of an agent.
It does not define the notation for orchestration logic.

The recommended separation is:

- **Agent Blueprint Canvas**: capability definition
- **Workflow Diagram**: orchestration, sequencing, conditions, loops, approvals, and routing

In other words, the Agent Blueprint Canvas defines what an agent is and what it can do.
The workflow layer defines when and how agents are invoked.

Established workflow notation may be used for the orchestration layer, including BPMN, flowchart notation, or similar deterministic workflow notations.
When explicit workflow semantics are required, they should be modeled in that workflow layer rather than added to the canvas itself.

### Integration Contract

The integration points between workflow and canvas are:

- workflow enters an agent through its **Inputs**
- workflow leaves an agent through its **Outputs**
- external capability access leaves an agent through its **Tools**

### Out of Scope for the Canvas Spec

The Agent Blueprint Canvas does not standardize:

- connector line styles
- conditional gateway notation
- approval gate notation
- loop or retry notation
- BPMN pool/lane semantics
- multiplicity notation on arrows

These belong to the workflow layer, not the canvas specification.

### Human Approval Semantics

The canvas declares approval requirements through tool-level markers such as `⚠`.

This means:

- the canvas defines which capability requires approval
- the workflow defines where and how the approval happens

No separate human-actor primitive is required in the canvas specification itself.

### Dynamic Multiplicity

The Agent Blueprint Canvas defines a single agent type.

If the surrounding workflow invokes the same agent many times, multiplicity should be expressed in the workflow diagram or connector labeling, not by changing the canvas definition.

---

## Agent Blueprint Canvas

An Agent Blueprint Canvas describes one agent.

The canvas contains eight sections:

1. Header
2. Inputs
3. Mission
4. Context View
5. Skills
6. Tools
7. Output
8. Controls

---

## Header

The header identifies the agent.

Fields:

- Agent Name
- Role
- Mode

### Mode

The execution mode declares whose identity the agent uses when acting.

- **Autonomous**: the agent acts under its own service identity
- **Delegated**: the agent acts under the calling principal's identity

Mode is a design-level property, not a derived runtime label.

---

## Inputs

Inputs define how the agent may be invoked.

An input maps to a request contract: the information the agent expects to receive when execution begins.

Typical input types are:

- **Query**: the caller expects a direct answer
- **Command**: the caller expects an action to be performed
- **Event**: the agent reacts to an external signal or state change
- **Handoff**: the agent receives an upstream artifact or delegated work item

Inputs may be structured or unstructured.
In implementation terms, this may correspond to an API request, event payload, message envelope, or user message.

Cardinality: **1..N**

---

## Mission

Mission is the condensed intent of the agent.

It is a single free-form statement describing:

- what the agent is responsible for
- within what scope it operates
- the constraints that belong inside prompt-level behavior

Mission is the design-time short form of the system prompt.
It should be understandable to a stakeholder, while still precise enough to guide prompt design.

There is exactly one Mission per agent.

Cardinality: **1**

---

## Context View

Context View defines which subset of the shared session context an agent reads.

Context itself is not part of the Agent Blueprint Canvas because it is not owned by any single agent.
Context belongs to the surrounding Context Boundary.

Context View exists on the canvas because each agent typically uses only part of the shared context available inside that boundary.

Typical examples of Context View include:

- conversation summary
- research brief
- uploaded documents
- tool results
- intermediate artifacts
- notes produced by other agents

Context View is optional.

Cardinality: **0..1**

---

## Skills

Skills are internal behavioral modules the agent can load when needed.

They represent reusable procedural intelligence, not external capabilities.
In implementation terms, a skill typically corresponds to a dynamic prompt fragment, often stored as a `skill.md` file or equivalent artifact.

The important design property is:

- the agent is aware the skill exists
- the agent can load the skill when needed
- the skill temporarily enriches the working context
- the skill can drop out of context once the task is complete

Skills are suitable for:

- SOPs
- reasoning procedures
- reusable instructions for specialized tasks

Skills are not tools.
They describe how the agent should think or proceed, not what external system it can call.

Cardinality: **0..N**

---

## Tools

Tools are external callable capabilities.

In implementation terms, a tool may correspond to:

- an MCP tool
- an API integration
- a function call
- a database operation
- an outbound communication capability
- another agent invoked as an agent tool

Tool types:

- **Read**: query-only access, no side effects
- **Knowledge**: specialized read tool for retrieval from knowledge sources
- **Write**: mutates external state
- **Agent**: invokes another agent as an external capability

Tool approval markers are tool-level execution constraints.
They indicate that a specific tool invocation requires human approval before execution.

Knowledge is treated as a special tool type rather than a separate section.

Cardinality: **0..N**

---

## Output

Output defines the response contract of the agent.

The canvas declares the set of output outcomes the agent is allowed to produce.
This may include one or more direct response forms, and may also include routing outcomes such as handoff to another agent.

Supported output forms:

- **Text**
- **Structured**
- **Handoff**

Artifacts created through write tools are not part of the Output section.
They are considered side effects of tool execution.
The Output section describes what the agent may return or where it may route execution next.

Cardinality: **1..N**

---

## Controls

Controls are technical guardrails implemented outside the prompt.

They define which middleware or execution-boundary mechanisms must exist in the final solution.
Unlike prompt constraints, controls are intended to operate independently of the agent's reasoning and cannot be bypassed through prompt behavior alone.

Controls are represented as a fixed checklist in v1.

The v1 control set is:

- **Input Scope**
- **Data Sensitivity**
- **Injection Defense**
- **Output Validation**
- **Audit Trail**

Each checked control means:

> this control category must be present in the implemented execution pipeline.

The canvas does not prescribe how the control is implemented.
That remains an engineering decision.

Cardinality: **0..N** from a fixed set

---

## Section Mapping

| Canvas Section | Synonym(s) | Primary Meaning | Typical Implementation Mapping | Cardinality |
|---|---|---|---|---|
| Header | identity | Agent identity | Name, role, mode metadata | 1 |
| Inputs | trigger, request | Request contract | API schema, message schema, event payload | 1..N |
| Mission | system prompt | Condensed operating intent | System prompt, base instruction, policy prompt | 1 |
| Context View | attention scope | Agent-visible subset of shared context | Context projection, selected artifacts, retrieval scope | 0..1 |
| Skills | SOPs, prompt modules | Dynamic internal capability modules | `skill.md`, SOP prompt chunk, procedural prompt fragment | 0..N |
| Tools | MCP, API, function, delegation | External callable capabilities | MCP tool, function, API client, agent tool | 0..N |
| Output | response contract | Allowed response and routing outcomes | Response model, structured response, handoff contract | 1..N |
| Controls | guardrails, middleware | External guardrail requirements | Middleware, validators, filters, audit components | 0..N |

---

## Composition Rules

The canvas is intentionally minimal.

To preserve clarity:

- prompt-level behavioral constraints belong in **Mission** or **Skills**
- external capability declarations belong in **Tools**
- response and routing outcomes belong in **Output**
- technical enforcement outside the prompt belongs in **Controls**

This separation is essential.
If a constraint can be bypassed by the model through prompt failure, it is not a Control.
If a behavior belongs inside the agent's reasoning, it should not be modeled as middleware.

---

## Multi-Agent Interpretation

When moving from a single canvas to a larger system view:

- each agent remains represented by one Agent Blueprint Canvas
- agents that share the same session context should be enclosed by the same Context Boundary
- agents without an explicit shared boundary are assumed to operate in separate implicit contexts
- calls to external agents are modeled through **Agent** tools
- handoff between agents may be represented as an allowed **Output** outcome when control moves from one agent to another
- orchestration emerges from the placement of canvases within a larger workflow or system diagram
- connectors between canvases are standard diagramming elements and are outside the Agent Blueprint Canvas specification
- shared runtime or context relationships may be represented at the system layer, not inside the canvas itself

This keeps the canvas stable while allowing the overall system view to scale to hierarchical and swarm-like designs.

---

## Design Principles

### 1. Minimal but Sufficient

The canvas should expose only the concepts needed to reason about the agent.

### 2. Prompt vs. Pipeline Separation

Mission and Skills shape reasoning.
Controls shape execution boundaries.

### 3. SDK Independence

The canvas must remain portable across implementation frameworks.

### 4. Workshop Usability

A business stakeholder should be able to understand and contribute to the artifact.

### 5. Engineering Traceability

Each canvas section should map cleanly to implementation responsibilities.

---

## Non-Goals for v1

The following are intentionally out of scope for v1:

- detailed technical design and component interaction design
- explicit model configuration
- memory topology details
- memory persistence, retrieval, retention, synchronization, and lifecycle rules
- knowledge ingestion pipelines, indexing flows, database synchronization, refresh jobs, and ETL-style content preparation
- deployment/runtime topology
- protocol bindings and interface contracts such as MCP, A2A, REST, RPC, webhooks, or event schemas
- implementation of tool adapters, transport layers, and agent communication interfaces
- evaluation metrics, benchmarks, test harnesses, and acceptance thresholds
- observability design including logs, traces, metrics, alerts, and audit pipeline implementation
- runtime hooks, extension points, lifecycle callbacks, and middleware wiring
- vendor-specific guardrail products
- tool parameter schemas
- orchestration DSLs
- workflow connector semantics
- gateway and decision notation
- approval flow notation
- multiplicity notation for repeated agent invocation

These may be added later if they prove necessary, but are excluded from the initial canvas to preserve clarity.

---

## Status

- **Document**: Agent Blueprint Canvas Manifest
- **Version**: v1
- **Status**: Draft

---

## Canonical Structure

```yaml
agent_blueprint_canvas:
  header:
    name: "ChatGPT"
    role: "General purpose AI assistant"
    mode: "delegated" # autonomous | delegated

  inputs:
    - type: "query" # query | command | event | handoff
      name: "user_question"

  mission: >
    Help the user understand, create, transform, and organize
    information, act when needed, reason over uploaded context,
    and hand over specialist work when the request calls for it.

  context_view:
    - "current conversation"
    - "uploaded files"
    - "tool results"
    - "artifacts produced during the session"

  skills:
    - "summarize"
    - "explain"
    - "plan"
    - "write"

  tools:
    - type: "read" # read | knowledge | write | agent
      name: "web_search"
    - type: "knowledge"
      name: "uploaded_files"
    - type: "write"
      name: "canvas_edit"
      approval_required: false

  output:
    - type: "text" # text | structured | handoff
      name: "response"
    - type: "structured"
      name: "research_brief"

  controls:
    input_scope: true
    data_sensitivity: true
    injection_defense: true
    output_validation: true
    audit_trail: true
```

This structure is the canonical canvas-level representation for v1.

Shared context itself is not part of the Agent Blueprint Canvas structure.
Shared context is represented by the Context Boundary in the larger system view.
