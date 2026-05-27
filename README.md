# Order Export

Genererar:
- CSV på ordernivå
- CSV på item-/detaljnivå
- CSV för slutrapport (`description`, `quantity`, `created`)
- XLSX med en flik (`Report`)
- XLS med en flik (`Report`)

`Report` innehåller kolumnerna:
- `description` (från item-detaljer)
- `quantity` (från item-detaljer)
- `created` (från orderraden)

Datat begränsas till poster med `created` inom senaste två månaderna.

## Lokalt

```bash
python order_export.py \
  --base-url "https://example.com/espressi/" \
  --username "$EXPORT_USER" \
  --password "$EXPORT_PASS" \
  --output orders_latest.csv \
  --items-output order_items_latest.csv \
  --report-output report_latest.csv \
  --xlsx-output report_latest.xlsx \
  --xls-output report_latest.xls
```

Miljövariabler:
- `EXPORT_BASE_URL`
- `EXPORT_USER`
- `EXPORT_PASS`

## GitHub Actions

Workflow:
- `.github/workflows/data-export.yml`
- skarpt utskick: sista tisdagen varje månad kl. 08:05 och 08:25 (Europe/Stockholm)
- förkörning: dagen före (måndagen före sista tisdagen) kl. 08:05 och 08:25 till `mattias.hallberg@gmail.com`
- inbyggd låsning per månad för att undvika dubbelskick:
  - skarpt utskick: `docs/latest/last_sent_month.txt`
  - förkörning: `docs/latest/last_precheck_month.txt`

Setup-guide:
- `GITHUB_ACTIONS_SETUP.md`

Valfritt:
- e-postnotis via Gmail SMTP (`SMTP_USER`, `SMTP_PASS`, `MAIL_FROM`, `MAIL_TO`)
  - ämnesrad: `Månadsrapport - Astice`
  - förkörning använder ämnesrad: `Förkörning (dagen före) - Månadsrapport Astice`
  - mejlet innehåller datumfilterinfo (utan tabell)
- varningsmail via Resend om utskicket misslyckas (`RESEND_API_KEY`, `RESEND_FROM`)
  - varning skickas till `mattias.hallberg@gmail.com`

## Webbsida

Sidan finns i:
- `docs/index.html`

Den:
- kräver ingen PAT
- visar status
- laddar ner senaste publicerade `.xls`

Publicerad fil:
- `docs/latest/report_latest.xls`
- `docs/latest/last_updated_utc.txt`

## Sökmotorer

Sidan är markerad för att inte indexeras med:
- `meta robots noindex,nofollow`
- `docs/robots.txt` med `Disallow: /`

Obs: det är en stark signal men ingen absolut garanti för att all indexering stoppas i alla motorer.
