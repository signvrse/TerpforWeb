# 🤟 Signvrse Terp for Web (Terp-Web)
### *Next-Generation Web Accessibility & Sign Language Video Interpretation Platform*

[![License: Proprietary](https://img.shields.io/badge/License-Proprietary-blue.svg)](LICENSE)
[![Java: 25](https://img.shields.io/badge/Java-25-orange.svg)](https://openjdk.org/)
[![Spring Boot: 3.5.3](https://img.shields.io/badge/Spring_Boot-3.5.3-6DB33F.svg)](https://spring.io/projects/spring-boot)
[![Next.js: 16](https://img.shields.io/badge/Next.js-16.1.3-black.svg)](https://nextjs.org/)
[![React: 19](https://img.shields.io/badge/React-19.2-61DAFB.svg)](https://react.dev/)
[![TypeScript: 5.3](https://img.shields.io/badge/TypeScript-5.3-3178C6.svg)](https://www.typescriptlang.org/)
[![WCAG: 2.2 Ready](https://img.shields.io/badge/WCAG-2.2_Ready-success.svg)](https://www.w3.org/WAI/standards-guidelines/wcag/)

---

## 📌 Executive Summary

**Signvrse Terp for Web** is an enterprise-grade digital accessibility platform that breaks communication barriers on the internet. By combining **intelligent DOM scanning**, **viewport-synchronized sign language interpretation (Kenyan Sign Language / KSL & International Sign)**, and a **comprehensive digital accessibility suite**, Terp-Web transforms standard websites into inclusive digital spaces for the Deaf and Hard-of-Hearing (D/HH) community—all through a single line of JavaScript.

Website owners can onboard their domain, visually select and map content sections, and have certified sign language interpreters produce synchronized video translations. End-users experience smooth, non-intrusive, interactive video overlays that automatically follow page scroll, respond to user gestures, and adapt to individual accessibility needs (dyslexia fonts, contrast controls, text-to-speech, and voice dictation).

---

## 🎯 The Problem & Social Impact

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│  466M+ People Worldwide with Disabling Hearing Loss                                    │
│  Sign Language is often their FIRST & NATURAL language (text is a secondary language) │
│  < 3% of global websites offer native sign language accessibility                     │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

* **Cognitive Barrier**: For many culturally Deaf individuals, written text is a second language with different syntactic and grammatical rules compared to native sign languages.
* **Legal & Regulatory Compliance**: Global legislation (e.g., European Accessibility Act 2025, US Section 508, ADA Title III, and Kenya Persons with Disabilities Act) increasingly mandates barrier-free digital access.
* **Frictionless Inclusion**: Traditional website accessibility tools only offer basic color inversion or machine text-to-speech, completely omitting the Deaf community. **Terp-Web bridges this critical divide.**

---

## 🌟 Key Platform Features

### 1. 🪟 Interactive Client Widget (`/widget`)
* **Zero-Dependency & Ultra-Lightweight**: Built with TypeScript and bundled via `esbuild` into an optimized distribution (~25KB gzipped) with zero external runtime dependencies.
* **Draggable & Resizable Avatar Panel**: Users can freely reposition, collapse, expand, or adjust the video player window anywhere on screen.
* **Smart DOM & Viewport Synchronization**: Leverages `IntersectionObserver` and `MutationObserver` to automatically highlight content sections, reveal sign language indicator icons, and queue relevant interpretation videos as users navigate.
* **Touch & Gesture Recognition**: Integrated touch gestures (e.g., swipe right on target elements) to trigger instant sign interpretation on mobile and tablet devices.
* **Full-Spectrum Accessibility Toolbar**:
  * 🔤 **Dyslexia-Friendly Typography** (OpenDyslexic style adjustments)
  * 🌓 **Contrast & Color Tools**: High contrast, invert colors, grayscale, dark mode
  * 🔍 **Visual Guides**: Text scaling (100%–150%), line-height, letter-spacing, large cursor, and reading mask
  * 🗣️ **Voice Tools**: Built-in Web Speech API Text-to-Speech (TTS VoiceOver) and speech-to-text Voice Dictation

### 2. 🔍 Automated Visual Scanner & Route Detection (`/backend` + `/dashboard`)
* **Dual-Engine Web Scraper**: Powered by **Jsoup** (for ultra-fast static HTML analysis) and **Playwright (Headless Chromium)** (for JavaScript-heavy, client-rendered SPAs).
* **Automated Route Discovery**: Crawls target domains and sitemaps to detect available routes, pages, and headings automatically.
* **Interactive Visual Bounding Box Tool**: Administrative dashboard lets teams inspect full-page screenshots, filter DOM nodes by tag (`H1`–`H6`, `P`, `LI`, `BUTTON`, `IMG`), and draw custom bounding boxes for targeted video translations.

### 3. 🎬 Quality-Assured Studio Workflow & Gemini Agent Validation
* **Interpreter / Animator Workspace**: Dedicated studio interface for managing script sections, recording sign language interpretations, and tracking production status.
* **Gemini Agent Verification**: The workflow features automated handovers to **Gemini Agents** to verify and ensure the correctness, contextual precision, and quality of all output translations and content mappings before publishing.
* **Seamless Media Pipeline**: Streamlined video asset processing and delivery pipeline ensuring synchronized, high-definition sign playback with zero broken links.

### 4. 🏢 Business Portal & Enterprise Billing
* **Self-Service Dashboard**: Client management of pages, API keys, and widget customization (placement, branding colors, theme).
* **Tiered Subscription Engine**: Integration with **Paystack** for monthly/annual recurring billing, automatic tier synchronization, setup fee handling, and view-based overage tracking.
* **Real-time Telemetry & Analytics**: Session tracking, section watch durations, engagement metrics, and monthly view limits.
* **System-wide Broadcast & Alerts**: Global banner notification system and transactional/broadcast email infrastructure.

---

## 💻 Tech Stack & Infrastructure

| Layer | Technologies | Description |
| :--- | :--- | :--- |
| **Backend API** | **Java 25, Spring Boot 3.5.3** | High-performance reactive and REST controllers, Spring Data JPA, Spring Security |
| **AI & Validation** | **Gemini Agents** | Automated handovers for translation verification, quality assurance, and output correctness |
| **Web Crawling** | **Microsoft Playwright, Jsoup** | Headless browser page scanning, bounding-box coordinate detection, full-page screenshots |
| **Database** | **PostgreSQL 16** | Relational schemas with UUIDs, JSONB metadata, foreign-key indexing |
| **Caching & Security** | **Redis 7, Asymmetric RSA-256 JWT** | Sub-millisecond rate-limiting, session revocation, token blacklisting |
| **Frontend Portal** | **Next.js 16.1.3, React 19, TypeScript** | Responsive web dashboard, Server & Client Components, CSS design system |
| **Embed Widget** | **TypeScript, esbuild, CSS Tokens** | Zero-dependency standalone JavaScript library (< 30KB bundle) |
| **Payments** | **Paystack API** | Recurring subscription plans, webhook handlers, local currency (KES) checkout |
| **Communications** | **SendGrid API** | Automated transactional emails, verification links, broadcast announcements |
| **Containers** | **Docker & Docker Compose** | Multi-stage Dockerfiles with Playwright dependencies pre-configured |

---
