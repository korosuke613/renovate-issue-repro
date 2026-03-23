# Renovate Issue Reproduction

Minimal reproduction for [renovatebot/renovate Discussion #42042](https://github.com/renovatebot/renovate/discussions/42042).

## Current behavior

After upgrading from Renovate 43.83.2 to 43.84.0, the `github-digest` datasource fails with `bad-credentials` when resolving digests for private repositories using a GitHub App installation token passed via `--token`.

The `.github/workflows/example.yaml` references a reusable workflow from a **private** repository (`korosuke613/renovate-issue-repro-private`) using a branch ref (`@main`). In 43.84.0+, #40225 causes Renovate to process non-semver refs via the `github-digest` datasource, which returns `bad-credentials` with a GitHub App installation token.

In 43.83.2, branch refs were treated as "unsupported/unversioned value" and skipped.

## Expected behavior

The `github-digest` datasource should resolve the digest successfully using the platform token, or gracefully skip/fall back as in 43.83.2.

## How to reproduce

1. Create a GitHub App with access to both this repository and `korosuke613/renovate-issue-repro-private`
2. Generate an installation token scoped to the owner
3. Run `pnpm dlx renovate@43.84.2 --token <token> korosuke613/renovate-issue-repro`
4. Observe `bad-credentials` from `github-digest` datasource
5. Run `pnpm dlx renovate@43.83.2 --token <token> korosuke613/renovate-issue-repro`
6. Observe success (branch ref skipped)

## Reproduction runs

- [43.83.2 (success)](https://github.com/korosuke613/renovate-issue-repro/actions/runs/23434045316)
- [43.84.2 (failure)](https://github.com/korosuke613/renovate-issue-repro/actions/runs/23434090149)
