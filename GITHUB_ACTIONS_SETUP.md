# Export via GitHub Actions (utan fysisk klientdator)

Lösningen kör exporten i GitHub Actions och:
- laddar upp resultat som artifacts
- publicerar senaste `.xls` i `docs/latest/` för nedladdning via Pages

## 1) Filer i repot

Säkerställ att följande finns:
- `order_export.py`
- `.github/workflows/data-export.yml`
- `docs/index.html`

## 2) Sätt secrets i repot

GitHub:
`Settings -> Secrets and variables -> Actions -> New repository secret`

Skapa:
- `EXPORT_BASE_URL`
- `EXPORT_USER`
- `EXPORT_PASS`

## 3) Första körning

Gå till:
`Actions -> Data Export -> Run workflow`

Valfritt:
- `view` (t.ex. `all` eller `inprocess`)
- `max_orders` (`0` = alla)

## 4) Resultatfiler

Artifacts per körning:
- `report_latest.xlsx`
- `report_latest.xls`
- `orders_latest.csv`
- `order_items_latest.csv`

Publik fil (Pages):
- `docs/latest/report_latest.xls`

## 5) Automatik

Workflowen körs:
- manuellt (`workflow_dispatch`)
- automatiskt var 15:e minut (`cron`)

## Viktigt

- Om källsystemet kräver IP-whitelist kan GitHub-hosted runners blockeras.
- Om repot är publikt blir även `docs/latest/report_latest.xls` publik.
