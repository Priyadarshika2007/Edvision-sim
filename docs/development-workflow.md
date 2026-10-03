# Development workflow

## Branch ownership

- `main` is the stable branch.
- `frontend` belongs to frontend development.
- `ai-engine` belongs to AI development.
- `backend` belongs to backend development.

## Rules

1. Do not develop directly on `main`.
2. Commit and push changes to the appropriate module branch.
3. Merge changes into `main` through pull requests.
4. All members must follow the API and data contracts before changing interfaces.
5. When an interface change is necessary, update the relevant files in [`docs/`](.) and have all three module owners agree before implementation.

## Typical change flow

1. Create or switch to the branch matching the module being changed.
2. Review [`architecture.md`](./architecture.md), [`api-contract.md`](./api-contract.md), and [`data-model.md`](./data-model.md).
3. Make the smallest complete change within the module boundary.
4. Validate the change using the module's agreed checks when implementation exists.
5. Commit with a clear message and push the branch.
6. Open a pull request into `main` for review.
7. Merge only after the relevant owners confirm contract compatibility.
