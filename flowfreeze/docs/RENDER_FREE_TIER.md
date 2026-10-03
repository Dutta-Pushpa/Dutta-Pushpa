# Render free-tier deployment

The repository includes [`render.yaml`](../render.yaml) with two explicit services:

- **`flowfreeze-api`**: Python web service running FastAPI on Render's `$PORT`.
- **`flowfreeze-web`**: static Vite site whose `VITE_API_BASE_URL` points at the API.

## What the API build does

The API build command installs the pinned Python dependencies and runs
`scripts/render-build.sh`. That script deterministically performs both generated
runtime steps before the service starts:

1. `python -m data_generator.generate --seed "$RANDOM_SEED"`
2. `python -m ml.train --seed "$RANDOM_SEED"`

It then checks that `data/flowfreeze.db`, `fraud_model.joblib`, and
`next_move_model.joblib` exist. The default seed is `42`; change `RANDOM_SEED`
only when a different synthetic snapshot is intentional and documented.

The runtime database is SQLite at `DATABASE_URL=sqlite:///./data/flowfreeze.db`.
It is a generated synthetic snapshot, not durable application storage.

## URL and CORS configuration

The static site gets its API origin from `VITE_API_BASE_URL` at **frontend build
time**. The API gets allowed browser origins from the comma-separated
`CORS_ORIGINS` environment variable. If a custom Render domain is added, update
both values and redeploy the affected service. Do not use `*` for CORS.

The blueprint uses the default service URLs:

- `https://flowfreeze-api.onrender.com`
- `https://flowfreeze-web.onrender.com`

If Render assigns different names or a custom domain is used, update the
corresponding environment values in the Render dashboard before rebuilding.

## Public-write safeguards

The frontend's demo login is a UI affordance, **not authentication**. This
repository's Render blueprint intentionally enables synthetic decision testing:

- `APP_ENV=production`
- `ENABLE_DEMO_WRITES=true`
- `DEMO_WRITE_KEY=public-synthetic-demo-v1`
- the same value is built into the frontend as `VITE_DEMO_WRITE_KEY`
- decisions write only synthetic audit entries and outcome estimates; there are
  no wallet-control routes
- `POST /api/demo/reset` remains development-only

The shared key is committed and shipped to browsers, so it is intentionally
public and is **not authentication** or a real access-control boundary. Anyone
can submit synthetic decisions through the demo. Keep all data synthetic and
never connect this deployment to customer or production wallet systems. For a
read-only deployment, set `ENABLE_DEMO_WRITES=false` and remove both write-key
environment variables. Any other public deployment that embeds a key has the
same limitation, regardless of key entropy.

## Free-tier behavior and live-demo expectations

Render free web services **spin down after 15 minutes without inbound traffic**.
The next request can take about one minute while the service spins back up, so
open `/health` shortly before a presentation and allow the first page load to
finish. Do not promise continuous availability or low latency from the free
tier.

The service's local filesystem is ephemeral. A restart, redeploy, spin-down,
instance replacement, or manual reset can discard SQLite audit decisions and
simulated outcomes. The build regenerates the same synthetic CSVs, SQLite
snapshot, and model artifacts from `RANDOM_SEED`, so the deployed demo returns to
its clean seeded state after the next build. Treat audit history as disposable;
do not store real customer data or rely on this SQLite file for retention.

The static frontend is separately deployed and does not sleep in the same way as
the API, but it cannot make data durable. Free-tier quotas, build minutes,
bandwidth, and service availability are governed by Render's current plan terms.

## Deploy checklist

1. Push this repository to GitHub and create the Render Blueprint from
   `render.yaml`.
2. Confirm the API health check passes at `/health`.
3. Confirm `CORS_ORIGINS` exactly contains the deployed frontend origin.
4. Confirm the frontend's built `VITE_API_BASE_URL` is the deployed API origin.
5. Confirm the public demo remains synthetic-only; set `ENABLE_DEMO_WRITES=false`
   and remove the frontend key if read-only operation is preferred.
6. Before presenting, wake the API with `/health` and verify that the first page
   loads from the static site.
