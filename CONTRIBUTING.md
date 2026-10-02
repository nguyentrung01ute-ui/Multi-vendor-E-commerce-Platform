# Contributing

## Branches

- `main`: stable branch
- `develop`: integration branch
- `feature/*`: new features
- `fix/*`: bug fixes
- `refactor/*`: refactoring
- `test/*`: tests
- `docs/*`: documentation
- `chore/*`: maintenance

## Pull Requests

1. Create a branch from `develop`.
2. Implement one focused change.
3. Run tests and formatting checks locally.
4. Open a PR into `develop`.
5. Request review when the change is ready.
6. Merge only after CI passes and review requirements are satisfied.

## Commit Messages

Follow Conventional Commits:

```text
feat: add product creation API
fix: prevent inventory overselling
refactor: extract payment service
test: add checkout integration tests
docs: update API documentation
chore: update dependencies
```

## Development Principles

- Keep module boundaries clear.
- Do not access another module's repository directly.
- Keep DTOs separate from persistence entities.
- Validate ownership at the service layer.
- Keep transactions explicit.
- Never commit secrets or local environment files.
- Add tests for important business rules and concurrency-sensitive code.
