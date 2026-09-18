# Seenical Skills

This public multi-Skill repository contains the platform-maintained Seenical
skills. Each directory under `skills/` is independently installable by its
GitHub path:

- `skills/seenical-api`: Butler API contracts and the Seenical Tool runtime.
- `skills/seenical-console`: Seenical Console UI operating guidance.
- `skills/seenical-product-onboarding`: first-run product setup guidance.

Each Skill uses the standard `SKILL.md`, optional `agents/`, and `references/`
layout. Compatible Agents can install only the directory they need.

`.seenical/manifest.json` is Seenical-specific publication metadata.
`skills/seenical-api/.seenical/runtime.json` selects the host-provided Butler
API runtime and points to that Skill's `references/api/*.json` contracts. The
other two Skills are instruction-only and declare no executable Tools. Domains,
tokens, headers, tenant data, and Console configuration must never be committed
here.

## Repository layout

```text
.seenical/manifest.json
.github/workflows/notify.yml
skills/
  seenical-api/
    SKILL.md
    agents/openai.yaml
    .seenical/runtime.json
    references/
  seenical-console/
    SKILL.md
    agents/openai.yaml
    references/console-operations.md
  seenical-product-onboarding/
    SKILL.md
    agents/openai.yaml
```

To update the repository:

1. Edit only the relevant directory under `skills/`.
2. Keep each manifest descriptor's `path` aligned with its directory, and keep
   its `name_zh`, `name_en`, `description_zh`, and `description_en` fields in
   sync with the public catalog copy.
3. Keep the `seenical-api` Tool list aligned with its `references/api/*.json`.
4. Merge the reviewed change to `main`.

`SKILL.md` may link to text resources under `references/` and optional Agent UI
metadata at `agents/openai.yaml`. Connector validates and stores these files as
read-only Skill resources; it never executes repository content. Scripts,
executables, authentication handlers, tokens, passwords, and tenant-specific
configuration are not accepted. Public callback URLs are data passed only to
the reviewed Butler endpoint and are subject to runtime validation.

The filename `agents/openai.yaml` is the standard Skill UI metadata location
used by OpenAI clients such as Codex. It contains only display and invocation
metadata, does not restrict the Skill to one model provider, and is not read by
the Seenical Butler API runtime.

Compatibility is intentionally additive:

- Codex reads `SKILL.md`, `agents/openai.yaml`, and the selected
  `references/api/*.json` file.
- Claude and WorkBuddy can read `SKILL.md` and `references/`; they may ignore
  the OpenAI-specific UI metadata.
- Seenical/Connector reads `.seenical/manifest.json` and
  `.seenical/runtime.json`, then exposes the same API contracts as tools.

Unknown client-specific metadata can be ignored. Install a Skill with its
directory URL, for example:

```text
https://github.com/maxim-top/seenical-skills/tree/main/skills/seenical-api
```

No product or tenant configuration is stored here.

This repository does not ship a standalone CLI or MCP server. Seenical Console
supplies its authenticated HTTP client to `butler_api/v1`. Codex, Claude Code
and other Agents with an HTTP or shell tool can also execute `seenical-api`
directly after the user configures these process environment variables outside
the conversation:

```text
SEENICAL_API_BASE=https://your-seenical-butler-host
SEENICAL_APP_ID=your-app-id
SEENICAL_ADMIN_TOKEN=your-seenical-api-token
```

The Skill reads the API method, relative path, argument placement and schema
from `references/api/*.json`, sends the Token through the `access-token` header
(or a Bearer header), and remains limited by the Token's bound App and Butler
API policy. Never commit these values or paste the Token into a conversation.
If the Agent has neither the Seenical host runtime nor the three configured
environment variables, it can explain the operation but must not attempt it.

## Update notification

Configure the repository Actions variable `NOTIFY_URL` under
**Settings > Secrets and variables > Actions > Variables** when update
notifications are needed. No GitHub secret is required. If the variable is not
configured, the notification step is skipped and the workflow succeeds. If it
is configured, the workflow sends repository and commit metadata after Skill
content is pushed to `main`; the receiver decides how to process the update.

Use **Actions > Notify Skill Update > Run workflow** to retry a failed
notification or trigger the first import. A successful publication updates all
three shared public Skill versions. `seenical-api` becomes available to
Seenical Agent conversations after the App binds its Seenical IM user; the
other Skills remain downloadable guidance and are not injected as Agent Tools.
