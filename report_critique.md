# Report Critique: Majles – Secure Discord Clone

> **Overall Assessment**: The report is technically ambitious and shows a strong grasp of modern cryptography and secure system design. However, it frequently states *what* was done without adequately explaining *why* specific choices were made over alternatives. Several sections feel incomplete (mockups are all placeholders), and the narrative consistency between chapters needs strengthening.

---

## Abstract

### ✅ What works
- Covers all core technologies concisely.
- Both English and French abstracts are present and well-balanced.
- Keywords are appropriate and complete.

### ⚠️ Issues & Missing Justifications
- **Argon2 is mentioned** in the abstract but **never explained in any chapter**. Where is the Argon2 implementation section? There is no Sprint that discusses password hashing, which creates a broken promise between the abstract and the body.
- The abstract mentions "scalable collaboration ecosystem" but scalability is never discussed or tested anywhere in the report.

---

## General Introduction

### ✅ What works
- Strong framing of the privacy problem. The Single Point of Failure (SPoF) argument is well-articulated.
- The chapter roadmap at the end is clear and useful.

### ⚠️ Issues & Missing Justifications
- The introduction has **unnumbered sections** (`\section*{}`), which means they do not appear in the Table of Contents. This is an intentional academic choice but should be consistent throughout, and a reader navigating the PDF cannot jump to sub-sections.
- The motivation jumps immediately to technical objectives without a transition. A brief sentence connecting the problem to the proposed solution would improve the flow.

---

## Chapter 1 – General Project Framework and State of the Art

### ✅ What works
- Good market data with specific figures (USD 60B market, 16.5% CAGR, 30% enterprise privacy demand). These give academic weight.
- The critique of Discord's data policy is specific and factual.
- The comparison with Matrix and Signal is honest and identifies real adoption barriers.

### ⚠️ Issues & Missing Justifications

**Critical gap – why *this* stack?**  
The chapter proposes the solution but does not justify the technology choices. After identifying that Matrix/Signal are complex and Discord is insecure, you should ask: *"Why did we not build on top of Matrix? Why did we choose Spring Boot, WebRTC, and Next.js over alternatives?"* This analysis is missing entirely.

**Missing: Threat Model**  
Chapter 1 identifies *who* the adversary is (centralized platforms) but never produces a formal threat model. A proper academic report would include:
- A table of attack scenarios (e.g., database breach, rogue admin, government subpoena, MITM).
- Per-scenario analysis of how your architecture mitigates each threat.

**The Gantt Diagram caption is in French** (`Diagramme de Gantt du projet.`) while the rest of the report is in English — inconsistency.

**"60 Story Points total"** is mentioned but the backlog in Chapter 2 adds up to approximately 56 points. This discrepancy is never resolved.

---

## Chapter 2 – Requirements Specification, Analysis, and Architecture

### ✅ What works
- The actor identification is clean: two actors (User/Client and Neutral Server) align well with the Zero-Knowledge architecture narrative.
- The functional and non-functional requirements are properly separated and labeled (FR/NFR).
- The global product backlog table is comprehensive and covers all 5 sprints.

### ⚠️ Issues & Missing Justifications

**Technology justification is absent.**  
The Work Environment section simply *lists* the technologies with brief descriptions. It reads like a Wikipedia entry. A good academic report requires comparative justification:

| Question Missing | Example of What Should Be Said |
|---|---|
| Why PostgreSQL and not MySQL or MongoDB? | "We chose PostgreSQL for its support of JSONB columns, which allow us to store opaque ciphertext blobs alongside typed metadata." |
| Why Spring Boot and not Node.js/Express? | "Spring Boot's built-in Spring Security and WebSocket support provides enterprise-grade JWT infrastructure out of the box, reducing custom security code surface." |
| Why Next.js and not React/Vue? | "Next.js's server-side rendering and API routes allow us to co-locate our client-side encryption logic with rendering, ensuring no plaintext escapes the browser context." |

**The MVC Architecture justification is generic.**  
The text says "the MVC architecture proves highly effective, hence our choice." This is circular reasoning — *it is effective, therefore we chose it* — which is not a justification. You should explain: MVC specifically enables separation of the encryption layer (Model) from routing (Controller) and UI (View), making security auditing scoped and clear.

**The Physical Architecture section** describes the diagram well but does not explain the NAT traversal strategy (STUN/TURN servers). This is a critical element of the P2P architecture that is only briefly mentioned in the conclusion.

**The `Discord` entry in the complementary technologies section** is bizarre and out of place. Discord is the platform you are *replacing*, not a development tool you used. This section should be removed or reframed as "Reference Platform" with a sentence explaining you used it as a UX/feature benchmark.

**Missing: Security Architecture Diagram.** There is no dedicated diagram showing the Zero-Knowledge data flow — i.e., where encryption happens, what the server receives vs. what the client holds. The physical architecture diagram shows deployment but not the security boundary.

---

## Chapter 3 – Release 1 (Cryptographic Foundations and Access Control)

### ✅ What works
- Sprint backlog tables are detailed with acceptance criteria. This is a good engineering practice.
- The Sprint 1 Retrospective contains a specific, technical insight (WebCrypto key generation takes <20ms; localStorage cache wiping is a real problem).
- The Sprint 2 Retrospective identifies a scalability concern (TreeKEM group key exchange) which shows forward-thinking.

### ⚠️ Issues & Missing Justifications

**Critical: All mockups are "(Placeholder for Sprint X Mockup)".**  
This is the single biggest problem in the report. Sprints 1 and 2 show **no actual UI screenshots or mockups**. If the application was implemented and demoed, there should be real screenshots here (like the ones you have for Sprint 3 and 4). Submitting a report with placeholders signals an incomplete deliverable.

**Sprint 1 Review is thin.**  
The review says a "Registration Interface" was demonstrated and a "console capture" was shown to prove localStorage storage. But no screenshot of either is included. Compare this to Chapter 4 (Sprints 3 & 4) which includes actual test screenshots — that standard of evidence is expected in Sprint 1 too.

**The Sprint 2 Backlog table** has the column header "Privacy Priority" instead of just "Priority" (a leftover from an earlier edit). This is inconsistent with all other sprint tables.

**JWT is named but not explained.**  
The class diagram references JWT but there is no prose explanation of how the JWT token lifecycle works: when it is issued, what claims it contains, how it is used to authorize WebSocket connections, and how it is invalidated on logout. These are critical security details missing from the report.

**The Argon2 promise from the abstract is broken here.**  
Sprint 1 covers authentication, yet `Argon2` (mentioned as the password hashing algorithm in the abstract) is never discussed. Was it actually implemented? Is it configured with appropriate parameters (time cost, memory cost, parallelism)? This is a security-critical omission.

---

## Chapter 4 – Release 2 (End-to-End Encrypted Communication and P2P)

### ✅ What works
- Sprint 3 is the strongest sprint in the whole report. Real test screenshots validate the implementation.
- The Sprint 3 Retrospective's insight about client-side search being necessary is excellent — it shows you understood a real architectural consequence of Zero-Knowledge design.
- The Sprint 4 Retrospective's honest acknowledgment of Symmetric NAT limitations is academically strong.
- The Security Class Diagram (DTLS/SRTP components) adds real depth to the P2P section.

### ⚠️ Issues & Missing Justifications

**Diffie-Hellman key exchange is under-explained.**  
The report mentions DH key exchange as a bullet point but never explains the protocol in detail. Academic questions a reviewer will ask:
- Is it ECDH (Elliptic Curve) or classic DH? What curve (P-256, X25519)?
- How is the DH public key distributed? Is it stored on the server? Is it signed to prevent impersonation/MITM?
- How does a new member of a channel get the shared key of an existing conversation?

**Sprint 5 has no sprint-level review or retrospective in narrative.**  
The Extended Backlog (Sprint 5) includes six user stories but the section skips directly to a general "Designing the Dynamic View" section without any review of what was *actually implemented and tested* in Sprint 5. Compare the detail of Sprint 3's review versus Sprint 5 — there is no evidence Sprint 5 was completed.

**The Sprint 3 Mockup section** says: *"Graphical design, otherwise referred to as visual design, is an important part of visual product creation..."* This boilerplate introduction is repeated verbatim at the start of every Mockup section (Sprints 1, 2, 3, 4, 5). It adds no value and should be removed or replaced with specific UI design decisions made for that sprint.

**Missing in Sprint 4: STUN/TURN server setup.**  
The report claims P2P bypasses central servers, but in practice, many users are behind NAT and require STUN/TURN relays to establish a peer connection. The report briefly mentions this issue in the retrospective but never explains how it was resolved in the implementation. Was a TURN server deployed? What is its privacy guarantee?

---

## General Conclusion

### ✅ What works
- The "Cryptographic vs. UX Reflection" section is genuinely insightful — the three bullet points (handshake processing, Zero-Knowledge search, NAT traversal) show honest self-assessment.
- The future perspectives are concrete and technically grounded (TreeKEM → SFU, third-party audit, contributor documentation).

### ⚠️ Issues & Missing Justifications
- The conclusion says *"the platform is fully completed"* — but the report contains placeholder mockups for Sprints 1, 2, and 3, and Sprint 5 has an incomplete review. This statement overclaims.
- There are no quantitative results. A conclusion should answer: How many messages/sec can the system handle? What is the measured latency of the DH key exchange? What is the WebRTC connection setup time? Even informal benchmarks from the sprint reviews would strengthen this enormously.
- The security validation is only qualitative ("we proved the server stores ciphertexts"). An actual penetration test result, or at minimum a structured checklist against a threat model, is missing.

---

## Cross-Cutting Issues

| Issue | Where | Severity |
|---|---|---|
| Missing Argon2 implementation section | Sprint 1, Abstract | 🔴 High |
| All mockups are placeholders (Sprints 1, 2, 3) | Ch.3, Ch.4 | 🔴 High |
| Technology choices not justified | Ch.2, Ch.1 | 🟠 Medium |
| No formal threat model or attack surface analysis | Ch.1, Ch.2 | 🟠 Medium |
| Boilerplate "Graphical design is an important part..." repeated 5 times | Ch.3, Ch.4 | 🟡 Low |
| Gantt chart caption in French in an English report | Ch.1 | 🟡 Low |
| "Discord" listed as a complementary technology | Ch.2 | 🟡 Low |
| Sprint 2 table header says "Privacy Priority" instead of "Priority" | Ch.3 | 🟡 Low |
| DH key exchange details never specified (curve, distribution, signing) | Ch.4 | 🟠 Medium |
| No quantitative performance benchmarks in conclusion | Conclusion | 🟠 Medium |

---

## Priority Action List

If you have limited time, focus on these in order:

1. **Add Sprint 1 and Sprint 2 actual screenshots** (registration UI, server creation UI, role assignment UI). Remove all `(Placeholder for Sprint X Mockup)` text.
2. **Explain *why* each technology was chosen** with one comparative sentence per technology in Chapter 2.
3. **Add an Argon2 explanation** inside Sprint 1 (or add a sentence clarifying it is handled by Spring Security's PasswordEncoder — even one paragraph is sufficient).
4. **Add a Zero-Knowledge data flow diagram** showing what is encrypted on the client, what travels over the network, and what is stored in the database.
5. **Clean the boilerplate mockup introductions** and the French Gantt diagram caption.
