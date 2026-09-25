# Ruflo Owl Architecture Study

This branch is an isolated research workstream for studying Ruflo as an agent meta-harness without modifying the fork's clean `main`.

## Baseline

- Fork: `Opiuz/ruflo`
- Research branch: `owl/ruflo-architecture-study`
- Upstream-derived baseline commit: `88955d9fa9c60836a3f9a3d0e47fcabdc5af4ff5`
- Baseline commit message: `fix(cli): pin @claude-flow/memory exactly and warn in doctor on a stale copy (#3392)`
- Root package version observed at study start: `3.45.0`

## Objective

Determine what Ruflo actually implements in code, distinguish implementation from documentation/claims, and extract architecture lessons that can be compared against our own harness work.

The study focuses on:

1. CLI/bootstrap and runtime composition
2. Task/model/capability routing
3. Swarm coordination and agent lifecycle
4. Scheduler, workers, autonomy and goal execution
5. MCP/tool registration and authorization boundaries
6. Memory, AgentDB, RAG and persistence
7. ReasoningBank and trajectory learning
8. SONA and any online/self-optimization path
9. Local model / Ollama routing
10. MetaHarness integration and harness self-evaluation
11. Federation, identity, transport and trust
12. Security controls, secrets, supply chain and plugin boundaries
13. Observability, evidence, verification and CI
14. Plugin architecture and extension model
15. Failure recovery, rollback and regression controls

## Research rules

- `main` remains untouched.
- Every architectural claim should point to code, a test, an ADR, or runtime evidence.
- README/marketing statements are treated as claims until implementation evidence is found.
- "Implemented", "documented", "proposed", "optional", "stubbed" and "runtime verified" are different states.
- Security-sensitive behavior is not executed against external services unless explicitly approved.
- No dependency-install or postinstall scripts are trusted by default.
- We prefer static analysis first, then controlled local execution.

## Initial implementation landmarks

The first inspection found the following concrete surfaces:

- CLI command registry: `v3/@claude-flow/cli/src/commands/index.ts`
- ReasoningBank implementation: `v3/@claude-flow/neural/src/reasoning-bank.ts`
- ReasoningBank hooks path: `v3/@claude-flow/hooks/src/reasoningbank/index.ts`
- SONA MCP tooling: `v3/mcp/tools/sona-tools.ts`
- Persistent SONA memory: `v3/@claude-flow/memory/src/persistent-sona.ts`
- Coherence scheduler / economic governor: `v3/@claude-flow/guidance/src/coherence.ts`
- MetaHarness hard-dependency decision: `v3/docs/adr/ADR-321-metaharness-hard-dependency.md`
- ChatGPT federation connector decision: `v3/docs/adr/ADR-387-chatgpt-federation-connector.md`
- Root dependency and workspace contract: `package.json`

## Early observations

These are provisional and require deeper tracing:

- Ruflo is not a single monolithic harness; it is a large composition of CLI, MCP, plugins, neural/memory packages, ADR-governed features and optional/native dependencies.
- ReasoningBank exists as executable TypeScript, not merely documentation.
- SONA exposes trajectory-oriented MCP surfaces and has a persistence path.
- A concrete coherence scheduler computes a score and maps it to privilege levels, which is directly relevant to boundary-based harness control.
- MetaHarness integration has evolved from optional augmentation to an accepted hard-dependency decision for selected packages.
- Federation has explicit identity, key-custody and OAuth decisions, including separation of model-visible arguments from transport credentials.

## Study phases

### Phase 1 — Static topology
Map packages, entry points, dependency edges, runtime bootstrap, plugin registration and command surfaces.

### Phase 2 — Execution paths
Trace representative flows from user/CLI/MCP input through router, swarm/agent dispatch, tools, memory and result handling.

### Phase 3 — Learning and memory
Trace trajectory capture, verdicts, pattern distillation, retrieval, consolidation, persistence and mutation authority.

### Phase 4 — Security and trust
Trace tool authorization, secret flow, MCP boundaries, federation identity, plugin trust, install/update paths and dependency supply chain.

### Phase 5 — Verification
Run controlled tests against selected subsystems only after static review of install/build scripts and dependencies.

### Phase 6 — Comparative analysis
Create an evidence-based comparison of Ruflo mechanisms against our harness architecture, identifying reusable patterns, risks and gaps.

See `ARCHITECTURE_MAP.md` and `EVIDENCE_LEDGER.md` for the living technical record.
