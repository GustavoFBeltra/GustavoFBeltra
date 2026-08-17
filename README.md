<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/banner-dark.svg">
  <img alt="Gustavo F. Beltra — Beltra Industries LLC. Full-stack and native mobile engineer, Pensacola, Florida." src="assets/banner-light.svg" width="100%">
</picture>

`ACTIVITY`

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/GustavoFBeltra/GustavoFBeltra/output/snake-dark.svg">
  <img src="https://raw.githubusercontent.com/GustavoFBeltra/GustavoFBeltra/output/snake-light.svg" width="100%" alt="Contribution graph" />
</picture>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/card_activity_dark.svg">
  <img src="assets/card_activity_light.svg" width="49%" alt="Contributions: 868 total, 120 active days, current streak 2, longest streak 17" />
</picture>
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/card_repos_dark.svg">
  <img src="assets/card_repos_light.svg" width="49%" alt="Repository totals: 540 commits, 22 pull requests, 3 products built, 100% solo" />
</picture>

---

I build the whole thing: schema, backend, interface, payments, and the deployment pipeline. Three
platforms so far, all solo. Most of the work sits in private repositories, so this page is the
catalog.

---

`THE CATALOG`

## BI-01 · TAB Point of Sales

`COMMERCE & OPERATIONS`  `ACTIVE DEVELOPMENT`

High-performance POS for hospitality and retail. Seven applications built solo over three years: an
Electron terminal, a handheld server app, a manager desktop, a kitchen display system, an Android
payment app running on Stripe S700 hardware, plus the marketing site and waitlist.

The parts worth talking about: offline order queuing that reconciles on reconnect, terminal batch
reconciliation against ticket-level records, and a relational schema covering menus, modifiers,
tickets, seats, employees, shifts, and payments.

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/tab-manager-dark.png">
  <img alt="TAB Manager dashboard: weekly revenue, labor cost, labor percentage, active staff, sales and labor trends, revenue by department" src="assets/tab-manager-light.png" width="100%">
</picture>

<sub>Manager desktop. Sales and labor trends, department revenue split, and live labor percentage
against target. The offline indicator is real: the terminal keeps taking orders when the connection
drops and reconciles them on reconnect.</sub>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/tab-pos-dark.png">
  <img alt="TAB POS terminal: menu search with on-screen keyboard, category tabs, and the open ticket panel" src="assets/tab-pos-light.png" width="100%">
</picture>

<sub>Server terminal. Touch-first, on-screen keyboard, category tabs, and the open ticket held
alongside the menu.</sub>

![TypeScript](https://img.shields.io/badge/TypeScript-4A4A48?style=flat-square&logo=typescript&logoColor=FAFAF9)
![React](https://img.shields.io/badge/React-4A4A48?style=flat-square&logo=react&logoColor=FAFAF9)
![Next.js](https://img.shields.io/badge/Next.js-4A4A48?style=flat-square&logo=nextdotjs&logoColor=FAFAF9)
![Node.js](https://img.shields.io/badge/Node.js-4A4A48?style=flat-square&logo=nodedotjs&logoColor=FAFAF9)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4A4A48?style=flat-square&logo=postgresql&logoColor=FAFAF9)
![Electron](https://img.shields.io/badge/Electron-4A4A48?style=flat-square&logo=electron&logoColor=FAFAF9)
![Stripe](https://img.shields.io/badge/Stripe%20Terminal-4A4A48?style=flat-square&logo=stripe&logoColor=FAFAF9)
![Supabase](https://img.shields.io/badge/Supabase-4A4A48?style=flat-square&logo=supabase&logoColor=FAFAF9)
![Vercel](https://img.shields.io/badge/Vercel-4A4A48?style=flat-square&logo=vercel&logoColor=FAFAF9)

[tab-pos.com](https://tab-pos.com)

---

## BI-02 · Yapr

`LANGUAGE & COMMUNICATION`  `ACTIVE DEVELOPMENT`

Real-time voice translation, built twice natively. Swift and SwiftUI on iOS, Kotlin and Jetpack
Compose on Android, no shared UI code, six product surfaces held at parity by a 33-point audit.

A five-stage voice pipeline runs against a sub-1.5 second budget and streams first audio before
translation finishes. Median time-to-first-audio is 311ms. Per-turn cost came down 61% after I
worked out that synthesis was about 87% of unit cost and routed across two vendors with in-request
failover.

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#4a4a48','primaryTextColor':'#fafaf9','primaryBorderColor':'#4a4a48','lineColor':'#9c9c9a','secondaryColor':'#4a4a48','tertiaryColor':'#4a4a48','fontFamily':'ui-monospace, monospace','fontSize':'13px'}}}%%
flowchart LR
    A["VAD-gated<br/>capture"] --> B["Streaming<br/>STT"]
    B --> C["LLM<br/>translation"]
    C --> D["Voice<br/>synthesis"]
    D --> E["Streamed<br/>playback"]
    D -.->|"in-request failover"| F["Vendor B"]
    F -.-> E
    E ==>|"311ms median<br/>time-to-first-audio"| G(("Sub-1.5s<br/>budget"))
```

Playback starts before translation completes, so what the user hears begins at the first stage while
the rest of the pipeline is still running.

<div align="center">
  <video src="https://github.com/user-attachments/assets/7d9d2d71-7bd0-47c5-a304-375c91deae03" width="300" controls muted></video>
</div>

<div align="center">
<sub>Yapr on Android. Language pair, then the mic, then translated speech in the other person's language.</sub>
</div>

Translation is the core, and it sits inside a travel dashboard. Arrive somewhere and the app resolves
the destination into what you need in the first ten minutes: local time, currency, weather, the
emergency number, whether to tip, which plug and what voltage, whether the tap water is safe. Under
that, a quick-phrase bar and a local news feed with each headline translated and the original kept
underneath.

There is also an emergency screen with country-specific service numbers, Medical ID, and an SOS
beacon, and its translations work with no connection at all.

The third piece is a course that does not exist until you ask for it. You state a goal, an edge
function generates and validates a curriculum, and the app plays it through seven exercise types.
Speaking drills score against the same streaming STT the translator uses; the rest grade on-device.
Completed material decays back into review on an SM-2-style schedule, and each unit ends in a scoped
roleplay conversation that gets graded.

Backend is 23 Deno edge functions with row-level security and rate limiting. EU AI Act Article 50
disclosure and US state biometric consent gating are enforced server-side.

Also in there: an **Android Auto** surface built on the Car App Library, so the interpreter and the
emergency screens project onto a head unit through the host's templates instead of Compose. TOTP
two-factor with server-side step-up on sensitive operations, and a biometric app lock. GDPR export
and deletion, both backed by their own edge functions. A home-screen widget that keeps the streak
current without opening the app.

Both platforms carry the full screen set. Android is feature-complete, iOS is in final pre-release
testing. Neither app is published to a store.

![Swift](https://img.shields.io/badge/Swift-4A4A48?style=flat-square&logo=swift&logoColor=FAFAF9)
![SwiftUI](https://img.shields.io/badge/SwiftUI-4A4A48?style=flat-square&logo=swift&logoColor=FAFAF9)
![Kotlin](https://img.shields.io/badge/Kotlin-4A4A48?style=flat-square&logo=kotlin&logoColor=FAFAF9)
![Jetpack Compose](https://img.shields.io/badge/Jetpack%20Compose-4A4A48?style=flat-square&logo=jetpackcompose&logoColor=FAFAF9)
![Deno](https://img.shields.io/badge/Deno-4A4A48?style=flat-square&logo=deno&logoColor=FAFAF9)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4A4A48?style=flat-square&logo=postgresql&logoColor=FAFAF9)
![WebRTC](https://img.shields.io/badge/WebRTC-4A4A48?style=flat-square&logo=webrtc&logoColor=FAFAF9)

[yap-r.com](https://yap-r.com)

---

## BI-06 · Kinly

`ACCESSIBILITY & ASSISTANCE`  `ACTIVE DEVELOPMENT`

Technology, on your terms. An Android accessibility companion for older adults and people with
visual, motor, or cognitive disabilities. You state a goal in plain language and it explains the
screen, highlights the right control, or walks you through it.

It treats model output as untrusted input. Strict-schema parsing against an action allowlist,
structural validation against the live screen, then a deterministic fail-closed policy engine.
On-screen text is never executed as an instruction, so a malicious app cannot steer the agent
through content it renders. Card numbers, SSNs, and one-time codes are masked on the device before
anything reaches the model.

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#4a4a48','primaryTextColor':'#fafaf9','primaryBorderColor':'#4a4a48','lineColor':'#9c9c9a','secondaryColor':'#4a4a48','tertiaryColor':'#4a4a48','fontFamily':'ui-monospace, monospace','fontSize':'13px'}}}%%
flowchart TB
    A["Live screen"] --> B["On-device masking<br/>card numbers, SSNs, one-time codes"]
    B --> C["Model"]
    C -->|"output treated as untrusted"| D["Strict-schema parse"]
    D --> E["Action allowlist"]
    E --> F["Structural validation<br/>against the live screen"]
    F --> G{"Policy engine"}
    G -->|"every check passes"| H["Execute"]
    G -->|"anything unresolved"| R["Refuse"]
    D -.->|"malformed"| R
    E -.->|"not on allowlist"| R
    F -.->|"target absent"| R
```

Nothing reaches the model unmasked, and nothing reaches the screen unvalidated. Every rejection path
converges on refusal, so an unexpected state fails closed instead of guessing.

92 tests across 53 source files. Green build gate on compilation, tests, lint, and detekt.

![Kotlin](https://img.shields.io/badge/Kotlin-4A4A48?style=flat-square&logo=kotlin&logoColor=FAFAF9)
![Jetpack Compose](https://img.shields.io/badge/Jetpack%20Compose-4A4A48?style=flat-square&logo=jetpackcompose&logoColor=FAFAF9)
![Hilt](https://img.shields.io/badge/Hilt-4A4A48?style=flat-square&logo=android&logoColor=FAFAF9)
![Room](https://img.shields.io/badge/Room-4A4A48?style=flat-square&logo=android&logoColor=FAFAF9)
![Gradle](https://img.shields.io/badge/Gradle-4A4A48?style=flat-square&logo=gradle&logoColor=FAFAF9)

---

`TECHNOLOGY`

**Languages**

![TypeScript](https://img.shields.io/badge/TypeScript-4A4A48?style=flat-square&logo=typescript&logoColor=FAFAF9)
![JavaScript](https://img.shields.io/badge/JavaScript-4A4A48?style=flat-square&logo=javascript&logoColor=FAFAF9)
![Kotlin](https://img.shields.io/badge/Kotlin-4A4A48?style=flat-square&logo=kotlin&logoColor=FAFAF9)
![Swift](https://img.shields.io/badge/Swift-4A4A48?style=flat-square&logo=swift&logoColor=FAFAF9)
![Python](https://img.shields.io/badge/Python-4A4A48?style=flat-square&logo=python&logoColor=FAFAF9)
![SQL](https://img.shields.io/badge/SQL-4A4A48?style=flat-square&logo=postgresql&logoColor=FAFAF9)
![Bash](https://img.shields.io/badge/Bash-4A4A48?style=flat-square&logo=gnubash&logoColor=FAFAF9)

**Interface**

![React](https://img.shields.io/badge/React-4A4A48?style=flat-square&logo=react&logoColor=FAFAF9)
![Next.js](https://img.shields.io/badge/Next.js-4A4A48?style=flat-square&logo=nextdotjs&logoColor=FAFAF9)
![SwiftUI](https://img.shields.io/badge/SwiftUI-4A4A48?style=flat-square&logo=swift&logoColor=FAFAF9)
![Jetpack Compose](https://img.shields.io/badge/Jetpack%20Compose-4A4A48?style=flat-square&logo=jetpackcompose&logoColor=FAFAF9)
![Electron](https://img.shields.io/badge/Electron-4A4A48?style=flat-square&logo=electron&logoColor=FAFAF9)
![Tailwind CSS](https://img.shields.io/badge/Tailwind%20CSS-4A4A48?style=flat-square&logo=tailwindcss&logoColor=FAFAF9)
![Three.js](https://img.shields.io/badge/Three.js-4A4A48?style=flat-square&logo=threedotjs&logoColor=FAFAF9)

**Backend and data**

![Node.js](https://img.shields.io/badge/Node.js-4A4A48?style=flat-square&logo=nodedotjs&logoColor=FAFAF9)
![Deno](https://img.shields.io/badge/Deno-4A4A48?style=flat-square&logo=deno&logoColor=FAFAF9)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4A4A48?style=flat-square&logo=postgresql&logoColor=FAFAF9)
![Supabase](https://img.shields.io/badge/Supabase-4A4A48?style=flat-square&logo=supabase&logoColor=FAFAF9)
![Firebase](https://img.shields.io/badge/Firebase-4A4A48?style=flat-square&logo=firebase&logoColor=FAFAF9)
![Room](https://img.shields.io/badge/Room-4A4A48?style=flat-square&logo=android&logoColor=FAFAF9)

**Machine learning and vision**

![TensorFlow](https://img.shields.io/badge/TensorFlow-4A4A48?style=flat-square&logo=tensorflow&logoColor=FAFAF9)
![Keras](https://img.shields.io/badge/Keras-4A4A48?style=flat-square&logo=keras&logoColor=FAFAF9)
![OpenCV](https://img.shields.io/badge/OpenCV-4A4A48?style=flat-square&logo=opencv&logoColor=FAFAF9)
![scikit-learn](https://img.shields.io/badge/scikit--learn-4A4A48?style=flat-square&logo=scikitlearn&logoColor=FAFAF9)

**Infrastructure**

![Vercel](https://img.shields.io/badge/Vercel-4A4A48?style=flat-square&logo=vercel&logoColor=FAFAF9)
![Docker](https://img.shields.io/badge/Docker-4A4A48?style=flat-square&logo=docker&logoColor=FAFAF9)
![Stripe](https://img.shields.io/badge/Stripe-4A4A48?style=flat-square&logo=stripe&logoColor=FAFAF9)
![Gradle](https://img.shields.io/badge/Gradle-4A4A48?style=flat-square&logo=gradle&logoColor=FAFAF9)
![Git](https://img.shields.io/badge/Git-4A4A48?style=flat-square&logo=git&logoColor=FAFAF9)
![GitHub](https://img.shields.io/badge/GitHub-4A4A48?style=flat-square&logo=github&logoColor=FAFAF9)

---

`BEFORE THIS`

Five years at a 780-seat restaurant, promoted from expo to back of house manager, running up to 26
kitchen staff on nights around $139,000 in sales. The bar team was counting liquor on clipboards for
two hours, so I built them a web app that got it to 45 minutes. That is where the software started.

B.S. Engineering Technology, University of West Florida.

---

<div align="center">

`CONTACT`

[![Website](https://img.shields.io/badge/beltraindustries.com-4A4A48?style=flat-square)](https://beltraindustries.com)
[![Portfolio](https://img.shields.io/badge/gustavobeltra.com-4A4A48?style=flat-square)](https://gustavobeltra.com)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-4A4A48?style=flat-square)](https://linkedin.com/in/GustavoFBeltra)
[![Email](https://img.shields.io/badge/Email-4A4A48?style=flat-square)](mailto:Gustavo.F.Beltra@outlook.com)

Native English and Spanish.

`EST. U.S.A. · BUILT FOR PRODUCTION`

</div>
