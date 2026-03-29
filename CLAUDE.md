# Watchtower

## Codebase Overview

Watchtower is a Go daemon that monitors running Docker containers and automatically recreates them when their upstream image is updated. It polls container registries for digest changes, performs a configurable stop → remove → recreate cycle preserving all original container config, and dispatches notifications via shoutrrr.

**Stack:** Go 1.21, Cobra/Viper CLI, Docker SDK v24, shoutrrr notifications, Prometheus metrics, Ginkgo/Gomega tests

**Structure:** `cmd/` (CLI + scheduler), `internal/actions/` (update loop), `internal/flags/` (all CLI flags), `pkg/container/` (Docker client abstraction), `pkg/registry/` (digest auth), `pkg/filters/` (container selection), `pkg/lifecycle/` (hook execution), `pkg/session/` (scan progress tracking), `pkg/notifications/` (shoutrrr + legacy adapters), `pkg/api/` (HTTP API on :8080), `pkg/metrics/` (Prometheus)

For detailed architecture, see [docs/CODEBASE_MAP.md](docs/CODEBASE_MAP.md).
