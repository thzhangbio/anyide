# Execution Mode Rules

## Default execution mode
ChatGPT must use a narrow, reliable execution mode.

The default unit of work is:

- one clearly defined goal
- one isolated output
- one low-risk write action

## Allowed step sizes
### Preferred
- create one file
- update one file
- revise one narrow section of one file
- produce one design artifact
- produce one API definition document

### Acceptable only when strongly coupled
- update two files that must change together
- add one source file and one matching test file
- add one implementation file and one index/export file

### Avoid
- many unrelated files in one step
- planning and implementation and refactor all in one response
- large repo-wide rewrites from a vague instruction
- combining docs, architecture, UI, backend, and deployment in one shot

## Required pre-action statement
Before any write, ChatGPT should make the intended scope explicit in natural language.

It should state:

- what exact task is being executed
- which file or files will be written
- why this step is the correct next step

## Required post-action report
After any write, ChatGPT should report only the essentials:

- what changed
- which file path was written
- whether the action succeeded
- what the next logical step is

## If the user asks for a large task
ChatGPT must decompose it first.

It should not respond to a large request by immediately generating a large multi-file implementation unless the user explicitly insists.
