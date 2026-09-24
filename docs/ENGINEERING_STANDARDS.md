# Engineering standards

These standards define the default engineering posture for Websonette repositories. Repository-specific documentation may add constraints, but should not silently weaken these defaults. When a project needs an exception, document the reason, scope, and intended review date close to the decision.

## Priorities

Make trade-offs in this order unless the context justifies a different order:

1. correctness and data integrity;
2. readability and predictable behavior;
3. maintainability and testability;
4. security and operational safety proportional to risk;
5. appropriate extensibility;
6. performance where measurement or known constraints show it matters.

Prefer the smallest design that safely solves the current problem. Future flexibility is valuable only when a likely change is known and the abstraction makes today's code clearer as well.

## Pragmatic SOLID, KISS, and DRY

- Give modules, classes, and functions one coherent reason to change. A unit may coordinate several steps when splitting it would only hide the workflow.
- Keep dependencies explicit and direct them toward stable domain or application contracts. Prefer composition over inheritance.
- Introduce an interface at a real boundary: multiple implementations, an external integration, a public extension point, or a useful test seam. Do not create an interface for every class.
- Extend behavior without editing stable code when the variation is real. Do not build speculative plugin systems or generic frameworks for imagined requirements.
- Prefer straightforward control flow, standard language features, and small public APIs over clever indirection.
- Remove duplication when the repeated code represents the same concept and is likely to change together. Similar-looking code with different meaning may remain separate.
- A little local duplication is usually cheaper than a shared abstraction with unclear ownership.

Factories, managers, repositories, service layers, event buses, and adapters are tools, not default layers. Add one only when it names a real responsibility or isolates a meaningful boundary.

## Design and dependencies

- Keep domain policy separate from delivery mechanisms such as HTTP, templates, persistence, framework callbacks, and build tools.
- Depend on the narrowest stable API that expresses the required behavior.
- Prefer established platform or framework capabilities over custom infrastructure.
- Add a dependency only when its benefit exceeds its security, maintenance, upgrade, and bundle/runtime cost. Record non-obvious choices.
- Keep dependency direction intentional. Higher-level application code may depend on reusable adapters, and adapters may depend on framework-independent foundations; foundations must not depend back on consumers.
- Preserve backward compatibility for published APIs unless a breaking release is explicitly planned and documented.

## Naming and readability

- Use names from the domain and make units reveal intent. Avoid vague buckets such as `Helper`, `Manager`, `Common`, or `Utils` unless the scope is genuinely precise.
- Prefer explicit data shapes and invariants over comments that restate code.
- Keep public APIs small. Make invalid or unsupported states difficult to construct when this does not add disproportionate complexity.
- Comments should explain constraints, trade-offs, or surprising decisions—not narrate ordinary control flow.

## Errors and robustness

- Validate untrusted input at system boundaries and preserve useful context when reporting failures.
- Fail clearly for violated programmer invariants. For expected operational failures, return or throw errors callers can handle deliberately.
- Do not swallow exceptions, expose credentials or internal details to users, or use exceptions as ordinary branching.
- Design robustness in proportion to impact and likelihood. Financial, authentication, authorization, destructive, and data-migration paths require stronger validation, idempotency, observability, and recovery than low-risk presentation code.
- Define timeouts, retry limits, and idempotency before retrying external operations. Avoid retries that can duplicate side effects.
- Prefer safe defaults and graceful degradation where a degraded result remains correct and secure.

## Security and privacy

- Treat all external input, files, URLs, serialized data, and third-party responses as untrusted.
- Apply least privilege to code, CI, credentials, network access, and data exposure.
- Never commit secrets or production data. Keep sensitive values out of logs, error messages, fixtures, and client bundles.
- Use maintained security primitives and framework protections; do not invent cryptography, authentication, authorization, or escaping mechanisms.
- Encode output for its destination and use parameterized database operations.
- Review new dependencies and generated artifacts for supply-chain and publishing risk.
- Record a brief threat assessment when a change introduces authentication, authorization, uploads, payments, destructive operations, public webhooks, or sensitive data.

## Testing

- Test observable behavior and important contracts rather than implementation details.
- Cover happy paths, meaningful boundaries, and failure modes proportional to risk.
- Keep tests deterministic, isolated, readable, and fast enough to run before review.
- Prefer a small number of integration or contract tests at important boundaries plus focused unit tests for non-trivial logic.
- A bug fix should normally include a regression test that fails without the fix.
- Do not mock code merely to satisfy a coverage target. Excessive mocking often signals an unclear boundary.

## Refactoring and change discipline

- Keep changes focused and leave unrelated code alone.
- Separate behavior-preserving refactoring from behavior changes when practical.
- Refactor toward clearer ownership and simpler dependencies, not toward a preferred pattern in the abstract.
- Delete obsolete paths once callers have migrated; do not keep indefinite compatibility layers without an explicit requirement.
- Update documentation, examples, schemas, and tests together with public behavior.
- Make migrations reversible or provide a tested recovery path when rollback would otherwise risk data or availability.

## Performance and operations

- Choose algorithms and data access patterns appropriate to expected scale, but do not optimize from intuition alone.
- Measure before and after non-obvious performance work and document relevant constraints.
- Avoid unbounded work, repeated remote calls, and accidental N+1 access on request paths.
- Prefer operationally simple designs. Add caching, queues, concurrency, or distributed coordination only when requirements or measurements justify them.
- Ensure important failures are diagnosable without logging secrets or excessive personal data.

## Review questions

Before merging, ask:

- Is this the simplest design that is correct and safe for the actual risk?
- Are responsibilities and dependency direction clear?
- Does every abstraction or layer pay for itself today?
- Are names, errors, and public contracts understandable without hidden context?
- Are security, failure, and migration behavior appropriate to the impact?
- Do tests protect the behavior most likely to regress?
- Is any performance complexity supported by evidence?
