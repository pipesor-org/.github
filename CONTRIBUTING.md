# Contributing to Pipesor

Pipesor repositories may be private. Contributions are currently accepted from collaborators with repository access or through a maintainer-approved process. Public visibility of this policy does not imply public access to source code or permission to reuse it.

## Before contributing

1. Confirm the work is approved or tracked in a GitHub issue or agreed task.
2. Read the repository's `README.md`, `AGENTS.md` (if present), and domain-specific documentation.
3. Keep changes focused. Avoid unrelated refactors, dependency additions, or invented business rules.
4. Never commit credentials, tokens, personal data, `.env` files, or generated output.

## Branching — Trunk-Based Development

- `main` is the shared trunk. Keep it healthy and releasable; integrate small changes frequently.
- Create short-lived `feature/<topic>` or `fix/<topic>` branches from `main`, open a PR into `main`, and delete the branch after merge. Do not push directly to protected branches.
- Validate the PR with the repository's required checks and a review. Use a Conventional Commit PR title and squash merge when practical. A PR build may check that an artifact builds, but does not publish the release artifact.
- Do not use long-lived `develop` or routine release branches. Repository procedures should implement this standard; existing conflicting guidance needs migration.

## Target artifact promotion and releases

This is the target model for services with staged deployments, not a claim that automation exists today. Repositories document their actual implementation and applicable environments.

1. After merge, verify the merged commit and build an immutable artifact once, identified by commit and digest. Deploy that artifact to development automatically; record the artifact, environment, and deployment result.
2. Promote the same digest on demand to QA only after a successful development deployment. Promote to staging only after successful QA and independent QA approval. Check health and record each result.
3. Release only a commit reachable from `main` with a successful staging deployment of the same digest. Assign a SemVer version from Conventional Commits since the previous version tag, associate it with that digest without rebuilding, tag the commit `vX.Y.Z`, and publish release notes. Protect release tags.
4. Promote the versioned artifact to production only after the same digest succeeds in staging and an independent product owner approves. Record the outcome. Approvers must not approve runs they started.

For an urgent production fix when `main` cannot be released, merge the fix into `main` first. Create a short-lived `hotfix/X.Y.Z` from the production tag `vX.Y.Z`, cherry-pick the fix with `-x`, and build a new immutable artifact. After green CI, promote it through QA and staging, release the next patch version without rebuilding, and promote to production with the same approvals. Run this exceptional path from the hotfix branch and delete it after release. Retain artifacts used in QA, staging, production, or releases; clean up older unpromoted commit artifacts according to a documented retention policy.

The landing page documents Cloudflare Workers Builds for static production and branch previews; its deployment setup is a separate implementation, not the staged service flow above. The future Azure-based frontend/backend should implement the target model using its own documented infrastructure and controls. No particular registry, host, or workflow is required organization-wide.

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
- Obtain review and pass required checks before merging into `main`; prefer squash merges.
- Follow the documented promotion and release controls for the repository.

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
