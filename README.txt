# 📰 MND — Multilingual News Digest

> **MND (Multilingual News Digest)** is an Indian news aggregation platform delivering real-time, curated stories from trusted national sources across multiple categories. Every article is enriched with concise AI summaries, key bullet highlights, text-to-speech audio narration, and translation across 12 Indian regional languages. Users can track their daily reading streaks, bookmark articles for offline reading, share stories natively, and submit feedback backed by a resilient email delivery pipeline.

[![Next.js](https://img.shields.io/badge/Next.js-16.3.3-black?logo=next.js)](https://nextjs.org/)
[![React](https://img.shields.io/badge/React-19-blue?logo=react)](https://react.dev/)
[![Neon Database](https://img.shields.io/badge/Neon-PostgreSQL-00e599?logo=postgresql)](https://neon.tech/)
[![Drizzle ORM](https://img.shields.io/badge/Drizzle-ORM-C5F74F)](https://orm.drizzle.team/)
[![NextAuth.js](https://img.shields.io/badge/NextAuth.js-v5_beta-purple)](https://authjs.dev/)
[![Resend](https://img.shields.io/badge/Resend-Email_API-black?logo=resend)](https://resend.com/)
[![Vercel](https://img.shields.io/badge/Vercel-Deployed-success?logo=vercel)](https://multilingual-news-digest1-aqjw.vercel.app)

---

## 🚀 How to Access & Explore the Project

### Live Web Application
* **Production URL:** [https://multilingual-news-digest1-aqjw.vercel.app](https://multilingual-news-digest1-aqjw.vercel.app)
* **GitHub Repository:** [https://github.com/Rithvikk561/multilingual-news-digest1](https://github.com/Rithvikk561/multilingual-news-digest1)

---

### What to Look For Inside the App (Guided Tour)

1. **Top Dateline & Masthead:**
   * **Live Indian Dateline:** Formatted specifically for Indian regional locales.
   * **State & Region Selector:** Filter national stories or narrow coverage to specific Indian states (*Maharashtra, Karnataka, Tamil Nadu, Delhi, Kerala, Uttar Pradesh, etc.*).
   * **12-Language Dropdown:** Instantly translate the user interface and articles across 12 Indian regional languages (*Hindi, Bengali, Telugu, Marathi, Tamil, Urdu, Gujarati, Kannada, Odia, Malayalam, Punjabi, English*).
   * **Masthead Share:** One-click sharing for the entire newspaper digest.

2. **Category Navigation Bar:**
   * Filter news across desks: **Politics**, **Business**, **Technology**, **Sports**, **Education**, and **Entertainment**.
   * Supports hash-based deep linking (e.g., `#category-technology`) with smooth auto-scroll.

3. **Interactive News Cards:**
   * **Lead Story Feature:** The top headline of each section is styled with an editorial badge and featured layout.
   * **AI Summary & Bullet Highlights:** Skim bullet-point key takeaways directly on the card.
   * **Audio Reader (Listen):** Integrated speech synthesis reads the story aloud in the active regional language.
   * **Save (Bookmark):** Save stories with optimistic UI updates.
   * **Share Button:** Native Web Share API integration on mobile/desktop, with fallback to clipboard copying (`"Link copied!"` toast).
   * **In-Website Full Reader:** Click any story or *"Read Story →"* to view the complete context inside an accessible modal without external redirects.

4. **User Accounts & Daily Reading Streaks:**
   * Register with email and password, or sign in to synchronize activity.
   * **Daily Reading Streaks:** Daily logins and reading build your streak (`🔥 Day X Streak`), calculated in Indian Standard Time (IST).
   * **My Account Slide-in Drawer:** Inspect member stats, current streak, best streak, and manage up to 500 saved bookmarks.

5. **Direct User Feedback Drawer:**
   * Click **Feedback** to report bugs, suggest features, or share comments.
   * Backed by a PostgreSQL **persist-first architecture** with automated email dispatch to site administrators via Resend.

---

## ✨ Features Overview

### 1. News Aggregation & AI Processing
* **Multi-Source RSS Pipeline:** Continuously ingests and normalizes news feeds from trusted national publications (*The Hindu, Indian Express, NDTV, Times of India, etc.*).
* **Automated Categorization:** Classifies articles accurately into standard news desks.
* **Instant Summaries & Highlights:** Extracts core bullet points for quick skimming.
* **Smart Image Fallback:** Filters broken, placeholder, or low-resolution thumbnail images, replacing them with clean editorial placeholders.
* **Date Switcher & Archive:** View today's feed or step back in time (*Yesterday, 2 Days Ago, 3 Days Ago, Past Week, or Custom Date*).
* **Headline Search Filter:** Real-time client-side search filtering across all loaded headlines.

### 2. Regional Languages & Accessibility
* **12 Indian Regional Languages Supported:**
  * English (`en`), Hindi (`hi`), Bengali (`bn`), Telugu (`te`), Marathi (`mr`), Tamil (`ta`), Urdu (`ur`), Gujarati (`gu`), Kannada (`kn`), Odia (`or`), Malayalam (`ml`), Punjabi (`pa`).
* **Complete UI Localization:** Every button, tab, date, badge, and modal message is translated natively.
* **Client Translation Cache:** Caches on-demand article translations to prevent duplicate API calls.
* **Text-to-Speech (TTS) Engine:** Built-in audio speech synthesis honoring the active regional language dialect.

### 3. User Authentication & Security
* **NextAuth v5 (Auth.js) JWT Sessions:** Fast, stateless, rolling JWT session cookies with 30-day expiration.
* **Encrypted Passwords:** Passwords salted and hashed with `bcryptjs` (12 rounds).
* **Brute-Force & Rate Limiting:** Server-side rate limiting on registration endpoints (5 attempts per IP per hour) to prevent credential stuffing.
* **Clear User Feedback:** Prominent in-modal and floating on-screen success popups confirming account creation and sign-in.

### 4. Reading Streaks & Saved Articles
* **IST-Synchronized Daily Streaks:** Automatically tracks daily reading habits in Indian Standard Time (`UTC+5:30`), supporting same-day idempotency, consecutive increments, and grace periods.
* **Canonical URL Normalization:** Uses deterministic 32-character SHA-256 hashes that strip tracking parameters (`utm_*`, `gclid`, `fbclid`, etc.) to guarantee bookmark integrity across sources.
* **Saved Articles Drawer:** Slide-in management panel to review and remove bookmarked articles with optimistic UI response.

### 5. Universal Sharing & Deep Linking
* **Multi-Level Share Buttons:** Embedded on every single news card, the in-modal reader, category headers, and saved bookmarks.
* **Web Share API with Clipboard Fallback:** Launches the mobile OS share drawer on mobile devices, or copies direct article links to clipboard with animated `"Link copied!"` toasts.

---

## 📧 Backend Feedback Pipeline & Email Delivery Engine

MND features a resilient, enterprise-grade feedback architecture designed to prevent data loss even during network failures or third-party email outages:

```
[ User Submits Feedback ]
           │
           ▼
[ Server-Side Validation ] ──> Length (10-2000 chars), category enum check, name sanitation
           │
           ▼
[ SHA-256 IP Hashing ] ──────> Privacy preservation (never stores raw IP addresses)
           │
           ▼
[ Concurrency-Safe Rate Limit ] ──> Max 3 submissions per IP per hour via atomic SQL upsert
           │
           ▼
[ 10-Minute Deduplication ] ──> Suppresses accidental double-clicks & spam via content hash
           │
           ▼
[ 1. Persist to PostgreSQL ] ──> Durably writes feedback with status 'pending' (NEVER LOST)
           │
           ▼
[ 2. Resend API Dispatch ] ───> Sends email with 5s timeout & stable idempotency key
     ├── Success ─────────────> Updates delivery_status to 'sent', records message ID
     └── Failure / Timeout ───> Marks 'failed', records error message, schedules for worker
           │
           ▼
[ Background Retry Worker ] ──> Exponential backoff (1m, 2m, 4m, 8m, 16m), atomic lease locks
```

### Key Backend Technical Capabilities:
* **Persist-First Guarantee:** Submissions are saved to the Neon PostgreSQL database *before* attempting network requests to Resend. If Resend is unreachable or times out, the user's message is safely preserved.
* **Atomic Rate Limiting:** Uses PostgreSQL atomic `INSERT ... ON CONFLICT DO UPDATE` on `feedback_rate_limit` to prevent race conditions during concurrent submissions.
* **Privacy by Design:** Raw IP addresses are immediately hashed using one-way `SHA-256` before hitting database records or logs.
* **Spam & Deduplication Guard:** Generates a deterministic content hash (`sha256(ip_hash + message)`), rejecting identical submissions sent within a 10-minute sliding window.
* **Resend Email Integration:**
  * Structured HTML/text notification templates with category, sanitized name, timestamp, and preview text.
  * Custom idempotency keys (`feedback_${id}`) preventing duplicate email deliveries during network retries.
  * Controlled 5-second request timeouts via `AbortSignal.timeout(5000)`.
* **Autonomous Background Retry Worker (`scripts/retry-feedback-email.js`):**
  * Implements exponential backoff: `1 minute * 2^retry_count`.
  * Atomic lease claiming with `claim_started_at` to avoid duplicate processing across distributed workers.
  * 10-minute stale lease recovery if an active worker crashes mid-delivery.
  * Terminal failure threshold: transitions to `permanently_failed` after 5 unsuccessful attempts.

---

## 🗄️ Database Architecture & Schema

Built with **Neon Serverless PostgreSQL** and mapped with **Drizzle ORM**:

```
┌────────────────────────┐       ┌────────────────────────┐
│         users          │       │      user_streaks      │
├────────────────────────┤       ├────────────────────────┤
│ id (PK, UUID)          │◄──┐   │ id (PK, UUID)          │
│ email (VARCHAR, UNIQUE)│   └───┼ user_id (FK, UNIQUE)   │
│ password_hash (VARCHAR)│       │ current_streak (INT)   │
│ created_at (TIMESTAMP) │       │ longest_streak (INT)   │
└────────────────────────┘       │ last_login_date (DATE) │
            │                    │ today_counted (BOOL)   │
            ▼                    └────────────────────────┘
┌─────────────────────────────────┐
│         saved_articles          │
├─────────────────────────────────┤
│ id (PK, UUID)                   │
│ user_id (FK -> users.id)        │
│ article_stable_id (VARCHAR(32)) │
│ article_link (TEXT)             │
│ article_title (TEXT)            │
│ article_source (VARCHAR)        │
│ article_image_url (TEXT)        │
│ article_summary (TEXT)          │
│ article_category (VARCHAR)      │
│ saved_at (TIMESTAMP)            │
└─────────────────────────────────┘

┌─────────────────────────────────┐       ┌───────────────────────────────┐
│            feedback             │       │      feedback_rate_limit      │
├─────────────────────────────────┤       ├───────────────────────────────┤
│ id (PK, UUID)                   │       │ id (PK, UUID)                 │
│ category (VARCHAR)              │       │ ip_hash (VARCHAR(64), UNIQUE) │
│ name_provided (VARCHAR)         │       │ submission_count (INT)        │
│ message (TEXT)                  │       │ window_start (TIMESTAMP)      │
│ ip_hash (VARCHAR(64))           │       └───────────────────────────────┘
│ content_hash (VARCHAR(64))      │
│ email_sent (BOOLEAN)            │       ┌───────────────────────────────┐
│ delivery_status (VARCHAR)       │       │    registration_rate_limit    │
│ resend_message_id (VARCHAR)     │       ├───────────────────────────────┤
│ delivery_error (TEXT)           │       │ id (PK, UUID)                 │
│ delivery_attempted_at (TS)      │       │ ip_hash (VARCHAR(64), UNIQUE) │
│ claim_started_at (TS)           │       │ registration_count (INT)      │
│ retry_count (INT)               │       │ window_start (TIMESTAMP)      │
│ created_at (TIMESTAMP)          │       └───────────────────────────────┘
└─────────────────────────────────┘
```

---

## 🛠️ Tech Stack

| Layer | Technology | Purpose |
| :--- | :--- | :--- |
| **Framework** | [Next.js 16.3.3](https://nextjs.org/) (App Router & Turbopack) | Server-side rendering, routing, & API endpoints |
| **Frontend UI** | [React 19](https://react.dev/) | Component architecture & client-side state |
| **Database** | [Neon](https://neon.tech/) Serverless PostgreSQL | High-performance serverless cloud relational database |
| **ORM** | [Drizzle ORM 0.45](https://orm.drizzle.team/) & Drizzle Kit | Type-safe SQL query generation and schema migrations |
| **Authentication** | [NextAuth.js v5](https://authjs.dev/) (Auth.js beta) | Stateless session tokens & credential authorization |
| **Password Security** | `bcryptjs` (12 rounds) | Password hashing & salt generation |
| **Email Service** | [Resend SDK](https://resend.com/) | Transactional feedback email dispatch |
| **Content Ingestion** | `rss-parser` & `sanitize-html` | RSS feed parsing, sanitization, & normalization |
| **Styling** | Custom Editorial CSS (Design Tokens) | High-contrast newspaper aesthetics & responsive layouts |
| **Hosting & CI/CD** | [Vercel](https://vercel.com/) | Continuous deployment, serverless edge functions, SSL |

---

## 💻 Local Development Setup

### 1. Clone the repository
```bash
git clone https://github.com/Rithvikk561/multilingual-news-digest1.git
cd multilingual-news-digest1
```

### 2. Install dependencies
```bash
npm install
```

### 3. Configure Environment Variables
Create a `.env.local` file in your root folder:
```env
AUTH_SECRET=your-random-32-byte-secret
NEXTAUTH_SECRET=your-random-32-byte-secret
AUTH_TRUST_HOST=true
DATABASE_URL=postgresql://neondb_owner:password@your-neon-host.neon.tech/neondb?sslmode=require
RESEND_API_KEY=re_your_api_key
RESEND_FROM_EMAIL="onboarding@resend.dev"
FEEDBACK_TO_EMAIL=your-inbox@gmail.com
```

### 4. Push Database Schema
```bash
npm run db:generate
npm run db:migrate
```

### 5. Run Development Server
```bash
npm run dev
```
Open **[http://localhost:3000](http://localhost:3000)** in your browser.

### 6. Run the Feedback Email Retry Worker
```bash
node scripts/retry-feedback-email.js
```

---

## 🧪 Testing & Verification

MND is backed by automated unit and integration tests:

```bash
# Run complete test suite (unit + feedback pipeline)
npm test

# Run isolated unit tests
npm run test:unit

# Run route handlers suite
npm run test:routes

# Run dedicated feedback worker test suite
npm run test:feedback

# Production build check
npm run build
```

* **97 Unit & Integration Tests Passing:** Validates URL normalization, streak mathematics, password encryption, registration rate limits, and authentication callbacks.
* **Phase 6 Feedback Suite Passing:** Verifies atomicity, exponential backoff timings, deduplication filters, and lease recovery.

---

## 📂 Project Directory Structure

```text
multilingual-news-digest1/
├── app/
│   ├── api/
│   │   ├── auth/[...nextauth]/route.js   # NextAuth session & token handlers
│   │   ├── feedback/route.js              # Feedback persistence & Resend email API
│   │   ├── news/route.js                  # Multi-category RSS aggregation
│   │   ├── summarize/route.js             # AI headline & bullet summarization
│   │   ├── translate/route.js             # Regional language translation endpoint
│   │   ├── tts/route.js                    # Text-to-speech audio reader API
│   │   └── user/
│   │       ├── profile/route.js           # User account profile stats
│   │       ├── register/route.js          # User registration with rate limiting
│   │       ├── saved/route.js             # Saved bookmarks list & creation
│   │       └── streak/route.js            # IST-based streak calculation
│   ├── globals.css                        # Newspaper editorial design system
│   ├── layout.js                          # Root HTML layout with providers
│   ├── page.js                            # Main newspaper feed view
│   └── providers.js                       # NextAuth session provider wrapper
├── components/
│   ├── ArticleReaderModal.js              # In-site full reader with audio & share
│   ├── AuthModal.js                       # Sign In & Registration dialog
│   ├── CategoryNav.js                     # News desk category navigation bar
│   ├── CategorySection.js                 # Category grid container with lead story
│   ├── DateSwitcher.js                    # Archive dateline switcher & search bar
│   ├── FeedbackModal.js                   # Feedback form with instant status
│   ├── Footer.js                          # Editorial footer & quick links
│   ├── Header.js                          # Masthead, state selector, & language menu
│   ├── NewsCard.js                        # Individual story card with audio & bookmark
│   ├── ProfilePanel.js                    # User profile, streak badges, & bookmarks
│   ├── ShareButton.js                     # Universal Web Share & clipboard button
│   └── StreakPopup.js                     # Animated streak milestone notifications
├── lib/
│   ├── articleId.js                       # 32-hex canonical URL identification
│   ├── auth.js                            # NextAuth v5 configuration & credentials
│   ├── db.js                              # Neon Serverless PostgreSQL connection
│   ├── fetchNews.js                       # RSS fetching & client cache manager
│   ├── languages.js                       # 12 Indian regional language configs
│   ├── schema.js                          # Drizzle ORM PostgreSQL schema
│   ├── streakLogic.js                     # Pure IST-based streak calculations
│   └── translations.js                    # UI localization dictionary
└── scripts/
    └── retry-feedback-email.js            # Resend email retry worker
```

---

## 📄 License
This project is open-source and available under the [MIT License](LICENSE).
