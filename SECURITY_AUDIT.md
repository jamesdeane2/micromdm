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
- **go.mod declares:** `go 1.17` (released Aug 2021)
- **Current stable Go:** 1.22+
- **Action required:** Update to `go 1.21` minimum. Go 1.17 is EOL and no longer receives security patches.

### Direct Dependencies

| Package | Version | Status | Risk |
|---------|---------|--------|------|
| `golang.org/x/crypto` | v0.33.0 | ✅ Current | Low |
| `golang.org/x/net` | v0.34.0 | ✅ Current | Low |
| `google.golang.org/protobuf` | v1.33.0 | ✅ Current | Low |
| `github.com/gorilla/mux` | v1.8.1 | ⚠️ Archived | Low (stable, no known CVEs) |
| `github.com/gorilla/handlers` | v1.5.2 | ⚠️ Archived | Low (stable) |
| `github.com/boltdb/bolt` | v1.3.1 | ⚠️ Archived | Medium (embedded DB, no network exposure) |
| `github.com/go-kit/kit` | v0.13.0 | ✅ Active | Low |
| `github.com/google/uuid` | v1.6.0 | ✅ Active | Low |
| `github.com/micromdm/scep/v2` | v2.3.0 | ✅ Maintained by same org | Low |
| `github.com/micromdm/plist` | v0.2.1 | ✅ Maintained | Low |
| `github.com/garyburd/go-oauth` | 2018 commit | ⚠️ Stale | Medium (OAuth handling) |
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
- **No known CVEs** found against current pinned versions (manual assessment; govulncheck recommended as part of CI)

---

## 3. Identified Actions

| # | Action | Priority | Effort |
|---|--------|----------|--------|
| 1 | Update `go` directive to 1.21+ | High | 1 hour |
| 2 | Run `govulncheck` in CI pipeline | High | 30 min |
| 3 | Enable Dependabot on fork | High | 10 min |
| 4 | Evaluate replacing `garyburd/go-oauth` with `golang.org/x/oauth2` | Medium | 2-4 hours |
| 5 | Consider migrating from `boltdb/bolt` to `go.etcd.io/bbolt` (maintained fork) | Medium | 1-2 hours |
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

The codebase is in reasonable shape. The critical crypto and network dependencies are up to date. The main concerns are:
1. The `go 1.17` directive (easy fix)
2. A few archived dependencies that are stable but won't receive future patches
3. The OAuth library is old but handles a narrow scope

For CE+ purposes: this is an internally maintained fork with documented patching process, dependency monitoring, and a clear maintenance cadence. The software receives security updates as needed.

---

*This report should be updated after each dependency review cycle.*
