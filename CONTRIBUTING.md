# Contributing to Pipesor

Pipesor repositories may be private. Contributions are currently accepted from collaborators with repository access or through a maintainer-approved process. Public visibility of this policy does not imply public access to source code or permission to reuse it.

## Before contributing

1. Confirm the work is approved or tracked in a GitHub issue or agreed task.
2. Read the repository's `README.md`, `AGENTS.md` (if present), and domain-specific documentation.
3. Keep changes focused. Avoid unrelated refactors, dependency additions, or invented business rules.
4. Never commit credentials, tokens, personal data, `.env` files, or generated output.

## Branching — Git Flow

- `main`: stable, approved baselines and releases.
- `develop`: integration branch for ongoing work.
- `feature/<short-topic>` and `fix/<short-topic>`: branch from `develop`; open a PR into `develop`.
- `release/<version>`: branch from `develop`; integrate into `main` **and** back into `develop`.
- `hotfix/<short-topic>`: branch from `main`; integrate into `main` **and** `develop`.

Do not push directly to protected branches. Use pull requests and follow branch-specific policies. A repository may document more specific merge and release procedures.

## Commits — Angular-style Conventional Commits

Format: `type(scope): imperative summary` (scope optional). Write summaries in English.

Allowed types: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`, `revert`.

Examples:

```text
feat(operations): add incident validation queue
fix(sidebar): prevent profile menu overlap
docs(repo): clarify contribution guidelines
```

Use `!` and/or a `BREAKING CHANGE:` footer for incompatible changes. Do not use `feat` for purely documentary or maintenance changes.

## Pull requests

- Use a Conventional-Commit-style PR title.
- Describe the purpose, key changes, how you validated them, and anything not verified.
- Attach screenshots or recordings for visual changes when practical.
- Obtain review before merging shared branches; prefer squash merges for feature/fix PRs into `develop`.
- Use the repository's documented Git Flow merge strategy for releases and hotfixes; do not squash away required branch ancestry without reviewing the implications.

## Validation

Run the repository's declared checks. For `pipesor-frontend`, the current baseline uses:

```bash
bun run typecheck
bun run lint
bun run build
```

Do not claim browser interactions were tested unless they were actually exercised.

## Licenses and ownership

The project's licensing decision is pending. Do not assume code is open source, accept third-party contributions with unclear rights, or add a license without approval from the rights holders.
