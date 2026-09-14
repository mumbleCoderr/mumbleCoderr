## mumbleCoderr

### Experience

I'm Mateusz - a developer focused on mobile and backend. I build things end to end: shared Kotlin Multiplatform clients, serverless backends, AI content pipelines. I finished a Computer Science B.Eng. at the Polish-Japanese Academy of Information Technology and I'm starting a Master's in Innovation Design and Artificial Intelligence at WSB Merito. In 2025 I spent 3 months as an AEM Developer Intern at Sii Polska, rebuilding their careers page from scratch in a 4-person team.

### Tech stack

Mostly Kotlin - Kotlin Multiplatform, Compose Multiplatform, Jetpack Compose, Clean Architecture, MVVM, Room, Koin, Ktor. On the backend Java with Spring Boot, Node.js on Cloud Functions and Python. Firebase for most of my production work (Firestore, Auth, Functions, FCM, App Check), plus React, Docker, SQL and a lot of LLM plumbing - structured outputs, prompt engineering, cost-aware pipelines.

### Projects

**Right** - social quiz app for Android and iOS, one Kotlin Multiplatform codebase. [video](https://www.youtube.com/watch?v=RyvzOGbR4K8)
- one shared Kotlin Multiplatform codebase for both platforms, with a serverless Firebase backend
- Clean Architecture + MVVM enforced consistently: ViewModels expose a StateFlow of UiState and accept a sealed UiEvent, composables only ever get state and lambdas
- platform layer solved through expect/actual - auth clients, ads, image picker, photo cropper, compression, haptics, locale, share sheet, splash screen and video playback
- server-authoritative anti-cheat: points, streaks and premium status are computed only in transactional Cloud Functions, the client is treated as untrusted
- answer submission validates every answer against the card, deduplicates against history so a card scored once never scores again, replays the streak in answer order and applies the series multiplier
- Firestore security rules define fields the client can never write (points, Pro status, streak), force starting account values and deny everything else by default; App Check with Play Integrity is required on all callable functions
- custom Firestore content-serving algorithm (Scout + Digger) with constant read cost regardless of collection size, local history filtering, per-category in-memory cache and background prefetch of the next batch
- offline-first answers stored in Room with sync in batches of up to 200, marked per batch so a mid-batch failure doesn't resend accepted chunks
- monetization via RevenueCat (subscription + non-consumable + consumable) with a signed webhook granting entitlements server-side, event deduplication and refund clawback
- AdMob rewarded ads granted only after RSA signature verification of the server-side verification callback against Google's public key, with transaction deduplication and a daily cap; native ads woven into the card pager with a preload queue and lifecycle control
- social layer: friends with automatic acceptance of mutual invites, blocks, user search over a normalized index, card and profile sharing - cards opened from a share are ephemeral and count toward nothing, which closes the score-farming vector
- leaderboards computed server-side with cache and TTL, a daily friends snapshot with point deltas, index and histogram rebuilt by a scheduled job, and a warm-up ping that takes the cold start off the first open
- rank system of 6 levels split into divisions, 25 achievements with a trophy shelf, and a stats screen computed from real answer history rather than counters kept next to it
- player-written cards: in-app creator, automoderation through the same chain of steps as the seed bank, daily limits counted server-side, and a report threshold that pulls a card out of circulation
- moderation panel built on a private Discord server (Ed25519-verified interactions, best-effort so it can never break the trigger that called it) instead of a separate admin app
- 12 languages of UI and content, deep links on both platforms (App Links + Universal Links with landing pages and Open Graph previews), in-app notification center over 7 event types with FCM push, cosmetics system with animated frames, gradients and video backgrounds, and my own library of reusable Compose components
- scheduled jobs keeping the database healthy: weekly promo with broadcast, cleanup of notifications older than 30 days and removal of abandoned unconfirmed registrations
- unit tests in KMP commonTest plus backend tests on pure logic functions run by `node --test` without an emulator
- **Stack:** Kotlin Multiplatform, Compose Multiplatform, Material 3, Clean Architecture, MVVM, Koin, KSP, Room, Coroutines, Flow, Ktor, kotlinx.serialization, Coil, Media3, Firebase (Firestore, Auth, Cloud Functions, Realtime Database, Storage, FCM, App Check), Node.js, RevenueCat, AdMob, Python, Gemini API

**Right AI pipeline** - the content pipeline that filled the app's question bank. [video](https://www.youtube.com/watch?v=RyvzOGbR4K8)
- deterministic steps deliberately separated from model steps - anything computable in code costs zero tokens
- every model call uses structured output with a Pydantic JSON schema, so no stage ever parses free text and none can break on formatting
- shipped 5,015 cards in 12 languages from 8,167 raw inputs (98.4% survived cleaning: normalization of two source formats, rejection of truncated or out-of-range sentences, deduplication by text hash)
- batch classification assigns each fact a category and tag from a 101-item taxonomy, flags opinions, time-dependent claims, unverifiable and offensive content, and decomposes the claim into subject, attribute and value in the same call
- facts batched per call so the system prompt overhead is paid once per batch instead of once per fact, and the few-shot set deliberately capped at 10 hand-written examples - at this scale every extra example is paid for thousands of times
- true/false assignment done deterministically in code, alternating within category buckets so the bank is exactly 50/50 and neither swipe direction becomes statistically profitable
- controlled mutation for false statements: the model swaps exactly one number, date or name and declares what it swapped, so falsity comes from construction rather than from trusting the model
- deterministic quality gates (anti-paraphrase, answer leak, filler) plus an AI judge checking the finished card, and a hard manual checkpoint before any translation is paid for
- translation in one call from English to 11 languages with a schema pinning exactly 11 keys, then a separate verification call reading all 12 versions with a per-language verdict: OK, meaning drift, truth inversion or unnatural phrasing - retranslation runs only for failed languages and only once
- cost optimization built into the architecture: cheaper model for classification and translation, pricier one for generation and verification, reasoning tokens off, stages ordered so the cheapest gate discards the most data first
- rewritten to Node.js and wired into Cloud Functions as the automoderation path for player-written cards, with daily budget reservation in Realtime Database so two simultaneous submissions can't read the same counter, and overflow drained hourly instead of hitting the API at once
- **Stack:** Python, Pydantic, Gemini API, structured outputs, prompt engineering, Cloud Firestore, Realtime Database, Node.js, Cloud Functions

**Take or Make** - Android marketplace for service and product listings, my engineering thesis. [repo](https://github.com/mumbleCoderr/Take_or_Make) · [video](https://youtu.be/yacjtEgf6FU)
- two tabs the name comes from: Take browses other people's listings, Make is where you post, edit and delete your own
- core/features split in the spirit of Clean Architecture, with MVVM on top
- domain model separating offer from request and product from service, with category, price unit, item condition and publication status
- multi-step listing wizard, Firebase Auth with Google sign-in via Credential Manager, Koin + KSP dependency injection
- UiText layer keeping labels as resource references instead of hardcoded strings inside domain models
- listing photos stored as a field on the listing document in Cloud Firestore, no separate storage layer
- ViewModel unit tests with MockK and a custom MainDispatcherRule, plus Compose UI tests
- **Stack:** Kotlin, Jetpack Compose, Material 3, Clean Architecture, MVVM, Koin, KSP, Coroutines, Flow, kotlinx.serialization, Navigation Compose, Coil, Firebase Auth, Cloud Firestore, MockK, Gradle

**Job Sniper** - CV generator tailored to a specific job offer
- pipeline: requirement extraction from the posting, deterministic match scoring, content writing, ATS-friendly PDF render
- anti-hallucination validator checking every fact in the generated CV against a source-of-truth profile - skills, projects, technologies and numbers must be covered, otherwise the model gets a repair round-trip with the list of violations
- two cost gates before paid stages: a prefilter discarding offers by title and tags without calling the model, and a scoring threshold guarding the most expensive writing stage
- LLM provider abstraction with separate parser, writer and repair slots, each with its own model, temperature and rate limit
- rate limiter based on a minimum interval plus retry with exponential backoff and jitter, using the retryDelay returned by the API instead of guessing how long to wait
- technology-name normalizer handling Polish inflection, so an occurrence of "w Pythonie" counts as Python without false hits like Java inside Javascript
- scrapers for justjoin.it and nofluffjobs with deduplication by company and position, so the same role on two boards doesn't produce two applications
- PDF through Jinja2 and headless Chromium driven by Playwright instead of WeasyPrint, which removes the native GTK dependency on Windows
- ATS keyword coverage metric measuring how many of the offer's requirements actually appear in the generated CV
- unit tests covering the whole deterministic part without network or API key, including validator tests on a deliberately fabricated CV
- **Stack:** Python, Pydantic, Streamlit, Jinja2, Playwright, SQLite, Gemini API, structured outputs, prompt engineering

**Fruit Shop** - fullstack online store, my first fullstack project. [repo](https://github.com/mumbleCoderr/FRUIT_SHOP) · [video](https://youtu.be/Wy5izuefwd8)
- spent the semester learning Spring Boot, Spring Data JPA, Hibernate and Spring Security specifically to build this in the stack I wanted to work in
- JWT authentication with a custom JwtAuthenticationFilter plugged into the Spring Security chain
- role-based authorization separating user from admin, with a dedicated admin panel on the frontend
- data model covering user, authorities, address, order, order item and product, mapped through Spring Data JPA and Hibernate
- REST API split into auth/products/orders/users controllers over service and repository layers, with DTOs at the API boundary
- global exception handling through a GlobalExceptionHandler and input validation through Spring Validation
- React 18 frontend with React Router, Axios and client-side JWT decoding: cart, product details, checkout with delivery address, order history and an admin order view
- Dockerfiles for backend and frontend plus a SQL script creating the database schema
- **Stack:** Java, Spring Boot, Spring Data JPA, Spring Security, Hibernate, JWT, Maven, Lombok, MySQL, REST API, React, Vite, Axios, Docker

**Digital Diary** - memory journal with geolocation and voice notes. [repo](https://github.com/mumbleCoderr/DIGITAL_DIARY) · [video](https://youtu.be/_k5QQEMhMmI)
- memories made of a photo, voice recording, description, mood rating, city and date
- local database on Room with a memory entity, DAO and a ViewModel layer built on the State/Event pattern
- my own audio recorder handling start and stop and returning the path to the audio file
- last-known-location lookup through Google Play Services Location with reverse geocoding to a city name
- Google sign-in on Firebase Auth with a separate GoogleAuthUiClient and a sign-in result model
- UI entirely in Jetpack Compose with Navigation Compose and image loading through Coil
- **Stack:** Kotlin, Jetpack Compose, Material 3, MVVM, Room, Coroutines, Flow, Navigation Compose, Coil, Firebase Auth, Play Services Location

**Film Library** - movie and series catalog, my first Kotlin app. [repo](https://github.com/mumbleCoderr/FILM_LIBRARY) · [video](https://youtu.be/CQT7N4gSDBg)
- each production has a rating, comment, cover and watched status
- sealed class domain model separating a movie described by runtime from a series described by episode count
- persistence through object serialization to a file with ObjectOutputStream and ObjectInputStream
- filtering and sorting by title, by genre, by genre combined with watched status, plus collection sorting
- covers handled as byte arrays with bitmap conversion
- list screen, details screen and Navigation Compose, all in Jetpack Compose
- **Stack:** Kotlin, Jetpack Compose, Material 3, Navigation Compose, Coil, Gradle

... and a few more

### Languages

- Polish - native
- English - C1
- Spanish - A2

### Contact

[Linkedin](https://www.linkedin.com/in/mateusz-biernat-b3273027b)
