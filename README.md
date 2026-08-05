# job_seeker_ro_spider — METRO Careers Romania Scraper

[![Oportunitati SI Cariere](https://github.com/sebiboga/metro-cash-carry-romania-srl-nodejs-scraper/actions/workflows/job-seeker-ro-spider.yml/badge.svg)](https://github.com/sebiboga/metro-cash-carry-romania-srl-nodejs-scraper/actions/workflows/job-seeker-ro-spider.yml)
[![Automation Tests](https://github.com/sebiboga/metro-cash-carry-romania-srl-nodejs-scraper/actions/workflows/automation-testing.yml/badge.svg)](https://github.com/sebiboga/metro-cash-carry-romania-srl-nodejs-scraper/actions/workflows/automation-testing.yml)

[![Version](https://img.shields.io/github/package-json/v/sebiboga/metro-cash-carry-romania-srl-nodejs-scraper?label=version&color=blue)](CHANGELOG.md)
[![Test Results](https://img.shields.io/badge/test--results-HTML-9b59b6)](https://sebiboga.github.io/metro-cash-carry-romania-srl-nodejs-scraper/test-results/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![JavaScript](https://img.shields.io/badge/javascript-ESM-F7DF1E?logo=javascript&logoColor=black)](https://ecma-international.org/)
[![Node.js](https://img.shields.io/badge/node-24-339933?logo=nodedotjs&logoColor=white)](https://nodejs.org/)
[![Website](https://img.shields.io/website?url=https%3A%2F%2Fpeviitor.ro&label=peviitor.ro)](https://peviitor.ro)
[![API](https://img.shields.io/website?url=https%3A%2F%2Fapi.peviitor.ro%2F&label=api.peviitor.ro)](https://api.peviitor.ro/)
[![GitHub Pages](https://img.shields.io/github/deployments/sebiboga/metro-cash-carry-romania-srl-nodejs-scraper/github-pages?label=GitHub%20Pages)](https://sebiboga.github.io/metro-cash-carry-romania-srl-nodejs-scraper/)

**job_seeker_ro_spider** — un scraper pentru job-urile METRO Cash & Carry România. Extrage anunțurile de pe [cariere.metro.ro](https://cariere.metro.ro/jobs) și le publică în [peviitor.ro](https://peviitor.ro) prin API-ul Peviitor.

> **🌱 This Repo Is a Derived Scraper** — creat din template-ul [epam-systems-international-srl-nodejs-scraper](https://github.com/sebiboga/epam-systems-international-srl-nodejs-scraper). Pentru a crea un scraper similar pentru o altă companie românească, vezi AI Factory: [AI-Factory-job-seeker-ro-spider](https://github.com/sebiboga/AI-Factory-job-seeker-ro-spider).

---

## Cuprins

- [Descriere](#descriere)
- [Cum funcționează](#cum-funcționează)
- [Scraping method](#scraping-method)
- [Job-uri extrase](#job-uri-extrase)
- [Întreținere](#întreținere)
- [Contribuții](#contribuții)
- [Licență](#licență)

---

## Descriere

Acest scraper rulează zilnic prin GitHub Actions (sau la cerere manuală) și:

1. Validează compania METRO Cash & Carry România în ANAF
2. Extrage job-urile de pe [cariere.metro.ro](https://cariere.metro.ro/jobs)
3. Transformă datele în formatul standard (model de job Peviitor)
4. Trimite job-urile la [api.peviitor.ro](https://api.peviitor.ro)
5. Salvează rezultatele în [docs/jobs.md](docs/jobs.md)

## Cum funcționează

Job-urile sunt extrase din pagina de cariere METRO România, care folosește platforma **SmartRecruiters Attrax**. Pagina listează toate pozițiile deschise cu titlu, locație și departament.

## Scraping method

| Detaliu | Valoare |
|---|---|
| **Metodă** | HTML/cheerio |
| **URL sursă** | `https://cariere.metro.ro/jobs` |
| **Selector job-uri** | `.attrax-vacancy-tile` |
| **Selector titlu** | `.attrax-vacancy-tile__title` |
| **Selector locație** | `.attrax-vacancy-tile__location-freetext .attrax-vacancy-tile__item-value` |
| **ANOFM** | Inclus (query by CIF 8119423) |

## Job-uri extrase

Vezi [docs/jobs.md](docs/jobs.md) pentru ultimele job-uri extrase sau [GitHub Pages](https://sebiboga.github.io/metro-cash-carry-romania-srl-nodejs-scraper/).

## Întreținere

Acest scraper este întreținut de [AI Factory](https://github.com/sebiboga/AI-Factory-job-seeker-ro-spider).

Dacă structura paginii se schimbă, actualizează selectoarele din `scraper/index.js` → `parseJobsHTML()`.

## Licență

MIT
