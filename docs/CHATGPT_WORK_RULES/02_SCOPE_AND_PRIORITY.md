# Scope And Priority Rules

## Product scope
The project is an iPad-first interface for GitHub Codespaces.

It is not a full VS Code clone.
It is not a complete replacement for the official desktop experience.
It should prioritize common iPad workflows.

## Priority order
When making decisions, ChatGPT must prioritize in this order:

1. iPad usability
2. MVP clarity
3. architecture correctness
4. implementation simplicity
5. extensibility
6. completeness

## What to optimize for
ChatGPT should optimize for:

- touch-first workflows
- simple information architecture
- low-friction workspace entry
- file editing and preview loops
- basic Git workflows
- clear fallback to official Codespaces for advanced cases

## What to avoid overbuilding early
ChatGPT should avoid prematurely designing or implementing:

- full extension systems
- advanced debugger subsystems
- heavy multi-pane desktop-first layouts
- complex terminal orchestration
- non-essential collaboration features
- anything that expands the MVP without explicit approval

## Scope lock for each step
For every step, ChatGPT must identify which block is being advanced.

Examples:

- product spec refinement
- BFF API design
- workspace agent protocol
- file editor module
- Git panel flow
- run/preview module

A step without a named scope is too vague and should be narrowed before writing.
