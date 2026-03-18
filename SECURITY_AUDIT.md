# MicroMDM v1 — Dependency Audit & Maintenance Plan

**Date:** 2026-03-18
**Auditor:** Iglu Technology (automated + manual review)
**Source:** github.com/micromdm/micromdm @ v1.13.1 (commit c4878e2, 2025-10-17)

---

## 1. Project Status

MicroMDM v1 entered maintenance mode in 2024 with official support ending December 2025. The upstream project recommends migration to NanoMDM. Last upstream commit: October 2025.

**This fork is maintained by Iglu Technology as an internal tool for Apple device management.**

---

## 2. Dependency Audit

### Go Version
- **go.mod declares:** `go 1.25.0`
- **Status:** ✅ Current (updated 2026-03-18 from go 1.17)

### Direct Dependencies

| Package | Version | Status | Risk |
|---------|---------|--------|------|
| `golang.org/x/crypto` | v0.49.0 | ✅ Current | Low |
| `golang.org/x/net` | v0.52.0 | ✅ Current | Low |
| `google.golang.org/protobuf` | v1.36.11 | ✅ Current | Low |
| `github.com/gorilla/mux` | v1.8.1 | ⚠️ Archived | Low (stable, no known CVEs) |
| `github.com/gorilla/handlers` | v1.5.2 | ⚠️ Archived | Low (stable) |
| `github.com/boltdb/bolt` | v1.3.1 | ⚠️ Archived | Medium (embedded DB, no network exposure) |
| `github.com/go-kit/kit` | v0.13.0 | ✅ Active | Low |
| `github.com/google/uuid` | v1.6.0 | ✅ Active | Low |
| `github.com/micromdm/scep/v2` | v2.3.0 | ✅ Maintained by same org | Low |
| `github.com/micromdm/plist` | v0.2.2 | ✅ Maintained | Low |
| `github.com/garyburd/go-oauth` | v0.0.0-20250708 | ⚠️ Stale | Medium (OAuth handling) |
| `github.com/pkg/errors` | v0.9.1 | ⚠️ Archived (replaced by stdlib) | Low |
| `github.com/RobotsAndPencils/buford` | v0.14.0 | ⚠️ Low activity | Low (APNS push) |
| `github.com/smallstep/pkcs7` | v0.2.1 | ✅ Active | Low |

### Indirect Dependencies
- `github.com/smallstep/scep` — v0.0.0-20241223 (recent commit, active)
- `github.com/korylprince/go-macos-pkg` — v1.4.2 (maintained)

### Summary
- **3 archived dependencies** (gorilla/mux, gorilla/handlers, boltdb/bolt) — all stable with no active CVEs
- **1 stale dependency** (go-oauth from 2018) — potential concern for OAuth token handling
- **Core crypto/network deps are current** — x/crypto and x/net are recent versions
- **No known CVEs** found against current pinned versions (`govulncheck ./...` — zero findings as of 2026-03-18)

---

## 3. Identified Actions

| # | Action | Priority | Effort |
|---|--------|----------|--------|
| 1 | ~~Update `go` directive to 1.21+~~ | ✅ Done | Bumped to go 1.25.0 |
| 2 | ~~Run `govulncheck` in CI pipeline~~ | ✅ Done | Added security.yml workflow |
| 3 | ~~Enable Dependabot on fork~~ | ✅ Done | Weekly Go modules + monthly Actions |
| 4 | Evaluate replacing `garyburd/go-oauth` with `golang.org/x/oauth2` | Medium | 2-4 hours |
| 5 | ~~Migrate boltdb/bolt to bbolt~~ | ⏸️ Blocked | scep/v2 v2.3.0 requires boltdb/bolt; no bbolt-compatible version exists. No CVEs on boltdb. Will revisit if scep updates. |
| 6 | Replace `pkg/errors` with stdlib `errors`/`fmt.Errorf` | Low | 1 hour |

---

## 4. Maintenance Policy

### Patching Cadence
- **Critical/High CVEs:** Patch within 14 days of disclosure (aligns with CE+ requirements)
- **Medium CVEs:** Patch within 30 days
- **Dependency updates:** Review monthly via Dependabot alerts
- **Go version:** Track supported Go releases, update within 1 release cycle

### Monitoring
- GitHub Dependabot alerts enabled on fork
- `govulncheck` runs on every push via GitHub Actions
- Apple MDM protocol changes monitored via Apple developer release notes

### Change Log
All patches, dependency updates, and configuration changes documented in CHANGELOG.md with dates.

### Responsible Party
James Deane, Iglu Technology — sole maintainer of this fork.

---

## 5. Overall Assessment

**Risk: LOW-MEDIUM**

The codebase is in good shape after initial patching. All critical dependencies are current, Go version is up to date, and automated vulnerability scanning is in place. Remaining concerns:
1. `boltdb/bolt` is archived but blocked by scep/v2 dependency — zero CVEs, embedded DB with no network exposure
2. A few other archived dependencies (gorilla/mux, gorilla/handlers, pkg/errors) — all stable, no CVEs
3. The OAuth library is old but handles a narrow scope

**govulncheck result: ZERO vulnerabilities** (2026-03-18)

For CE+ purposes: this is an internally maintained fork with documented patching process, automated dependency monitoring (Dependabot + govulncheck CI), and a clear maintenance cadence. The software receives security updates as needed.

---

*This report should be updated after each dependency review cycle.*
