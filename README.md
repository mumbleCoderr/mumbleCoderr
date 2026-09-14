## mumbleCoderr

### Experience

I'm Mateusz - a developer focused on mobile and backend. I build things end to end: shared Kotlin Multiplatform clients, serverless backends, AI content pipelines. I finished a Computer Science B.Eng. at the Polish-Japanese Academy of Information Technology and I'm starting a Master's in Innovation Design and Artificial Intelligence at WSB Merito. In 2025 I spent 3 months as an AEM Developer Intern at Sii Polska, rebuilding their careers page from scratch in a 4-person team.

### Tech stack

Mostly Kotlin - Kotlin Multiplatform, Compose Multiplatform, Jetpack Compose, Clean Architecture, MVVM, Room, Koin, Ktor. On the backend Java with Spring Boot, Node.js on Cloud Functions and Python. Firebase for most of my production work (Firestore, Auth, Functions, FCM, App Check), plus React, Docker, SQL and a lot of LLM plumbing - structured outputs, prompt engineering, cost-aware pipelines.

### Projects

**Right** - social quiz app for Android and iOS, one Kotlin Multiplatform codebase. [video](https://www.youtube.com/watch?v=RyvzOGbR4K8)
- one shared Kotlin Multiplatform codebase for both platforms, with a serverless Firebase backend
- server-authoritative anti-cheat: points, streaks and premium status are computed only in transactional Cloud Functions, the client is treated as untrusted
- custom Firestore content-serving algorithm (Scout + Digger) with constant read cost regardless of collection size, local history filtering and background prefetch
- offline-first answers stored in Room with sync in batches of up to 200, marked per batch so a mid-batch failure doesn't resend accepted chunks
- monetization via RevenueCat (subscription + non-consumable + consumable) with a signed webhook, event deduplication and refund clawback; AdMob rewarded ads granted only after RSA signature verification of the SSV callback
- 12 languages of UI and content, deep links on both platforms, in-app notification center, cosmetics system and my own library of reusable Compose components
- moderation panel built on a private Discord server (Ed25519-verified interactions) instead of a separate admin app

**Right AI pipeline** - the content pipeline that filled the app's question bank. [video](https://www.youtube.com/watch?v=RyvzOGbR4K8)
- deterministic steps deliberately separated from model steps - anything computable in code costs zero tokens
- every model call uses structured output with a Pydantic JSON schema, so no stage ever parses free text
- shipped 5,015 cards in 12 languages from 8,167 raw inputs (98.4% survived cleaning)
- controlled mutation for false statements: the model swaps exactly one number, date or name and declares what it swapped
- deterministic quality gates (anti-paraphrase, answer leak, filler) plus an AI judge before any translation is paid for
- rewritten to Node.js and wired into Cloud Functions as the automoderation path for player-written cards, with daily budget reservation in Realtime Database

**Take or Make** - Android marketplace for service and product listings, my engineering thesis. [repo](https://github.com/mumbleCoderr/Take_or_Make) · [video](https://youtu.be/yacjtEgf6FU)
- core/features split in the spirit of Clean Architecture, with MVVM on top
- domain model separating offer from request and product from service, with category, price unit, item condition and publication status
- multi-step listing wizard, Firebase Auth with Google sign-in via Credential Manager, Koin + KSP dependency injection
- ViewModel unit tests with MockK and a custom MainDispatcherRule, plus Compose UI tests

**Job Sniper** - CV generator tailored to a specific job offer
- pipeline: requirement extraction from the posting, deterministic match scoring, content writing, ATS-friendly PDF render
- anti-hallucination validator checking every fact in the generated CV against a source-of-truth profile, with a repair round-trip when something isn't covered
- two cost gates before paid stages, and an LLM provider abstraction with separate parser/writer/repair slots, each with its own model, temperature and rate limit
- scrapers for justjoin.it and nofluffjobs with deduplication, PDF via Jinja2 + headless Chromium, unit tests covering the whole deterministic part without network or API key

**Fruit Shop** - fullstack online store, my first fullstack project. [repo](https://github.com/mumbleCoderr/FRUIT_SHOP) · [video](https://youtu.be/Wy5izuefwd8)
- Spring Boot backend with JWT auth (custom filter in the Spring Security chain) and role-based authorization with an admin panel
- REST API split into auth/products/orders/users controllers over service and repository layers, DTOs at the API boundary
- React 18 frontend with cart, product details, checkout with delivery address and order history; Dockerfiles for both sides

**Digital Diary** - memory journal with geolocation and voice notes. [repo](https://github.com/mumbleCoderr/DIGITAL_DIARY) · [video](https://youtu.be/_k5QQEMhMmI)
- memories made of a photo, voice recording, description, mood rating, city and date, stored locally in Room
- custom audio recorder, last-known-location lookup with reverse geocoding to a city name, Google sign-in via Firebase Auth

**Film Library** - movie and series catalog, my first Kotlin app. [repo](https://github.com/mumbleCoderr/FILM_LIBRARY) · [video](https://youtu.be/CQT7N4gSDBg)
- sealed class domain model separating a movie (runtime) from a series (episode count)
- persistence through object serialization, filtering and sorting by title, genre and watched status, cover images handled as byte arrays

... and a few more

### Languages

- Polish - native
- English - C1
- Spanish - A2

### Contact

[Linkedin](https://www.linkedin.com/in/mateusz-biernat-b3273027b)
