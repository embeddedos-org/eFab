# eFab — Architecture Note

## One-liner

```
ebuild   ──→ how to build a single repo.
eFab     ──→ which repos to build together, at which versions, with what
            integration test, for a named real-world use case.
```

eFab sits **above** `ebuild` and **beside** the canonical 13 product
repos. It owns no source. It owns *recipes*.

## Where it sits

```
                     ┌──────────────────────────────────────────┐
                     │           consumer / integrator          │
                     └───────────────────┬──────────────────────┘
                                         │  "I want an
                                         │   eai-edge stack"
                                         ▼
                     ┌──────────────────────────────────────────┐
                     │  eFab :: manifests/<profile>.yml         │  ← THIS REPO
                     │  pins versions, declares dataflow, owns  │
                     │  the integration test.                   │
                     └───────────────────┬──────────────────────┘
                                         │  expand pins
                                         ▼
   ┌───────────────────────┬─────────────────────┬───────────────────────┐
   │ ebuild                │ ebuild              │ ebuild                │
   │ build eNI@v0.1.0      │ build eIPC@v0.1.0   │ build eAI@v0.1.0      │
   └───────────────────────┴─────────────────────┴───────────────────────┘
                                         │
                                         ▼
                ┌──────────────────────────────────────────────┐
                │  smoke_test.py  ::  ENI → EIPC → eAI         │
                └──────────────────────────────────────────────┘
```

## Profile lifecycle

1. **Propose** — open a PR adding `manifests/<name>.yml`.
2. **Pin** — every `repos[]` entry must declare an immutable tag.
3. **Build** — add `superproject/<name>/CMakeLists.txt` driving
   FetchContent.
4. **Verify** — add `tests/<name>/smoke_test.py` exercising the
   declared dataflow.
5. **Document** — add a page at
   `embeddedos-org.github.io/stacks/<name>.html`.
6. **Release** — minor-bump eFab (`v0.X.0`) when the profile graduates
   green CI on all `known_good_targets`.

## What eFab is *not* responsible for

- Bug-fixing the products it pulls (file in the upstream repo).
- Per-product API documentation (lives in
  `embeddedos-org.github.io/docs/<product>.html`).
- Cross-compile toolchains (that's `ebuild`'s job).
- Per-product release tagging (each repo tags its own `vX.Y.Z`).

## Why a manifest, not a merged repo

A merged `eFab` mega-repo would:

- Force every consumer of `eIPC` (a generic secure-IPC layer) to also
  ship eAI + eNI — sinking adoption.
- Collapse three release cadences into one.
- Break `github.com/embeddedos-org/{eAI,eIPC,eNI}` inbound links.
- Delete useful test isolation.

A manifest repo gets the "single install" benefit without any of those
costs. See the original design rationale in the project's
[architecture-decision discussion thread](https://github.com/embeddedos-org/eFab/discussions).
