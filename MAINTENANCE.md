# Maintenance Policy — MicroMDM (Iglu Fork)

This is a maintained fork of [MicroMDM v1](https://github.com/micromdm/micromdm), originally developed by the MicroMDM project. The upstream project entered maintenance mode in 2024 with support ending December 2025.

**This fork is actively maintained by Iglu Technology for internal Apple device management.**

## Patching Cadence

| Severity | Response Time |
|----------|--------------|
| Critical / High CVE | Within 14 days of disclosure |
| Medium CVE | Within 30 days |
| Low / Informational | Next scheduled review |

## Monitoring

- **Dependabot** enabled for Go modules and GitHub Actions (weekly scan)
- **govulncheck** runs on every push to `main` and weekly via scheduled CI
- Apple MDM protocol changes monitored via Apple developer release notes

## Review Schedule

- **Weekly:** Dependabot alerts reviewed and merged/dismissed
- **Monthly:** Full dependency review, Go version check
- **Quarterly:** Codebase security review, audit report updated

## Responsible Party

James Deane — Iglu Technology
Contact: mobile.james.deane@gmail.com

## Change Log

See [CHANGELOG.md](CHANGELOG.md) for detailed release history.

### Fork Maintenance Log

| Date | Action |
|------|--------|
| 2026-03-18 | Fork created from upstream v1.13.1. Initial dependency audit completed. Dependabot enabled. govulncheck CI added. |
