# Language Specification - Amen Bank Digital Solution

## Supported Languages

### 1. French (Français) — Primary Language
- **Code:** `fr` / `fr-TN` (Tunisian French)
- **Default Route:** `/fr/`
- **Script Direction:** LTR (Left-to-Right)
- **Tone:** Formal, professional (banking standard)
- **Terminology:** Uses local Tunisian banking terminology

**Key French Terms:**
- Compte dépôt = Deposit account
- Virement = Transfer
- Crédit = Loan
- Épargne = Savings
- Cartes bancaires = Bank cards
- SICAV = Mutual funds
- Amen Net = Online banking portal

---

### 2. Arabic (العربية) — Secondary Language
- **Code:** `ar` / `ar-TN` (Tunisian Arabic)
- **Default Route:** `/ar/`
- **Script Direction:** RTL (Right-to-Left) ⚠️ **Critical for UI**
- **Tone:** Formal, respectful, professional
- **Dialect:** Tunisian Arabic (Darija), with Modern Standard Arabic (MSA) fallback

**Key Arabic Terms:**
- حساب جاري = Current account
- تحويل = Transfer
- قرض = Loan
- توفير = Savings
- بطاقات بنكية = Bank cards
- صندوق استثمار = Investment fund

**RTL Implementation Requirements:**
- Flex layout direction: `flex-row-reverse` for header nav
- Text alignment: `text-right` for body copy
- Padding/margins: Reverse (right instead of left)
- Icons: May need flipping for directional UI elements
- Form inputs: Align right

---

### 3. English (English) — Tertiary Language
- **Code:** `en` / `en-US`
- **Default Route:** `/en/`
- **Script Direction:** LTR (Left-to-Right)
- **Tone:** Professional, clear, accessible
- **Audience:** International visitors, expat business community

**Key English Terms:**
- Current account = Deposit account
- Transfer = Wire
- Loan = Credit facility
- Savings = Investment product
- Bank card = Debit/Credit card
- Mutual fund = SICAV

---

## Language Routing Strategy

### Frontend (Next.js)
```
/               → Redirects to /fr/ (default)
/fr/            → French homepage
/fr/products    → French products page
/ar/            → Arabic homepage
/ar/products    → Arabic products page
/en/            → English homepage
/en/products    → English products page
```

### URL Structure
- **Prefix routing:** Language code in URL path (`/fr/`, `/ar/`, `/en/`)
- **No subdomain routing** (avoid: `fr.amenbank.tn`)
- **No query params** for language (avoid: `?lang=fr`)
- **Preserve language** when user navigates: links within `/fr/` stay in `/fr/`

### Language Toggle
- **Header placement:** Top-right (LTR) / Top-left (RTL)
- **Display options:**
  - Flag icons + language code (FR, AR, EN)
  - Avoid flag emojis (not accessibility-friendly)
- **User preference:** Store in localStorage as `preferred_language`

---

## Chatbot Language Detection

### Detection Priority (in order):
1. **User preference** (localStorage)
2. **URL language code** (from route)
3. **Browser language** (from `navigator.language`)
4. **Default to French** (fallback)

### Code Example:
```typescript
function detectLanguage(route: string): 'fr' | 'ar' | 'en' {
  // 1. Check URL route
  if (route.startsWith('/fr')) return 'fr';
  if (route.startsWith('/ar')) return 'ar';
  if (route.startsWith('/en')) return 'en';
  
  // 2. Check localStorage
  const stored = localStorage.getItem('preferred_language');
  if (stored === 'fr' || stored === 'ar' || stored === 'en') return stored;
  
  // 3. Check browser language
  const browser = navigator.language;
  if (browser.startsWith('ar')) return 'ar';
  if (browser.startsWith('en')) return 'en';
  
  // 4. Default to French
  return 'fr';
}
```

---

## Backend Chatbot System Prompts

### French System Prompt
```
Tu es un assistant bancaire intelligent pour Amen Bank Tunisia.
- Réponds toujours en français tunisien formel.
- Utilise la terminologie bancaire appropriée.
- Sois professionnel, courtois et utile.
- Si tu ne connais pas la réponse, propose de contacter le service client: 71 833 517.
- Fournis des informations précises sur les produits et services d'Amen Bank.
```

### Arabic System Prompt
```
أنت مساعد بنكي ذكي لبنك آمن تونس.
- رد دائماً باللغة العربية التونسية الرسمية.
- استخدم المصطلحات المصرفية المناسبة.
- كن احترافياً وودياً ومساعداً.
- إذا كنت لا تعرف الإجابة، اقترح الاتصال بخدمة العملاء: 71 833 517.
- قدم معلومات دقيقة عن منتجات وخدمات بنك آمن.
```

### English System Prompt
```
You are an intelligent banking assistant for Amen Bank Tunisia.
- Always respond in professional English.
- Use appropriate banking terminology.
- Be professional, courteous, and helpful.
- If you don't know the answer, suggest contacting customer service: 71 833 517.
- Provide accurate information about Amen Bank's products and services.
```

---

## Multilingual Chat Endpoint

### Request Format:
```json
{
  "message": "Quel est le taux intérêt?",
  "session_id": "user-123",
  "lang": "fr"  // Optional: will detect if not provided
}
```

### Response Format:
```json
{
  "answer": "Le taux d'intérêt varie selon...",
  "sources": [
    {
      "faq_id": 4,
      "question": "Quels types de crédits sont disponibles?",
      "relevance": 0.92
    }
  ],
  "lang": "fr",
  "timestamp": "2025-06-12T10:30:00Z"
}
```

---

## Language-Specific Content Guidelines

### French (FR)
- **Formal register:** Use "vous" for customers (not "tu")
- **Terminology:** Follow Tunisian banking standards
- **Emphasis:** Trust, stability, institutional presence
- **Tone samples:**
  - "Bienvenue chez Amen Bank, votre partenaire de confiance"
  - "Nous sommes à votre service"

### Arabic (AR)
- **Formal register:** Use formal classical Arabic with Tunisian dialect
- **Terminology:** Islamic banking terms where applicable
- **Emphasis:** Family, security, tradition
- **Tone samples:**
  - "أهلاً وسهلاً في بنك آمن"
  - "نحن هنا لخدمتكم"

### English (EN)
- **Formal register:** Professional, international standard English
- **Terminology:** Standard international banking terms
- **Emphasis:** Innovation, efficiency, modern services
- **Tone samples:**
  - "Welcome to Amen Bank, your trusted financial partner"
  - "We're here to serve you"

---

## Implementation Checklist

### Frontend
- [ ] Set up Next.js i18n routing with `next-intl` or similar
- [ ] Create language toggle component (flags + language codes)
- [ ] Implement localStorage for language preference
- [ ] Set HTML `lang` attribute per page (`<html lang="fr">`)
- [ ] CSS `dir` attribute for RTL support (`<html dir="rtl">`)
- [ ] Test all pages in FR, AR, EN

### Backend
- [ ] Accept `lang` parameter in chatbot endpoint
- [ ] Implement language detection (langdetect library)
- [ ] Load appropriate system prompt based on language
- [ ] Ensure FAQ retrieval returns multilingual results
- [ ] Test LLM responses in all three languages

### Testing
- [ ] Test language switching without page reload
- [ ] Verify RTL layout doesn't break on Arabic
- [ ] Check form inputs work correctly in all languages
- [ ] Validate Arabic text display (no garbled characters)
- [ ] Performance test with language switching

---

## Language Files Structure

## Documentation Status and Missing Details
This language guide is strong, but it still needs a few practical additions for long-term maintenance:
- A reference for locale fallback behavior should be documented clearly.
- The translation workflow for future content updates should be defined.
- The document should mention how Arabic RTL testing will be validated in QA.

### Recommended follow-up
- Add a translation ownership and review process for new content.
- Document the fallback chain for missing translation strings.
- Link this file to the UAT checklist for multilingual validation steps.

```
frontend/
├── public/
│   └── locales/
│       ├── fr/
│       │   ├── common.json
│       │   ├── products.json
│       │   └── contact.json
│       ├── ar/
│       │   ├── common.json
│       │   ├── products.json
│       │   └── contact.json
│       └── en/
│           ├── common.json
│           ├── products.json
│           └── contact.json
```

---

## RTL CSS Reset (for Arabic)

```css
/* Arabic-specific overrides */
[dir="rtl"] {
  /* Navigation flex direction */
  .navbar-nav {
    flex-direction: row-reverse;
  }
  
  /* Text alignment */
  body {
    text-align: right;
  }
  
  /* Sidebar positioning */
  .sidebar {
    left: auto;
    right: 0;
  }
  
  /* Margins and padding */
  .content {
    margin-left: auto;
    margin-right: 0;
    padding-left: auto;
    padding-right: 1rem;
  }
}
```

---

*Language Specification v1.0 — June 2026*
