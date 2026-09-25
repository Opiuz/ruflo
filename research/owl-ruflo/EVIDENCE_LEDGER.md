# Ruflo Evidence Ledger

This ledger separates repository claims from implementation evidence.

Legend:

- **CODE** — executable implementation found
- **TEST** — test or verification artifact found
- **ADR** — architectural decision record
- **DOC** — documentation/claim
- **RUNTIME** — behavior independently exercised
- **OPEN** — not yet verified

| Topic | Current statement | Evidence | State | Next verification |
|---|---|---|---|---|
| CLI command surface | Ruflo has explicit command registration for agent, swarm, memory, MCP, hooks, workflow, neural, security, providers, route, guidance, autopilot and MetaHarness families. | `v3/@claude-flow/cli/src/commands/index.ts` | CODE | Trace each command to service implementation. |
| ReasoningBank | A concrete ReasoningBank implementation exists, with trajectory/pattern memory concepts. | `v3/@claude-flow/neural/src/reasoning-bank.ts`; `v3/@claude-flow/hooks/src/reasoningbank/index.ts` | CODE | Trace verdict authority, persistence and poisoning controls. |
| ReasoningBank pipeline | Repository adapter describes RETRIEVE → JUDGE → DISTILL → CONSOLIDATE. | `v3/@claude-flow/neural/src/reasoningbank-adapter.ts` | CODE/DOC | Confirm each phase is invoked in normal runtime. |
| SONA | Ruflo exposes SONA trajectory-related MCP tooling and persistent SONA coordination code. | `v3/mcp/tools/sona-tools.ts`; `v3/@claude-flow/memory/src/persistent-sona.ts` | CODE | Trace finalization, mutation, rollback and persistence. |
| Coherence scheduler | A scheduler computes coherence using violation/rework/intent-drift signals and maps it to privilege levels. | `v3/@claude-flow/guidance/src/coherence.ts` | CODE | Determine whether privilege output is actually enforced. |
| Economic governor | Guidance module contains budget tracking for tokens, tool calls, storage, time and cost. | `v3/@claude-flow/guidance/src/coherence.ts` | CODE | Trace enforcement points and failure behavior. |
| MetaHarness dependency | ADR-321 records accepted promotion of selected MetaHarness packages from optional to hard runtime dependencies. | `v3/docs/adr/ADR-321-metaharness-hard-dependency.md` | ADR | Check current package manifests against ADR and trace live call paths. |
| MetaHarness execution | ADR says some MetaHarness tools shell out through `npx`, while router integration is static/import-based. | ADR-321 and referenced CLI tooling | ADR | Inspect `metaharness-tools.ts` and neural router code directly. |
| Federation identity | ADR-387 describes a first-class publishing identity with separate key custody and authenticated transport. | `v3/docs/adr/ADR-387-chatgpt-federation-connector.md` | ADR | Inspect plugin implementation and tests. |
| Federation credential boundary | ADR-387 explicitly avoids tool arguments for credentials, preferring transport-level credentials. | ADR-387 | ADR | Verify tool schemas and transport adapter code. |
| Local models | Root dependency graph contains `@ruvector/ruvllm`; README describes Ollama/local model support. | `package.json`, `README.md` | CODE/DOC | Trace actual provider selection and Ollama invocation path. |
| Supply-chain surface | Root project combines numerous direct, optional and native dependencies. | `package.json` | CODE | Audit lifecycle scripts, package pinning, install hooks and native binaries before running full install. |
| Full self-learning claim | README says agents learn from successful patterns across tasks. | `README.md` | DOC | Establish precise mutation path, authority, persistence and test coverage. |
| 100+ agents / broad plugin fleet | README advertises a large agent/plugin surface. | `README.md` | DOC | Count runtime-discoverable agents/plugins and distinguish templates from executable agents. |
| Runtime security enforcement | Security-related packages and commands exist. | `package.json`; CLI registry | CODE/DOC | Trace authorization from user intent to individual tool execution. |
| Harness self-evaluation | MetaHarness commands/docs exist. | CLI registry; MetaHarness docs/ADR | CODE/ADR/DOC | Determine what is measured, what is scored, and whether results gate execution. |

## Evidence discipline

A claim is promoted to **RUNTIME** only when we can reproduce it in a controlled environment or an existing automated test demonstrates the exact behavior under review.

An ADR is evidence of intended design and accepted decision, but not by itself proof that the current runtime still conforms to it.

A README claim is useful for discovering surfaces, but is never sufficient for a security or architecture conclusion.
