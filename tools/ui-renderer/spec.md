# Agent Blueprint Canvas — UI Renderer

A client-side React application that renders Agent Blueprint Canvases from YAML input, allowing users to visualize multi-agent system architectures and export them as PNG images.

This document should describe the current production behavior and visuals closely enough that the same app can be reproduced later without relying on the original builder session or project history.

**Experience Qualities**:
1. **Immediate** - Live rendering with no submit buttons; see your canvas update as you type
2. **Precise** - Pixel-precise replication of the current Agent Blueprint Canvas renderer, including the visual proportions and pill treatment used in production
3. **Effortless** - One-click PNG export to clipboard for seamless integration with Excalidraw, Miro, or documentation

**Complexity Level**: Light Application (multiple features with basic state)
This is a focused utility with two main features (YAML parsing and canvas rendering) plus image export. No authentication, no backend, no multi-view navigation.

---

## Essential Features

### YAML Editor with Live Parsing
- **Functionality**: Monospace text editor that accepts YAML input describing agent blueprints
- **Purpose**: Provides a fast, keyboard-friendly way to define multiple agent architectures in a structured format
- **Trigger**: App loads with example YAML pre-filled
- **Progression**: User types or pastes YAML -> 300ms debounce -> parser validates -> valid data flows to renderer -> invalid shows error banner
- **Success criteria**: YAML changes update canvases within 300ms; invalid YAML shows clear error message without crashing

### Canvas Rendering Engine
- **Functionality**: Parses agent data and renders each as a visual canvas card with dark header, three-column body, controls band, and handoffs band
- **Purpose**: Transforms abstract YAML into a standardized visual representation for workshop and documentation use
- **Trigger**: Valid YAML is available (either initial load or after edit)
- **Progression**: Parser extracts agent array -> for each agent render CanvasHeader -> CanvasBody -> ControlsRow -> HandoffsRow -> stack vertically
- **Success criteria**: All agents render with correct layout proportions, semantic color coding, and exportable fidelity

### PNG Export to Clipboard
- **Functionality**: Captures any rendered canvas card as a PNG image and copies it to the system clipboard
- **Purpose**: Enables seamless pasting into Excalidraw, Miro, documentation, or presentations
- **Trigger**: User clicks `Copy as Image` on any canvas card
- **Progression**: Click button -> capture DOM element as PNG blob -> write to clipboard -> show `Copied!` feedback for 2s -> revert button text
- **Success criteria**: PNG appears in clipboard and can be pasted into external tools; button provides clear feedback

---

## Edge Case Handling

- **Empty YAML** - Show helpful placeholder message in right panel: `Enter YAML to see your canvas`
- **Malformed YAML** - Display error banner below editor with specific parse error; preserve last valid render
- **Missing Required Fields** - Validate agent structure; show which agent and which field is missing
- **Invalid Enum Values** - For mode/types, show validation error listing valid options
- **Very Long Text** - Mission text wraps naturally; context view list wraps; canvas height expands vertically when needed
- **Empty Sections** - Show section label only; no `empty` placeholder text
- **Clipboard API Unavailable** - Fall back to downloading PNG file named from the agent title
- **Multiple Agents** - Stack vertically with 48px gaps; each independently exportable

---

## Design Direction

The design should feel **technical, precise, and architectural** — like developer tooling meets system design documentation.

The app chrome should be minimal. The rendered canvases are the hero artifact.

Compared to the earlier Excalidraw-derived spec, the production renderer is slightly softer and more productized:
- larger corner radii
- filled icon badge in the header
- pills used for controls and handoffs
- lighter section treatment with stronger semantic color coding

---

## Current Production Visual Reference

The current production renderer has these visible characteristics and they should be treated as authoritative:

### Overall Card
- Wide landscape card, approximately 1120px content width
- Rounded outer corners, visually around 16px
- Dark navy header band
- Very light body background
- Thin, cool-gray dividers between columns and rows
- No decorative shadows required, but a subtle shadow is acceptable

### Header
- Dark background close to `#060b1f`
- Left side contains a rounded square badge with a single initial (`C`, `D`, etc.)
- Badge uses a blue to violet accent fill
- Agent title is large, bold, white
- Role subtitle is smaller, muted, and light gray-blue
- Mode label is right-aligned in white (`Delegated` / `Autonomous`)

### Body Grid
- Three columns: Incoming/Outgoing, Mission/Context View/Skills, Tools
- Section labels are uppercase, muted slate-blue, bold, and compact
- Content cards use rounded rectangles with thin semantic borders and very light semantic fills

### Semantic Pills in Current Production
- `incoming.query` currently renders as a mint/green-tinted pill
- `incoming.command` currently renders as a pale blue-tinted pill
- `outgoing.event` currently renders as a pale orange-tinted pill
- `tools.read` currently renders as mint/green-tinted pills
- `tools.write` currently renders as pale blue-tinted pills
- `tools.knowledge` currently renders as mint/green-tinted pills in the current build
- `tools.agent` currently renders as pale red/pink-tinted pills
- `skills` currently render as mint/green-tinted pills
- `controls` render as lavender-outline pills arranged in a grid
- `handoffs` render as pale yellow pills with amber borders

If reproducing the current app exactly, preserve those mappings even if they differ from earlier design intent.

---

## Color Selection

This application uses a slate-based architectural palette with semantic color coding for sections and items.

- **Primary Color**: `oklch(0.108 0.012 256)` (`#0f172a`) — structural framing and deep text
- **Secondary Colors**:
  - `oklch(0.986 0.003 256)` (`#f8fafc`) — body backgrounds
  - `oklch(0.426 0.016 250)` (`#64748b`) — section labels
- **Accent Color**: `oklch(0.479 0.131 142)` (`#16a34a`) — success/active accents where needed

### Production-Oriented Semantic Pairings

- **Mint / Green tint**
  - Background: `#eefcf3` to `#f0fdf4`
  - Border: `#bbf7d0`
  - Use for: current query pills, read tools, knowledge tools, skills

- **Pale Blue tint**
  - Background: `#eff6ff`
  - Border: `#bfdbfe`
  - Use for: current command pills, write tools

- **Pale Orange tint**
  - Background: `#fff7ed`
  - Border: `#fdba74`
  - Use for: current event pills

- **Pale Pink / Red tint**
  - Background: `#fef2f2`
  - Border: `#fecaca`
  - Use for: current agent tools

- **Lavender tint**
  - Background: `#f8f7ff`
  - Border: `#d8b4fe`
  - Use for: controls pills

- **Pale Yellow tint**
  - Background: `#fffbeb`
  - Border: `#fbbf24`
  - Use for: handoffs pills

---

## Font Selection

**JetBrains Mono** for YAML editor and code-like pill text.
**Inter** for titles, labels, subtitles, and mission prose.

### Typographic Hierarchy
- Agent Name: Inter Bold / 22px / tight tracking
- Role: Inter Regular / 14px / muted slate
- Mode: Inter Semibold / 14px / white
- Section Labels: Inter Bold / 13px / uppercase / light tracking
- Pill Text: JetBrains Mono Regular / 13px
- Mission Text: Inter Regular / 13px / 1.5 line height
- Context View list text: Inter Regular / 13px

---

## Layout and Spacing

### App Layout
- Two-panel workspace
- Left: YAML editor panel
- Right: canvas preview panel
- Responsive split on desktop; stacked vertically below ~1024px is acceptable

### Canvas Dimensions
- Base target width: 1120px
- Scale down proportionally on smaller screens, but preserve proportions
- Height is content-driven, not fixed

### Vertical Structure
1. Header band
2. Body grid
3. Controls band
4. Handoffs band

### Spacing Rules
- Canvas stack gap: 48px
- Section gap inside columns: 24px
- Pill gap: 12px
- Internal padding: 28px to 32px
- Body columns separated by thin vertical rules

### Controls Layout
Controls are no longer inline text.
They render as individual pills in a responsive multi-column grid.
In the current production screenshots, 5 controls appear as:
- row 1: 3 pills
- row 2: 2 pills

### Handoffs Layout
Handoffs render as one or more horizontal pills in a row.
If multiple handoffs exist, use a wrapping grid or horizontal row with gaps.

---

## Current Canvas Structure

Each agent canvas should render these sections in this order:

1. Header
2. Incoming
3. Outgoing
4. Mission
5. Context View
6. Skills
7. Tools
8. Controls
9. Handoffs

The body is still conceptually three columns:
- Left column: Incoming + Outgoing
- Center column: Mission + Context View + Skills
- Right column: Tools

---

## YAML Editor Behavior

### Editor
- Monospace textarea is sufficient
- No code editor dependency is required
- Pre-load with example YAML
- 300ms debounce before parsing
- Preserve last valid rendered state when YAML becomes invalid

### Validation
- `agents` must be an array with at least one item
- Each agent must have `header.name`, `header.role`, `header.mode`, and `mission`
- All list sections default to empty arrays if omitted
- `mode` must be `autonomous` or `delegated`
- `incoming` / `outgoing` item `type` must be `query`, `command`, or `event`
- `tools` item `type` must be `read`, `knowledge`, `write`, or `agent`
- `controls` must come from the fixed v1 set
- `handoffs` items must have `target`; `name` is optional

---

## YAML Input Format

The app accepts an `agents` array whose items follow the Agent Blueprint Canvas structure.

```yaml
agents:
  - header:
      name: "Chat Agent"
      role: "General-purpose conversational agent"
      mode: "delegated"
    incoming:
      - type: "query"
        name: "UserQuery"
    outgoing: []
    mission: >
      Accept user queries, answer directly when possible, use search
      and image generation when useful, and delegate long-form
      investigation to Deep Research through an agent tool.
    context_view:
      - "current conversation"
      - "uploaded files"
      - "tool results"
    skills:
      - "research"
      - "design images"
    tools:
      - type: "read"
        name: "internet search"
      - type: "write"
        name: "create image"
      - type: "knowledge"
        name: "historical conversations"
      - type: "agent"
        name: "deep research"
    controls:
      - "Input Scope"
      - "Data Sensitivity"
      - "Injection Defense"
      - "Output Validation"
      - "Audit Trail"
    handoffs:
      - target: "Human Expert"
        name: "escalate complex query"
      - target: "Supervisor Agent"
        name: "request approval"

  - header:
      name: "Deep Research"
      role: "Long-form research agent"
      mode: "autonomous"
    incoming:
      - type: "command"
        name: "DeepResearchCommand"
    outgoing:
      - type: "event"
        name: "ReportCreated"
    mission: >
      Receive a deep research request, plan the investigation,
      gather and synthesize sources, and return a structured
      research result to the calling agent.
    context_view: []
    skills:
      - "plan"
      - "execute"
      - "check facts"
      - "write report"
    tools:
      - type: "read"
        name: "web search"
      - type: "read"
        name: "document reader"
      - type: "write"
        name: "create report"
    controls:
      - "Input Scope"
      - "Data Sensitivity"
      - "Injection Defense"
      - "Output Validation"
      - "Audit Trail"
    handoffs: []
```

---

## Copy as Image

Each rendered agent canvas has a `Copy as Image` control.

### Implementation
- Use `html-to-image` as the preferred library
- Capture the individual canvas DOM node
- Convert to PNG blob
- Write to clipboard using the Clipboard API
- Show temporary `Copied!` feedback for 2 seconds

### Fallback
- If clipboard write is unavailable, download a PNG file named from the agent title

---

## Tech Stack

- React (functional components + hooks)
- `js-yaml` for parsing
- `html-to-image` for PNG export
- Tailwind or plain CSS are both acceptable; use whichever best reproduces the current production visuals
- No backend
- No persistence
- No routing required

---

## Component Structure

```text
App
├── YamlEditorPanel
├── PreviewPanel
│   └── AgentCanvas[]
│       ├── CanvasHeader
│       ├── LeftColumn
│       ├── CenterColumn
│       ├── RightColumn
│       ├── ControlsGrid
│       ├── HandoffsRow
│       └── CopyImageButton
└── ErrorBanner
```

---

## Behavior Summary

1. App loads with example YAML
2. Valid YAML renders canvases immediately
3. User edits YAML -> re-render after ~300ms
4. Invalid YAML -> error banner, last valid preview remains visible
5. User clicks `Copy as Image` -> current canvas copied to clipboard
6. Mobile or narrow widths scale the canvas down proportionally

---

## Out of Scope for This Version

- Context Boundary grouping around multiple canvases
- Channel connector lines between canvases
- Shared system view layout tools
- Multi-user collaboration
- Persistence of YAML between sessions
- Server-side storage
- Authentication
- Full schema-driven editor UI

---

## Reproduction Rule

If there is any tension between this document and the older Excalidraw-derived spec, prefer this document and the current production visuals.

The goal is to reproduce the current working app, not the earlier pre-build concept.