# JD Parfum — SQL (Supabase)

Run these **in order** in the **JD Parfum** Supabase project → SQL Editor → New query → Run.

Do **not** run them on AmirCars.

| Order | File | Purpose |
|------:|------|---------|
| 1 | `001_amircars_schema.sql` | Base schema, RLS, inventory ledger |
| 2 | `002_archive_purge.sql` | Controlled archive purge helper |
| 3 | `003_stock_solo_cliente.sql` | Stock calculated client-side (drop remote stock trigger) |
| 4 | `004_soft_delete_catalogo.sql` | Soft-delete (`is_active=false`) instead of hard DELETE |
| 5 | `005_reactivar_fotos_nube.sql` | Reactivate Cloudinary-backed images for active products |
| 6 | `006_public_catalog_anon.sql` | **Required for public catalog** — anon SELECT on active `productos` / `producto_imagenes` |

After `006`, the publishable/`anon` key can power `GET /api/catalog/products` without `SUPABASE_SERVICE_ROLE_KEY`.

Optional: leave `SUPABASE_SERVICE_ROLE_KEY` blank on Render if `006` is applied.
