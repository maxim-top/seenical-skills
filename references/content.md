# Content plans and publishing

Create plans without running them and always send `run_immediately: false`.
The legacy HTTP API still treats an omitted `run_immediately` as immediate
execution for compatibility, so omission is not equivalent to `false`.
`prompt` describes the continuing content theme while `article_prompt` guides each article. Updating configuration never
resumes a paused plan. Run, retry, preview, publish, discard, and deploy are
separate operations. Read the plan/run/preview state before each mutating step.
