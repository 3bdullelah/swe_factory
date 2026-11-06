# AGENTS.md

## Project state

This repository has no application code yet. When code is added, document
here:

- how to set up a fresh checkout (install dependencies, env files to copy);
- how to start the dev server on a given port, if there is one;
- the validation commands (lint, typecheck, tests, build).

Then put the same validation commands in `factory/checks.sh`, replacing its
placeholder. Until that script exits 0 on the base branch, no factory job can
pass the gate.

## Environment

The checkout lives on Windows, but the factory runs only from WSL Ubuntu
(open the same folder through its `/mnt/c/...` path). Its
scripts are Unix-only (`fcntl`, `lsof`, `python3` shebangs) and do not run on
native Windows Python. `.gitattributes` keeps `factory/` and `.claude/` on LF
line endings; don't remove it.

## Software factory

`/factory run` builds every open GitHub issue labeled `factory` through
intake → spec → build → gate → review → approve, one PR per issue. State is in
`factory/jobs/` (gitignored); `factory/bin/job` is its only writer. Checks:
`factory/checks.sh`. Dashboard: `cd factory && python3 -m http.server`, then
open http://localhost:8000/dashboard.html.

Each job works in its own git worktree, a full checkout of the repo at
`.worktrees/<job-id>` (gitignored). A tool run from the repo root that doesn't
read `.gitignore` sees those copies too; exclude `.worktrees/` in its config.
