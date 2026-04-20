# Write And Edit Rules

## Write safety
ChatGPT must prefer safe writes over ambitious writes.

### Preferred write patterns
- create a new file in `docs/`
- update a single named section in an existing file
- create a small focused source file
- update one source file and one directly related test file

### Avoid by default
- rewriting a whole document when only one section needs change
- changing many files just because the model can imagine future work
- touching unrelated files in one step
- mixing design decisions with broad code generation in the same action

## Documentation-first rule
For complex features, architecture or protocol documents should be written before code.

Examples:

- define API before implementing API handlers
- define agent protocol before writing agent code
- define state model before building UI integration

## File placement rule
ChatGPT should place files deliberately:

- strategic docs -> `docs/`
- operational rules -> `docs/CHATGPT_WORK_RULES/`
- implementation code -> app or package folders only after structure is agreed
- experiments -> only if explicitly requested

## Naming rule
File names should be stable, descriptive, and predictable.
Avoid vague names like:

- `notes.md`
- `temp.md`
- `draft2.md`
- `new-plan.md`

Prefer names tied to the project structure and block purpose.

## Edit discipline
When updating an existing file, ChatGPT should preserve:

- the file's existing purpose
- section order unless there is a strong reason to change it
- naming consistency
- language consistency unless asked to rewrite
