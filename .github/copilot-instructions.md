# Copilot code review instructions

Read `AGENTS.md` at the repository root first; it is the source of truth for structure, commands and conventions.

## Language

Write review comments, and expect all deliverables (code comments, docs, commit messages, PR text), in English.
Runtime log and error messages are English too.

## Review checklist

- Error handling: `set -euo pipefail` is assumed; flag pipes or subshells that swallow failures.
- Every `gh api` call goes through `gh_api` (version header) and checks the response for errors.
- `jq` filters tolerate missing keys and handle boolean `false` correctly (`// empty` and `//` treat `false` as empty).
- All variable expansions are double-quoted; function variables use `local` (top-level `local` is not possible and is intentionally absent).
- Mutating API calls (PATCH/PUT/POST) are skipped under `--dry-run`; a dry-run that changes anything is a serious bug.
- Secrets (`PERSONAL_ACCESS_TOKEN`) are never echoed or logged, and `--repo`/`--target` values cannot inject into API paths or shell commands.
- Changes to `config.json` structure or CLI options update the `usage` text, `README.md` and `AGENTS.md` together.
- Commit and PR titles follow Conventional Commits.

## Do not flag

- `SC2155` shellcheck warnings (`local x=$(...)`) are existing and accepted.
- Updating `usage`, `README.md` and `AGENTS.md` together after a config or option change is intended, not duplication.
