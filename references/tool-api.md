# Seenical Tool API

This file defines the version 1 logical Tool API. Tool names, argument names,
and risk levels are stable contracts. The runtime owns transport,
authentication, confirmation, idempotency, and audit logging.

## Common rules

- All arguments are JSON objects. Undeclared fields are rejected.
- IDs are strings. Revisions and numeric limits are integers.
- `expected_revision` is required for updates, schedule changes, archives, and
  configuration rollbacks.
- Read operations run automatically. `write` operations require a field-level
  preview and approval. `execute` and `destructive` operations require explicit
  confirmation.
- Credential-like fields are forbidden in every request.

## Plans

### `seenical.plan.list` — read

Arguments: `{}`.

### `seenical.plan.get` — read

Arguments: `{ "task_id": string }`.

### `seenical.plan.create` — write

Required arguments: `name`, `prompt`.

Optional arguments: `note`, `article_prompt`, `article_language`, `keywords`,
`word_count_min`, `word_count_max`, `image_count`, `article_count`,
`cycle_type`, `cycle_interval`, `title_reuse`, `site_id_list`, `target_dir`,
`commit_type`, `target_summary_dir`, `auto_deploy`.

Enums: `article_language` is `auto`, `zh-hans`, or `en`; `cycle_type` is
`none` or `cycle`; `title_reuse` and `auto_deploy` are `on` or `off`;
`commit_type` is `branch` or `pull_request`.

Creating a plan does not run it.

### `seenical.plan.update` — write

Arguments:

```json
{
  "task_id": "string",
  "expected_revision": 0,
  "changes": {}
}
```

`changes` accepts the optional plan fields documented by
`seenical.plan.create`. It must contain only fields being changed.

### `seenical.plan.schedule` — write

Arguments: `{ "task_id": string, "schedule": "on" | "off", "expected_revision": integer }`.

### `seenical.plan.run` — execute

Arguments: `{ "task_id": string }`.

### `seenical.plan.archive` — destructive

Arguments: `{ "task_id": string, "expected_revision": integer }`.

### `seenical.plan.rollback` — destructive

Arguments: `{ "task_id": string, "revision": integer, "expected_revision": integer }`.

### `seenical.plan_run.list` — read

Arguments: `{ "task_id": string }`.

## Previews and publishing

### `seenical.preview.create` — execute

Arguments: `{ "task_run_id": string }`.

### `seenical.preview.get` — read

Arguments: `{ "preview_id": string }`.

### `seenical.preview.publish` — destructive

Arguments: `{ "preview_id": string }`.

### `seenical.preview.discard` — destructive

Arguments: `{ "preview_id": string }`.

### `seenical.deploy.rollback` — destructive

Arguments: `{ "site_id": string }`. The runtime refuses this operation when
there is no recorded recoverable deployment or the branch changed externally.

## Agents

### `seenical.agent.get` — read

Arguments: `{ "chatbot_id"?: string }`. If omitted, the current Agent is used.

### `seenical.agent.update` — write

Arguments:

```json
{
  "chatbot_id": "optional string",
  "expected_revision": 0,
  "changes": {
    "model": "optional string",
    "vendor": "optional string",
    "system_prompt": "optional string",
    "plugin_ids": ["optional plugin id"]
  }
}
```

### `seenical.agent.rollback` — destructive

Arguments: `{ "chatbot_id"?: string, "revision": integer, "expected_revision": integer }`.

## Sites

### `seenical.site.list` — read

Arguments: `{}`.

### `seenical.site.get` — read

Arguments: `{ "site_id": string }`.

### `seenical.site.update` — write

Arguments:

```json
{
  "site_id": "string",
  "expected_revision": 0,
  "changes": {}
}
```

`changes` may contain: `name`, `footer_note`, `lanying_link`, `title`,
`copyright`, `canonical_link`, `meta_keywords`, `official_website_url`,
`max_latest_num`, `language`, `commit_type`, `icp_number`,
`hook_sentence_slogan`, `hook_sentence_image`, and `collaborator`.

Enums: `language` is `zh-hans` or `en`; `commit_type` is `branch` or
`pull_request`.

### `seenical.site.rollback` — destructive

Arguments: `{ "site_id": string, "revision": integer, "expected_revision": integer }`.

## Console navigation

### `seenical.console.navigate` — local

Arguments: `{ "target": value }`, where `value` is `agent`, `loop`,
`knowledge`, `site`, `deployment`, or `skills`.

This action only opens the matching Console view and reports the local result.
