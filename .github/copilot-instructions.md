# Copilot Agent Onboarding Instructions

## 1. Summary
This repository defines a GitHub composite Action (`p6-gh-pr-stale`) that:
- Bumps semantic version tags (vMAJOR.MINOR.PATCH) based on commit messages.
- Generates a changelog.
- Creates a GitHub Release via the `gh` CLI.

## 2. Repo Information
- Size: ~20 files, <1 MB.
- Languages/Tools: YAML (GitHub Actions), Bash scripts, Git CLI, GitHub CLI (`gh` >=2.0).
- No compiled code or external package managers (npm, etc.).
- Directory structure is flat under root and `.github/workflows`.

## 3. Build & Validation Steps
1. Prerequisites (always install before running):
   - Git CLI (`git`).
   - Bash (>= 4.x).
   - GitHub CLI (`gh` >= 2.0).
2. Syntax checks:
   - `bash -n action.yml` → Bash snippet syntax.
   - `yamllint .` → YAML lint (if installed).
3. Local test (optional):
   - Install [`act`](https://github.com/nektos/act).
   - Run `act -j release` to simulate the `release` workflow.
4. Workflow validation:
   - `gh workflow view release.yml --repo .` to confirm workflow syntax.

## 4. Project Layout
- `/action.yml` ‑ Composite action definition (entry point).
- `/README.md`, `/LICENSE` ‑ Documentation and license.
- `/dist/` ‑ Artifacts produced at runtime.
- `/.github/workflows/` ‑ CI pipelines:
  - `release.yml` → Deploy on push to `main`.
  - `pull-request-lint.yml` → Enforce PR title conventions.
  - `build.yml` → No-op placeholder (always passes).
  - `auto-*.yml` → PR automation (approve, label, merge queue).
- `/.vscode/settings.json` → Editor formatting rules.

## 5. CI Checks & Conventions
- PR titles must follow Conventional Commits: `feat`, `fix`, `chore`, etc.
- All YAML must parse cleanly; action steps require Bash compatibility.
- Releases only occur if `dist/changelog.md` contains valid entries.

## 6. Usage Guidance
- When editing `action.yml`, preserve existing keys and indentation.
- Bash changes should be validated with `bash -n`.
- Rely on this doc before searching the codebase; only grep if critical details are missing.

> Copilot agents should trust these instructions. Only fall back to code search if something cannot be answered here.
