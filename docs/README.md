# job_seeker_ro_spider

**job_seeker_ro_spider** — scraper pentru job-urile METRO Cash & Carry din România.

Extrage anunțurile de pe [cariere.metro.ro](https://cariere.metro.ro/jobs) și le publică în [peviitor.ro](https://peviitor.ro) prin API-ul Peviitor.

> **🌱 Derived scraper.** Acest repo este **derivat din** [epam-systems-international-srl-nodejs-scraper](https://github.com/sebiboga/epam-systems-international-srl-nodejs-scraper) — template-ul de referință pentru scraper-ele Node.js din ecosistemul peviitor.ro.

## Identificare

Toate request-urile HTTP folosesc User-Agent-ul:

```
job_seeker_ro_spider
```

## Ce face

1. **Validează compania** — interoghează API-ul public ANAF ([demoanaf.ro](https://demoanaf.ro)) după CIF-ul METRO (8119423) și verifică:
   - Denumirea oficială: METRO CASH & CARRY ROMANIA SRL
   - Status: activ/inactiv/radiat
   - Adresa completă din registrul comerțului
2. **Cross-validează cu Peviitor** — verifică existența companiei în API-ul Peviitor
3. **Scrape-uiește job-urile** — extrage lista completă de job-uri din pagina [cariere.metro.ro/jobs](https://cariere.metro.ro/jobs) (HTML parse-uit cu cheerio, selector `.attrax-vacancy-tile`)
4. **Transformă datele** — normalizează locațiile (doar orașe românești), tag-urile (lowercase), workmode-ul (remote/on-site/hybrid)
5. **Stochează în Peviitor** — upsert prin API-ul Peviitor (job-uri și date companie)
6. **Generează jobs.md** — fișier markdown cu informații companie + toate job-urile curente

## API-uri folosite

| API | URL | Autentificare |
|---|---|---|
| METRO Careers | `https://cariere.metro.ro/jobs` (HTML) | Public |
| ANAF (demoanaf) | `https://demoanaf.ro/api/...` | Public |
| ANOFM | `https://mediere.anofm.ro/api/entity/vw_public_job_posting` | Public |
| Peviitor | `https://api.peviitor.ro/v1/company/` | Public |

## Robots.txt

METRO Careers [robots.txt](https://cariere.metro.ro/robots.txt) permite `/` și dezactivează doar:
- `/jobs?*` — job-uri cu filtre/search (query string)
- `/admin*`, `/login*`, `/register*`, `/workflow*` — zone interne

Scraperul accesează doar `/jobs` (fără query string) — cale permisă. Face cereri sequential (concurrency 1) cu un singur User-Agent identificabil.

Pentru analiza completă, vezi [ai/ROBOTS.md](../ai/ROBOTS.md).

## Testare

```bash
# Toate testele
npm test

# Doar unitare
npm run test:unit

# Doar integrare (necesită ANAF live, Peviitor API conditional)
npm run test:integration

# Doar E2E (pagina reală METRO + ANAF + Peviitor)
npm run test:e2e
```

Testele Peviitor API folosesc `itIfApi` — se auto-skip dacă API-ul Peviitor nu e disponibil.
