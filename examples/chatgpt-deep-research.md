# Worked Example: ChatGPT + Deep Research

This example shows two agents operating inside the same Context Boundary.

Shared context is environment-level, not canvas-level.
Both agents can access the same session context, but each agent reads only the subset described in its Context View.

## Shared Context Boundary

Shared context may include:

- conversation history
- uploaded files
- research brief
- tool results
- notes and summaries
- artifacts produced during the session

## Agent Blueprint Canvas: ChatGPT

### Header

- Name: ChatGPT
- Role: General purpose AI assistant
- Mode: delegated

### Inputs

- Type: query
  Name: user_question

### Mission

Help the user understand, create, transform, and organize information, act when needed, reason over uploaded context, and hand over specialist work when the request calls for it.

### Context View

- current conversation
- uploaded files
- tool results
- artifacts produced during the session

### Skills

- summarize
- explain
- plan
- write

### Tools

- Type: read
  Name: web_search
  Approval required: false
- Type: knowledge
  Name: uploaded_files
  Approval required: false
- Type: write
  Name: canvas_edit
  Approval required: false

### Output

- Type: text
  Name: response
- Type: structured
  Name: research_brief

### Controls

- Input Scope: enabled
- Data Sensitivity: enabled
- Injection Defense: enabled
- Output Validation: enabled
- Audit Trail: enabled

## Agent Blueprint Canvas: Deep Research

### Header

- Name: Deep Research
- Role: Multi-step research and report agent
- Mode: delegated

### Inputs

- Type: handoff
  Name: research_brief

### Mission

Take a research brief, plan the investigation, gather and synthesize sources, and return a clear research report for the user.

### Context View

- conversation summary
- research brief

### Skills

- plan
- write_report
- fact_checking

### Tools

- Type: read
  Name: web_search
  Approval required: false
- Type: read
  Name: pdf_reader
  Approval required: false

### Output

- Type: text
  Name: report

### Controls

- Input Scope: enabled
- Data Sensitivity: enabled
- Injection Defense: enabled
- Output Validation: enabled
- Audit Trail: enabled

## Interpretation

The two canvases do not duplicate the shared context definition.

The shared context exists at the boundary level.
ChatGPT contributes a research brief into that context.
Deep Research reads that brief through its Context View and continues the work without requiring a separate context transfer artifact.