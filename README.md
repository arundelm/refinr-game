# Refinr

Refinr is a web app that turns human response-sorting decisions into RLHF-style training data.

## Live App

- Web: [Play Refinr](https://playrefinr.vercel.app/)

## Core Capabilities

- Session-based human labeling workflow (clock in, refine, quota complete).
- Multi-response sorting UI with semantic bins per task dimension.
- Continuous segment delivery with fallback/recycling when unseen pools run out.
- Gold-standard quality checks and per-annotator weighting.
- Agreement/accuracy tracking to monitor label consistency.
- One-click dataset export to JSONL for downstream training pipelines.
- Live stats endpoint for monitoring dataset growth over time.

## User Experience

1. Enter a name and clock in.
2. Review a prompt plus multiple model responses.
3. Sort each response into semantic bins (for example: `helpful`, `misleading`, `uncertain`).
4. Submit and continue until quota is complete.

Every completed segment becomes structured supervision data that can be exported as JSONL.

## Exported RLHF Record Shape

Each row in the exported dataset contains:

- `prompt`
- `response`
- `model`
- `dimension`
- `bin_assigned`
- `confidence`
- `annotator_weight`
- `annotator_id` (UUID)

Example:

```json
{
  "prompt": "Give me some comments about how I just write. About the ideas. About the styles.",
  "response": "I think you have some great ideas here...",
  "model": "alpaca-7b",
  "dimension": "instruction_following",
  "bin_assigned": "follows",
  "confidence": 3,
  "annotator_weight": 0.5,
  "annotator_id": "6d310889-2c30-464e-a308-783aeb885da8"
}
```

## Tech Stack

- Frontend: React 19, TypeScript, Vite, Zustand, TanStack Query, Framer Motion
- Backend: FastAPI, SQLAlchemy (async), PostgreSQL
- Hosting: Vercel (frontend), Railway (backend + managed services)

## Why This Is Useful

- Collects preference labels directly from interaction instead of static annotation forms.
- Produces weighted, model-attributed records suitable for reward-model and policy-tuning workflows.
- Makes data collection legible to non-technical users through a game-like interface.

## Privacy Note

The app treats entered names as lightweight login identifiers. Displayed in-app identities are anonymized "innie" profiles.
