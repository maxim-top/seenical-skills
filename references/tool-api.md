# Seenical Tool API

The authoritative machine-readable definitions are in
`.seenical/tools.json`. The host loads `.seenical/runtime.json`, supplies its
authenticated Console HTTP client, validates every argument, and asks for the
confirmation required by each Tool's risk.

## Plans

- `seenical.plan.list`: list plans.
- `seenical.plan.create`: create a plan without running it.
- `seenical.plan.update`: partially update a plan. Send `task_id` plus only the
  fields requested by the user.
- `seenical.plan.schedule`: set `schedule` to `on` or `off`.
- `seenical.plan.run`: immediately run a plan.
- `seenical.plan_run.list`: list runs for one plan.

The plan schemas include the article prompt and article language. Updating
either field does not implicitly resume a paused plan.

Use the current plan `revision` as `expected_revision` for partial plan
updates. Site and Agent list responses expose `agent_tools_revision`; pass that
value as `expected_revision` when changing those resources. If a revision
conflict is returned, read the current resource again instead of retrying the
old write.

## Previews and publishing

- `seenical.preview.create`: create a preview for `task_run_id`.
- `seenical.preview.publish`: publish a preview.
- `seenical.preview.discard`: discard a preview.

Preview creation requires an explicit execute confirmation. Publishing and
discarding use the stronger destructive confirmation.

## Sites, Agents, and plugins

- `seenical.site.list` and `seenical.site.update` list sites and partially
  update non-sensitive fields.
- `seenical.agent.list` and `seenical.agent.update` list and configure Agents.
- `seenical.plugin.list` lists AI plugins.

Only APIs present in `.seenical/tools.json` are available. Do not invent a URL,
header, credential, or unlisted Butler endpoint. The public repository contains
no Butler domain or authentication material.
