# Agent Blueprint Canvas Template

Use this template to define one Agent Blueprint Canvas.

## Header

- Name:
- Role:
- Mode: `autonomous | delegated`

## Inputs

- Type:
  Name:
  Notes:

## Mission

Write the agent's condensed operating intent.

## Context View

List the subset of shared context this agent reads.

-

## Skills

List reusable internal capability modules.

-

## Tools

List external callable capabilities.

- Type: `read | knowledge | write | agent`
  Name:
  Approval required: `true | false`
  Notes:

## Output

- Type: `text | structured | handoff`
  Name:
  Notes:

## Controls

- Input Scope:
- Data Sensitivity:
- Injection Defense:
- Output Validation:
- Audit Trail:

## Context Boundary Note

Do not describe the shared context itself inside the canvas.
If the agent shares context with other agents, show that in the larger system view with a Context Boundary.