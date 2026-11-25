# ADR-0005: Keep the source open and preserve inherited license obligations

- **Status:** Accepted
- **Date:** 2026-11-25
- **Scope:** Source distribution and legal notices

## Context

The project owner selected an open-source source-code direction. The repository is a fork of QuantumNous/new-api and currently contains its AGPL-3.0 license, NOTICE, third-party license inventory, and additional attribution requirements described in the root README.

## Decision

Keep the source repository open. Preserve the inherited AGPL-3.0 license and all applicable additional terms, attribution notices, copyright headers, upstream identity references, and required visible UI link to the original project.

Do not replace the inherited license with MIT, Apache-2.0, or another license without a reviewed legal basis and authorization from the relevant rights holders.

## Consequences

- Distributed/modified versions must comply with the applicable AGPL-3.0 obligations.
- Preserve the attribution notice and visible original-project link required by the root README.
- Preserve `LICENSE`, `NOTICE`, `THIRD-PARTY-LICENSES.md`, and source headers.
- New dependencies and copied code require license/provenance review.
- This ADR records repository policy; it is not legal advice.
