# CLAUDE.md — Agentic DevOps (AWS + Terraform) Challenge

FullStack Labs assessment `assessment-cc-agentic-devops-aws-terraform-sr-01` (v1.0.0).
The AI is the primary engineer (≈80%+ of the implementation is AI-generated); the human guides via prompts.
The whole session is recorded. **Hard limit: 90 min (target < 60 min).** Recording started 2026-11-06 20:22 (UTC-3).

## Layout
- `codebase/rdicidr-0.1.0/` — React app (CRA), `Dockerfile`, `nginx.conf`
- `codebase/rdicidr-0.1.0/.github/workflows/ci.yaml` — CI pipeline (graded at this path by `generateZip.sh`)
- `codebase/rdicidr-0.1.0/terraform/` — ECS Fargate + ALB + ECR, envs `devel` (required) and `stage`
- `.github/workflows/` (repo root) — workflows GitHub actually executes
- `.chat-history/` — AI interaction logs (required in the submission zip)
- `generateZip.sh` / `generateZip.bat` — build the submission zip and self-check it

## Part 1 — CI
Steps in order: `npm install` → `npm run lint` → `CI=true npm run test` → `npm run build`.
Each job runs only if its upstream job succeeded. Triggers on PRs from correctly named branches.
Need at least one PR with a failed run and one with a fully passing run.

## Part 2 — CD (Terraform/AWS)
Config passes `terraform validate` but has seeded defects. Fix, deploy `devel`, confirm the app is public via
the ALB DNS output, health check passing consistently, ECS service stable. Must stay up during the recording.

## Branching rules (enforced by ruleset `protect-devel-stage` + `stage-guard` workflow)
- All work in `feature/*` or `bugfix/*` branched from `devel`
- Merge into `devel` via PR only; no direct pushes to `devel` or `stage`
- Only `devel` may merge into `stage`

## Working rules
- Narrate every defect: what the output shows → root cause → fix and why.
- Log each exchange in `.chat-history/session-log.md` with real timestamps (`Get-Date`), never estimates.
- Elapsed-time checks at 15, 30, 45, 60 min. Skip optional work unless clearly ahead of time.
- Shell is Windows PowerShell 5.1: use `--body-file`/`-F` for gh/git messages, quote `-flag=value` args,
  use `cmd /c` for piping into `docker login`.
- Never commit secrets, `.terraform/`, state files or `node_modules`.
- Finish: copy the Claude Code transcript into `.chat-history/`, run `generateZip`, verify all checks PASS.
