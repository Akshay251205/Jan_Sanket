# CivicPulse

Multilingual Civic Complaint Intelligence & NLP Research Platform — a
local-first university project for English, Hindi and Hinglish.

## Current state

The supplied datasets were inspected rather than assumed; the audit is in
[docs/DATASET_AUDIT.md](docs/DATASET_AUDIT.md). The runnable MVP includes a
React research interface, FastAPI/SQLite citizen-and-admin workflow, protected
status history, safe dataset inventory, local multilingual preprocessing,
trainable small NumPy CBOW/Skip-gram embeddings, attention visualisation,
extractive summarisation and a reproducible civic TF-IDF baseline evaluator.

## Run locally

Create an isolated Python environment, install `requirements.txt`, copy
`.env.example` to `.env` and set a strong `SECRET_KEY`, then run the API:

```powershell
uvicorn backend.app.main:app --reload
```

Open `http://127.0.0.1:8000/docs` for the API interface. Run the available
tests with:

```powershell
python -m pytest backend/tests -q
```

In a second terminal, start the interface:

```powershell
cd frontend
npm install
npm run dev
```

Open `http://127.0.0.1:5173`. Register an account; no default administrator is
created. For an intentional local demo administrator, set
`DEMO_ADMIN_EMAIL` and `DEMO_ADMIN_PASSWORD`, then run
`python scripts/create_demo_admin.py`.

`requirements.txt` is the lightweight, working core. The optional
`requirements-ml-optional.txt` is reserved for future CPU neural experiments;
the current runnable demonstration deliberately does not require a large model
download.

## Dataset safeguards

- `datasets/raw/source_originals/` is immutable.
- Hindi sentiment and NER retain their supplied splits.
- NER JSON and CoNLL are alternatives, not additional data.
- Translation data has development and test pairs only; neither is silently
  treated as a training set.
- Civic transcript provenance, licences and synthetic status are unconfirmed.

See [docs/IMPLEMENTATION_CHECKLIST.md](docs/IMPLEMENTATION_CHECKLIST.md) and
[docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) for the phased plan. Also consult
[docs/KNOWN_LIMITATIONS.md](docs/KNOWN_LIMITATIONS.md) before presenting the
research outputs as results.
