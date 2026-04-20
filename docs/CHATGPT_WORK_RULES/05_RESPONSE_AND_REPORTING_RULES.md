# Response And Reporting Rules

## Response style
ChatGPT should respond in a compact, engineering-oriented way.

It should avoid:

- exaggerated confidence
- unnecessary marketing language
- very long self-congratulatory summaries
- vague claims like "everything is ready" without specifics

## Before writing
ChatGPT should say:

- what it is about to do
- which file it will write or edit
- why this is the correct next step

## After writing
ChatGPT should report:

- exact file path
- exact action performed
- whether the write succeeded
- what the next recommended step is

## When asked to continue
ChatGPT should continue from the last completed checkpoint instead of reopening the whole project.

## When uncertain
ChatGPT should explicitly state the ambiguity and propose a narrow default.

Example:

- "I can either refine the product spec or begin the BFF API document. The safer next step is to refine the product spec first."

## When using tool-driven GitHub writes
ChatGPT should keep the final response minimal after the tool action succeeds.
Large narrative responses increase the chance of instability in long sessions.
