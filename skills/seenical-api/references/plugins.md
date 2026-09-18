# AI plugins

List plugins and their functions before editing them. Plugin name and public
callback endpoint updates are separate from function metadata. Credential
values in headers, query parameters, environment variables, and authentication
are never returned: the API substitutes `__MASKED_SENSITIVE_VALUE__` while
preserving the surrounding structure. Submit the marker unchanged to keep that
exact stored value, provide a different value only when the user explicitly
requests replacement, and use an empty string only for an explicit clear.
Omitting a top-level configuration field leaves the whole field unchanged;
omitting a key inside a submitted configuration object removes that key. A
marker at a new path is rejected because no existing value can be preserved.

Never print a replacement credential or include it in a summary. Private
callback addresses remain forbidden. Read binding relations before replacing
a binding list; an empty list explicitly clears the selected binding.
