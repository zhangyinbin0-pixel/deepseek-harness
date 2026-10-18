# Owner–Lead–Consultant collaboration: outcome-based development

Status: proposed owner workflow policy for the Owner fork. This note does not change runtime behavior, grant tool permissions, or supersede repository safety rules.

## Decision

Manage development in coherent, end-to-end capability packages rather than turning individual fields, interfaces, or unit tests into separate Owner approvals. The Owner sets the goal, permitted repository/workspaces, writer scopes, test and deployment boundaries, and acceptance criteria once per package. Within that authorization, the DSH Lead plans, delegates, implements, tests, repairs, and consolidates without requesting approval after every ordinary step.

The ChatGPT collaborator serves as **architecture consultant, escalation reviewer, and package-level acceptance adviser**, not a per-step approver, code writer by default, Owner surrogate, or source of machine permissions. Its technical recommendations are advisory; the Owner alone grants or extends operational authority. The Lead owns execution decisions inside the existing mandate and escalates only material ambiguities or blockers.

## Normal execution loop

1. Define one capability outcome, permitted writer boundary, observable acceptance criteria, rollback conditions, and package-level resource budget.
2. The Lead chooses sequential or parallel agents based on independent work, dependencies, available capabilities, and authorized write isolation. Do not spawn agents merely to satisfy a count.
3. Delegate implementation and routine checking inside approved scopes. Run focused unit, integration, build and regression checks automatically; store compact evidence references instead of generating a separate approval request for every assertion.
4. Address ordinary failures inside the authorized package, without recurring Owner consultations.
5. Return one package acceptance card covering achieved behavior, negative-path checks, actual commands/results, known gaps, and decisions requiring Owner action. Distinguish **execution completed** from **independently accepted**.

## Escalate only when

- A source repository, writer scope, credential, external access, production data, publishing/merge/deployment permission, or other authority is missing or exceeded.
- Concurrent writes lack enforceable isolation, identity assertions cannot be trusted, or a security/safety invariant fails.
- An architectural incompatibility, unexpected destructive change, irreversible operation, unresolved critical test failure, or repeated unsuccessful repair requires a decision.
- The package outcome or acceptance boundary materially changes.

Do not repeatedly reapprove already agreed invariants. An accepted static design or a passing isolated test must not be represented as runtime validation. An Attempt/Receipt alone is not a hard write lease, and task `completed` is not independent `accepted`.

## Verification policy

Use **risk-based evidence**: routine checks are automated and summarized; independently inspect the few failure modes that could invalidate the entire capability. Never silently waive a failing essential gate. Environment-specific failures may be documented with a focused reproduction and their actual scope, not mislabeled as green. Do not repeat passed checks without an identified reason.

Prefer one substantial deliverable and one package-level review to a long cycle of micro-task → proof → permission → next micro-task. Evidence supports a decision; evidence production is not itself the development objective.

## Ownership and scope

- **Owner:** defines objectives, authority, integrations, protected environments, and merge/release decisions.
- **DSH Lead:** autonomously coordinates authorized implementation, agent creation, debugging, routine verification, and result consolidation.
- **Executing agents:** operate only in delegated scopes; cannot self-authorize broader writes or declare their own work independently accepted.
- **Independent evaluator/reviewer:** checks package outcomes and critical negative paths with separation from execution.
- **ChatGPT adviser:** challenges consequential architecture and security decisions, provides options when blocked, and reviews package-level outcomes; does not invent local repository facts or claim to have performed tests.

This process note applies only to the Owner's development workflow in this fork. It is not a claim that the current DSH release already implements autonomous orchestration, lease enforcement, credential binding, or an independent Evaluator.

## Trade-offs and verification

Bigger packages reduce handoff overhead and Owner bottlenecks, but require explicit scope, reproducible evidence, and disciplined rollback. Before adopting this policy as a standing agent instruction, review existing process notes for overlap, confirm its compatibility with [repository instructions](../../../AGENTS.md) and [documentation conventions](../../../docs/AGENTS.md), and run the appropriate docs checks in an authorized source workspace.