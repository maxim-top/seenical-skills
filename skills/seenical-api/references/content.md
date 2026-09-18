# Content plans and publishing

Create plans without running them and always send `run_immediately: false`.
The legacy HTTP API still treats an omitted `run_immediately` as immediate
execution for compatibility, so omission is not equivalent to `false`.
Plan text fields are not interchangeable:

- `name` is the Loop/plan name, not an article title.
- `prompt` is the continuing content theme (计划主题).
- `article_prompt` is the extra instruction applied to every generated article
  (文章提示词).
- `keywords` is a legacy API field name for the article-title pool. It contains
  exactly one complete article title per line; it is not an SEO keyword list.
- `note` is descriptive or legacy plan text.

When a user asks for an article prompt, use only `article_prompt`. Do not fall
back to `prompt` or `note`; report an empty `article_prompt` as not configured.
Never put a requested article title into `article_prompt`. To add or remove a
title, read the current plan first and replace `keywords` with the complete
updated title pool, preserving every title the user did not ask to change. Do
not use commas as title separators. Resolve a numbered response to the exact
previously offered title text; a choice number or A/B/C action marker is never
the title itself.

`article_count` is the number generated per recurring run. A non-recurring run
consumes all available unused titles, so changing `article_count` does not cap
that run. `title_reuse` controls whether later runs may wrap and reuse the title
pool, including when a non-recurring plan is run manually again.

Before running, verify that the current plan has at least one title in
`keywords` or an existing uploaded title file in `file_list`. File upload is a
Console-only operation; if neither source exists, add titles through `keywords`
or direct the user to Console file upload instead of starting a run that will
fail.
Updating configuration never resumes a paused plan. Run, retry, preview,
publish, discard, and deploy are
separate operations. Read the plan/run/preview state before each mutating step.

Use `seenical.plan.results` to browse results across all runs of a plan. Use
`seenical.plan_run.results` only when a specific `task_run_id` is already known.
When `has_more` is true, pass the returned `next` value back as `cursor` without
interpreting or modifying it.
