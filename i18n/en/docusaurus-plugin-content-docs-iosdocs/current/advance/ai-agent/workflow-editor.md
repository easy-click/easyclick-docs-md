---
title: Workflow editor
description: Create, edit, and trial-run automation workflows
sidebar_label: Workflow editor
keywords:
 - workflow
 - visual editor
 - trial run
 - variable pool
---

# Workflow editor

:::tip Confirm environment before trial run

Before **Run** or **Trial run**, complete **USB device license** and the matching **automation / Bluetooth environment** in [Prerequisites & licensing](./prerequisites). Otherwise common steps fail immediately.

:::

A **workflow** saves steps like “tap here, wait, open which App” as a **repeatable** flow. Click **"Workflow editor"** at the top to enter the editor.

## Layout

- **Left**: workflow library (tree — main flows and subflows)
- **Center top**: open flow tabs (multiple at once)
- **Center bottom**: **visual flowchart** or **text mode** (advanced)
- **Right**: selected step parameters, **variable pool**, AI assist panel

## Workflow library (left)

- **Root**: standalone main flows, runnable directly
- **Child**: subflows invoked by main flows, shown indented
- Click name to **open for edit**; hover for **Open folder**, **Delete**
- Left panel can **collapse** for a larger canvas
- **Root** flows can **export as.spk package** (see [Workflow.spk](./workflow-spk)); **"密"** marks imported read-only packages

## Create and save

| Action | How |
|------|--------|
| **New** | Top bar **New**, enter name |
| **Save** | **Save** button or **Ctrl+S** (Mac: **Cmd+S**) |
| **Validate** | **Validate** before save — missing fields, bad connections |
| **Layout** | Auto-arrange flowchart |

Enable **Auto-save on tab switch** in **Settings**.

Step cards have **Simple / Advanced** toggle top-right: Simple hides JSON and advanced fields; Advanced shows full config.

## Two editing modes

| Mode | Best for |
|------|--------|
| **Visual** | Most users: drag nodes, connect, click steps to edit params |
| **Text** | Users familiar with config files: edit JSON directly |

Start with **Visual**.

## Flowchart elements

- **Boxes (states)**: groups of steps run in order
- **Arrows**: on success go to next step; also **failure** and **condition** branches
- **Diamonds**: branch on conditions (e.g. “tap if found, else wait”)
- **Subflow**: one step invokes another saved flow (**invoke**)

Canvas supports **pan, zoom**, **minimap**; **Ctrl+Z** (Mac: **Cmd+Z**) undo; right-click copy, delete, **insert step** on a connection.

After trial run, nodes **highlight green/red/blue**; select one to see logs and I/O.

## Variables: flow config and variable pool

Two kinds of data to remember:

| Type | Meaning | Where to configure |
|------|------|--------|
| **Flow config** | Defaults before the flow starts (package name, keywords, etc.) | **Flow config** dialog |
| **Step results** | Data left after a step (screenshot, OCR, match result, etc.) | **Save to variable** on the step |

**Variable pool** (canvas toolbar) shows both:

- **Design** view: infers config and step results from flow structure
- **Run** view: after trial run, shows **actual variable values**
- Click a variable to **highlight** steps that reference it
- Optional **Trial run variable flow** — data lines between nodes visited this run only

When **invoking subflows**, use the **checklist** to pick pass-through / return variables — no hand-written JSON (Advanced mode still allows editing).

## Trial run (important)

After editing, **trial run** before production:

1. Pick an **online device** in the toolbar
2. Click **Trial run** / **Run** (or **Trial from this step** / **Run selected steps**)
3. Steps **change color** on canvas: running, success, failure
4. **Force stop** anytime

Trial runs create real tasks — see [Tasks & monitoring](./tasks). From the task panel, **"Workflow editor"** returns to the editor with **playback** of each step.

## AI-assisted flow authoring

Right **AI panel** (configure LLM first — [Getting started](./getting-started)) can:

- Describe automation in Chinese and let AI **generate the flowchart**
- Check **Edit selected state only** for **partial rewrites**
- Multi-turn chat until satisfied

After generation, still **validate** and **trial run**.

## Flow config (global settings)

Click **Flow config** to set:

| Item | Purpose |
|----|------|
| **Workflow default / This flow startup** | Two config layers — override rules for trial run vs. run |
| **Input params (trial run)** | Key-value overrides for trial run |
| **Working directory** | **Optional**. When set, file steps and **match templates** prefer this folder; create `assets/templates/` for small images |

**Working directory** differs from **Album import** `folderPath`: album steps need an absolute PC path; `save_file` / `read_file` without working directory write under `runtime_data/` in the data directory — **no** working directory required first.

## Step previews while editing

With a trial device selected, some steps support **preview**:

| Step type | Preview |
|----------|----------|
| **OCR** | Capture and recognize text (default non-automation capture) |
| **Image match / color-offset match** | **Match preview**: match box, search region, crop template — see [Image/color matching](./image-match) |
| **Node capture** | Clickable elements on current screen (automation required) |
| **VLM visual locate** | Mark location on screenshot from Chinese description |
| **Coordinate tap** | Pick point on screenshot |
| **pick data extraction** | Preview from **last trial run** I/O snapshot |

**Screenshot** and **OCR** are separate **non-automation** and **automation** steps — same names as [AI chat](./ai-chat#screenshots-and-ocr).

## Sub-workflows

- Add **Invoke subflow** step, pick existing workflow ID
- **Double-click** invoke node or right-click **Enter subflow** to edit (subgraph not expanded on canvas by default)
- Configure **Input params**, **Write back variables**, **Success record (optional)**

## Open flow file location

**Folder icon** next to a library item opens that flow's **file directory** on your PC for backup or copy.

## Next steps

- Image/color matching details → [Image/color matching](./image-match)
- Step parameter reference → [Workflow step reference](./workflow-steps)
- Execution progress and pause → [Tasks & monitoring](./tasks)
