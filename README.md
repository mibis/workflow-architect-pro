# Workflow Architect Pro

A zero-dependency, single-file browser application for building, visualizing, and exporting professional workflow diagrams.

![Version](https://img.shields.io/badge/version-2.9-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![No Dependencies](https://img.shields.io/badge/dependencies-none-brightgreen)
[![Live Demo](https://img.shields.io/badge/Live%20Demo-Open%20App-2563eb?style=flat&logo=github)](https://mibis.github.io/workflow-architect-pro/Workflow_Builder_v2_9_6.html)

---

## 🚀 Try it instantly — no install needed

**[▶ Open Live Demo](https://mibis.github.io/workflow-architect-pro/Workflow_Builder_v2_9_6.html)**

> Works directly in your browser. Nothing to install, no account required.

---

## Overview

Workflow Architect Pro is a self-contained HTML application that lets you design flowcharts and procedural workflows directly in the browser — no installation, no build step, no backend. Open the file, build your workflow, export it.

It uses [Mermaid.js](https://mermaid.js.org/) for diagram rendering (loaded via CDN) and stores state entirely in `localStorage`. Everything else is vanilla HTML, CSS, and JavaScript.

---

## Getting Started

```bash
git clone https://github.com/your-username/workflow-architect-pro.git
cd workflow-architect-pro
open Workflow_Builder_v2_9.html   # macOS
# or simply double-click the file in any OS
```

No npm install. No build process. Just open the file.

---

## Features

### Step Management

| Feature | Description |
|---|---|
| **Add Step** | Create new Action or Decision steps |
| **Duplicate Step** | Clone any step including all its fields (⧉ button on each card) |
| **Remove Step** | Delete a step with the ✕ button |
| **Drag & Drop Reorder** | Grab the ⠿ handle to reorder steps — drag is disabled when a card is expanded to prevent accidental moves |
| **Step Counter** | Live badge showing total steps and decision count |

### Accordion Step Cards

Each step lives in a collapsible card. Cards are closed by default, keeping the sidebar clean.

- **Collapse All / Expand All** toggle switch at the top of the step list
- Click any card header to open/close it individually
- Drag & drop only activates via the grip handle — expanded cards remain fully interactive

### Step Fields

Each step card contains:

| Field | Description |
|---|---|
| **Step Label** | The text displayed inside the diagram node |
| **Owner / Department** | Optional assignee (e.g. IT, HR, Finance) — drives swimlane grouping |
| **Type** | `Action` (rectangle) or `Decision` (rounded diamond) |
| **If Yes → Go to** | For Decision steps: defines the target on a positive outcome — next step, any other step, or Stop |
| **If No → Go to** | For Decision steps: defines the target on a negative outcome — any earlier step or Stop |
| **Intermediate Stop** | Marks a step as a terminal point — forward flow stops here, no STOP node is drawn |
| **Dual Start Label** | Assigns a second entry label to a step, enabling multi-origin workflows |
| **Extra Links** | Add unlimited additional outgoing arrows from any step, each with an optional label |
| **Color Label** | 49-color palette (Tailwind-based) — reflected on the card border, diagram node, and step counter dot |
| **Details / Notes** | Free-text notes included in the text log export |

### Branching & Flow Control

#### If Yes / If No Routing (Decision Steps)
Decision steps support fully independent routing for both outcomes:
- **If Yes** can point to the next step (default), any other step (forward or backward), or Stop
- **If No** can point to any earlier step or Stop

#### Extra Links (Multi-Branch from Any Step)
Any step — Action or Decision — can have additional outgoing connections via the **Extra Links** panel:
- Click **+ Add** to create a new link
- Choose a target step from the dropdown (any step in the workflow)
- Optionally label the arrow (e.g. "Escalate", "Notify")
- Remove any link with the ✕ button
- Useful for cross-branch connections, parallel notifications, or steps that feed into multiple downstream nodes

#### Intermediate Stop
Marking a step as an **Intermediate Stop** halts the automatic forward connection from that step. The step itself remains in the diagram as a normal node — no separate STOP shape is drawn. Use this when a branch should simply end at that point without continuing to the next sequential step.

#### Dual Start
Any step can be designated as a **second entry point** by providing a Dual Start label:
- When any Dual Start label is set, the main START node becomes invisible (layout anchor only)
- Each Dual Start step acts as an independent origin — its label is shown in the diagram as the entry node for that branch
- Multiple Dual Starts are supported simultaneously
- All Dual Start nodes and their arrows are invisible in the diagram; only the target steps and their connections are shown

### Swimlane Diagrams

When any step has an **Owner** value set, the diagram automatically switches from a flat flowchart to a **swimlane layout** — steps are grouped into Mermaid subgraphs by owner. Removing all owner values reverts to the standard layout.

Cross-subgraph connections (Extra Links or If Yes/No targets pointing across departments) are fully supported.

### Color System

- 49 colors across 15 Tailwind color families (Red, Orange, Amber, Yellow, Lime, Green, Teal, Cyan, Blue, Indigo, Violet, Purple, Pink, Rose, Slate)
- Each family has 3 shades: light / mid / dark
- Text color inside diagram nodes is **automatically calculated** using WCAG luminance (`0.299R + 0.587G + 0.114B`) — light backgrounds get dark text, dark backgrounds get white text

### Undo / Redo

- Full history stack (up to 50 snapshots)
- Keyboard shortcuts: `Ctrl+Z` (undo), `Ctrl+Y` or `Ctrl+Shift+Z` (redo)
- Snapshots are taken on every structural change: add, remove, duplicate, reorder, type/color/owner/routing change

### Auto-Save

- All changes are automatically persisted to `localStorage` 1.5 seconds after the last edit
- The indicator dot next to the version label turns **amber** while a save is pending, **green** when saved
- On page reload, the previous session is restored with a toast notification

### 12 Built-in Templates

Load a ready-made workflow with one click:

1. ECM Publication Workflow
2. Document Approval Process
3. IT Change Management
4. Incident Response
5. Employee Onboarding
6. Purchase Request
7. Software Release Process
8. Expense Approval
9. Recruitment Process
10. Customer Complaint Handling
11. Contract Review & Approval
12. Access Request (IAM)

### Validation Engine

Runs automatically after every diagram render. Highlights issues directly on the affected step cards and lists them below the step list.

| Severity | Condition |
|---|---|
| 🔴 Error | Empty step label |
| 🔴 Error | "If No" target points to the same step (infinite loop) |
| 🔴 Error | "If No" target index is out of range (step was deleted) |
| 🔴 Error | "If Yes" target points to the same step (infinite loop) |
| 🔴 Error | "If Yes" target index is out of range (step was deleted) |
| 🟡 Warning | Decision step's "If No" terminates — may be intentional |
| 🟡 Warning | Label exceeds 80 characters (may overflow in diagram) |

A summary badge (`⚠ 2 error(s)` / `⚠ 1 warning(s)`) appears next to the step counter.

### Zoom & Pan

- Zoom buttons (`−` / `+`) and a reset button (`⊙`) above the diagram
- Mouse wheel zoom over the diagram area
- Click and drag to pan
- Range: 30% – 300%
- Zoom state is correctly reapplied after every diagram render

### Export Options

| Format | Button | Notes |
|---|---|---|
| **Markdown** | Copy Decorated Markdown | Copies a formatted table to clipboard |
| **Plain Text** | Download Plain Text Log | `.txt` procedural step log with notes, routing, Dual Start and Intermediate Stop flags |
| **SVG** | Download SVG | High-resolution vector export |
| **PDF** | Download PDF | Generates a print-ready `.html` file with diagram image + step log; open in browser → `Ctrl+P` → Save as PDF |
| **JSON** | Save as JSON | Full workflow state including colors, owners, notes, routing, extra links |
| **JSON** | Load from JSON | Restore any previously saved workflow |

---

## Keyboard Shortcuts

| Shortcut | Action |
|---|---|
| `Ctrl+Z` | Undo |
| `Ctrl+Y` / `Ctrl+Shift+Z` | Redo |
| `Enter` (in Owner field) | Confirm and apply owner value |

---

## Project Structure

```
workflow-architect-pro/
└── Workflow_Builder_v2_9.html   # Entire application — single file
└── README.md
```

The entire application is contained in a single HTML file. There are no external assets, no JavaScript modules, and no CSS files. The only network dependency is the Mermaid.js CDN script loaded in `<head>`.

---

## Architecture Notes

### Why a single file?

The goal was maximum portability — the file should work when opened directly from a filesystem, sent as an email attachment, or hosted on any static server without configuration.

### State model

```
steps[]  ←→  renderSteps()  →  draw()  →  mermaid.run()  →  applyZoom()
   ↑                                                              |
localStorage ←─────────── scheduleAutoSave() ←──────────────────┘
   ↑
history[] (undo/redo stack)
```

Each user action updates the `steps` array, triggers a re-render of the sidebar cards and the Mermaid diagram, then schedules an auto-save. Zoom is always applied as the final step after Mermaid finishes rendering, ensuring correct SVG dimensions.

### Step Data Model

Each step object carries the following fields:

```json
{
  "text": "Step label",
  "type": "process | decision",
  "yesTarget": "next | stop | <stepIndex>",
  "failTarget": "stop | <stepIndex>",
  "isIntermediateStop": false,
  "dualStartLabel": "",
  "extraLinks": [
    { "target": "<stepIndex> | stop", "label": "" }
  ],
  "color": "#hexcolor | ''",
  "owner": "Department name | ''",
  "notes": "Free text"
}
```

### Diagram Generation

The `draw()` function translates the `steps` array into a Mermaid `graph TD` definition:

1. **START node** — rendered normally when no Dual Starts exist; hidden (invisible layout anchor) when any Dual Start is defined
2. **Dual Start nodes** — invisible in the diagram; only their target steps and connections appear
3. **Swimlane subgraphs** — generated per unique owner value when any step has an owner set
4. **Forward edges** — Action steps connect to the next step unless `isIntermediateStop` is true; Decision steps use YES/NO labels with configurable targets
5. **Extra Links** — emitted as additional edges after all standard edges, with optional labels
6. **Intermediate Stop** — suppresses the automatic forward edge; no STOP node is drawn
7. **STOP node** — a single shared `End` node is declared once and referenced by any step whose path terminates

### Drag & Drop

Drag is intentionally disabled by default (`draggable=false`) and only enabled via `mousedown` on the grip handle (⠿). This prevents accidental drags when interacting with inputs inside an expanded card.

### Swimlane Logic

When any step has a non-empty `owner` field, the diagram switches to `graph TD` with `subgraph` blocks grouped by unique owner values, preserving the order of first appearance. Extra Links and Yes/No routing targets work across subgraph boundaries.

---

## Browser Compatibility

Tested in Chrome, Firefox, Edge, and Safari (latest versions). Requires a browser with support for:
- CSS Custom Properties
- ES2020 (async/await, optional chaining)
- `localStorage`
- `FileReader` API (for JSON import)
- `Canvas` API (for PDF export)

---

## Changelog

### v2.9
- Extra Links panel on every step card — add unlimited additional outgoing arrows with optional labels
- Invisible layout anchors (`~~~`) for Dual Start and Dual Stop nodes to improve Mermaid positioning
- Responsive sidebar width using CSS `clamp()` — scales with viewport, chart area receives more space
- All UI strings translated to English

### v2.8
- Intermediate Stop — marks a step as a terminal branch point without drawing a STOP node or arrow
- Dual Start — designates any step as a second workflow entry point; START node hidden when active
- If Yes routing — Decision steps now support configurable Yes targets (next, any step, or Stop)
- If No target list expanded to all steps (previously only earlier steps)
- Validation extended to cover If Yes routing errors

### v2.7 / v2.6
- Dual Start initial implementation
- Intermediate Stop initial implementation
- isDualStop feature (subsequently removed in v2.9)

---

## License

MIT — free to use, modify, and distribute.
