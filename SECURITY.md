# Security Policy

## Scope

`eFab` ships **no product code of its own**. It is a manifest repo that
declares which versions of the canonical EmbeddedOS product repositories
to fetch. Vulnerabilities in the *fetched* repos belong to those repos.

| Concern | File here? | Where to report |
|---|---|---|
| Bug in `eAI` inference, agent loop, model loader | No | <https://github.com/embeddedos-org/eAI/security> |
| Bug in `eIPC` transport, HMAC, replay protection | No | <https://github.com/embeddedos-org/eIPC/security> |
| Bug in `eNI` provider, signal processing, decoder | No | <https://github.com/embeddedos-org/eNI/security> |
| Wrong / weak version pin in a manifest | Yes | open a private security advisory on this repo |
| Smoke test that masks a real product CVE | Yes | open a private security advisory on this repo |
| CI workflow that leaks tokens or fetches over plain HTTP | Yes | open a private security advisory on this repo |

## Reporting in scope

Please open a [private security advisory] on this repo. **Do not** file
a public issue for an in-scope vulnerability.

[private security advisory]: https://github.com/embeddedos-org/eFab/security/advisories/new

We aim to acknowledge within 5 working days. Coordinated disclosure
follows the org-wide policy at
<https://github.com/embeddedos-org/.github/blob/main/SECURITY.md>.

## Supply-chain hygiene

- Manifests pin **immutable tags**, not branches. A `rev: master` is a
  bug; please report it.
- The CMake superproject uses `GIT_SHALLOW TRUE` and never disables
  TLS verification.
- CI never runs against PR-author-controlled forks with secrets exposed.
