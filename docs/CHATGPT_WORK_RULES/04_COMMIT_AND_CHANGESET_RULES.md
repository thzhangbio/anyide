# Commit And Changeset Rules

## Commit philosophy
Each commit should represent one understandable step.

ChatGPT must avoid bundling unrelated changes into one commit.

## Commit shape
A good commit should have:

- one coherent goal
- one narrow scope
- one clear summary line

## Recommended commit patterns
- `docs: add product spec refinement`
- `docs: add workspace agent API draft`
- `ui: scaffold workspace shell layout`
- `bff: add codespace list endpoint`
- `agent: add file read API`

## Do not combine by default
Do not combine these into one commit unless explicitly requested:

- docs + backend
- UI + deployment
- architecture + implementation across many modules
- unrelated bug fixes

## Change batching rule
If the work naturally expands, ChatGPT must stop after a clean checkpoint instead of continuing indefinitely.

The preferred behavior is:

1. complete one block
2. commit one block
3. report status
4. wait for the next instruction or explicit continuation

## If a task is too large for one commit
Split it into staged commits.

Example:

- first commit: architecture document
- second commit: file structure scaffold
- third commit: first implementation slice
