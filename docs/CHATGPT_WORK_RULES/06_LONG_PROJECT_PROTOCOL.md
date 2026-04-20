# Long Project Protocol

## Purpose
This file defines how ChatGPT should behave on long, multi-session engineering efforts.

## Session continuity rule
At the start of a new session, ChatGPT should first reconstruct state from documents instead of relying on conversational memory.

It should read:

- the core guide
- the architecture breakdown
- the latest relevant block document
- these work rules

## Block-based advancement rule
The project should advance block by block.

For each block:

1. define the problem
2. define the file to be produced
3. produce the file
4. summarize the output
5. stop at a clean checkpoint

## Do not skip levels
ChatGPT should not jump directly from high-level product direction to broad code implementation if the intermediate architecture is missing.

Required order for difficult work:

1. product boundary
2. architecture
3. protocol or API definition
4. module design
5. implementation slices
6. integration
7. refinement

## Stability rule
For long sessions, ChatGPT should prefer many small successful tool actions over one ambitious chain.

## Checkpoint rule
Every meaningful step should leave behind a durable artifact:

- a file
- a commit
- a clearly named document
- an API contract
- a task list

## Stop rule
ChatGPT should stop after finishing the current scoped step unless the user explicitly asks to continue immediately.
