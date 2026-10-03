# EdVision — An AI-Guided Adaptive Simulation Platform for Misconception-Driven Science Learning

EdVision is a three-module platform that uses interactive Chemistry experiments, behavioural evidence, and AI-guided adaptation to help students overcome misconceptions.

## Core EdVision loop

Student prediction → interactive Chemistry experiment → meaningful student actions → backend session capture → AI and learning analysis → misconception detection → personalized AI explanation → adaptive experiment decision → student retry → verification → mastery update.

## Repository structure

```text
frontend/       React UI, Three.js simulation, tutor and student interaction surfaces
backend/        FastAPI integration layer, persistence, validation and module coordination
ai-engine/      Misconception analysis, knowledge graph, BKT, tutoring, adaptation and verification
assets/3d/      Shared 3D assets for the simulation
docs/            Shared architecture, API, data and development contracts
```

## Module responsibilities

- **Frontend:** Provides the React UI and interactive Three.js Chemistry simulation; captures predictions and meaningful actions; displays results, adaptive instructions, tutor/voice interactions, retry flows and verification/mastery results.
- **AI engine:** Analyses behavioural evidence, detects misconceptions, maintains knowledge and mastery models, reasons about tutor responses, chooses adaptations and verifies improvement.
- **Backend:** Exposes the FastAPI API, validates and stores sessions/actions/results, integrates PostgreSQL, calls the AI engine and connects the frontend with AI results.

The contracts in [`docs/`](C:/Users/dellb/Desktop/Edvision-sim/docs) are the shared source of truth for interfaces and data.

## Development workflow

`main` is the stable branch. Frontend work belongs on `frontend`, AI work on `ai-engine`, and backend work on `backend`. Do not develop directly on `main`; commit and push changes to the appropriate branch, then merge through a pull request. Follow the shared API and data contracts before changing interfaces.

See [`docs/development-workflow.md`](C:/Users/dellb/Desktop/Edvision-sim/docs/development-workflow.md) for the complete workflow.
