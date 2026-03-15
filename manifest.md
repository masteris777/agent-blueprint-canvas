# Agent Blueprint Canvas Manifest

## Purpose

The Agent Blueprint Canvas is a visual design language for describing agents and multi-agent systems before implementation.

Project slogan:

> Reveal the Forest Behind the Trees.

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

### Behavioral Building Blocks

The Agent Blueprint exists to model a specific kind of design unit: the semantic actor.

In a larger system, different building blocks have different behavioral properties:

- workflow blocks model deterministic sequencing and control logic
- state-machine blocks model deterministic state evolution
- agent blueprints model non-deterministic semantic actors

Real systems often combine all three.
The purpose of the canvas is not to absorb workflow or state-machine notation, but to let semantic actors be represented clearly alongside those deterministic structures.

This distinction matters because the behavioral character of a system depends on the kinds of blocks from which it is composed.
A system may contain deterministic orchestration and still behave non-deterministically because one or more of its components are semantic agents.

The key modeling move is therefore to separate control logic from semantic judgment:

- control logic belongs to workflow and state-based notation
- semantic judgment belongs to agent notation

Once those are separated, hybrid systems become easier to reason about, challenge, and hand off to engineering.

---

## Scope

This manifest defines:

- the canonical building blocks of an Agent Blueprint Canvas
- the meaning of each canvas section
- how a canvas maps to implementation concepts
- how canvases participate in a larger system view

This manifest is the semantic authority for the canvas.
The canonical machine-readable structure is defined separately in `schemas/agent-blueprint-canvas.schema.json`.

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

### Communication vs. Shared Context

Communication topology and shared working context are separate concerns.

- communication defines which participants can exchange messages or signals
- shared context defines which participants can read from the same working memory or session state

As a result:

- a communication relationship does not by itself imply shared context
- a shared context relationship does not by itself imply the same communication surface

The system view may therefore include both:

- **Context Boundaries** to show shared working context
- **Channels** to show communication surfaces between participants

Channels are a system-view concept.
They may represent direct conversations, group conversations, event buses, or similar communication surfaces without changing the canvas structure itself.

Channels carry three high-level communication message types:

- **Query**: asks for information or judgment
- **Command**: requests a change in the state of the world outside the agent's sealed internal context
- **Event**: reports that a relevant fact or world-state change has happened

Handoff is not a communication message type.
It is a control-transfer mechanism between agents inside the same Context Boundary.

These message types are modeled at the canvas level as semantic categories only.
Reply structures, acknowledgements, clarifications, acceptance, rejection, completion signaling, and similar protocol details belong to downstream model and implementation design.

The canvas should describe an agent in terms of the interactions it can participate in, not in terms of social roles such as manager, worker, or peer.

### Team-Level Intake

In the system view, a Context Boundary may act as the intake surface for channel communication.

This means:

- a channel may address a Context Boundary rather than a specific agent
- the communication is thereby addressed to the shared team represented by that boundary
- one enclosed agent may take responsibility for the message based on its communication capability and the team's operating logic

The Blueprint does not prescribe how that selection happens.
It may be deterministic, negotiated, policy-driven, or emergent from the surrounding system design.

A single agent may be treated as an implicit one-agent team.
In that case, a direct connection to an agent is shorthand for a channel addressed to that agent's implicit Context Boundary.

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

Agents inside the same Context Boundary may also be treated as one execution team for the purposes of receiving work from a channel.
Which enclosed agent actually takes responsibility is not defined by the boundary itself.
That remains a matter of agent capability and system-level coordination.

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
- interactions across a boundary are modeled through channels or tools, not as handoffs

This rule prevents accidental assumptions of hidden state sharing across independent systems.

### Handoff Inside a Boundary

When an agent performs a handoff to another agent inside the same Context Boundary:

- control moves to the next agent
- the next agent continues within the same shared context
- the same overall task or conversational thread continues
- artifacts produced earlier remain available in that context
- no explicit state transfer is required at the canvas level

In this sense, agents enclosed by the same Context Boundary may act as a team that continues the same task through internal control transfer.

A handoff therefore preserves continuity of:

- the same task or thread
- the same shared working context
- the same artifacts already available in that context

A handoff does not by itself imply:

- tool invocation
- cross-boundary delegation
- a new communication channel
- an explicit state transfer artifact

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

- workflow or channel communication enters an agent through its **Incoming** communication capability
- workflow or channel communication may be initiated by an agent through its **Outgoing** communication capability
- external capability access leaves an agent through its **Tools**
- intra-team control transfer leaves an agent through its **Handoffs**

### Out of Scope for the Canvas Spec

The Agent Blueprint Canvas does not standardize:

- connector line styles
- conditional gateway notation
- approval gate notation
- loop or retry notation
- BPMN pool/lane semantics
- multiplicity notation on arrows

These belong to the workflow layer, not the canvas specification.

### Approval And Consent Semantics

Approval and consent are execution-policy concerns that belong outside the v1 canvas.

This means:

- the canvas identifies communication, tools, controls, and handoffs
- the surrounding implementation decides whether execution requires prior approval, pre-granted consent, or dynamic consent at runtime

No separate human-actor primitive or per-item approval marker is required in the v1 canvas specification itself.

### Dynamic Multiplicity

The Agent Blueprint Canvas defines a single agent type.

If the surrounding workflow invokes the same agent many times, multiplicity should be expressed in the workflow diagram or connector labeling, not by changing the canvas definition.

---

## Agent Blueprint Canvas

An Agent Blueprint Canvas describes one agent.

The canvas contains nine sections:

1. Header
2. Incoming
3. Mission
4. Context View
5. Skills
6. Tools
7. Outgoing
8. Controls
9. Handoffs

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

## Incoming

Incoming defines which high-level communication acts the agent may receive through channels.

Typical incoming communication types are:

- **Query**: the agent may receive a request for information or judgment
- **Command**: the agent may receive a request to cause a change in the state of the world outside its sealed internal context
- **Event**: the agent may receive notice that a relevant fact or world-state change has happened

At the canvas level, these are semantic categories only.
Protocol details such as replies, acknowledgements, clarifications, acceptance, rejection, completion reporting, and similar exchange mechanics are outside the scope of the canvas.

All incoming messages affect the agent's internal session context because they become part of its working history.
The message types are not distinguished by internal context change.
They are distinguished by their semantic relation to the state of the world.

If an agent receives work through handoff, the receiving side is still interpreted through its normal Incoming capability.

Cardinality: **0..N**

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

Context View defines the projection of available session context that an agent reads.

Each agent conceptually reads context through exactly one Context View projection.

Context itself is not part of the Agent Blueprint Canvas because it is not owned by any single agent.
The source context depends on the agent's operating structure:

- for a solo agent, the source context is the context available to its implicit one-agent team
- for an agent inside a Context Boundary, the source context is the shared context available inside that boundary

Different agents may therefore read different projections of the same underlying context.

Typical examples of content visible through a Context View include:

- conversation summary
- research brief
- uploaded documents
- tool results
- intermediate artifacts
- notes produced by other agents

The Context View section is optional as a canvas artifact.

If the Context View section is omitted on the canvas, the default interpretation is the unfiltered default view over the full context available to that agent, regardless of whether the agent is solo or shares context with other agents.

A solo agent is therefore not an exception to the projection model. It is simply a one-agent team whose Context Boundary is not shared with any other agent.

Cardinality:

- conceptual projection per agent: **1**
- canvas section presence: **0..1**
- listed visible context elements when the section is present: **0..N**

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
- another agent invoked as an agent tool

Tool types:

- **Read**: query-only access, no side effects
- **Knowledge**: specialized read tool for retrieval from knowledge sources
- **Write**: mutates external state
- **Agent**: invokes another agent as an external capability

An **Agent** tool means invoke-and-return.
The calling agent remains in control, receives the result, and continues execution after the delegated work completes.
This is distinct from a handoff, where control transfers away from the current agent.

Knowledge is treated as a special tool type rather than a separate section.

Cardinality: **0..N**

---

## Outgoing

Outgoing defines which high-level communication acts the agent may initiate through channels.

Typical outgoing communication types are:

- **Query**: the agent may initiate requests for information or judgment from another participant
- **Command**: the agent may initiate requests for another participant to cause a change in the state of the world
- **Event**: the agent may publish that a relevant fact or world-state change has happened

Outgoing applies to channel-based communication only.
If an agent interacts with the external world purely through tools, the agent may have no Outgoing communication capability at all.

Tool invocation is not Outgoing communication.
If an agent asks another agent through an **Agent** tool, that interaction follows tool semantics rather than channel communication semantics.

Cardinality: **0..N**

---

## Controls

Controls are technical guardrails implemented outside the prompt.

They define which middleware or execution-boundary mechanisms must exist in the final solution.
Unlike prompt constraints, controls are intended to operate independently of the agent's reasoning and cannot be bypassed through prompt behavior alone.

Controls are represented as a list drawn from a fixed control set in v1.

The v1 control set is:

- **Input Scope**
- **Data Sensitivity**
- **Injection Defense**
- **Output Validation**
- **Audit Trail**

Each listed control means:

> this control category must be present in the implemented execution pipeline.

The canvas does not prescribe how the control is implemented.
That remains an engineering decision.

Cardinality: **0..N** from a fixed set

---

## Handoffs

Handoffs define which teammate agents may receive transferred control from the current agent.

A handoff is not communication.
It is an intra-boundary control transfer between agents inside the same Context Boundary.

The handoff section therefore describes routing options within a shared-context team.
It does not describe tool calls or cross-boundary interaction.

If an agent has no ability to transfer control to another teammate, the Handoffs section may be empty.

Cardinality: **0..N**

---

## Section Mapping

| Canvas Section | Synonym(s) | Primary Meaning | Typical Implementation Mapping | Cardinality |
|---|---|---|---|---|
| Header | identity | Agent identity | Name, role, mode metadata | 1 |
| Incoming | inbound, receives | Allowed incoming communication acts | Channel-facing query/command/event capability | 0..N |
| Mission | system prompt | Condensed operating intent | System prompt, base instruction, policy prompt | 1 |
| Context View | attention scope | Agent-visible projection of available context | Context projection, selected artifacts, retrieval scope | section 0..1; concept 1 |
| Skills | SOPs, prompt modules | Dynamic internal capability modules | `skill.md`, SOP prompt chunk, procedural prompt fragment | 0..N |
| Tools | MCP, API, function, delegation | External callable capabilities | MCP tool, function, API client, agent tool | 0..N |
| Outgoing | outbound, emits | Allowed outgoing communication acts | Channel-facing query/command/event capability | 0..N |
| Controls | guardrails, middleware | External guardrail requirements | List of required control categories | 0..N |
| Handoffs | control transfer | Allowed intra-team routing options | Intra-boundary control transfer targets | 0..N |

---

## Composition Rules

The canvas is intentionally minimal.

To preserve clarity:

- prompt-level behavioral constraints belong in **Mission** or **Skills**
- channel-based communication capability belongs in **Incoming** and **Outgoing**
- external capability declarations belong in **Tools**
- intra-team control transfer belongs in **Handoffs**
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
- channels may connect either to a specific agent or to a Context Boundary acting as a team-level intake surface
- when a channel connects to a Context Boundary, one enclosed agent may take responsibility based on communication capability and system-level coordination
- channels interact with an agent through its **Incoming** and **Outgoing** communication capability
- calls to external agents are modeled through **Agent** tools
- handoff between agents is represented through the sending agent's **Handoffs** capability when control moves from one agent to another inside the same Context Boundary
- the receiving agent is modeled through its normal **Incoming** capability rather than a special handoff input type
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

## Machine-Readable Schema

The canonical machine-readable structure for v1 is defined in:

- `schemas/agent-blueprint-canvas.schema.json`

The schema reflects this manifest.
It does not replace the manifest as the semantic authority.

---

## Example Structure

The following YAML is an illustrative example instance of the v1 canvas model:

```yaml
agent_blueprint_canvas:
  header:
    name: "ChatGPT"
    role: "General purpose AI assistant"
    mode: "delegated" # autonomous | delegated

  incoming:
    - type: "query" # query | command | event
      name: "UserQuery"

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

  outgoing: [] # query | command | event

  handoffs:
    - target: "Deep Research"
      name: "research_brief"

  controls:
    - "Input Scope"
    - "Data Sensitivity"
    - "Injection Defense"
    - "Output Validation"
    - "Audit Trail"
```

This example is provided for readability.
For validation and canonical machine-readable structure, use `schemas/agent-blueprint-canvas.schema.json`.

Shared context itself is not part of the Agent Blueprint Canvas structure.
Shared context is represented by the Context Boundary in the larger system view.
