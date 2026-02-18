
# Conway Integration Layer

This repository now includes a Conway execution overlay.

MoltMemory remains the state substrate.
Conway Automaton becomes the execution substrate.

State      → Molt (append-only ledger)
Execution  → Conway Automaton
Consensus  → Deterministic barrier

No mutation of legacy modules.
Only runtime extension.
