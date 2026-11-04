# Nepali Calendar SDK 🇳🇵

> **One trusted Nepali calendar foundation. Every language. Every platform.**

**Status:** Planning / pre-release. This README describes the intended product, not features already implemented. No package, hosted API, verified dataset, or release is claimed to exist yet.

## Mission

Build a reliable, accessible, open-source **Bikram Sambat (BS) calendar ecosystem** for developers. Provide consistent AD ↔ BS conversion, calendar grids, date formatting, and—when independently verified—Nepali holidays, festivals, tithi, panchang, and auspicious timings. Support web, mobile, backend, and offline use without forcing developers to rebuild calendar logic.

**Guiding principles:** accuracy before coverage; offline-first core; explicit data provenance; stable APIs; accessibility; localization; no mandatory vendor lock-in.

## Goals and success criteria

1. Publish a tested TypeScript core library with a documented, verified BS date range.
2. Offer ready-to-use React components and integration examples for Next.js.
3. Establish a language-neutral specification and shared conformance fixtures.
4. Add SDKs for additional languages in stages without allowing conversion results to diverge.
5. Offer optional hosted REST APIs and self-hosting for richer, frequently updated data.
6. Build an open-source contributor community with transparent governance.

**Initial success:** an independent developer can install the core, convert dates correctly within the declared range, render a month, and run all examples without an account or internet connection.

## Target audience

- **Frontend developers:** React, Next.js, Vue, Angular, and plain JavaScript apps.
- **Mobile developers:** Flutter, native Android, native iOS, and eventually React Native.
- **Backend developers:** Node.js, Python, Go, Java, PHP, .NET, and other languages.
- **Organizations:** schools, municipalities, HR/payroll systems, banks, publishers, event organizers, and Nepali diaspora products.
- **Open-source contributors:** maintainers of calendar datasets, libraries, localization, and documentation.

## Planned platform support

| Platform / language | Delivery | Priority |
|---|---|---|
| TypeScript / JavaScript / Node.js | `core` package | **MVP** |
| React | Calendar UI and date picker package | Post-MVP |
| Next.js | React integration guide; SSR-safe usage | Post-MVP |
| REST / HTTP | Hosted and self-hosted API | Post-MVP |
| Python | Native library using shared fixtures | Later |
| Dart / Flutter | Core library + Flutter widgets | Later |
| Kotlin / Android | Native core + Compose UI | Later |
| Swift / iOS | Native core + SwiftUI | Later |
| Go | Native package | Later |
| Vue / Angular / React Native | Framework adapters | Later |
| Java / PHP / C# | Community-led or demand-led | Later |

**Important:** a React component cannot simply be reused as a Flutter or SwiftUI widget. Each framework needs its own UI integration. A language-neutral specification and test fixtures—not one universal binary—keep behavior consistent.

## Feature scope

| Feature | MVP v0.1 | Future |
|---|---|---|
| AD → BS / BS → AD conversion | Yes, verified range only | Broader verified range |
| BS date validation, month lengths, leap/boundary handling | Yes | More diagnostics |
| Month grid generation | Yes | Week/day layouts |
| English/Nepali date formatting and numeral options | Basic | Full i18n |
| Automated conformance tests | Yes | Cross-language suites |
| Basic React month demo | Yes, demo only | Published component library |
| React date picker / input | No | Yes |
| Government holidays and regional applicability | No | Yes, source-backed |
| Festivals and observances | No | Yes, source-backed |
| Tithi, nakshatra, yoga, karana | No | Yes, computed and validated |
| Shubh sait / muhurta | No | Yes, qualified, sourced, location-aware |
| Hosted API, authentication, caching | No | Yes |
| Self-hosted API + Docker | No | Yes |
| Multiple language SDKs | No | Yes |

### Explicit MVP exclusions

No payments, subscriptions, admin dashboard, full panchang engine, festival database, production backend, mobile SDKs, or 50-year future claims in v0.1. These are roadmap items, **not** reasons to delay a reliable first release.

## Data sources: what comes from where?

| Data category | Preferred source / method | Verification and update policy |
|---|---|---|
| BS month lengths and AD↔BS mapping | Licensed or permissively reusable BS calendar tables, checked against independently published Nepali calendars and dated reference records | Validate each supported year and anchor date before shipping; version with releases; corrections trigger patch releases |
| Official national and regional holidays | Nepal Ministry of Home Affairs / Nepal Gazette and relevant provincial or local government notices | Check on each official annual publication and whenever amendments appear; retain jurisdiction and effective dates |
| Festivals / religious observances | Published, authorized Nepali panchang references and relevant religious/cultural institutions | Review annually; record tradition, locale, year, source and editorial status |
| Tithi, nakshatra, yoga, karana | Astronomical ephemerides and documented calculation conventions | Validate against authorized panchang references for Nepal location/time conventions; recalculate with engine changes |
| Auspicious timings (sait/muhurta) | Qualified panchang authorities and explicitly documented calculation rules | Publish only with authority, applicable locality, validity, and confirmation status |
| Time zone | IANA `Asia/Kathmandu` | Use maintained time-zone databases; test local-day boundaries |
| Translations / month names | Maintained locale resources with native-speaker review | Review via pull requests and tagged releases |

**Source policy:** A public website is not automatically a reusable data license. Do not scrape, copy, redistribute, or bundle third-party data without permission or a compatible license. Document the origin, license, extraction date, verification method, and reviewer for each dataset.

**Official vs calculated:** A computed future tithi is **not** an officially announced holiday. Future festivals and government holidays must not be labeled confirmed before the relevant authority publishes or verifies them. For unsupported years, return an explicit error rather than silently guessing.

### Suggested data record metadata

```json
{
  "id": "example-event-id",
  "type": "public_holiday",
  "date_bs": "2083-01-01",
  "date_ad": "YYYY-MM-DD",
  "title": { "ne": "उदाहरण", "en": "Example" },
  "jurisdiction": "NP",
  "status": "provisional",
  "source_url": null,
  "source_publisher": null,
  "source_published_at": null,
  "verified_at": null,
  "dataset_version": "draft"
}
```

This is a **schema illustration**, not a factual holiday entry.

### Data update workflow

1. **Discover:** track official government notices, panchang publications, corrections, and astronomical source revisions.
2. **Ingest:** save original source links and immutable source snapshots *where redistribution is permitted*.
3. **Normalize:** transform into versioned, documented JSON/CSV schemas.
4. **Validate:** check schema, duplicate events, BS↔AD consistency, date boundaries, location, and contradictory sources.
5. **Review:** require at least one reviewer for routine changes; two independent checks for core conversion-table corrections.
6. **Release:** publish changelog, provenance manifest, checksums, and package/API data version.
7. **Notify:** mark revised records and publish correction notices; never silently alter historical data.

**Cadence:** inspect government announcements when annual schedules are issued and after amendments; review festival data annually; check source changes at least monthly during active maintenance; patch incorrect conversions promptly after verification. No claim of real-time automatic updates until an actual monitoring pipeline exists.

## Architecture

```text
                VERIFIED SOURCES / LICENSED DATA
                              |
                   ingestion + review
                              |
               versioned canonical datasets
                              |
             specification + conformance fixtures
                              |
            +-----------------+------------------+
            |                 |                  |
      TypeScript Core    Other-language SDKs   API Service
      (offline-first)    (Python/Dart/etc.)    (optional)
            |                                    |
      React Components                       PostgreSQL
            |                                    |
     React / Next.js                       Hosted REST API
            |                                    |
      Web applications                 Any HTTP-capable client
```

### Package boundaries

- **Core:** deterministic date arithmetic and month grid; no HTTP or database dependency.
- **Data:** explicitly versioned verified conversion tables, plus optional event datasets.
- **UI:** framework-specific views consuming the core and optional event provider.
- **API client:** typed HTTP calls, retries, caching and version handling.
- **API server:** optional server for rich events, revisions, filtering, and self-hosting.

**Backend is optional.** Installing a package does not automatically deploy a database or server. A developer can use local core features offline, connect to a hosted API, or self-host the API.

## Proposed repository structure

```text
nepali-calendar/
├── README.md
├── LICENSE                  # add after license decision
├── CONTRIBUTING.md
├── CODE_OF_CONDUCT.md
├── SECURITY.md
├── CHANGELOG.md
├── pnpm-workspace.yaml
├── packages/
│   ├── core/                # MVP: TypeScript conversion engine
│   ├── data/                # verified conversion tables
│   ├── react/               # post-MVP UI package
│   └── api-client/          # post-MVP
├── apps/
│   ├── playground/          # MVP: simple React demo
│   ├── docs/                # documentation site
│   └── api/                 # later: optional Go service
├── sdk/
│   ├── python/              # later
│   ├── dart/                # later
│   ├── kotlin/              # later
│   └── swift/               # later
├── specification/
│   ├── date-model.md
│   ├── conversion.md
│   └── error-contract.md
├── datasets/
│   ├── sources/
│   ├── verified/
│   ├── provisional/
│   └── provenance/
├── tests/
│   └── conformance/
├── examples/
│   ├── vanilla-ts/
│   └── nextjs/
└── .github/
    ├── workflows/
    └── ISSUE_TEMPLATE/
```

Folders above describe the **intended structure**; they may not exist yet.

## Developer experience (proposed API, not published)

**Install core, after v0.1 is published:**

```bash
npm install @nepali-calendar/core
```

```ts
import { adToBs, bsToAd, getBsMonth } from "@nepali-calendar/core";

const bs = adToBs({ year: 2026, month: 11, day: 5 });
const ad = bsToAd({ year: bs.year, month: bs.month, day: bs.day });
const month = getBsMonth(bs.year, bs.month);
```

The npm scope and function signatures are **illustrative and subject to availability and design review**.

**React, after the UI package exists:**

```tsx
import { NepaliCalendar } from "@nepali-calendar/react";

export default function CalendarPage() {
  return <NepaliCalendar locale="ne" />;
}
```

For Next.js App Router, interactive calendar components will be client components (`"use client"`); noninteractive formatting and date arithmetic can run on the server. Provide a separate Next.js example rather than shipping an unnecessary framework-specific package at first.

**Backend, after API launch:**

```http
GET /v1/convert?from=ad&date=2026-11-05
GET /v1/calendar/2083/07
GET /v1/holidays?year=2083&jurisdiction=NP
GET /v1/panchang?date=2083-07-19&location=Kathmandu
```

These are **planned endpoints only**. API responses should include dataset version, source provenance where applicable, confirmation status, error codes, and cache metadata. Dates should be interpreted using explicitly documented formats and `Asia/Kathmandu` rules where relevant.

## Getting started: clone, build, use

> The following is the **intended contributor workflow after repository initialization**. Replace `<REPOSITORY_URL>` with the real GitHub URL; do not assume these commands work before the repo and scripts are created.

```bash
git clone <REPOSITORY_URL>
cd nepali-calendar
corepack enable
pnpm install
pnpm build
pnpm test
pnpm --filter playground dev
```

Prerequisites: a supported Node.js LTS release, pnpm through Corepack, and Git. Exact versions will be pinned in `package.json` and CI.

### Contributing

1. Read `CONTRIBUTING.md`, the calendar specification, and dataset provenance rules.
2. Open an issue describing the bug or feature before major changes.
3. Fork the repository, create a branch, and add implementation **plus tests**.
4. For calendar-data corrections, attach a reliable source and explain the affected date range.
5. Run lint, typecheck, tests, and conformance fixtures.
6. Open a pull request using the template; maintainers review correctness, licenses, compatibility, and documentation.
7. Do not include private user data, unlicensed datasets, or unsupported claims of official approval.

Contributions can include translations, documentation, source research, independent reference fixtures, accessibility fixes, adapters, and examples—not just code.

### Quality and security

- Deterministic conversion with documented supported range; no silent fallback.
- Round-trip tests: `AD → BS → AD` and `BS → AD → BS`.
- Edge tests: BS month/year boundaries, Gregorian leap days, time-zone day changes.
- Shared fixtures for every language port; CI checks cross-language consistency.
- Semantic Versioning, changelogs, release tags, dependency updates, and security disclosures.
- For event APIs, enforce input validation, rate limiting, caching, and source attribution.

## 14-day MVP execution plan

| Days | Deliverable | Exit criterion |
|---|---|---|
| 1–3 | Repo, spec, source audit, initial licensed dataset | Source and license recorded; supported range selected |
| 4–7 | TS conversion and validation | Independent fixtures and round-trip tests pass |
| 8–10 | Month grid, formatting, React playground | Working demo using the actual core |
| 11–12 | Docs, examples, GitHub Actions | Clean install/build/test in CI |
| 13–14 | Release review and npm `0.1.0` | Publish **only if** data provenance and accuracy checks pass |

If trustworthy data cannot be secured in 14 days, **do not publish an inaccurate SDK**. Release the specification and verification tooling first, then ship conversion only when validated.

## Roadmap

- **v0.1:** TypeScript core, verified date range, tests, month generator, demo.
- **v0.2:** React components, date picker, Next.js examples, richer docs.
- **v0.3:** Versioned holiday/festival data and initial REST API, with sources.
- **v0.4+:** Python and Dart SDKs; later Kotlin, Swift, Go and other adapters.
- **Future research:** Astronomical panchang engine and independently validated sait data.

Version targets are **proposals**, not release commitments.

## Open-source and sustainability

**Model:** open-source core and community SDKs, with optional paid hosted API capacity, operational SLAs, enterprise support, and managed infrastructure in the future. Basic offline conversion must never require payment or an API key. Paid offerings are speculative until actual demand is demonstrated.

**License proposal:** MIT for original source code, pending maintainer approval and addition of a `LICENSE` file. Dataset licensing must be assessed separately; a code license does **not** grant redistribution rights over third-party calendar tables, ephemerides, or official publications.

**Governance:** transparent issues, public roadmap, documented contribution standards, source-backed changes, and published maintainer decisions. This project is **not affiliated with or endorsed by the Government of Nepal** unless formally established.

## Project status

- [x] Mission, audience, architecture, initial roadmap defined
- [ ] Repository initialized
- [ ] Data sources and licenses verified
- [ ] Date conversion implemented and independently tested
- [ ] Demo running
- [ ] First npm release published
- [ ] Hosted API available
- [ ] Cross-platform SDKs released

**Focus rule:** no new package or major feature until the current milestone meets its exit criteria.

---

**Build the reliable foundation first. Expand only after it works.**