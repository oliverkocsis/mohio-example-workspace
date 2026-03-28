# Mohio Architecture v2

`ARCHITECTURE.md` is the living engineering and technical-direction document for Mohio.

Update this document whenever the plat form direction, major stack choices, document model, editor architecture, storage model, or other engineering constraints change.

## Lorem Ipsum

> Hellow World

Hello World

What if I do this?

1.  HEllo
2.  What haldv

When efd f

1.  I

> \## Whaslfmadpf

### asfasf

1.  Hello `***~World~***`

What id I do this. then edit here. 

## Purpose

Mohio is being devaeloped as a local-first, document-centred knowledge workspace. The architecture should sudpport a polished editing experience for non-technical users while preserving plain Markdown as the durable source format.

This document captures the current technical direction. It is not a promise that every prototype already implements everything described here.

## Current State

1.  The repository is in active development.
2.  Multiple prototype directories exist to explore layout, editor, and desktop-shell decisions.
3.  Current prototypes are used to validate product and implementation direction rather than represent a finished application architecture.
4.  A new `desktop/` Electron application now exists as the starting point for the real product codebase.
5.  The desktop app now includes local workspace selection and Markdown file-tree enumeration through the Electron main process, while document content loading and AI workflows are still ahead.

## Architectural Goals

1.  Keep content stored as portable Markdown files.
2.  Keep the default workflow local-first.
3.  Hide Git complexity from everyday users.
4.  Support bring-your-own AI workflows, including Codex and Claude Code, without giving AI silent write access to shared knowledge.
5.  Separate private editing, reviewable changes, checkpoints, and publishing.
6.  Build a desktop-first foundation before expanding companion mobile workflows.

## Platform Direction

### Desktop

1.  Current primary platform direction: `Electron`
2.  Goal: polished desktop workspace with strong local file-system integration

### Mobile

1.  Planned companion platform: `Flutter`
2.  Goal: support capture, reading, and lightweight review workflows on smaller screens

## Application Model

Mohio is organized around a workspace containing Markdown documents and related product state.

### Workspace

1.  A workspace maps to a local folder.
2.  Markdown files in the workspace are treated as documents.
3.  The product should make workspace context obvious without exposing raw filesystem complexity unnecessarily.

### Documents

1.  Markdown is the durable source of truth.
2.  Documents should remain portable outside Mohio.
3.  Internal links should use relative Markdown linking where practical.

### Editing

1.  Default editing mode: formatted `Quill`\-based WYSIWYG editor
2.  Source editing mode: `CodeMirror`\-based Markdown editor
3.  Future code editing workflows for AI-generated apps and tools: `CodeMirror`

### History and Publishing

1.  Editing should persist continuously.
2.  Named checkpoints capture meaningful document states.
3.  Publishing remains explicit and separate from editing.
4.  Shared changes should become increasingly reviewable as the product matures.

## Tech Stack Direction

The current repo direction and prototype work imply the following stack choices.

### Product Stack

1.  Desktop shell: `Electron`
2.  Desktop renderer foundation: `React` + `TypeScript` + `Vite`
3.  AI integration direction: bring-your-own assistants with Codex and Claude Code support
4.  Mobile app: `Flutter`
5.  Rich text editor: `Quill`
6.  Markdown source editor: `CodeMirror`
7.  Durable content format: `Markdown`
8.  Workspace storage model: local files in a Git-backed workspace

### Prototype Stack

Several prototypes use:

1.  `React`
2.  `TypeScript`
3.  `Vite`
4.  `Vitest`

These support fast iteration for interface and workflow exploration. Prototype stack choices should not automatically override product-level architecture decisions.

## System Areas

### 1\. Desktop Shell

Responsibilities:

1.  native desktop windowing
2.  local workspace selection
3.  filesystem integration
4.  application lifecycle and packaging
5.  preload bridge for controlled renderer access to native capabilities

Current implementation status:

1.  single-window Electron shell
2.  secure preload bridge exposing app info plus workspace-selection and workspace-loading actions
3.  renderer shell with a real workspace selector and Markdown file tree in the left navigation

### 2\. Workspace and File Layer

Responsibilities:

1.  enumerate Markdown files
2.  create, rename, and delete documents
3.  read and write document contents
4.  resolve internal links
5.  power filename and content search

### 3\. Document Model

Responsibilities:

1.  represent editable note state
2.  preserve Markdown fidelity
3.  support formatted editing without losing durable source compatibility
4.  support checkpoint metadata and publish metadata

### 4\. Editor Layer

Responsibilities:

1.  provide polished default writing UX
2.  support rich formatting operations
3.  allow advanced source editing when needed
4.  preserve readable editing at typical document lengths

### 5\. AI Assistance Layer

Responsibilities:

1.  embedded interaction layer for connected assistants such as Codex and Claude Code
2.  document summarization, rewriting, organization, and expansion
3.  controlled creation or modification of workspace documents
4.  explicit, reviewable changes rather than opaque mutation

### 6\. Publish and Review Layer

Responsibilities:

1.  distinguish local draft work from shared published state
2.  create and restore checkpoints
3.  support clearer review and change awareness over time

## Engineering Principles

1.  Prefer local-first behavior by default.
2.  Preserve user ownership of content.
3.  Keep file formats inspectable and portable.
4.  Make important changes reviewable.
5.  Avoid requiring users to understand Git internals.
6.  Keep implementation complexity behind a simple product surface.

## Open Technical Questions

1.  How should formatted editing map cleanly back to durable Markdown in all supported cases?
2.  How should checkpoints be stored and restored at the file level?
3.  What is the right local architecture for Codex- and Claude Code-driven edits and reviewable diffs?
4.  Which parts of publishing should be represented in files versus app metadata?
5.  How should search scale from simple local filtering to richer workspace discovery?

## Maintenance Rules

1.  Update this file in the same PR as any major engineering decision change.
2.  Keep user-facing scope in `docs/FEATURES.md` rather than duplicating it here.
3.  Keep system-wide design language in `docs/DESIGN.md` rather than turning this into a visual spec.
