# Stiddi

**Product and AI Case Study**

An iPhone application combining astronomical calculations, personalized astrology readings, and contextual AI conversations.

This is a documentation-only portfolio repository. The application source code remains private.

## Product Overview

Stiddi brings natal charts, forecasts, relationship readings, and AI conversations into one experience in **English, French, and Russian**.

The product goal was to make complex chart information easier to explore through clear navigation, personalized explanations, and conversations connected to the user's own readings.

The project covers product definition, visual design, AI integration, monetization, quality assurance, and App Store preparation. Astrological interpretations are designed for reflection and entertainment, not as medical, legal, or financial advice.

## My Role

I created and led Stiddi from the initial idea through product design, development, testing, and App Store preparation.

I defined the concept, features, user journeys, and visual design. I created the illustrations in Ideogram from my own prompts and worked on the presentation of charts, readings, onboarding, and purchase screens.

I designed and refined the AI prompts, evaluated responses across three languages, and worked on personalization, clarity, and consistency.

I managed development using **Codex as an AI coding assistant**. I prioritized changes, defined acceptance criteria, tested the application, identified issues, reviewed fixes, and made product decisions throughout the project. This included preparing review accounts, screenshots, and App Store metadata.

## Main Features

- **Natal charts:** planetary positions, houses, aspects, and chart visualization.
- **Personal readings:** personality analysis and four life areas.
- **Forecasts:** daily, weekly, monthly, and annual readings.
- **Relationship readings:** comparison of two natal charts, also known as synastry.
- **Additional charts:** child charts and adult charts for other people.
- **Contextual AI chat:** questions about a specific chart, forecast, or relationship reading.
- **Saved content:** readings that users can revisit and local daily reminders.
- **Accounts and purchases:** Apple, Google, and email sign-in; monthly and yearly Premium subscriptions and separate content purchases.

## System Architecture

Stiddi connects a React Native mobile application to a Python backend. Calculations, AI integration, accounts, and application data are handled on the server.

| Layer | Responsibility |
| --- | --- |
| Mobile application | Navigation, chart visualization, readings, chat, and purchase screens |
| Backend API | Accounts, access management, calculation requests, and saved content |
| Calculation engine | Astronomical positions and chart-specific calculations |
| AI integration | Personalized interpretations and contextual answers |
| Database | Application accounts, charts, readings, and purchase-related data |
| Apple services | Sign-in, in-app purchases, subscriptions, and iOS distribution |

**Astronomical calculations:** Stiddi uses a custom calculation engine built around established libraries, including Skyfield, and NASA/JPL astronomical data. The backend calculates positions and prepares chart information before the AI interpretation stage.

**Infrastructure:** The application backend is hosted on **Hetzner Cloud in Germany**. **Scaleway Object Storage** is the selected provider for a prepared off-site backup integration; live backup operation has not yet been verified.

A typical reading starts with the user's chart inputs, continues through backend calculations and AI interpretation, and is saved for later access in the application.

## Technology Stack

| Area | Technologies |
| --- | --- |
| Mobile | React Native, Expo, TypeScript |
| Backend | Python, FastAPI, SQLite |
| AI | OpenAI API, prompt engineering |
| Astronomy | Skyfield, NASA/JPL data |
| Infrastructure | Hetzner Cloud; Scaleway Object Storage integration prepared |
| Testing and Release | GitHub Actions, TestFlight, App Store Connect |
| Development Assistance | Codex |
| Visual Assets | Ideogram |

## AI Integration & Testing

The AI layer turns calculated chart information into readable interpretations in the selected language. Contextual conversations use the relevant chart, forecast, or saved reading.

A key architectural decision was to **separate astronomical calculations from AI interpretation**: the calculation engine supplies the data, while the model explains that material. Model requests are handled by the backend.

My work on AI quality included refining task-specific prompts, reviewing tone and structure, checking personalization, and investigating inconsistent or unsuitable responses across English, French, and Russian.

Testing combined automated backend and mobile checks with TestFlight and real-device testing. The main user journeys reviewed included:

- Sign-in, onboarding, navigation, and account switching.
- Purchases, subscription access, and purchase restoration.
- First chat entry, question balances, and access to purchased content.
- Reading presentation, localization, and error states.

**Security and reliability:** server-side authentication, secure purchase verification, error handling, data protection, and automated testing.

One important lesson was to test complete journeys rather than isolated screens. A successful payment, for example, also needs to result in the correct content access and chat experience. Localization similarly involves presentation and behavior, not just translated text.

## Links & Contact

- [Stiddi website](https://stiddi.app/)
- [Support](https://stiddi.app/support/)
- [Privacy policy](https://stiddi.app/privacy/)
- **Contact:** contact@stiddi.app

This case study covers development, testing, and App Store preparation. An App Store link will be added after public release.

Stiddi's application code and brand assets remain proprietary. Third-party libraries, assets, and datasets retain their respective licenses and attribution requirements.
