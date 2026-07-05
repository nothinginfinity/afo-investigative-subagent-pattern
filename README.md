# afo-investigative-subagent-pattern — v0.1.0

A **subagent worker**: answers questions about any GitHub repo by reading it
entirely server-side and running Workers AI (`@cf/zai-org/glm-4.7-flash` by
default) over the contents. The MCP caller only pays tokens for the compact,
cited answer — never for the file contents.

Connect as a no-auth MCP (Claude, ChatGPT):
`https://afo-investigative-subagent-pattern.jaredtechfit.workers.dev/mcp`

## Tools
- `subagent_status` — bindings, model, limits.
- `list_repo_files(repo, ref?, filter?)` — cheap tree listing, no LLM.
- `ask_repo(repo, question, ref?, include?, exclude?, max_files?, model?)`
  — selects the most relevant files (path/keyword heuristic), reads up to
  25 files / ~180KB server-side, answers concisely with file-path citations,
  and reports exactly what it read.
- `ask_files(repo, question, paths[], ...)` — same, but over explicit paths.

## Bindings
- `AI` (Workers AI) — required for ask_* tools.
- `GITHUB_TOKEN` (secret) — optional; without it only public repos work.
  Status endpoint reports this visibly.
- `DEFAULT_OWNER`, `WORKER_NAME` — plain text.

## Design rules (the subagent pattern)
Return summaries + pointers, never payloads. Heavy reading and model calls
happen inside the worker; responses stay a few hundred tokens. For deep
persistence, pair with CairnStone: stone the answer if it's worth keeping.
