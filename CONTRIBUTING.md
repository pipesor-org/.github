# Contributing to Pipesor

Pipesor repositories may be private. Contributions are currently accepted from collaborators with repository access or through a maintainer-approved process. Public visibility of this policy does not imply public access to source code or permission to reuse it.

## Scope and preparation

This guide is the source of organization-wide contribution rules. Each repository's README, AGENTS.md and technical guides define its setup, checks, scopes and implementation procedures within these rules. Document and resolve conflicts before changing an established workflow.

1. Confirm the work is approved or tracked in a GitHub issue or agreed task.
2. Read the repository's instructions and relevant domain documentation.
3. Keep changes focused. Avoid unrelated refactors, dependency additions or invented business rules.
4. Never commit credentials, tokens, personal data or .env files. Keep build output and machine-specific state out of source control unless the repository explicitly requires a reviewed generated artifact.

## Branching — Trunk-Based Development

- Keep main healthy and releasable.
- Create small, short-lived feature/<topic> or fix/<topic> branches from main and open PRs back into main. Delete branches after merge.
- Do not push directly to main, maintain a long-lived develop branch or create routine release branches.
- Follow the exceptional hotfix process below when a production release cannot take the head of main.

## Commits — Angular-style Conventional Commits

Use `type(scope): imperative summary`, with an optional scope and an English summary. Repository configuration defines additional formatting constraints.

| Type | Purpose |
| --- | --- |
| feat | A new user-facing capability. |
| fix | A bug fix. |
| docs | Documentation changes. |
| style | Formatting changes that do not change behavior. |
| refactor | Code restructuring without a feature or bug fix. |
| perf | Performance improvements. |
| test | Test additions or corrections. |
| build | Build tooling or dependency changes. |
| ci | Continuous integration changes. |
| chore | Maintenance outside the other types. |
| revert | Reverting a previous change. |

Examples: feat(operations): add incident validation queue; fix(sidebar): prevent profile menu overlap; docs(repo): clarify contribution guidelines.

Use `!` and/or a `BREAKING CHANGE:` footer for incompatible changes and explain the migration. Do not use feat for purely documentary or maintenance changes.

## Pull requests and validation

- Use a Conventional Commit title. Describe the problem, resulting behavior and related task.
- Report the applicable repository checks, their results and anything not verified. Do not claim browser or functional behavior was tested unless it was exercised.
- Attach visual evidence for UI changes when practical.
- Obtain review, resolve comments and pass required checks before merging. Prefer squash merge and respect the repository's configured merge method.
- Keep PR titles within 65 characters so the squash header, with its ` (#N)` suffix, stays within the 72-character commit limit.
- Every repository runs the shared CI from [pipesor-ci](https://github.com/pipesor-org/pipesor-ci): canonical commitlint/Lefthook configuration, PR title, description and commits, workflow linting, and the repository's own checks. Default branches are protected by rulesets that require pull requests and squash merges.
- Where the squash message uses the PR title and description, put migration instructions and any BREAKING CHANGE: footer in the description, even if already present in individual commits. Preserve that information when merging.

## Target artifact promotion and releases

These are target principles for services with staged deployments. Today every container-delivered repository verifies the merged commit, publishes one immutable image with a candidate manifest and never deploys; the environment Terraform roots in pipesor-iac own deployed images, so promotion and rollback are reviewed plans with a digest. Approval gates beyond development are not configured yet. Each repository documents its actual deployment model, applicable environments and implementation procedures. Deployment platforms, commands, Terraform plans and state handling belong in those repository guides.

Verify the merged commit before creating a deployment candidate; PR validation alone is insufficient. Build one immutable artifact, identified by commit and digest, and promote that same digest through development, QA, staging and production. The target flow deploys to development automatically; later stages require evidence of the preceding stage's success. Require independent QA approval before staging and independent product-owner approval before production. Approvers must not approve runs they started.

Release only commits reachable from main whose artifact passed staging. Derive SemVer from Conventional Commits, protect vX.Y.Z release tags, and associate the version and notes with the same artifact without rebuilding. Record deployment results and approvals. Retain artifacts used in QA, staging, production or releases, and document cleanup of older unpromoted artifacts.

For an urgent production fix that cannot release main, merge the fix into main first, then cherry-pick it with -x onto a short-lived hotfix/X.Y.Z branch from the production tag. Build a new immutable artifact, pass CI, promote through QA and staging, release the next patch version without rebuilding, and require the same production approval. Delete the hotfix branch after release.

## Repository metadata

Every repository has a description and topics that follow this standard. Set them when creating the repository and update them when its role, scope or stack changes.

- **Description:** one English sentence in the present tense that states what the repository does, ending with "for Pipesor." or "in Pipesor.". Do not include temporary status (planned, demo, pilot) or the technology stack, which belongs in topics.
- **Topics**, in layers:
  1. `pipesor` on every repository.
  2. `aiops` on repositories that are part of the product (services, frontend, landing, documentation, infrastructure and evaluation), not on tooling repositories.
  3. One role topic, such as `microservice`, `dashboard`, `landing-page`, `infrastructure-as-code`, `documentation`, `evaluation`, `developer-tools`, `reusable-workflows` or `community-standards`.
  4. One or two capability topics, such as `data-ingestion`, `incident-management`, `evidence-correlation`, `log-processing`, `retrieval-augmented-generation` or `root-cause-analysis`.
  5. The main stack: for example `java` and `spring-boot`, `python` and `fastapi`, `react` and `typescript`, `terraform` and `azure`, or `github-actions`.
- **Homepage:** only for repositories that publish a public site.

Example: `pipesor-pipeline-service` — "Manages monitored workflows, executions and incidents for Pipesor." with `pipesor`, `aiops`, `microservice`, `ci-cd`, `incident-management`, `java`, `spring-boot`.

## Licenses and ownership

The project's licensing decision is pending. Do not assume code is open source, accept third-party contributions with unclear rights or add a license without approval from the rights holders.
