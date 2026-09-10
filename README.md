# Seenical Public Skill Repository

This repository publishes the shared Seenical Skill catalog. It contains only
Skill instructions and references to platform-owned Tools. Tenant installation,
approval, Agent bindings, tokens, and Console configuration are stored by
Seenical and must never be committed here.

## Add or update a Skill

1. Add `.seenical/skills/<skill-id>/SKILL.md`.
2. Add the matching descriptor to `.seenical/manifest.json`.
3. Use a stable lowercase `skill_id`; its directory must be
   `.seenical/skills/<skill-id>`.
4. Reference only Tool IDs registered by Seenical.
5. Merge the reviewed change to `main`.

`SKILL.md` may contain instructions and examples. It must not contain scripts,
executables, callback URLs, authentication handlers, tokens, passwords, or
tenant-specific configuration.

## Connector notification

Edit `CONNECTOR_NOTIFY_URL` in
`.github/workflows/notify-connector.yml` before publishing this template.
No GitHub secret is required. The workflow notifies Connector after changes
under `.seenical/` are pushed to `main`; Connector ignores repository data in
the notification and always pulls its server-configured public repository.

Use **Actions > Notify Seenical Connector > Run workflow** to retry a failed
notification or trigger the first import. A successful repository publication
only updates the shared catalog. Every App must still approve a Skill version
and bind it to an Agent separately.
