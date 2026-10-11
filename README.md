# jan-mitra-welfare-assistant
# Jan-Mitra (जन-मित्र)

### AI-Powered Multilingual Voice Bridge for Last-Mile Welfare Access

**VBYLD Hackathon | Hack for Social Cause | Anand, Gujarat**

Jan-Mitra is a voice-first, multilingual citizen-assistance platform
designed to make Indian government welfare schemes easier to discover
and understand.

Citizens can speak naturally in Gujarati, Hindi or English and receive
plain-language scheme information, preliminary eligibility guidance,
document checklists and links to official service channels.

The project focuses on digital inclusion, transparent public service
delivery and reducing avoidable barriers between citizens and
government entitlements.

> Status: Hackathon prototype. The static demo uses a deterministic
> scheme-matching engine. Production AI, verified live scheme retrieval,
> official portal integrations and offline service-worker support
> require additional implementation.

---

## 1. The Problem

Government welfare information can be difficult to navigate when a
citizen must understand formal eligibility rules, identify the right
department, interpret documentation requirements and use a digital
portal.

These barriers can disproportionately affect:
- Smallholder and marginal farmers
- Traditional artisans and craftspeople
- Elderly citizens
- Semi-literate and first-time digital users
- People who prefer local languages
- Citizens with limited connectivity or digital assistance

A citizen who does not know a scheme exists may never begin an
application. A citizen who misunderstands a requirement may make
repeated visits to an office or service centre.

Jan-Mitra addresses the discovery and comprehension stage. It does not
replace official eligibility verification or government decisions.

## 2. Proposed Solution

Jan-Mitra provides a simple voice-led workflow:

1. Select Gujarati, Hindi or English.
2. Tap the microphone or choose a demonstration profile.
3. Describe an occupation or welfare need in everyday language.
4. Match the query against a curated scheme catalogue.
5. Show a plain-language explanation, preliminary eligibility
   conditions and a document checklist.
6. Direct the citizen to the official scheme portal or a Gram
   Panchayat/CSC assistance point for verification and application.

### Design principles

- Voice first, not voice only
- Large, readable controls and accessible contrast
- Local-language explanations
- No promise of guaranteed eligibility
- Official sources for scheme facts
- Minimal personal data collection
- Human assistance for uncertain cases
- Graceful operation when speech recognition is unavailable

## 3. Features

### Prototype features

- Gujarati, Hindi and English interface
- Web Speech API microphone interaction
- Listening-state animation
- Farmer and artisan demonstration profiles
- Keyword-based scheme matching
- Dynamic scheme and document cards
- Browser text-to-speech where available
- Manual demonstration fallback when speech recognition fails
- Responsive layout for phones, tablets and desktops

### Planned production features

- Next.js application with reusable components
- Verified scheme catalogue with versioned records
- Retrieval-augmented generation (RAG)
- Gemini or another approved LLM behind a secure server API
- Source citations and scheme-update timestamps
- Progressive Web App manifest and service worker
- Offline caching of approved public information
- Consent-based analytics with privacy-preserving aggregation
- Human escalation and correction workflow
- Accessibility testing with target users

## 4. System Architecture

                         CITIZEN
                            |
                 Voice / Manual Selection
                            |
                  Responsive PWA Interface
                 (Gujarati / Hindi / English)
                            |
               Web Speech API / Text Input
                            |
                    Query Normalization
                            |
                  Scheme Retrieval Layer
                            |
              Curated, Versioned Scheme Store
                            |
                Eligibility Rules / RAG
                            |
               Evidence-Grounded Explanation
                            |
                Validation and Guardrails
                            |
             Scheme Cards and Document Checklist
                            |
              Official Portal / Assisted Service

Production AI requests must go through a server-side API. Never
place a Gemini API key or other secret in browser JavaScript.

The static prototype uses local demo data instead of a live RAG
service. Its scheme matches are illustrative, not official decisions.

## 5. Technology Stack

| Component | Technology | Purpose |
|---|---|---|
| Production frontend | Next.js, React, TypeScript | Accessible application and reusable UI |
| Styling | Tailwind CSS | Responsive layouts and design consistency |
| Speech input | Web Speech API | Browser-supported speech recognition |
| Speech output | SpeechSynthesis API | Read results aloud where supported |
| AI generation | Gemini through a server API | Plain-language explanations |
| Retrieval | RAG over a curated catalogue | Ground answers in approved records |
| Knowledge store | PostgreSQL or versioned JSON for MVP | Scheme facts, rules and citations |
| Search | PostgreSQL full-text search or a vector index | Retrieve relevant scheme records |
| PWA | Web App Manifest and Service Worker | Installability and offline shell |
| Testing | Playwright, Vitest, manual device testing | Functional and regression testing |
| Hosting | HTTPS-capable static or application hosting | Secure access and deployment |
| Monitoring | Privacy-preserving application metrics | Reliability and service improvement |

The standalone `index.html` demo does not require this production
stack to be installed. It uses CDN assets for the demonstration UI.

## 6. Repository Contents

- `index.html`: Standalone interactive prototype
- `test-cases.json`: Example test scenarios and expected outputs
- `LICENSE`: MIT License
- `README.md`: Setup, architecture and implementation notes

## 7. Run Locally

### Option A: Open the HTML file

1. Save `index.html` to your computer.
2. Open it in a current browser.
3. Select a language.
4. Click Farmer or Artisan to run the demo.
5. Try the microphone on a supported browser.

Microphone access may not work from a `file://` URL.

### Option B: Use a local HTTP server

If Python is installed:

    python -m http.server 8000

Open:

    http://localhost:8000

Stop the server with Ctrl+C.

The prototype requires an internet connection for CDN-hosted Tailwind
and Font Awesome assets. Browser speech recognition may also rely on
browser-managed online services.

### Production Next.js setup

A future production implementation can be initialized using:

    npx create-next-app@latest jan-mitra --typescript --tailwind --app
    cd jan-mitra
    npm run dev

The static prototype and the production Next.js application are
different deployment targets. The static HTML does not itself contain
the Next.js runtime.

## 8. Environment Variables

The standalone prototype requires no secrets.

A production server may use a `.env.local` file:

    GEMINI_API_KEY=replace_with_secret
    SCHEME_DATA_VERSION=2026-10-01
    DATABASE_URL=postgresql://user:password@localhost:5432/janmitra

These are example variable names, not configured credentials.
Never commit `.env.local` to Git.

Keep provider API keys exclusively on the server. Configure production
secrets through the hosting provider's secret manager. Use a restricted
database account and encrypted connections.

## 9. RAG and Hallucination Prevention

The production assistant must not rely on an LLM's memory for scheme
rules.

1. Maintain approved records with scheme name, administering
   department, eligibility rules, exclusions, documents, official URL,
   language versions and last-verified date.
2. Retrieve relevant records for the citizen's query.
3. Apply deterministic eligibility checks wherever possible.
4. Ask a follow-up question when a required fact is missing.
5. Generate explanations only from retrieved evidence.
6. Attach the official source and date to each answer.
7. Return "unable to verify" when evidence is missing or contradictory.
8. Route uncertain or sensitive cases to an authorized human adviser.
9. Record catalogue versions and review corrections before release.

RAG reduces unsupported answers but does not eliminate hallucinations.
Eligibility output remains preliminary until confirmed by the relevant
government authority.

## 10. Privacy, Security and Citizen Trust

- Collect the minimum information needed for scheme discovery.
- Do not request Aadhaar numbers, bank account numbers, OTPs or
  passwords in the voice demo.
- Do not store raw audio or transcripts by default.
- Explain whether voice processing is handled by the browser or a
  remote service.
- Obtain meaningful consent before optional analytics or storage.
- Encrypt production traffic using HTTPS.
- Restrict access to operational logs and redact sensitive fields.
- Provide a clear correction and human-escalation route.
- Never imply that Jan-Mitra is an official government portal unless
  formally authorized.
- Publish privacy, retention and data-deletion policies before launch.

## 11. Example Test Cases

The JSON file includes illustrative farmer and artisan scenarios.
PASS indicates that the expected demo path is defined and matched
by the prototype logic. It does not indicate government verification.

| Test | Scenario | Expected result |
|---|---|---|
| JM-001 | Gujarati-speaking farmer | PM-KISAN information card |
| JM-002 | Hindi-speaking artisan | PM Vishwakarma information card |
| JM-003 | English-language selection | English UI labels |
| JM-004 | Microphone unavailable | Demo buttons remain usable |
| JM-005 | Unknown query | No unsupported eligibility promise |
| JM-006 | Missing eligibility details | Preliminary result and verification notice |

Run the demo manually using the Farmer and Artisan buttons. A future
automated test suite should assert scheme IDs, document lists,
language rendering, microphone error handling and source links.

## 12. Governance Metrics

Measure service quality and equitable access, not merely total clicks.

- Scheme discovery completion rate
- Correct scheme retrieval rate on a reviewed test set
- Unsupported-claim rate
- Percentage of answers with valid source citations
- Checklist comprehension in moderated user tests
- Successful assisted referrals
- Median response time under low-bandwidth conditions
- Voice interaction success rate by language and device
- Accessibility issues resolved
- Citizen-reported usefulness and trust
- Number of scheme records reviewed on schedule

Disaggregate metrics only when lawful, necessary and privacy-safe.
Avoid collecting sensitive demographic data solely to produce
dashboards. Do not claim reduced entitlement leakage without a
credible baseline and follow-up measurement.

## 13. UN Sustainable Development Goal Alignment

### SDG 1: No Poverty
Makes information about income support and livelihood schemes easier
to discover.

### SDG 10: Reduced Inequalities
Reduces language, literacy and digital-interface barriers.

### SDG 16: Peace, Justice and Strong Institutions
Supports more understandable public information, transparent
eligibility explanations and accountable assisted access.

Jan-Mitra is an enabling tool. Its impact depends on accurate
information, accessible service channels and government processes
that can act on citizens' needs.

## 14. Implementation Roadmap

### Phase 1: Demonstration
- Single-page multilingual prototype
- Farmer and artisan scenarios
- Local matching logic
- Usability review with representative users

### Phase 2: Validated pilot
- Verify a small scheme catalogue with official sources
- Add a server-side retrieval API
- Test Gujarati and Hindi speech quality
- Partner with a Gram Panchayat or CSC for supervised testing

### Phase 3: Production readiness
- Next.js application, database and access controls
- PWA manifest and service worker
- Monitoring, incident response and accessibility audit
- Formal content ownership and update workflow

### Phase 4: Responsible scale-up
- Add schemes only after verification
- Publish open interfaces and reusable documentation
- Evaluate outcomes with participating local institutions
- Consider integration with approved public digital infrastructure

## 15. Limitations

- Browser speech recognition varies by browser, operating system,
  device, accent and network conditions.
- The static demo uses illustrative records and keyword matching.
- The static demo does not connect to government beneficiary databases.
- It does not submit applications or verify identity.
- A PWA requires additional manifest and service-worker files for
  full installability and offline caching.
- Scheme eligibility, exclusions and required documents can change.
- Production deployment requires legal, security, accessibility and
  user testing.

## 16. Official Reference Points

PM-KISAN:
https://pmkisan.gov.in/

PM-KISAN official FAQs:
https://pmkisan.gov.in/Documents/FAQPMKISAN.PDF

PM Vishwakarma:
https://pmvishwakarma.gov.in/

UN Sustainable Development Goals:
https://sdgs.un.org/goals

Always verify current scheme rules and document requirements against
the administering authority before presenting an answer as current.

## 17. License

Released under the MIT License. See `LICENSE`.

---

**Jan-Mitra: Making welfare information easier to hear, understand,
and act on.**
