# FlowFreeze analyst frontend

A React + TypeScript + Vite analyst console for the existing FastAPI, SQLite, and synthetic-data project. The interface is a **decision-support demonstration**, not an upay BD production service.

## Run locally

From the project root (`flowfreeze/`):

```bash
python3 -m pip install -r requirements.txt
python3 -m data_generator.generate --seed 42
python3 -m uvicorn backend.main:app --host 0.0.0.0 --port 8000
```

In another terminal:

```bash
cd frontend
npm install
npm run dev
```

Open the Vite URL printed in the terminal. Vite proxies `/api/*` and `/health` to `http://127.0.0.1:8000`, so the browser uses a same-origin API. The generator creates the local SQLite database at `data/flowfreeze.db`; it also regenerates the synthetic CSV files in `data/`, so preserve/backup those files before re-running it if the original snapshot matters.

## Optional synthetic model outputs

The app runs without model artifacts and clearly marks risk and next-move outputs unavailable. To enable the existing synthetic-only models without rewriting the repository's tracked metrics file, train to a temporary directory and copy only the ignored model binaries:

```bash
python3 -m ml.train --data-dir data --artifact-dir /tmp/flowfreeze-models --seed 42
cp /tmp/flowfreeze-models/fraud_model.joblib /tmp/flowfreeze-models/next_move_model.joblib ml/artifacts/
```

The models and all displayed evaluation values are based on generated data. They are **not** upay BD production performance claims.

## Demo access and write controls

- The analyst-name gate is only a local display-name/session convenience; it is **not authentication** or an authorization boundary.
- Approve/reject/modify actions write synthetic analyst decisions and simulated outcomes to the local SQLite audit tables. They never hold, move, or otherwise control a real wallet.
- By default, demo writes are available only in the existing backend's demo-write mode. Configure `DEMO_WRITE_ENABLED` and `DEMO_WRITE_KEY` on the backend only in a controlled test environment. If a key is used, set the matching `VITE_DEMO_WRITE_KEY` in `frontend/.env.local`; do not commit secrets.
- All incident amounts, wallet identities, model scores, taint estimates, and what-if results are synthetic and should remain clearly labeled in any deployment or presentation.

## Frontend commands

- `npm run dev` — local development server
- `npm run build` — strict TypeScript check and production bundle
- `npm run preview` — serve the production bundle locally
