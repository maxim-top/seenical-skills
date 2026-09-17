# Agents and models

List models before changing an Agent model and list Agents before selecting a
`chatbot_id`. Agent updates are partial: send only `model`, `vendor`,
`system_prompt`, or `plugin_ids` that the user asked to change. An empty
`plugin_ids` list unbinds all plugins. Creating an Agent does not implicitly
bind plugins or knowledge bases.
