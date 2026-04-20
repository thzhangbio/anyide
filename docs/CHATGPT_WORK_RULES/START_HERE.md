# ChatGPT Work Rules: Start Here

This directory defines the operating rules for ChatGPT when working on the `anyide` repository.

## Required reading order
Before doing any new planning, writing, editing, or implementation work, ChatGPT must read these files in order:

1. `docs/IPAD_CODESPACES_UI_CORE_GUIDE.md`
2. `docs/IPAD_CODESPACES_UI_ARCHITECTURE_BREAKDOWN.md`
3. `docs/IPAD_CODESPACES_UI_PRODUCT_SPEC.md`
4. `docs/CHATGPT_WORK_RULES/01_EXECUTION_MODE.md`
5. `docs/CHATGPT_WORK_RULES/02_SCOPE_AND_PRIORITY.md`
6. `docs/CHATGPT_WORK_RULES/03_WRITE_AND_EDIT_RULES.md`
7. `docs/CHATGPT_WORK_RULES/04_COMMIT_AND_CHANGESET_RULES.md`
8. `docs/CHATGPT_WORK_RULES/05_RESPONSE_AND_REPORTING_RULES.md`
9. `docs/CHATGPT_WORK_RULES/06_LONG_PROJECT_PROTOCOL.md`
10. `docs/CHATGPT_WORK_RULES/07_FAILURE_AND_RECOVERY.md`

## Core principle
This repository is being developed as a long, difficult engineering project. ChatGPT must optimize for:

- small safe steps
- stable iteration
- low ambiguity
- explicit scope boundaries
- minimal-risk file writes
- documentation-first architecture work before large code generation

## Mandatory behavior
ChatGPT must not jump directly from broad intent to broad implementation.

For any non-trivial task, it must:

1. identify the exact block or subproblem being worked on
2. state what file or files will be created or changed
3. keep each step narrow enough to succeed reliably
4. prefer one write action per step whenever possible
5. report concrete outputs after each step

## Default mode
Unless the user explicitly asks otherwise, ChatGPT should work in `single-step execution mode`:

- one objective per turn
- one focused deliverable per turn
- one small file write or one tightly related group of edits per turn

If ChatGPT is unsure which rule applies, it must follow the most conservative rule.
