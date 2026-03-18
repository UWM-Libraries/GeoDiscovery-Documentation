# AGENTS.md

## Purpose
Guidance for Codex agents updating GeoDiscovery documentation in this repository.

## Primary docs in scope
- `docs/develop-env.md` (local WSL Ubuntu development environment)
- `docs/deploy.md` (local bootstrap + production deployment guidance)
- `docs/develop.md` (feature workflow and test references)

## Source-of-truth local development model
When updating local setup docs, align with this validated baseline unless maintainers say otherwise:

- Platform: WSL Ubuntu on Windows
- Runtime manager: `mise` (preferred over RVM for local setup)
- Project runtimes:
  - Ruby `3.2.1`
  - Node `20` (project-local via `mise.toml`)
- JS package manager: Yarn `1.22.22` via Corepack (`packageManager` in `package.json`)
- Local services/dependencies:
  - Java installed for Solr
  - SQLite for local dev/test bootstrap
  - Chrome-family browser required for Capybara system tests
- Local app start path:
  - `bundle exec rake uwm:server`

## Editing rules
- Keep docs practical and copy-pasteable.
- Prefer fenced `bash` code blocks for commands.
- Separate local development guidance from production deployment guidance.
- Do not introduce speculative steps that were not validated.
- Do not add unrelated schema/migration advice.
- Preserve existing local workflow that uses:
  - `.example.env.*` -> `.env.*` copies
  - SQLite setup
  - `bundle exec rake uwm:server`

## Scope boundaries
- Local runtime/tooling updates belong primarily in `docs/develop-env.md`.
- In `docs/deploy.md`, only make light consistency edits in the local section unless explicitly asked for broader production changes.
- Do not rewrite production sections (Capistrano, Sidekiq/systemd, server-specific instructions) unless required for consistency or requested.

## Consistency checks before finishing
1. Ensure local docs do not recommend RVM-first setup.
2. Ensure Node guidance does not imply apt-installed Node is preferred for project runtime.
3. Ensure Yarn guidance uses Corepack and respects pinned package manager version.
4. Ensure `RAILS_ENV=test` is explicit in test examples where relevant.
5. Ensure browser requirement language is clear but not over-specific to one Ubuntu package name.
6. Ensure local ports remain consistent:
   - Solr `8983`
   - Rails `3000`

## Output expectations for Codex responses
- Summarize exactly which files were edited.
- Call out any ambiguous existing docs needing maintainer decisions.
