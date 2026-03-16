# Agent Blueprint Canvas

_Reveal the forest behind the trees._

**Try it:** [GitHub Spark](https://agent-blueprint-canv--masteris777.github.app/) · [Firebase](https://studio-3006097909-4815a.web.app/)

The Agent Blueprint is a visual design canvas for defining AI agents before implementation.

It is meant for high-level, workshop-friendly design work: a shared artifact that business stakeholders, architects, and engineers can use to align on what an agent is, what it can do, what it must not do, and how multiple agents share context.

The canonical specification lives in [manifest.md](manifest.md).

## Why This Exists

Agent design conversations break down because too many concerns must be held in working memory at once: incoming communication, outgoing communication, tools, skills, controls, and shared context boundaries.

The Agent Blueprint externalizes that discussion into a structured visual form.

It is not a deployment spec and it is not a prompt template.
It is the design artifact that sits before detailed technical design.

## What It Does Not Cover

The Agent Blueprint Canvas is a pre-implementation design artifact.
It is not a substitute for downstream engineering artifacts.

It does not define:

1. Detailed technical design, component interactions, or runtime architecture.
2. Interface and protocol contracts such as MCP, A2A, REST, events, or transport bindings.
3. Memory implementation details such as storage, retrieval, retention, synchronization, or lifecycle rules.
4. Knowledge ingestion pipelines, database synchronization, indexing, refresh jobs, or ETL-style content preparation.
5. Evaluation design such as metrics, benchmarks, test harnesses, or acceptance thresholds.
6. Observability design such as logs, traces, metrics, alerts, or audit pipeline implementation.
7. Runtime hooks, middleware wiring, extension points, or execution lifecycle callbacks.
8. Workflow orchestration notation such as BPMN, gateways, loops, retries, approvals, or escalation semantics.

Those concerns belong to technical design, workflow design, and implementation artifacts that follow the canvas.

## Canvas Structure

The v1 Agent Blueprint Canvas contains nine sections:

1. Header
2. Incoming
3. Mission
4. Context View
5. Skills
6. Tools
7. Outgoing
8. Controls
9. Handoffs

Shared context itself is not part of the canvas.
Shared context is represented by a Context Boundary in the surrounding system view.

## Visual Examples

### Personal Watchtower

Independent watcher agents with no shared context. The email watcher emits breaking-news style events that the chat watcher can react to. This example demonstrates event choreography between autonomous specialists.

![Personal watchtower example](assets/personal-watchtower.png)

Source: [examples/personal-watchtower.excalidraw](examples/personal-watchtower.excalidraw)

### Chat Agent And Deep Research

A delegated chat agent that can answer directly, use read and write tools, and invoke a deep research agent through an agent tool. This example demonstrates tool-based delegation without a shared context boundary or event channel.

![Chat agent and deep research example](assets/chat-agent.png)

Source: [examples/chat-agent.excalidraw](examples/chat-agent.excalidraw)

### Developer Bot

Three delegated agents arranged as a shared-context team: Planner, Executor, and Tester. Work enters through Planner, moves internally by handoff, and only exits through Tester after verification. The exported image is tall, so it may not fit comfortably on one screen at full size; open the source canvas to inspect the full vertical flow and the handoff loop.

![Developer bot example](assets/developer-bot.png)

Source: [examples/developer-bot.excalidraw](examples/developer-bot.excalidraw)

## Repository Contents

- [manifest.md](manifest.md): canonical v1 specification
- [templates/agent-blueprint-canvas.md](templates/agent-blueprint-canvas.md): text-first fill-in template
- [templates/agent-blueprint-library.excalidrawlib](templates/agent-blueprint-library.excalidrawlib): reusable Excalidraw component library
- [tools/ui-renderer/spec.md](tools/ui-renderer/spec.md): builder-neutral spec for a YAML-to-canvas UI renderer
- [tools/ui-renderer/README.md](tools/ui-renderer/README.md): live app links and regeneration instructions
- [examples/personal-watchtower.excalidraw](examples/personal-watchtower.excalidraw): autonomous watcher network with event choreography
- [examples/chat-agent.excalidraw](examples/chat-agent.excalidraw): delegated chat agent with deep research tool delegation
- [examples/developer-bot.excalidraw](examples/developer-bot.excalidraw): shared-context Planner, Executor, Tester team
- [assets/](assets): exported PNG previews of the three canonical examples

## How To Use It

1. Define the Agent Blueprint Canvas using the section structure in [manifest.md](manifest.md).
2. Place one or more canvases inside a larger system view.
3. Use a dashed Context Boundary to show which agents share the same session context.
4. Use Context View on each canvas when you need to show the agent's projection of available context. If omitted, the default interpretation is the full available context for that agent.
5. Use BPMN or similar workflow notation if you need explicit sequencing, branching, retries, approvals, or escalation logic.
6. Treat the result as the handoff artifact for detailed technical design and workflow design.

## License

Created by Marijus Masteika.
Licensed under CC BY 4.0. See [LICENSE](LICENSE).