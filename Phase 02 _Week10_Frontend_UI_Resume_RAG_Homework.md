# Front-End UI Homework: Khalifa Talent Search

Enhancement of the Resume RAG application: smarter retrieval results, Khalifa branding, a professional and responsive
UI, clear messages for every state, and end-to-end testing.

---

## 1. Assignment checklist

| # | Requirement | Status | Where to see it |
|---|---|---|---|
| 1 | Re-ranking of retrieved resumes | ✅ Done | Hybrid mode: **"LLM rank · #1 · 95% match"** and **"Why this match: …"** on each card |
| 2 | Deduplication of repeated results | ✅ Done | Every mode: each person appears once, with a **"1 duplicate resume merged"** badge |
| 3 | Summarization of the final results | ✅ Done | Hybrid mode: **"AI SHORTLIST SUMMARY"** panel plus a fit summary on each card |
| 4 | Complete end-to-end pipeline integration | ✅ Done | One search runs: keyword + semantic search → merge → dedup → AI re-rank → summarize → display |
| 5 | Display all relevant results clearly | ✅ Done | Result cards, profile window, CSV export |
| 6 | Organization name "Khalifa" + logo | ✅ Done | Sidebar, header, upload page, browser tab |
| 7 | Maroon and white brand theme | ✅ Done | Whole app; palette defined once in `brand.css` |
| 8 | Professional UI layout | ✅ Done | Sidebar + conversation layout, cards, profile window |
| 9 | Responsive design | ✅ Done | Phone and tablet: slide-out menu, full-width results |
| 10 | Loading, success, validation and error messages | ✅ Done | Search progress, toasts, inline validation, error cards with Retry |
| 11 | Organization-specific enhancements | ✅ Done | Export shortlist to CSV; copy email/phone |
| 12 | End-to-end testing | ✅ Done | See `Khalifa_Talent_Search_Test_Report.md`: all suites pass |

---

## 2. Retrieval features as shown in the UI

### Re-ranking
- Keyword (BM25) and meaning-based (vector) results are merged, then an AI model (Groq) re-ranks them for the query.
- Each AI-ranked card shows its **% match** and a **one-line reason**, so recruiters see *why* a candidate ranks where they do.
- If the AI can't rank in time, results still appear in search order. Those cards are labelled **"Search rank"** instead of
  "LLM rank", with a plain-language notice explaining why.

### Deduplication
- The same resume found by both searches appears **once**, with its sources shown ("Matched by BM25 + Vector").
- **The same person uploaded more than once** (re-uploads, "(1)" copies) is merged into one card. A match requires the same
  email or phone **and** a matching name, so two different people sharing an HR inbox are never merged.

### Summarization
- One batched AI call returns a short, evidence-based **fit summary** for each top candidate and an **overview of the whole
  shortlist**. For example: *"GEANT CLEMENT RADJE is the strongest match, possessing Cypress and CI/CD skills… Common
  gaps include missing Cypress expertise…"*
- Summaries are plain text and grounded in the resume. They state gaps honestly ("no evidence of payroll experience").

### Result display
Each card shows: rank, name, role, company, years of experience, email and phone (with copy buttons), skills, % match,
reason, summary and match sources. Clicking a card opens the **profile window** with contact details, skills, experience
timeline and education.

---

## 3. Khalifa branding

| Element | Implementation |
|---|---|
| **Name** | "Khalifa Talent Search"; the assistant is "Khalifa Talent Assistant"; footer: "© 2026 Khalifa · Talent Acquisition" |
| **Logo** | Original mark: a white **arch** (a gateway, the entrance to the organisation) framing a geometric **"K"** on a maroon tile, with the wordmark "Khalifa / TALENT SEARCH". Also used as the browser-tab icon. |
| **Colours** | Maroon scale (primary `#7A1F2B`, deep `#3D0E15` for the sidebar) with white and warm neutrals |
| **Status colours** | Kept meaningful: green for online/success, amber for warnings, red for errors; blue and terracotta tags for Vector and BM25 modes |

Branding is centralised, so it can be changed in one place:
- Colours: `recruitbot-web/src/styles/brand.css` (all ~280 former theme colours now refer to these tokens)
- Names: `recruitbot-web/src/config/brand.ts`
- Logo: `recruitbot-web/src/components/brand/KhalifaLogo.tsx` and `recruitbot-web/public/favicon.svg`

### Brand name proposals
Alternative product names with original logos are in `docs/brand-proposals.html` (open it in a browser):

| Option | Meaning | Logo idea |
|---|---|---|
| ⭐ **Khalifa Mizan** (ميزان), recommended | "balance, scale" | Balance scale inside the arch: fair, evidence-based hiring |
| Khalifa Basira (بصيرة) | "insight, discernment" | Eye-shaped lens with a spark of insight |
| Khalifa Nukhba (نخبة) | "the select few" | One highlighted candidate in a grid |
| Khalifa Daleel (دليل) | "guide" and "evidence" | Compass needle inside the arch |

A trademark search and a native-speaker check of the transliteration are recommended before adopting a new name.

---

## 4. Professional and responsive layout

| Screen | Layout |
|---|---|
| **Desktop** | Fixed maroon sidebar (search mode, results limit, clear chat, upload link) and a conversation area with result cards |
| **Tablet and phone (below 900 px)** | The sidebar becomes a **slide-out menu** (☰ button). It closes with ✕, Escape, a tap outside, or by choosing a mode. Results use the full screen and the search box stays at the bottom. |

Tested at 390 px (phone) and 820 px (tablet): no horizontal scrolling on any page.

Accessibility:
- Keyboard focus moves into the menu and the profile window, and back out, correctly.
- The profile window traps focus and closes with Escape.
- Each candidate's name is a real button.
- Messages are announced to screen readers.
- Animations respect "reduce motion" settings.

---

## 5. Messages for every state

| State | What the user sees |
|---|---|
| **Loading** | A search progress panel. Hybrid: "Searching resumes ✓ → Re-ranking candidates with AI → Summarizing the shortlist", with a timer. Vector and BM25 show their own steps. The upload page shows upload and processing progress. |
| **Success** | Khalifa-styled toasts: "Conversation cleared", "Shortlist exported: 5 candidates", "Email copied: …", "Resume added to the Khalifa talent pool and ready to search" |
| **Validation** | Inline under the search box: empty search, fewer than 2 characters, and a live 400/500-character counter that warns when pasted text is shortened. Upload page: "Only PDF allowed", "Maximum 5MB allowed". |
| **Errors** | An error card with a clear cause and a **Retry search** button: "Can't reach the search service", "The search took too long", "Search is temporarily unavailable", "Please check your search" |
| **Partial results** | Plain-language notices, e.g. "AI ranking is busy right now, so results are in keyword and meaning order. Try again in a minute for AI ranking." |
| **Empty results** | "No candidates found", with a suggestion to try a different role or skill |

---

## 6. Organization-specific enhancements

- **Export shortlist to CSV.** One click downloads the current results (rank, name, role, company, experience, contact,
  skills, AI match %, reason, summary, sources, search details). The file opens correctly in Excel, including names with
  special characters. Cells that could be read as spreadsheet formulas are neutralised for safety.
- **Copy email / phone.** Copy buttons on result cards and in the profile window, with a confirmation toast.

---

## 7. Data quality improvements

Clean data makes the UI trustworthy, so the 122 stored resumes were cleaned. Each change was previewed, approved, and
backed up first.

| Field | Before | After |
|---|---|---|
| Wrong names ("SUMMARY", "Phone", unreadable text) | 41 | 1 |
| Resumes with no skills | 58 | 16 |
| Education shown as "me"/"be" | 92 | 0 |
| Real experience summaries | 7 | 80 |
| Real employer names | 2 | 73 |

---

## 8. Testing summary

Full details are in `Khalifa_Talent_Search_Test_Report.md`.

| Suite | Result |
|---|---|
| Backend unit and integration tests | 77 / 77 passed |
| API validation (bad and good requests) | 24 / 24 passed |
| Live pipeline (real database and AI services) | 2 / 2 passed; full AI-ranked search in 4.3 s |
| Browser, desktop journey | 23 / 23 passed, no console errors |
| Browser, phone and tablet | 4 / 4 passed |
| Browser, upload flow | 5 / 5 passed, database unchanged |

The browser tests are in `tests/e2e-browser/` and can be re-run (see the README there).

---

## 9. Known limitations

1. **AI rate limit:** the current Groq tier allows about one fully AI-ranked hybrid search per minute. Further searches
   within that minute still return results, in search order, with a notice explaining why.
2. **AI re-ranking and summaries run in Hybrid mode.** Vector and BM25 are fast single searches, and still include
   deduplication and the full display.
3. **Search progress steps are time-based**, because the search is a single request.
4. **Resume parsing is rule-based**, so some resumes still have an empty role, summary or company where the text gives no
   reliable signal.

---

## 10. How to run

```powershell
# Backend (project root)
npm install
npm run dev            # http://localhost:3000

# Frontend
cd recruitbot-web
npm install
npm run dev            # http://localhost:5173
```

Open http://localhost:5173. Use the sidebar to choose **Vector**, **BM25** or **Hybrid** search; **Resume intake** opens
the upload page.
