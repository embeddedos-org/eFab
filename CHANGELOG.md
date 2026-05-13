# Changelog

All notable changes to **eFab** (the EmbeddedOS stack-fabricator manifest
repo) are documented in this file. Format follows [Keep a Changelog],
versioning follows [SemVer].

[Keep a Changelog]: https://keepachangelog.com/en/1.1.0/
[SemVer]: https://semver.org/

## [0.1.0] — Initial release

### Added
- Repo skeleton: README, LICENSE (MIT), .gitignore, CHANGELOG, SECURITY.
- First profile: **eai-edge** (`manifests/eai-edge.yml`).
  - Pins `eNI v0.1.0`, `eIPC v0.1.0`, `eAI v0.1.0`.
  - Declared dataflow: `ENI ➜ EIPC ➜ eAI`.
  - Known-good targets: host-x86_64-linux, host-x86_64-darwin,
    host-aarch64-linux, target-stm32f4 (build only).
- CMake superproject for the eai-edge profile
  (`superproject/eai-edge/CMakeLists.txt`) using `FetchContent`.
- End-to-end smoke test (`tests/eai-edge/smoke_test.py`) that exercises
  the dataflow over an in-process loopback transport.
- CI workflow (`.github/workflows/ci.yml`) running build + smoke on
  ubuntu-latest and macos-latest.
- Org-wide canon validator wired to `.github/workflows/check-canon.yml`.

### Notes
- eFab is a meta-repo. It is excluded from the canonical 13-product /
  14-book counts. Adding eFab does **not** alter the canon. The check-
  product-canon.py script is included so the repo's own README and
  documentation are scanned for accidental drift.
- No source code from eAI / eIPC / eNI is vendored in this repo; the
  superproject fetches each at the pinned tag at configure time.

### Roadmap
- v0.2 — `embedded-core` profile (eos + eBoot + ebuild).
- v0.3 — `smart-edge` profile (embedded-core + eAI + eIPC, no BCI).
- v0.4 — `devkit` profile (ebuild + EoSim + EoStudio).
- v1.0 — `full-system` profile + per-profile per-target build matrix in CI.
