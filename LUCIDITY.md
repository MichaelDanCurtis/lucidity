# Lucidity System Map

Last updated: 2026-06-15

This directory is a multi-repo product family. Keep the repos separate unless a
specific implementation plan says otherwise. The boundaries are part of the
architecture.

## Session Bootstrap

When starting an AI/code session anywhere under this directory:

1. Read this file first.
2. Read the target repo's `AGENTS.md` if present.
3. Read the target repo's `README.md`.
4. Read the target repo's `ROADMAP.md` if present.
5. For cross-repo work, state which repos are being touched before editing.

Do not assume a repo owns a subsystem just because it currently incubates code
for it. Incubation can be temporary while contracts stabilize.

## Repo Map

| Repo | Role | Notes |
| --- | --- | --- |
| `Galvo/` | Agent orchestration, agent negotiation, A2A-facing behavior, and collaboration workflows. | Galvo should use GalvoFS for workspaces and evidence, but should not own GalvoFS internals. |
| `GalvoFS/` | Shared filesystem/storage substrate for Femto compute and Galvo agents. | Owns GalvoFS core, storage inputs, leases, manifests, receipts, snapshots, artifacts, and connector contracts. |
| `Femto/` | Agent-native execution fabric, A2E server, routing, runtime adapters, and compute receipts. | Consumes GalvoFS through a local path dependency during active development. |
| `tessera/` | Vault, key management, trust decisions, unwrap policy, certificates, revocation, and audit. | This is the trust substrate formerly discussed as RustyVault-shaped capability. |
| `KubeNyx/` | Kubernetes intelligence, operations, scheduling, GPU cluster posture, and Kubernetes handoff. | Femto should call or hand off to KubeNyx for live Kubernetes execution rather than becoming KubeNyx. |
| `galvo-memory/` | Memory/evidence subsystem for Galvo-family workflows. | Keep memory and evidence semantics aligned with GalvoFS receipts where they overlap. |
| `GalvoLite/` | Lightweight Galvo-facing app/workbench surface. | Treat as a product/UI repo unless current docs say otherwise. |
| `Prizmanyx/` | Separate infrastructure/migration/discovery product. | Related under Lucidity, but not part of the Femto/Galvo/GalvoFS execution loop by default. |

## GalvoFS Placement Decision

GalvoFS is now its own repo. The extraction happened after Storage Inputs V1
landed for local-path imports and uploaded-object promotion.

Current placement:

- GalvoFS core lives in `GalvoFS/crates/galvofs-core`.
- Connector-side GalvoFS planning code lives in `GalvoFS/connectors/galvofs`.
- Femto consumes GalvoFS by local path and keeps only integration code.
- Lucidity tracks `GalvoFS/` as a submodule-style child repo pinned to the
  GalvoFS repository commit.
- The source of truth for near-term GalvoFS work is `GalvoFS/ROADMAP.md` and
  `GalvoFS/docs/architecture/galvofs-shared-filesystem.md`.

Target growth shape:

```text
lucidity/
  GalvoFS/
    crates/galvofs-core
    crates/galvofs-server
    crates/galvofs-client
    connectors/
      csi/
      nfsd/
      storage-inputs/
```

Do not put GalvoFS under `Galvo/`. GalvoFS is shared infrastructure for Galvo
agents and Femto compute. If it lives under `Galvo/`, Femto becomes a downstream
consumer of an agent repo, which is the wrong ownership boundary.

## Subsystem Boundaries

### Galvo

Galvo owns:

- agent task negotiation;
- A2A-facing behavior;
- sub-agent orchestration;
- collaboration semantics;
- agent-visible workspace/evidence workflows.

Galvo does not own:

- runtime placement;
- Kubernetes or Slurm execution;
- storage object durability;
- key unwrap or trust decisions.

### Femto

Femto owns:

- A2E admission and routing;
- runtime adapter contracts;
- work order validation, planning, submission, cancellation, and metering;
- execution receipts and run state;
- handoff to KubeNyx, Slurm, workspace, function, and model runtimes.

Femto does not own:

- Galvo agent negotiation;
- Tessera key/trust policy internals;
- KubeNyx cluster operations internals;
- permanent ownership of GalvoFS after extraction.

### GalvoFS

GalvoFS owns:

- shared filesystem vocabulary for Galvo and Femto;
- workspaces, snapshots, clones, diffs, rollback;
- storage inputs, mount manifests, leases, artifacts, and storage receipts;
- backend-independent object/body storage semantics;
- connector-ready access endpoint metadata.

GalvoFS does not own:

- agent task semantics;
- runtime routing;
- trust-store authority;
- Kubernetes operations;
- Slurm site policy;
- HPC hot scratch filesystems.

### Tessera

Tessera owns:

- key management;
- trust and unwrap decisions;
- certificate profiles;
- revocation;
- audit trails for key/trust actions.

Tessera does not own:

- compute routing;
- filesystem metadata;
- agent task semantics.

### KubeNyx

KubeNyx owns:

- Kubernetes cluster intelligence;
- Kubernetes operational posture;
- GPU scheduling context;
- Kubernetes object/application of Femto handoff decisions.

KubeNyx does not own:

- A2E work order semantics;
- GalvoFS storage semantics;
- Galvo agent negotiation.

## Recommended Working Pattern

Use independent repos with a thin Lucidity coordination layer:

```text
lucidity/
  LUCIDITY.md
  AGENTS.md
  lucidity.code-workspace
  GalvoFS/
  Femto/
  Galvo/
  tessera/
  KubeNyx/
  galvo-memory/
  GalvoLite/
```

Keep GalvoFS shared code in `GalvoFS/`. Femto and Galvo should consume
GalvoFS through local path dependencies during active development, then through
versioned git or registry dependencies when the API stabilizes.

## Cross-Repo Rules

- Do not silently change multiple repos in one task. Name the repos first.
- Keep shared protocol contracts in the owning repo and link to them from
  consumers.
- Prefer generated clients or narrow shared crates over copy/pasted protocol
  structs.
- If a subsystem is shared by Galvo and Femto, it should not live permanently
  inside either one.
- If a subsystem is deployment-specific, put the core contract in the owning
  repo and the runtime apply code in a connector/operator boundary.
