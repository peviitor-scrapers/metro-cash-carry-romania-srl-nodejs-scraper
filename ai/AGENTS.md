# AGENTS.md — Rules for AI agents

## Project
METRO scraper for peviitor.ro (Node.js, ESM, Jest)

## 🌱 This Repo Is a Derived Scraper
This repo is **derived from** the [epam-systems-international-srl-nodejs-scraper](https://github.com/sebiboga/epam-systems-international-srl-nodejs-scraper) template.

When making changes to this derived scraper:
- **All company-specific identity lives in `scraper/config/company.json`** (id, company, brand, status, location, website, career, scraperFile). Read from `scraper/config/company.js` in Node code, or via `jq` in workflows. Never hardcode in source files.
- **Only the HTML parsing logic in `scraper/index.js`** (`fetchJobListings`, `parseJobsHTML`) is METRO-specific (cheerio over `cariere.metro.ro/jobs`). The output shape (`mapToJobModel`, `transformJobsForSOLR`) must stay uniform across derived scrapers.
- **If you add a new file, update [ai/files.md](files.md).**

## Directory Structure
```
scraper/           — scraper source files (moved from root)
├── config/        — company.json, scraper.json (company identity + API params)
├── index.js       — main scraper orchestrator (METRO HTML scraping)
├── api.js         — Peviitor API client
├── company.js     — company data fetcher
├── anaf.js        — ANAF/CUIScan/CUIFirma library
├── job-validator.js — shared job validation
├── validate-jobs.js — generic deep validator (manual use)
└── markdown-generator.js — generates docs/jobs.md
tests/             — test files
.github/workflows/ — CI workflows
ai/                — documentation for AI agents
docs/              — GitHub Pages content (jobs.md, test-results)
```

## Critical Rules

### 0. Background tasks — always pass `--repo` explicitly to `gh`

When polling a workflow run with `until [ "$(gh run view ID --json status -q .status)" = "completed" ]; do sleep N; done`, the `gh run view` command implicitly uses the current working directory's git remote. If the CWD is a different repo (e.g. you cd-ed elsewhere mid-task), `gh` looks in the wrong repo and returns 404 — the loop's check becomes `"" != "completed"` (always true) and the background task sleeps forever.

**Always specify the repo explicitly:**
```bash
gh run view <RUN_ID> --repo sebiboga/metro-cash-carry-romania-srl-nodejs-scraper --json status -q .status
```

Before starting any `gh run watch` or polling loop in the background, sanity-check:
- Does the command include `--repo`?
- Is the run ID from the same repo as `--repo`?

If you spawn a stuck task, kill it immediately rather than letting it hang.

### 1. Temporary Files
All temporary/scratch files MUST go in `tmp/` inside the project root.
NEVER use paths outside the project (e.g. `C:\Users\...\AppData\Local\Temp\opencode`).

### 2. Issues & GitHub
- **Orice modificare de cod trebuie să aibă un issue în GitHub Issues** (vezi [ISSUES.md](ISSUES.md))
- Excepții: typo-uri, whitespace, documentație minoră
- Create a GitHub issue before implementing any change
- Commit messages must reference the issue they close
- Never commit credentials (`.env.local`, `*.pem`, etc.)
- Push after commit

### 3. Environment Variables
- `.env.local` is NOT used — all operations go through the Peviitor API (no direct SOLR access)
- Consistency tests need `GITHUB_REPOSITORY` (format: `owner/repo`) and `GITHUB_TOKEN`

### 4. Testing
```bash
npm run test:unit
npm run test:integration   # needs ANAF
npm run test:e2e           # needs ANAF
npm run test:consistency   # needs GITHUB_REPOSITORY + GITHUB_TOKEN
```

### 5. ESM + Jest
- Use `jest.unstable_mockModule` (NOT `jest.mock`) for mocking ESM modules
- Run with `--experimental-vm-modules` flag

### 6. Verification
- După orice modificare, urmează [VERIFY.md](VERIFY.md) pas cu pas
- Ultimul pas = rulează scraperul prin GitHub Actions, verifică job-urile în Peviitor API, și verifică că `docs/jobs.md` a fost generat și este accesibil pe GitHub Pages
- Toate workflow-urile din `.github/workflows/` trebuie să treacă înainte de merge

### 7. DO NOT modify these files (derived from template)
- `scraper/anaf.js`
- `scraper/company.js`
- `scraper/job-validator.js`
- `scraper/validate-jobs.js`

### 8. Maintenance Agent
See [MAINTENANCE.md](MAINTENANCE.md) for the full maintenance workflow.

**On every session:**
1. Check open GitHub issues: `gh issue list --repo sebiboga/metro-cash-carry-romania-srl-nodejs-scraper --state open`
2. Prioritize: `critical` → `bug` → `enhancement` → `documentation`
3. Fix all issues, commit with `#issue` reference, close the issue
