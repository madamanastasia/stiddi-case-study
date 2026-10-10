# Stiddi: Product and Engineering Case Study

**A multilingual iPhone application combining astronomical calculations, AI-generated astrology readings, and contextual conversations.**

This is a documentation-only portfolio repository. Stiddi's application source code, deployment configuration, credentials, and operational records remain in a separate private repository.

| Project | Overview |
| --- | --- |
| Product | Stiddi |
| Platform | Native iPhone application built with React Native and Expo |
| Languages | English, French, and Russian |
| Scope | Product definition, UX, AI integration, purchases, testing, and release preparation |
| Infrastructure | Hetzner Cloud for the application backend; Scaleway for the prepared off-site backup integration |
| Distribution work | iOS builds, TestFlight testing, and App Store submission preparation |
| Source availability | Proprietary; this public repository contains only the case study |

[Product website](https://stiddi.app/) | [Support](https://stiddi.app/support/) | [Privacy](https://stiddi.app/privacy/)

## Product Context

Stiddi brings several related experiences into one application: a natal chart, personalized readings, forecasts, relationship comparisons, and conversations about those materials.

The product challenge was not simply to add a chatbot. It was to connect calculated data, understandable explanations, saved results, account identity, and paid access in a consistent experience across three languages.

Key goals:

- Keep astronomical calculations separate from AI interpretation.
- Make purchased content and remaining questions understandable to users.
- Preserve successful results instead of generating them again on every visit.
- Support contextual conversations about a specific chart, forecast, or relationship reading.
- Test account switching, purchases, restoration, failures, and localization before release.

Astrological interpretations are intended for reflection and entertainment. They are not medical, legal, or financial advice, and astronomical data does not establish the validity of an interpretation.

## My Role and Development Approach

As the product creator, I defined the concept, feature scope, user journeys, visual direction, and acceptance criteria. I tested the application, reported concrete issues, reviewed proposed fixes, and made product and release decisions.

The implementation was developed with AI-assisted coding tools, including Codex and ChatGPT. This case study distinguishes product ownership and validation from the software libraries, data sources, and implementation assistance used to build the system.

My work included:

- Defining the free, Premium, and separately purchased experiences.
- Reviewing navigation, onboarding, paywalls, profile summaries, and reading presentation.
- Testing the English, French, and Russian experiences.
- Investigating user-visible failures with technical diagnostics.
- Reviewing purchase and question-balance behavior across multiple accounts.
- Preparing review accounts, product screenshots, and App Store metadata.
- Coordinating privacy documentation, infrastructure choices, and launch preparation.

## Main Features

| Experience | Functionality |
| --- | --- |
| Natal chart | Calculated positions, aspects, houses, and chart visualization |
| Personal readings | Personality analysis and four life areas |
| Forecasts | Daily, weekly, monthly, and separately purchased annual forecasts |
| Synastry | Comparison of two charts with relationship readings and contextual questions |
| Additional charts | Child charts and additional adult charts |
| AI chat | Conversations grounded in the relevant chart or saved reading |
| Accounts | Apple, Google, and email-code sign-in |
| Purchases | Monthly/yearly Premium, one-time products, restoration, and question balances |
| Saved content | Previously generated readings and their source chart versions |
| Reminders | Local daily notifications, subject to permission and enabled settings |

Availability depends on verified purchases, feature flags, and store configuration. The current iOS release target is iPhone; native iPad support is not enabled. Android configuration exists, but this case study does not claim an Android release.

## System Architecture

The mobile application is native React Native, not a website embedded in a WebView.

| Layer | Responsibility |
| --- | --- |
| Mobile client | Navigation, chart rendering, reading presentation, account flows, and native purchase UI |
| Python API | Authentication, authorization, calculations, generation jobs, and purchase verification |
| Calculation engine | Produces structured astronomical and astrology-specific calculation results |
| OpenAI integration | Produces language-specific interpretations and contextual answers from supplied material |
| SQLite | Accounts, chart versions, saved results, purchases, credit balances, and durable job state |
| Apple services | iOS distribution, payment transactions, subscription events, and Apple sign-in |
| Backup integration | Consistent database snapshots and a gated encrypted off-site storage path |

A typical reading follows this sequence:

1. The user selects the required chart or forecast inputs.
2. The API validates the request and calculates the relevant data.
3. The backend prepares a structured, language-specific context for the model.
4. The generated output passes the applicable format and content checks.
5. The backend saves the result with references to its source data.
6. The application displays the saved result and can reopen it later.

The application does not ask the language model to invent planetary positions.

## Infrastructure: Hetzner and Scaleway

The infrastructure uses two providers with different roles, rather than two interchangeable application servers.

### Hetzner Cloud: Main Application Server

The documented application backend is hosted in Germany. It runs the Python API, calculation engine, application database, and local astronomical/location datasets.

The deployment uses Ubuntu, systemd, nginx, and HTTPS. Application code, persistent data, and runtime secrets are managed separately.

### Scaleway: Independent Backup Storage

Scaleway is the second infrastructure provider, intended for encrypted off-site database backups through S3-compatible Object Storage. The configured destination region is Paris, France.

The Scaleway account has been created. Backup scripts and configuration have been prepared using restic, but uploads are disabled by default in the checked-in configuration. Live automatic uploads are not verified by this case study.

Activation requires completing and testing deletion from backup snapshots and old object versions, configuring access and recovery credentials, updating processor/privacy documentation, and passing restore checks. The account's existence is not presented as proof of working disaster recovery.

OpenAI is a separate external processing service, not a second Stiddi hosting server. European application hosting does not mean that every external processing operation occurs in Europe.

## Technology Stack

| Area | Technologies |
| --- | --- |
| Mobile | React Native 0.86, Expo SDK 57, React 19.2, TypeScript 6 |
| Navigation and visualization | React Navigation 7, React Native SVG, Lucide icons |
| Native integrations | Apple authentication, Google Sign-In, StoreKit through `expo-iap`, SecureStore, local notifications |
| Backend | Python 3.12, FastAPI, Uvicorn, Pydantic |
| Storage | SQLite application database and a separate local GeoNames database |
| Astronomy | Skyfield, jplephem, NumPy, SpiceyPy, local NASA/JPL data, custom calculation modules |
| Time and location | GeoNames, timezonefinder, Python `zoneinfo` |
| AI | Server-side OpenAI API integration; English, French, and Russian prompt catalogs |
| Purchases | Apple's App Store Server Library and signed transaction/notification verification |
| Hosting and backup tooling | Hetzner, nginx, systemd, Scaleway Object Storage, restic |
| Testing | pytest, Jest, React Native Testing Library, TypeScript, ESLint, Maestro, Locust |
| CI and security tooling | GitHub Actions, dependency audits, Gitleaks, Semgrep, CycloneDX SBOM |
| Build and distribution tooling | Expo EAS, Xcode, TestFlight, App Store Connect |
| Visual assets | Ideogram-generated illustrations, Tenor Sans, Marck Script, Lucide |

Versions describe the implementation reviewed for this case study; they are not installation instructions for this documentation-only repository.

## Custom Calculation Engine

Stiddi uses its own calculation modules around established astronomical libraries and data. A custom engine does not mean creating new planetary ephemerides from scratch.

- **Planetary positions:** Skyfield reads a local NASA/JPL DE440s kernel for the Sun, Moon, and planets.
- **Chiron:** SpiceyPy reads a separate local SPK kernel based on the pinned JPL#171 solution.
- **Time conversion:** place coordinates, time zones, and historical time-zone rules support conversion from local birth time.
- **Application calculations:** houses, angles, lunar points, Part of Fortune, aspects, configurations, transits, stations, ingresses, progressions, and returns.
- **Forecast preparation:** calculated events are selected and organized into the context used for an interpretation.

The current runtime does not use Swiss Ephemeris or Kerykeion and does not call an external astrology calculation API.

Ephemeris files are prepared ahead of time and read locally during requests. The setup utility verifies the expected datasets. House calculations expose a fallback reason when Placidus cannot be calculated reliably and Porphyry is used; coordinates are not silently clamped to avoid the problem.

Regression tests and explicit date limits help identify unsupported inputs. They are not a claim of unrestricted historical coverage or universal numerical accuracy.

## AI Integration and Reliability

The AI layer turns structured calculation results into readable material in the selected language. It also supports conversations grounded in the relevant chart, forecast, or synastry.

The implementation includes:

- Separate prompt catalogs and task-specific model configuration.
- Durable generation jobs, ownership leases, and persisted request counters.
- Separate scheduling lanes for daily, weekly/monthly, and annual forecasts.
- Reuse of successful intermediate components after a partial failure.
- A maximum of three provider calls per resource in a generation attempt, including repair calls; a multi-section reading can contain several resources.
- Checks for language, length, structure, source references, and known unsafe patterns, depending on the text type.
- Explicit handling of provider rate limits and transient failures.
- Diagnostic records containing rejected sentences, reasons, and attempt timing with a seven-day expiry policy.

Automated checks are useful but imperfect. One practical lesson was to investigate false positives across all supported languages: a word related to idealization, for example, should not automatically be interpreted as a promise of a perfect relationship.

Model requests can include relevant names and user-written chat content. They are not described as anonymous. API keys and provider calls are handled by the backend, not exposed in the mobile bundle.

## Monetization and Purchase Integrity

Monthly and yearly Premium provide the same type of Premium access, including weekly/monthly forecasts, four life areas, and 10 regular-chat questions per subscription month.

Separate purchases cover additional questions, annual forecasts, synastry, child charts, and additional adult charts. Annual forecasts and synastry each include three questions about that specific material, tracked separately from the regular-chat balance.

An annual Premium subscription is not the same product as a forecast for one selected calendar year.

Purchase handling separates:

- **Apple identity:** the store account used for payment.
- **Stiddi identity:** the application account receiving verified access.
- **Entitlements:** access to a feature or subscription.
- **Consumable balances:** credits that can be spent once.
- **Saved content:** generated results that are distinct from a subscription's current status.

Apple transactions are verified server-side and processed idempotently. Restoration must not duplicate credits or silently move a purchase from another existing account. Prices shown to users come from localized StoreKit product information.

Testing covered account switching, subscription activation, question allowances, feature access, and restoration. A key lesson was that a successful payment message alone does not prove that every dependent feature has received the correct access state.

## Testing and Release Preparation

The private implementation includes automated checks for backend behavior, mobile components, type safety, linting, environment separation, release metadata, and repository hygiene.

Testing methods include:

- Backend unit and integration tests with pytest.
- Mobile tests with Jest and React Native Testing Library.
- Fixture-based Maestro smoke tests for navigation without real payments or AI calls.
- Separate TestFlight and real-device checks for native sign-in and Apple purchases.
- Load-test scenarios for explicitly scoped performance checks.
- Dependency audits, secret scanning, static analysis, and a software bill of materials.

Release tooling ties quality-check receipts and generated metadata to an exact source commit and verifies the mobile source archive before a production build. Mobile builds and backend deployments remain separate deliverables.

This repository does not contain the test suite or claim that a particular CI run, live deployment, App Store approval, or public launch has occurred. It documents the implemented approach and release preparation.

## Privacy and Security Design

- Production HTTPS, server-side authorization, verified account identities, and verified purchases.
- SecureStore for mobile session credentials and hashed session tokens/email codes on the backend.
- Encryption and revocation handling for retained Apple refresh tokens.
- Local city search without requiring GPS or forwarding city-search queries to an external geocoder.
- Allowlisted product telemetry rather than advertising identifiers or cross-service tracking.
- Account deletion that is distinct from logout and from Apple subscription cancellation.
- Separate requirements for removing deleted account data from backup copies.

These are technical design choices, not a certification of legal compliance or a guarantee that every operational control is active. Actual processing, retention, external providers, and deployment settings must remain consistent with the published privacy policy.

No real user records, credentials, private infrastructure addresses, review login codes, or internal operational documents are included in this public repository.

## Key Lessons

1. **Separate calculation from interpretation.** The model explains supplied context; the calculation engine remains responsible for positions and events.
2. **Treat access as server-managed state.** Subscriptions, one-time purchases, consumable questions, and saved readings need distinct rules.
3. **Test complete journeys.** Payment, account switching, restoration, first chat entry, and exhausted balances expose issues that isolated screens do not.
4. **Validate localization behavior, not just translations.** Grammar, heading styles, and content filters need checks in every supported language.
5. **Make failures inspectable.** Specific rejected sentences and attempt reasons are more useful than a generic generation error.
6. **Distinguish preparation from operation.** A configured backup provider, uploaded build, or successful unit test is not proof of active backups, public release, or working payments.

## Attribution and Source Access

Stiddi's illustrations were generated in Ideogram from the creator's prompts. Typography and icons use third-party assets with their respective licenses.

The calculation implementation uses Skyfield, jplephem, NumPy, SpiceyPy, and NASA/JPL data. The location database uses GeoNames data with attribution requirements.

This repository publishes a product and technical case study only. Stiddi-owned application source code and brand assets remain proprietary; public visibility of this document does not grant a license to redistribute the application or its artwork.

For product or professional inquiries: **contact@stiddi.app**.
