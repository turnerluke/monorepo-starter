# monorepo-starter

A Python monorepo template, batteries-included.

## What you get

- **uv workspace** — `pyproject.toml` configured as a virtual workspace root,
  ready to take subprojects under `[tool.uv.workspace].members`.
- **Pre-commit chain** — `ruff`, `markdownlint`, `yamllint`, `prettier`,
  `interrogate`, `gitlint`, `commitlint`, plus generic file/secret checks.
  Both `pre-commit` and `commit-msg` hooks auto-install with one
  `uv run pre-commit install`.
- **GitHub Actions** —
    - `lint.yml` — Ruff, Markdownlint, Yamllint, Prettier, Shellcheck,
      Gitleaks, Commitlint, plus a conventional-commit PR-title check, with
      PR-comment-driven feedback per linter.
    - `test.yml` — discovers subprojects with `pyproject.toml`, runs the
      repo-root standards tests, then runs `uv run pytest` in each changed
      subproject via matrix.
- **Repo-root standards tests** (`tests/`) — assert every subproject
  follows project-wide conventions (pytest config, coverage config,
  required addopts, project layout, lowercase project name) and that all
  visual gutter lines (`# ===`, `// ===`, `-- ===`) are exactly 128
  columns wide.
- **Conventional commits** — enforced both locally (commitlint at
  `commit-msg`) and in CI, with a custom rule that allows
  `` `Backticked` `` identifiers in subjects.
- **Branch-from-`main` workflow** — documented in `AGENTS.md`. Stacked
  PRs are explicitly avoided.
- **Dependabot** — weekly version updates for `uv`, `npm`, and
  `github-actions`, with conventional-commit prefixes that pass
  commitlint without manual edits.
- **Concurrency cancellation** on workflows so duplicate pushes don't
  stack runs.
- **Claude Code hooks** (`.claude/settings.json`) — a `Stop` hook that
  runs `pre-commit run --all-files` so the assistant cannot end a turn
  on broken lint, plus a `PostToolUse` hook that auto-runs
  `ruff format` + `ruff check --fix` on any `.py` file just edited.

## Usage

Click **Use this template** on GitHub, then in your new clone:

1. Rename the project in `pyproject.toml` (`name`, `description`,
   `authors`, `[project.urls]`).
2. Rename the project in `package.json` (`name`, `description`,
   `repository.url`).
3. Update `LICENSE` to your name + year if not MIT-licensed.
4. Update `.github/CODEOWNERS` to your GitHub handle.
5. Rewrite this `README.md` for your project.
6. `uv sync && uv run pre-commit install` — installs both pre-commit and
   commit-msg hooks.

## Adding a Subproject

Create a directory with its own `pyproject.toml` and `tests/`, then
register it in the root:

```toml
[tool.uv.workspace]
members = ["libs/my-lib"]
```

`uv sync` from the root and you're done. The repo-root standards tests
will start validating the subproject's `pyproject.toml`; the
`Test Subprojects` job will run its tests on every PR that touches it.

See [`docs/testing.md`](docs/testing.md) for the full set of standards
each subproject must meet.

## Conventions

See [`AGENTS.md`](AGENTS.md) for the project conventions: branching from
`main`, conventional-commit format, no co-author trailers, no empty PR
checklists, waiting for CI before reporting work as done.

## License

MIT — see [`LICENSE`](LICENSE).
