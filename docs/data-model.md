# EdVision shared data model

These entities describe the shared language between the frontend, backend and AI engine. Storage tables and implementation classes may differ, but their contract meanings must remain compatible.

## Student

Represents a learner.

- `student_id`: stable opaque identifier.
- `display_name`: optional student-facing name.
- `created_at`: ISO 8601 creation timestamp.

## Session

Represents one learner run through an experiment, including retry evidence.

- `session_id`: stable opaque identifier.
- `student_id`: owning student.
- `experiment_id`: generic experiment identifier.
- `prediction`: initial [`Prediction`](#prediction).
- `actions`: ordered [`Action`](#action) records.
- `result`: current [`Result`](#result).
- `status`: session lifecycle status.
- `started_at`, `completed_at`: timestamps.

## Experiment

Represents a generic Chemistry learning experience.

- `experiment_id`: stable opaque identifier.
- `title`: optional display title.
- `domain`: Chemistry for the MVP.
- `concept`: intentionally not finalized yet.
- `version`: contract/content version.

## Action

Represents a meaningful student interaction with the simulation.

- `action_id`: backend-generated identifier.
- `type`: generic action category.
- `target`: generic simulation target.
- `value`: JSON-compatible generic value.
- `timestamp`: event time.

## Prediction

Represents the student's pre-experiment expectation.

- `answer`: student-provided generic answer.
- `submitted_at`: timestamp.

## Result

Represents observations produced by an experiment attempt.

- `observations`: generic JSON object.
- `outcome`: optional generic outcome label.
- `submitted_at`: timestamp.

## Misconception

Represents an inferred incorrect model or reasoning pattern.

- `label`: AI-engine taxonomy label.
- `evidence`: behavioural or result evidence supporting the inference.
- `confidence`: normalized confidence from `0.0` to `1.0`.

## TutorInteraction

Represents one student question and AI tutor response.

- `interaction_id`: stable identifier.
- `session_id`: related session.
- `message`: student text or transcribed voice input.
- `reply`: tutor response.
- `input_mode`: text or voice.
- `created_at`: timestamp.

## Adaptation

Represents the next learning action selected for the student.

- `adaptation_id`: stable identifier.
- `instruction`: student-facing generic instruction.
- `changes`: generic experiment changes.
- `reason`: optional explanation for the selected adaptation.

## Mastery

Represents the student's current learning state for an experiment or concept.

- `student_id`: owning student.
- `experiment_id`: related experiment.
- `status`: generic mastery status.
- `score`: normalized score from `0.0` to `1.0` when available.
- `change`: update from the latest evidence.
- `updated_at`: timestamp.

## Relationships

```text
Student 1 ── * Session * ── 1 Experiment
Session 1 ── 1 Prediction
Session 1 ── * Action
Session 1 ── * Result
Session 1 ── * TutorInteraction
Session 1 ── * Adaptation
Student 1 ── * Mastery * ── 1 Experiment
```

The API payload examples and response fields are defined in [`api-contract.md`](./api-contract.md).
