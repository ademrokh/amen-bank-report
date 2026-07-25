# Routes Specification - Amen Bank Digital Solution

## Documentation Status and Missing Details
This route guide captures the intended site structure, but a few implementation details are still missing from the documentation:
- The final route map should reflect the exact current implementation under the dynamic language segments.
- Middleware and redirect behavior for locale detection need a formal write-up.
- 404 and fallback behavior for unsupported locales should be documented.
- Page-level route ownership for future feature work should be added.

### Recommended follow-up
- Align this document with the final deployed route structure after implementation is complete.
- Add a route ownership matrix for each major page and section.
- Document any redirects, rewrites, and locale fallbacks in one centralized section.

## URL Structure Overview

### Multilingual Routing Pattern
```
Base URL: https://amenbank.tn

Structure: https://amenbank.tn/{lang}/{route}
- {lang}: fr | ar | en
- {route}: specific page path
```

### Language-Specific Routes
```
French:  /fr/*
Arabic:  /ar/*
English: /en/*
```

### Root Redirect Behavior
```
/ → /fr/ (default to French)
Respects browser language preference if available
Falls back to French for unrecognized languages
```

---

## Page Structure & Routes

### 1. HOME PAGE

**Routes:**
```
/fr/ (French)
/ar/ (Arabic)
/en/ (English)
```

**URL Pattern (Dev):**
```
http://localhost:3000/fr/
http://localhost:3000/ar/
http://localhost:3000/en/
```

**Components:**
- **Hero Section**
  - Headline: "Votre Partenaire Financier de Confiance" (FR)
  - Sub-headline: Banking relationship statement
  - CTAs: "Ouvrez votre compte" + "Avez-vous besoin d'un prêt?"
  - Background: Professional image or gradient

- **Live Exchange Rate Ticker** (top of page)
  - EUR → TND (current rate)
  - USD → TND (current rate)
  - CAD → TND (current rate)
  - Auto-update every 60 seconds
  - Source: Real-time API or static JSON for MVP

- **SICAV Values Strip**
  - ALLIANCE SICAV (price)
  - SICAV AMEN (price)
  - AMEN PREMIERE (price)
  - TUNISIE SICAV (price)
  - Daily update, color-coded (green/red for change)

- **Featured Products Section**
  - Grid of top-4 products with icons
  - Quick links to detail pages

- **Trust Indicators Section**
  - "40+ years of experience"
  - "1.2M+ satisfied clients"
  - "164 branches across Tunisia"
  - "100% secure transactions"

- **Call-to-Action Section**
  - "Start your journey with Amen Bank"
  - Primary CTA: "Open Account"
  - Secondary CTA: "Learn More"

---

### 2. RETAIL BANKING PAGE

**Routes:**
```
/fr/particuliers
/ar/particuliers
/en/particuliers

/fr/retail (English version)
/ar/retail
/en/retail
```

**Page Components:**
- **Hero Section**
  - "Banking Solutions for Individuals"
  - Breadcrumb: Home > Retail Banking

- **Product Category Grid** (6 cards):
  - **Comptes Bancaires** (Bank Accounts)
    - Deposit accounts, current accounts, savings accounts
    - CTA: "Learn More" → `/particuliers/comptes`
  
  - **Cartes Bancaires** (Bank Cards)
    - Visa Classique, Visa Gold, Visa Platinum
    - CTA: "Learn More" → `/particuliers/cartes`
  
  - **Épargne & Placements** (Savings & Investments)
    - Fixed-term deposits, SICAV products
    - CTA: "Learn More" → `/particuliers/epargne`
  
  - **Crédits & Prêts** (Loans)
    - Home loans, car loans, consumer loans
    - CTA: "Learn More" → `/particuliers/credits`
  
  - **Amen First Bank** (Digital Banking)
    - 100% online, 24/7 access
    - CTA: "Learn More" → `/particuliers/amen-first-bank`
  
  - **Services Additionnels** (Additional Services)
    - Insurance, safe deposit boxes, other services
    - CTA: "Learn More" → `/particuliers/services`

- **Comparison Table**
  - Feature comparison of all products

- **FAQ Snippet**
  - 5-6 most common questions
  - Link: "See all FAQs" → `/faq`

---

### 3. CORPORATE BANKING PAGE

**Routes:**
```
/fr/entreprises
/ar/entreprises
/en/entreprises

/fr/business (English version)
/ar/business
/en/business
```

**Page Components:**
- **Hero Section**
  - "Banking Solutions for Businesses"
  - Breadcrumb: Home > Corporate Banking

- **Product Category Grid** (6 cards):
  - **Comptes Courants Pro** (Business Current Accounts)
    - Daily operations, payroll management
    - CTA: "Learn More" → `/entreprises/comptes`
  
  - **Financement CT/MT** (Short/Medium-Term Financing)
    - Working capital, investment financing
    - CTA: "Learn More" → `/entreprises/financement`
  
  - **Services de Change** (Foreign Exchange)
    - Amen FX, currency trading
    - CTA: "Learn More" → `/entreprises/change`
  
  - **Placements & SICAV** (Investments)
    - Corporate investment products
    - CTA: "Learn More" → `/entreprises/placements`
  
  - **Virement Salaires** (Payroll Transfer)
    - Automated salary distribution
    - CTA: "Learn More" → `/entreprises/virement-salaires`
  
  - **Trading Room** (Advanced Trading)
    - For corporate clients with advanced needs
    - CTA: "Learn More" → `/entreprises/trading-room`

- **Comparison Table**
  - Feature comparison of business products

- **Contact Sales Section**
  - "Get personalized quote"
  - Form or direct phone number

---

### 4. PRODUCT DETAIL PAGES

**Retail Routes:**
```
/fr/particuliers/comptes
/fr/particuliers/cartes
/fr/particuliers/epargne
/fr/particuliers/credits
/fr/particuliers/amen-first-bank
/fr/particuliers/services
```

**Corporate Routes:**
```
/fr/entreprises/comptes
/fr/entreprises/financement
/fr/entreprises/change
/fr/entreprises/placements
/fr/entreprises/virement-salaires
/fr/entreprises/trading-room
```

**Generic Detail Page Structure:**
- **Hero** with product name
- **Key Features** (3-5 main benefits)
- **Detailed Description** (how it works)
- **Fee Schedule** (transparent pricing)
- **Eligibility Requirements** (who can open)
- **FAQ Section** (product-specific questions)
- **CTA:** "Open Account" or "Contact Sales"

---

### 5. AMEN FIRST BANK PAGE

**Routes:**
```
/fr/amen-first-bank
/ar/amen-first-bank
/en/amen-first-bank
```

**Page Components:**
- **Hero Section**
  - "Banking 24/7 — Anywhere, Anytime"
  - "100% Digital. 100% Secure. 100% Yours."
  - App store badges (iOS, Android)

- **Feature Showcase** (4-5 cards):
  - Open account in <5 minutes
  - Real-time notifications
  - Zero-fee transfers
  - 24/7 customer support
  - Biometric security

- **Screenshots/Screenshots Carousel**
  - Mobile app UI mockups (actual screenshots)

- **Security Information**
  - Encryption details
  - Two-factor authentication
  - Fraud protection

- **Customer Testimonials**
  - 3-4 video or text testimonials

- **FAQ Section**
  - Common questions about digital banking

- **Download Section**
  - App Store link
  - Google Play link
  - QR code for mobile download

---

### 6. AGENCY LOCATOR PAGE

**Routes:**
```
/fr/reseau-agences
/ar/reseau-agences
/en/reseau-agences
```

**Page Components:**
- **Hero Section**
  - "Find Your Nearest Branch"
  - Breadcrumb: Home > Agencies

- **Search Bar**
  - Search by city, agency name
  - Filter by services (e.g., "Credit", "Cards")
  - Filter by region

- **Interactive Map**
  - Leaflet.js map showing all 164 branches
  - Markers for each agency
  - Click to see details

- **Agencies List**
  - Sidebar with agency listings
  - Name, address, phone, hours
  - Services available at branch
  - Distance calculation (from user location, if geo-enabled)

- **Agency Details Panel**
  - Selected agency information
  - Address (with map marker)
  - Phone number (clickable tel: link)
  - Hours of operation
  - Services available
  - Appointment booking CTA

- **Regional Directory**
  - Agencies grouped by region (14 regions)
  - Collapsible sections

---

### 7. ONBOARDING / BECOME A CLIENT PAGE

**Routes:**
```
/fr/devenir-client
/ar/devenir-client
/en/devenir-client
```

**Page Components:**
- **Hero Section**
  - "Join Thousands of Satisfied Customers"
  - Breadcrumb: Home > Become a Client

- **Eligibility Section**
  - Requirements (age, residency, documents)
  - Timeline: "5 minutes to open"

- **Step-by-Step Process**
  - Step 1: Documentation (what you need)
  - Step 2: Online Form (with form preview)
  - Step 3: Verification
  - Step 4: Account Activation

- **Benefits Highlight**
  - What you get after opening account
  - First month free (if applicable)
  - Welcome bonus information

- **Online Application Form**
  - Progressive form (visible fields: 5-7 max)
  - Personal info, ID, employment
  - Address verification
  - Phone verification
  - **Form submission → `/fr/devenir-client/confirmation`**

- **Comparison with Competitors** (optional)
  - Why choose Amen Bank?

- **FAQ Section**
  - "Why should I open an account?"
  - "What documents do I need?"
  - "How long does verification take?"

- **Support Section**
  - Live chat option
  - Phone support: 71 833 517

---

### 8. CONTACT PAGE

**Routes:**
```
/fr/contact
/ar/contact
/en/contact
```

**Page Components:**
- **Hero Section**
  - "Get in Touch with Amen Bank"
  - Breadcrumb: Home > Contact

- **Contact Information Card**
  - **Email:** amenbank@amenbank.com.tn
  - **Phone:** 71 833 517, 71 148 000
  - **Address:** Av. Mohamed V, 1002 Tunis, Tunisia
  - **Hours:** Mon-Fri 8h00-17h00, Sat 9h00-12h00

- **Contact Form Section**
  - Name (required)
  - Email (required)
  - Phone (optional)
  - Subject (dropdown: Account, Loan, Card, Other)
  - Message (required, textarea)
  - **Form submission → `/fr/contact/confirmation`**
  - **Backend endpoint:** `POST /api/contact`

- **Embedded Map**
  - Leaflet.js map with HQ location pinned
  - "Get Directions" button

- **Social Media Links**
  - Facebook
  - LinkedIn
  - Twitter/X
  - Instagram

- **Support Options**
  - **Live Chat** (bottom-right, powered by chatbot)
  - **Phone Support** (with hours)
  - **Email Support** (with response time)
  - **Branch Locator** (quick link)

- **FAQ Link**
  - "Can't find an answer?" → `/faq`

---

### 9. FAQ PAGE

**Routes:**
```
/fr/faq
/ar/faq
/en/faq
```

**Page Components:**
- **Hero Section**
  - "Frequently Asked Questions"
  - Search bar for FAQ lookup
  - Breadcrumb: Home > FAQ

- **Category Tabs/Accordion**
  - Particuliers (Retail)
  - Entreprises (Corporate)
  - Digital Banking
  - General
  - Network & Branches

- **Accordion List**
  - Question (clickable header)
  - Answer (expandable content)
  - Search highlights matching FAQs in real-time

- **Embedded Chatbot**
  - Below FAQ section
  - "Didn't find an answer? Ask our AI assistant"
  - Floating chat interface

- **Related Links**
  - "Contact us" (if FAQ doesn't answer)
  - "Find a branch" (location-specific questions)
  - "Open an account" (onboarding questions)

---

### 10. CONFIRMATION/SUCCESS PAGES

**Routes:**
```
/fr/contact/confirmation
/ar/contact/confirmation
/en/contact/confirmation

/fr/devenir-client/confirmation
/ar/devenir-client/confirmation
/en/devenir-client/confirmation
```

**Components:**
- Success checkmark icon
- "Thank you" message
- Confirmation details (email, ticket number)
- Next steps information
- Expected response time
- Return home link

---

### 11. ERROR PAGES

**Routes:**
```
/404 (not found)
/500 (server error)
/503 (service unavailable)
```

**Components:**
- Error code and message
- Simple explanation
- Return home link
- Contact support link

---

## Next.js File Structure

```
frontend/app/
├── [lang]/
│   ├── layout.tsx                 // Language wrapper layout
│   ├── page.tsx                   // HOME: /fr/, /ar/, /en/
│   ├── particuliers/
│   │   ├── page.tsx               // Retail banking main page
│   │   ├── comptes/page.tsx
│   │   ├── cartes/page.tsx
│   │   ├── epargne/page.tsx
│   │   ├── credits/page.tsx
│   │   ├── amen-first-bank/page.tsx
│   │   └── services/page.tsx
│   ├── entreprises/
│   │   ├── page.tsx               // Corporate banking main page
│   │   ├── comptes/page.tsx
│   │   ├── financement/page.tsx
│   │   ├── change/page.tsx
│   │   ├── placements/page.tsx
│   │   ├── virement-salaires/page.tsx
│   │   └── trading-room/page.tsx
│   ├── amen-first-bank/page.tsx   // Digital banking highlight
│   ├── reseau-agences/page.tsx     // Agency locator
│   ├── devenir-client/
│   │   ├── page.tsx
│   │   └── confirmation/page.tsx
│   ├── contact/
│   │   ├── page.tsx
│   │   └── confirmation/page.tsx
│   ├── faq/page.tsx
│   └── [notfound]/page.tsx        // 404 page
├── layout.tsx                      // Global layout (header, footer)
├── api/
│   ├── contact/route.ts           // POST /api/contact
│   ├── chat/route.ts              // POST /api/chat (to backend)
│   └── health/route.ts            // GET /api/health
└── globals.css
```

---

## Internationalization (i18n) Routing

### Middleware Route Handling
```typescript
// middleware.ts - Detect language and route accordingly
export function middleware(request: NextRequest) {
  const pathname = request.nextUrl.pathname;
  
  // Check if pathname has a language prefix
  const pathnameHasLocale = locales.some(
    (locale) => pathname.startsWith(`/${locale}/`) || pathname === `/${locale}`
  );
  
  if (!pathnameHasLocale) {
    // Redirect to French by default, or browser language preference
    return NextResponse.redirect(
      new URL(`/fr${pathname}`, request.url)
    );
  }
}

export const config = {
  matcher: [
    '/((?!api|_next/static|_next/image|favicon.ico).*)',
  ],
};
```

---

## API Routes (Backend Integration)

### Endpoints to Call from Frontend

#### 1. Chat Endpoint
```
POST /api/chat

Request:
{
  "message": "Quels sont les taux de crédit?",
  "session_id": "user-session-123",
  "lang": "fr"
}

Response:
{
  "answer": "Les taux de crédit varient selon...",
  "sources": [...],
  "lang": "fr"
}

Location: `/api/chat` (Next.js API route → FastAPI backend)
```

#### 2. Contact Form Submission
```
POST /api/contact

Request:
{
  "name": "Ahmed",
  "email": "ahmed@example.com",
  "phone": "+216 20 123 456",
  "subject": "Account",
  "message": "I have a question about...",
  "lang": "fr"
}

Response:
{
  "success": true,
  "ticket_id": "TKT-2025-001234",
  "message": "We'll respond within 24 hours"
}
```

#### 3. Health Check
```
GET /api/health

Response:
{
  "status": "ok",
  "version": "1.0.0",
  "timestamp": "2025-06-12T10:30:00Z"
}
```

---

## Sitemap & SEO Structure

### URL Priority (for robots.txt/sitemap)
```
Priority 1.0: /fr/, /ar/, /en/ (homepage)
Priority 0.9: /fr/particuliers, /fr/entreprises
Priority 0.8: Product detail pages
Priority 0.7: Agency locator, FAQ
Priority 0.6: Contact, onboarding confirmation
```

### Meta Tags per Page
```
/ :
  title: "Amen Bank - Your Trusted Financial Partner"
  description: "40+ years of banking excellence. Join 1.2M+ clients."

/particuliers :
  title: "Retail Banking - Amen Bank"
  description: "Accounts, cards, loans, savings — banking solutions for you."

/entreprises :
  title: "Corporate Banking - Amen Bank"
  description: "Business accounts, financing, forex services — for your business."

/faq :
  title: "FAQ - Amen Bank"
  description: "Find answers to common questions about our products and services."
```

---

## Performance & Accessibility Targets

### Page Load Time Targets
- Homepage: < 2s (First Contentful Paint)
- Product pages: < 1.5s
- Agency locator: < 2s (map initialization)

### Accessibility (WCAG 2.1 AA)
- Color contrast: 4.5:1 for body text
- Alt text on all images
- Keyboard navigation for all interactive elements
- Form labels associated with inputs
- Language declared in HTML (`lang` attribute)

### Mobile Responsiveness
- Tested on iPhone 12, Galaxy S21, iPad
- Touch targets: minimum 44x44px
- Viewport meta tag configured

---

*Routes Specification v1.0 — June 2025*
*Last Updated: 2025-06-12*
