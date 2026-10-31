# JD Parfum

Replica of the AmirCars inventory stack for **JD Parfum**, with a **public pre-login catalog** and shopping cart.

## Product shape

- **`/`** — public storefront (image, description, available qty, sale price REF) + localStorage cart + **Login**
- **`/login`** — Supabase Auth
- **`/panel`** — authenticated side-nav ops (inventory, entradas/salidas, sync, media)

## Stack

Flask + gunicorn · Supabase Auth/DB · Cloudinary · Dexie (offline panel) · Render

## Credentials (isolated)

Copy `.env.example` → `.env`. **Do not** reuse AmirCars live secrets.

| Variable | Required | Where to get it (JD only) |
|----------|----------|---------------------------|
| `FLASK_SECRET_KEY` | yes | `openssl rand -hex 32` |
| `SUPABASE_URL` | yes | Supabase → Project Settings → API → Project URL |
| `SUPABASE_KEY` | yes | Same page → `anon` / publishable key |
| `SUPABASE_SERVICE_ROLE_KEY` | no | Same page → `service_role` (prefer SQL `006` instead) |
| `CLOUDINARY_URL` **or** `CLOUDINARY_CLOUD_NAME` + `CLOUDINARY_API_KEY` + `CLOUDINARY_API_SECRET` | yes | Cloudinary console → API Keys (new JD cloud) |

Render service name: **`jd-parfum`** (`render.yaml`). SQL run order: see `sql/README.md` (include `006_public_catalog_anon.sql` for the public catalog).

## Brand assets

Shipped under `app/static/img/jdparfum/` (logo + luxury background) and source masters in `app/static/brand-source/`.

## Local run

```bash
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env   # fill JD-only values
gunicorn --config gunicorn.conf.py wsgi:app
```

## Public API

- `GET /api/catalog/products` — sanitized list (no `ref_entrada`, no suppliers)
- `GET /api/catalog/products/<id>` — sanitized detail

Sync and media upload remain login-protected.
