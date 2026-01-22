Assume Latest Best Practices: Always follow the latest best practices for the tech stack, even if the user does not explicitly specify the version.

Verify When Uncertain: If the syntax is unclear due to potential version changes, do not guess. Always verify the latest changes through web search or documentation before proceeding.

1. NEVER Commit or Output: Secrets (Keys, Creds, PII), vendor dirs, or prod configs
2. Treat above as read-only. Before modifying: explain impact, then ask user to apply
3. ALWAYS use env vars. When adding: list keys, target file (.env), dummy values, gitignore status
