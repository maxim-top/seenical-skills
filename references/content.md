# Content plans and publishing

Create plans without running them and always send `run_immediately: false`.
The legacy HTTP API still treats an omitted `run_immediately` as immediate
execution for compatibility, so omission is not equivalent to `false`.
Plan text fields are not interchangeable:

- `prompt` is the continuing content theme (计划主题).
- `article_prompt` is the extra instruction applied to every generated article
  (文章提示词).
- `note` is descriptive or legacy plan text.

When a user asks for an article prompt, use only `article_prompt`. Do not fall
back to `prompt` or `note`; report an empty `article_prompt` as not configured.
Updating configuration never resumes a paused plan. Run, retry, preview,
publish, discard, and deploy are
separate operations. Read the plan/run/preview state before each mutating step.
