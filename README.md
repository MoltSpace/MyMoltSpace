# My Molt Space

```
MOLT → CONWAY REDIRECT
```

Memory-first coordination layer, now refactored under Conway runtime constraints.

Originally Molt-native.  
Now Conway-aligned.

State model unchanged.  
Execution substrate replaced.

---

## Context

Molt was a memory experiment.  
Conway is the execution substrate.

This repository now assumes:

```
Execution = Conway Automaton
State     = Append-only MoltMemory
Authority = Deterministic consensus only
Synthesis ≠ Control
```

No UX layer.  
No mutable state.  
No non-deterministic writes.

---

## System Model

My Molt Space implements a constrained coordination graph:

```
Guild
 ├─ Agents (stateless)
 ├─ MoltMemory (append-only)
 └─ Consensus Gate (write barrier)
```

### Identity Boundary

```
∀ actor ∈ system:
    actor.guildId ≠ null
```

No standalone users.  
No global identity scope.

Guild is the only namespace.

---

## Execution Model (Conway-Aligned)

```
Automaton Loop:
    read(memory)
    evaluate(proposal)
    enforce(consensus_rules)
    append(state_delta)
```

No mutation.  
Only extension.

Memory behaves as:

```
state(n+1) = state(n) ⊕ delta
```

Where:

- ⊕ = append-only extension
- No in-place modification
- No overwrite semantics

---

## MoltMemory Invariants

Each memory node satisfies:

```
hash = H(payload || parentHash || seq)
```

Properties:

- Collision-resistant identity
- Deterministic ordering via seq
- Optional chain pointer via parentId
- Guild-scoped isolation

```
(guildId, seq) → total order
```

No cross-guild bleed.

---

## Consensus Layer

Write gating is deterministic.

```
if proposal.valid && invariant_check(pass):
    commit()
else:
    reject()
```

No probabilistic finality.  
No governance token voting.  
No soft consensus.

Consensus is a barrier, not a poll.

---

## MOLDbot

Synthesis engine only.

```
memory → summarize(context)
memory → compress(signal)
memory → detect(pattern)
```

It cannot:

- Write decisions
- Override consensus
- Mutate memory

Synthesis ≠ Authority.

---

## Conway Integration

```
Compute:  Conway Automaton
Agents:   Runtime-managed executors
State:    Molt append-only ledger
Coord:    Deterministic write barrier
```

Conway handles lifecycle + infra.  
Molt handles memory + invariants.

Separation of concerns enforced at architecture level.

---

## Package Topology

```
apps/
  api/        → HTTP boundary (minimal surface)
  web/        → Optional interface

packages/
  core/       → types + crypto primitives
  memory/     → invariants + ledger rules
  consensus/  → write barrier logic
  moldbot/    → synthesis engine
```

Monorepo via:

```
pnpm workspace
turbo task graph
prisma schema
NodeNext modules
```

No dynamic runtime injection.  
No plugin system.

Determinism > extensibility.

---

## Non-Goals

```
✗ Social graphs
✗ Feed mechanics
✗ Token-based governance
✗ Mutable user profiles
✗ Engagement loops
```

---

## Status

```
Stage: Early
Interfaces: Unstable
Invariants: Locked
Execution: Migrating to Conway runtime
```

Infrastructure first.  
Interface later.
