# Robots.txt Analysis — METRO Careers

Sursa: https://cariere.metro.ro/robots.txt

## Reguli

```
User-agent: *
Allow: /
Disallow: /jobs?*
Disallow: /admin*
Disallow: /register*
Disallow: /workflow*
Disallow: /login*
Disallow: /our-job-sectors*
Disallow: /newprofile*
```

## Interpretare

| Cale | Accesibil? | Ce conține |
|---|---|---|
| `/` | ✅ Da | Pagina principală |
| `/jobs` | ✅ Da | Listarea de job-uri — pagina pe care scraper-ul o parsează (cheerio, `.attrax-vacancy-tile`) |
| `/jobs?*` | ❌ Disallowed | Job-uri cu parametri de filtrare/search (query string) |
| `/admin*`, `/login*`, `/register*` | ❌ Disallowed | Zone administrative |
| `/workflow*`, `/newprofile*`, `/our-job-sectors*` | ❌ Disallowed | Zone interne |

## Recomandare

robots.txt NU este legal binding, dar reprezintă intenția proprietarului site-ului.

- Scraperul accesează doar `/jobs` (fără query string) — cale **allowed** de robots.txt.
- Paginile individuale de job sunt accesibile; le verificăm doar cu HEAD/GET în teste (validate-jobs), nu le scrape-uim în masă.
- Scraperul face cereri sequential (concurrency 1) cu User-Agent `job_seeker_ro_spider` — comportament rezonabil, nu agresiv.

**Concluzie**: Risc minim. Pagina de job-uri e publică și permisă de robots.txt, iar scraperul e politicos (rate limiting, User-Agent standard, o singură cerere simultană).
