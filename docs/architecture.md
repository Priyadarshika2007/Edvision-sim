# EdVision architecture

## Purpose

EdVision is an AI-Guided Adaptive Simulation Platform for Misconception-Driven Science Learning. The three modules are independently owned but connected through the backend and the shared contracts in [`api-contract.md`](./api-contract.md) and [`data-model.md`](./data-model.md).

## Common EdVision flow

```text
Student
  → Prediction
  → Interactive Chemistry Experiment
  → Student Behaviour / Actions
  → Backend
  → AI & Learning Engine
  → Misconception Detection
  → Personalized AI Explanation
  → Adaptive Experiment Decision
  → Backend
  → Frontend
  → Student Retry
  → Verification
  → Mastery Update
```

The frontend records the prediction and meaningful experiment actions. The backend validates and stores those records, sends the relevant evidence to the AI engine, and returns the resulting explanation, adaptation and verification requirements to the frontend. A retry is recorded and verified through the same contract, after which mastery is updated.

## Module responsibilities

### Frontend

The frontend owns:

- React UI and student-facing pages/components.
- Three.js interactive Chemistry simulation.
- Student prediction input and Chemistry experiment interactions.
- Meaningful student-action capture, including action type, target, value and timestamp.
- Experiment result and observation display.
- AI tutor and voice interaction interfaces.
- Adaptive experiment instructions and retry interface.
- Verification and mastery-result display.
- Use of permitted shared 3D assets.

The frontend sends requests to the backend API and does not call the AI engine directly. It renders the generic contract fields without assuming a final Chemistry concept.

### Backend

The backend owns:

- FastAPI API routes and request validation.
- Receiving frontend predictions, actions, questions, retries and verification submissions.
- Storing sessions, actions and results.
- PostgreSQL integration.
- Calling the AI engine with validated, contract-shaped data.
- Returning AI, adaptation, verification and progress results to the frontend.
- Connecting the frontend and AI engine while keeping their transport concerns separate.

The backend is the system boundary for module-to-module communication. It must reject malformed requests explicitly rather than silently filling in missing required information.

### AI engine

The AI engine owns:

- Behavioural evidence analysis and misconception detection.
- Knowledge Graph operations.
- Bayesian Knowledge Tracing (BKT), learner mastery and mastery updates.
- LLM tutor reasoning and personalized explanations.
- Adaptive experiment decisions.
- Verification of improvement.

The AI engine consumes validated generic experiment/session data and returns the common AI response structure. It must not require a Chemistry concept-specific field until the team finalizes that concept.

## Interface boundaries

1. **Frontend ↔ Backend:** HTTP/JSON requests described in [`api-contract.md`](./api-contract.md).
2. **Backend ↔ AI engine:** The backend passes validated session evidence and receives a common AI response. The internal transport is intentionally not fixed yet.
3. **Shared data:** Entity meanings and identifiers are defined in [`data-model.md`](./data-model.md). Contract changes require agreement from all module owners before implementation.

No application endpoints, database schema, or AI implementation is established by this document; this is the agreed design boundary for the MVP.
