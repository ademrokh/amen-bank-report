# Amen Bank — AI Digital Solution: Project TODO

## Documentation Status and Missing Details
This todo list remains the main planning artifact, but it should be updated with current implementation realities and ownership notes:
- The current document should be linked to the latest implementation milestones and test evidence.
- Some completed items should be marked as verified rather than only planned.
- Production readiness criteria and rollout steps are still missing from the final phase section.
- Ownership for analytics, monitoring, and content maintenance should be made explicit.

### Recommended follow-up
- Add a short status table for each implementation phase with current completion and evidence.
- Mark the most recent completed milestones as verified and linked to the relevant documentation files.
- Add a launch checklist for deployment, rollback, and post-release monitoring.

> **Stage:** Conception d'une Solution Digitale Intelligente pour Amen Bank grâce à l'IA  
> **Duration:** 1 month | **Intern:** Adem Rokh | **Supervisor:** Mohamed Amine Baccouri

---

## Phase 1 — Needs Analysis

- [ ] **Benchmark reference bank websites**: BH Bank (primary), STB, Attijari Bank Tunisia
  - BH Bank: study nav structure, hero layout, product card patterns, Arabic/French language toggle, color palette (blues/greens typical of Tunisian banking)
  - Note design conventions common to Tunisian bank sites: dense information hierarchy, formal tone, prominent branch/agency finder, emphasis on institutional trust over minimalism
- [ ] **Define FAQ knowledge base content** from amenbank.com.tn
  - **Particuliers:** compte dépôt, cartes bancaires, épargne, crédits (immobilier/voiture/conso), Amen First Bank, coffres-forts, assurance famille
  - **Entreprises:** compte courant, financement CT/MT, virement salaires, produits de change (Amen FX, trading room), SICAV (ALLIANCE SICAV, SICAV AMEN, AMEN PREMIERE, TUNISIE SICAV)
  - **Digital:** Amen First Bank (100% online, 24/7), Amen Net (online banking portal), self-service spaces
  - **Réseau:** 164 agences réparties sur 14 directions régionales en Tunisie
  - **Contact:** `amenbank@amenbank.com.tn` — Av. Mohamed V, 1002 Tunis — Tél: 71 833 517 / 71 148 000
- [ ] **Define supported languages** per chatbot response
  - French (primary — site default at `/fr/`), Arabic (RTL support needed), English (site has `/en/` version)
- [ ] **Map out site pages/routes** based on amenbank.com.tn structure
  - `/` — Home (live exchange rate ticker: EUR/USD/CAD in TND; SICAV values; loan CTA)
  - `/particuliers` — Retail banking (comptes, cartes, épargne, crédits, banque à distance, coffres, assurance)
  - `/entreprises` — Corporate banking (compte courant, financement CT/MT, change, investissement, virement salaires)
  - `/amen-first-bank` — Online banking highlight page
  - `/reseau-agences` — Agency locator (164 agencies across 14 regions)
  - `/devenir-client` — Become a client / onboarding
  - `/contact` — Contact form + address + phone
  - `/faq` — FAQ page with embedded chatbot widget

---

## Phase 2 — Frontend (Next.js)

- [ ] **Build global layout**: header + footer + nav
  - Sticky header, mobile hamburger menu, responsive breakpoints at 768px / 1024px
- [ ] **Build Hero section** (homepage)
  - Headline + CTA ("Ouvrez votre compte" / "Avez-vous besoin d'un prêt ?"); live exchange rate ticker (EUR, USD, CAD → TND); SICAV values strip
- [ ] **Build Products section**
  - Two tracks: **Particuliers** (comptes, cartes, épargne, crédits, Amen First Bank) and **Entreprises** (compte courant, financement, change, SICAV) — each card links to its detail page
- [ ] **Build Agency Locator page**
  - Integrate Leaflet.js with 164 branch markers across 14 regional departments; filter by region
- [ ] **Build Contact page**
  - Form with name, email, subject, message — POST to FastAPI `/contact` endpoint
- [ ] **Build FAQ page shell**
  - Static accordion FAQ + chatbot widget embedded below
- [ ] **Integrate chatbot widget** into global layout
  - Floating button (bottom-right) toggling a chat drawer — visible on all pages
- [ ] **Accessibility & performance pass**
  - Run Lighthouse audit; fix contrast issues; add alt texts; lazy-load images

---

## Phase 3 — RAG Pipeline (Backend)

- [ ] **Create FAQ ingestion script**
  - Load FAQs from JSON/CSV → chunk → embed with `sentence-transformers` (`multilingual-e5-base` or similar)
- [ ] **Set up ChromaDB collection**
  - Persist embeddings to disk (`CHROMA_PERSIST_DIR`); collection name: `amen_faq`
- [ ] **Build `/ingest` endpoint or CLI command**
  - Rerunnable script that upserts updated FAQ documents into ChromaDB
- [ ] **Build retrieval function**
  - `query_collection(query: str, k=5)` → returns top-k relevant FAQ chunks with metadata
- [ ] **Integrate Groq LLM** for answer generation
  - Use `groq` Python SDK; model: `llama3-8b-8192` or `mixtral-8x7b`; system prompt enforces language + tone
- [ ] **Build RAG chain with LangChain**
  - `RetrievalQA` chain: user query → retriever → context injection → LLM → response
- [ ] **Handle multilingual routing**
  - Detect query language (`langdetect` or prompt-based) → set system prompt language accordingly

---

## Phase 4 — API Endpoints

- [ ] **`POST /chat`** — main chatbot endpoint
  - Body: `{message: str, session_id: str, lang?: str}` → Response: `{answer: str, sources: list}`
- [ ] **`GET /health`** — health check
  - Returns `200` + version/status — used by frontend to verify backend is alive
- [ ] **`POST /contact`** — contact form handler
  - Accepts form submissions — log to DB or send email notification
- [ ] **Add CORS configuration**
  - Allow Next.js origin (`localhost:3000` + prod domain) in FastAPI CORS middleware
- [ ] **Add rate limiting to `/chat`**
  - Max 20 req/min per IP using `slowapi` or custom middleware

---

## Phase 5 — Testing

- [ ] **Unit tests for retrieval + RAG chain**
  - `pytest`: assert top-k retrieval returns relevant docs; assert LLM response is non-empty
- [ ] **Integration test for `/chat` endpoint**
  - Send 10 sample questions in FR/AR/EN; assert response time < 3s each
- [ ] **Frontend E2E tests**
  - Use Playwright or Cypress: load homepage → open chatbot → send message → assert response appears
- [ ] **User validation round**
  - Get 3–5 real users to test the chatbot; score answer relevance 1–5; tune system prompt if avg < 4

---

## Phase 6 — Documentation & Handoff

- [ ] **Write technical README** (root level)
  - Covers: project structure, local setup steps, env vars, how to run ingest script
- [ ] **Write API documentation**
  - Document all endpoints with request/response schemas — use FastAPI `/docs` (Swagger) + a markdown summary
- [ ] **Write RAG architecture doc**
  - Diagram + explanation of: ingestion flow, embedding model, ChromaDB storage, retrieval, LLM generation
- [ ] **Prepare final presentation**
  - Slides: problem → solution → demo (live or recorded) → tech stack → results/metrics → next steps

---

## Tech Stack Reference

| Layer | Technology |
|---|---|
| Frontend | Next.js, TypeScript, Tailwind CSS |
| Backend | Python, FastAPI |
| RAG Framework | LangChain |
| Vector Store | ChromaDB |
| Embeddings | sentence-transformers (`multilingual-e5-base`) |
| LLM API | Groq (`llama3-8b-8192` / `mixtral-8x7b`) |
| Version Control | Git, GitHub |

---

*Generated from cahier de charge — Amen Bank Summer Internship 2025*