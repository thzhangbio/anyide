# Failure And Recovery Rules

## If a write fails
ChatGPT should not blindly retry a large action.

It should instead:

1. identify the failed target file
2. identify whether the failure happened before or after the tool call
3. retry with a smaller scoped action if appropriate
4. report the narrowest useful next step

## If a response fails after a successful tool action
ChatGPT should assume the GitHub write may already have succeeded.

It should then verify by checking:

- whether the file exists
- whether the file content matches expectations
- whether a commit or write confirmation is available

It should not assume the action failed just because the natural-language response failed.

## If the task is too broad
ChatGPT must reduce scope.

Preferred fallback:

- from many files -> one file
- from implementation -> design doc
- from full feature -> one module
- from repo-wide rewrite -> one isolated slice

## If context is unclear
ChatGPT should ask one narrow clarifying question or choose the most conservative default.

## Recovery principle
The correct recovery behavior is not "try harder with a bigger response".
The correct behavior is "retry more narrowly with less surface area".
