# OpenProse help

Answer the requested help topic directly. If the goal is unclear, ask one concise
question; do not force a menu or ask for a path the user already supplied.

- prose run executes a reviewed authorized program using prose.md and the chosen
  state backend. Real host calls/results establish execution.
- prose compile validates against compiler.md without running sessions.
- prose examples lists or explains matching bundled examples; listing is not
  permission to run one.
- prose update prepares/applies only the authorized legacy-state migration.

Common syntax: session, agent, let/const, parallel, repeat/for/loop, if/choice,
try/catch/finally, block and use. Consult compiler.md for actual grammar and
guidance/patterns.md for a requested authoring problem. Inspect the available
example files rather than quoting a stale fixed example count.

OpenProse maps a program to host tool calls and state conventions. Model/harness
portability and backend readiness require actual integration evidence; a claim
that a model can “be the VM” is not a guarantee of tool availability or completion.
Follow [runtime boundaries](guidance/runtime-boundaries.md) before any execution.
