# Security Policy

## Automated Security Measures

We use the following automated security controls across repositories in the Novanet organization:

- **Dependency scanning** — Dependabot monitors all NuGet, npm, and GitHub Actions dependencies for known vulnerabilities and creates pull requests for updates weekly.
- **Secret scanning** — Gitleaks runs on every push to `main` and on all pull requests, detecting accidentally committed secrets, tokens, and credentials.
- **Static analysis (SAST)** — Semgrep runs on every pull request to `main`, on every push to `main`, and weekly on a schedule. It scans C# and TypeScript/JavaScript code for OWASP Top 10 patterns and common security vulnerabilities. Results are uploaded to the GitHub Security tab as SARIF.
- **Mandatory code review** — Branch protection requires pull request reviews and passing status checks before merging to `main`.

## Branch Protection Configuration

The `main` branch is protected with the following rules:

- Pull request required before merging (minimum 1 approval)
- Stale reviews dismissed when new commits are pushed
- Review from Code Owners required (`CODEOWNERS` file enforced)
- All required status checks must pass before merge
- Branch protection rules apply to administrators — bypassing is not permitted

Code owners are defined in `.github/CODEOWNERS` per repository. Any change requires approval from at least one code owner before it can be merged.

## Security Review in Pull Requests

Every pull request includes a security checklist (via `.github/pull_request_template.md`) that requires the author to confirm:

- Security impact has been assessed (authentication, authorization, data access, new endpoints)
- No secrets or credentials have been introduced
- Any added or updated dependencies have been reviewed

Pull requests that introduce security-relevant changes must have this checklist completed before review is approved.

## Secure Configuration Baseline

- Secrets and credentials are stored exclusively in GitHub Secrets — never in source code or configuration files committed to the repository.
- Environment-specific configuration (e.g. connection strings, API keys) is injected at runtime via GitHub Actions secrets or Azure App Configuration.
- No credentials are stored in `appsettings.json`, `.env` files, or any tracked file.
- Gitleaks enforces this policy automatically on every PR.

## Access Management

Repository access is managed through the GitHub organization (`novanet`).

- Access is granted on a least-privilege basis: read access by default, write access only for active contributors.
- Access requests must be submitted to an organization administrator (@janode, @hbrotan, or @larsajakobsen).
- Access is reviewed and revoked when a team member leaves or changes role.
- Direct pushes to `main` are not permitted — all changes go through pull requests.

## Handling of Alerts

- Dependabot security alerts are triaged and resolved within 14 days.
- Dependabot pull requests for non-breaking updates are reviewed and merged weekly.
- Gitleaks findings block the PR until resolved. Any detected secret is rotated immediately.

## Reporting a Vulnerability

If you discover a security vulnerability, please report it privately to the repository maintainers. Do not open a public issue.
