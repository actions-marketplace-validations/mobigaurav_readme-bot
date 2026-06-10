# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [1.0.1] - 2026-06-10

### Fixed
- **Action metadata syntax error on newer runners.** The `github-token` input
  previously defaulted to `${{ github.token }}`, which newer GitHub Actions
  runners (>= 2.334.0) reject with `Unrecognized named-value: 'github'` because
  the `github` context is not available in action metadata defaults — only
  `env` and `inputs` are. The default is now `""`, and callers must pass
  `github-token: ${{ github.token }}` explicitly in the workflow `with:`
  block. The action also falls back to `process.env.GITHUB_TOKEN` if set.

### Changed
- README quick-start and `examples/workflow.yml` now show explicit
  `github-token` in the `with:` block.

## [1.0.0] - 2026-06-09

### Added
- Initial public release of README Pilot.
- GitHub Action that posts a suggested README diff as a comment on every pull
  request, scoped to the README that owns each touched subtree.
- **`github-models` provider** (default) — uses the workflow's built-in
  `GITHUB_TOKEN` with `models: read` permission. Zero-config on GitHub Free,
  Pro, Enterprise Cloud, and any account with a Copilot subscription.
- **`openai` provider** — bring-your-own OpenAI API key.
- **`anthropic` provider** — bring-your-own Anthropic API key.
- **`gemini` provider** — bring-your-own Google AI Studio (Gemini) API key.
- Per-directory README scoping via simple glob rules
  (`backend/** -> backend/README.md` etc.).
- Idempotent comment updates via a hidden HTML marker so the bot edits its
  previous comment in place instead of spamming the PR.
- Diff truncation (`max-files`, `max-diff-bytes`) to keep prompts cheap.
- `dry-run` and `fail-on-error` knobs for safe rollout in protected
  pipelines.
- Self-contained Myers-LCS unified diff renderer; no `diff` package
  dependency.
- Single ~566 KB bundled `dist/index.js` with no vendor SDKs at runtime —
  native `fetch` only.

[Unreleased]: https://github.com/mobigaurav/readme-bot/compare/v1.0.1...HEAD
[1.0.1]: https://github.com/mobigaurav/readme-bot/releases/tag/v1.0.1
[1.0.0]: https://github.com/mobigaurav/readme-bot/releases/tag/v1.0.0
