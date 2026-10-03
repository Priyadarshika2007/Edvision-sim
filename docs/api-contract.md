# EdVision initial API contract

This document defines the initial JSON contract only. The endpoints are not implemented yet. The frontend sends requests to the backend; the backend validates and stores them, calls the AI engine where indicated, and returns responses to the frontend.

## Contract conventions

- JSON field names use `snake_case`.
- Identifiers are opaque strings such as `S001`, `EXP001`, and `STU001`.
- Timestamps are ISO 8601 strings in UTC.
- `experiment_id` and experiment-specific values remain generic until the Chemistry concept is finalized.
- Error responses are outside the initial MVP field contract and must be explicit when the API is implemented.

## Common experiment/session structure

```json
{
  "session_id": "S001",
  "student_id": "STU001",
  "experiment_id": "EXP001",
  "prediction": {
    "answer": "..."
  },
  "actions": [
    {
      "type": "...",
      "target": "...",
      "value": "...",
      "timestamp": "2026-10-03T11:18:00Z"
    }
  ],
  "result": {
    "observations": {}
  }
}
```

`actions` records meaningful student interactions rather than every rendering or pointer event. `result.observations` is deliberately generic.

## Common AI response structure

```json
{
  "misconception": {
    "label": "...",
    "evidence": ["..."]
  },
  "confidence": 0.0,
  "explanation": "...",
  "adaptation": {
    "instruction": "...",
    "changes": {}
  },
  "verification_requirement": {
    "required": true,
    "criteria": ["..."]
  },
  "mastery_update": {
    "status": "...",
    "score": 0.0,
    "change": 0.0
  }
}
```

`confidence` and mastery numeric values are normalized to the range `0.0`–`1.0` when numeric scoring is used. The exact misconception taxonomy and scoring policy remain AI-engine decisions.

## Endpoints

### `POST /api/session/start`

- **Purpose:** Start a student session for a generic experiment.
- **Request fields:** `student_id`, `experiment_id`, optional `prediction` with `answer`, and `started_at`.
- **Response fields:** `session_id`, `student_id`, `experiment_id`, `status`, `started_at`.
- **Request sender:** Frontend.
- **Receiver:** Backend; the backend creates the session record.

Example request:

```json
{
  "student_id": "STU001",
  "experiment_id": "EXP001",
  "prediction": {"answer": "..."},
  "started_at": "2026-10-03T11:18:00Z"
}
```

### `POST /api/experiment/action`

- **Purpose:** Record one meaningful student interaction during an experiment.
- **Request fields:** `session_id`, `experiment_id`, `action` containing `type`, `target`, `value`, and `timestamp`.
- **Response fields:** `session_id`, `action_id`, `accepted`, `recorded_at`.
- **Request sender:** Frontend simulation.
- **Receiver:** Backend; the backend validates and stores the action.

### `POST /api/experiment/submit`

- **Purpose:** Submit the current prediction, actions and observed experiment result for analysis.
- **Request fields:** `session_id`, `experiment_id`, `prediction` with `answer`, `actions`, and `result` with generic `observations`.
- **Response fields:** `session_id`, `analysis_status`, and the common AI response structure: `misconception`, `confidence`, `explanation`, `adaptation`, `verification_requirement`, `mastery_update`.
- **Request sender:** Frontend.
- **Receiver:** Backend, which validates/stores the submission and calls the AI engine.

### `POST /api/tutor/ask`

- **Purpose:** Ask the AI tutor a question in the context of an active session.
- **Request fields:** `session_id`, `student_id`, `message`, optional `voice_input` metadata, and optional `context` containing the latest experiment state.
- **Response fields:** `session_id`, `reply`, optional `voice_output` metadata, `misconception`, `confidence`, and `adaptation` when relevant.
- **Request sender:** Frontend tutor/voice interface.
- **Receiver:** Backend, which validates the interaction and calls the AI engine tutor service.

### `POST /api/experiment/retry`

- **Purpose:** Start a retry using the adaptation returned after analysis.
- **Request fields:** `session_id`, `experiment_id`, `adaptation_id` or the agreed adaptation payload, and `started_at`.
- **Response fields:** `session_id`, `retry_id`, `status`, and `adaptation` containing the instruction and generic changes.
- **Request sender:** Frontend retry interface.
- **Receiver:** Backend; the backend records the retry and returns the active adaptation.

### `POST /api/experiment/verify`

- **Purpose:** Submit retry evidence so the AI engine can verify improvement and update mastery.
- **Request fields:** `session_id`, `retry_id`, `actions`, `result` with generic `observations`, and `submitted_at`.
- **Response fields:** `session_id`, `retry_id`, `verified`, `verification_requirement`, `misconception`, `confidence`, and `mastery_update`.
- **Request sender:** Frontend after the retry.
- **Receiver:** Backend, which stores evidence and calls the AI engine verification service.

### `GET /api/progress/{student_id}`

- **Purpose:** Retrieve a student's current generic progress and mastery summary.
- **Request fields:** Path parameter `student_id`.
- **Response fields:** `student_id`, `mastery` entries containing `experiment_id`, `status`, `score`, and `updated_at`, plus `sessions_completed`.
- **Request sender:** Frontend progress or results view.
- **Receiver:** Backend; the backend reads stored progress and AI-engine mastery results.

## Change control

The endpoint names and shared fields are the initial contract. Any interface change must be discussed by all three module owners, updated here and in [`data-model.md`](./data-model.md), and merged through the workflow in [`development-workflow.md`](./development-workflow.md) before implementation diverges.
