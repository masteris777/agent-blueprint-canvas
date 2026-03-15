# Agent Blueprint Canvas Template

Use this template to define one Agent Blueprint Canvas.

## Header

- Name:
- Role:
- Mode: `autonomous | delegated`

## Incoming

- Type: `query | command | event`
  Name:

## Mission

Write the agent's condensed operating intent.

## Context View

Describe the projection of available context this agent reads.

If omitted, the default interpretation is the unfiltered default view over the full context available to the agent.

-

## Skills

List reusable internal capability modules.

-

## Tools

List external callable capabilities.

- Type: `read | knowledge | write | agent`
  Name:

## Outgoing

- Type: `query | command | event`
  Name:

## Controls

List the implemented control categories.

- Input Scope
- Data Sensitivity
- Injection Defense
- Output Validation
- Audit Trail

## Handoffs

- Target:
  Name:

## Context Boundary Note

Do not describe the shared context itself inside the canvas.
If the agent shares context with other agents, show that in the larger system view with a Context Boundary.

Channel communication and tool invocation are different concepts.
Use Incoming and Outgoing for channel-based Query / Command / Event capability.
Use Tools for external callable capabilities, including agent-as-tool delegation.
