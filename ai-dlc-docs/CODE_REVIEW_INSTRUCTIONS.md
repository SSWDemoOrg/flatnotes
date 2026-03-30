# Code review instructions

Team-specific rules for reviews. Loaded together with every `memory-bank/standards/*.md` file.

## Scope of review

- Default: all files in the provided change set (diff or path list).
- Extend or narrow per PR (e.g. exclude generated code) in the review request.

## Severity

- **Must fix**: *(define team bar—e.g. correctness, security, compliance, broken build.)*
- **Should fix**: *(e.g. maintainability, missing tests for risky logic.)*
- **Nit**: *(style, naming, minor refactors.)*

## Security and privacy

- *(e.g. authz checks, PII handling, secret scanning expectations.)*

## Tests

- *(e.g. required coverage for new modules, integration vs unit expectations.)*

## Performance

- *(e.g. latency budgets, query patterns, allocation hot paths.)*

## Style alignment with standards

- Follow the Markdown standards in `memory-bank/standards/`; call out deviations with a short reference to the rule title or section.

## PR hygiene

- *(e.g. size limits, conventional commits, required links to issues or `feature.md`.)*
