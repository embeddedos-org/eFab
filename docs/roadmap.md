# eFab — Profile Roadmap

eFab v0.1.0 ships exactly **one** profile (`eai-edge`). The roadmap below
captures the next batches. Each profile graduates into eFab only after:

1. A working manifest YAML.
2. A FetchContent-based CMake superproject.
3. An end-to-end smoke test exercising the declared dataflow.
4. Green CI on at least one host target and one embedded target.
5. A site page at `embeddedos-org.github.io/stacks/<name>.html`.

| Version | Profile | Repos pulled | Use case | Status |
|---|---|---|---|---|
| **v0.1.0** | **eai-edge** | eNI + eIPC + eAI | Intelligent edge node with neural-interface input. `ENI ➜ EIPC ➜ eAI`. | ✅ Shipped |
| v0.2 | embedded-core | eos + eBoot + ebuild | Minimum boot-and-run for any embedded target. | Planned |
| v0.3 | smart-edge | eos + eBoot + ebuild + eIPC + eAI | Production AI on MCU / Cortex-A; no BCI. | Planned |
| v0.4 | devkit | ebuild + EoSim + EoStudio | Desktop developer tooling stack. | Planned |
| v0.5 | appsuite | eApps + eOffice + eBrowser + eDB | User-facing applications stack (server + browser + office + DB). | Planned |
| v1.0 | full-system | All canonical 13 (sans dev tools) | Reference deployment of the entire EmbeddedOS runtime. | Planned |

## Out of scope (explicitly)

- A `kids-mode` profile — that's content, not a product bundle.
- A `hardware-only` profile — `eCAD-Hardware-Products` ships KiCad
  artefacts, not buildable C; manifests don't help here.
- A `books` profile — books are PDF artefacts, not a build graph.
- A `meta` profile bundling other meta-repos (`embeddedos-org`,
  `embeddedos-org.github.io`) — circular and pointless.

## Cadence

eFab patch releases (`v0.1.x`) bump only the `rev:` pins inside an
existing manifest. eFab minor releases (`v0.X.0`) add a new profile.
eFab major releases (`v1.0.0` onward) change the manifest schema.
