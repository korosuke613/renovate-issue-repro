# Renovate Issue Reproduction

Minimal reproduction for [renovatebot/renovate Discussion #42042](https://github.com/renovatebot/renovate/discussions/42042).

## Current behavior

After upgrading from Renovate 43.83.2 to 43.84.0, the `github-digest` datasource fails with `bad-credentials` when resolving digests for private repositories using a GitHub App installation token passed via `--token`.

The `.github/workflows/example.yaml` references a reusable workflow from a **private** repository (`korosuke613/renovate-issue-repro-private`) with digest pinning. Renovate 43.84.0+ routes this to the `github-digest` datasource via `queryBranches`, which returns `bad-credentials`.

## Expected behavior

The `github-digest` datasource should resolve the digest successfully using the platform token, as `github-tags` does in 43.83.2.

## How to reproduce

1. Create a GitHub App with access to both this repository and `korosuke613/renovate-issue-repro-private`
2. Generate an installation token scoped to the owner
3. Run `pnpm dlx renovate@43.84.2 --token <token> korosuke613/renovate-issue-repro`
4. Observe `bad-credentials` from `github-digest` datasource
5. Run `pnpm dlx renovate@43.83.2 --token <token> korosuke613/renovate-issue-repro`
6. Observe success
