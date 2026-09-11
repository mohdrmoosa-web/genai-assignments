# Khalifa AI GitHub Code Reviewer — Custom UI Master Roadmap

## 1. Purpose

Build a production-ready **GitHub Code Reviewer Agent with Custom UI** for the organization **Khalifa**.

The user will enter a GitHub URL pointing to a source-code file or repository path. The Custom UI will send the request to an **n8n Webhook**, n8n will retrieve the GitHub source, send it to a **Groq-powered LLM**, and return a structured code-review report to the UI.

### Target user flow

```text
Khalifa GitHub Code Reviewer UI
        |
        | POST JSON
        v
n8n Webhook
        |
        v
HTTP Request - GitHub source
        |
        v
Basic LLM Chain
        |
        +---- Groq Chat Model
        |
        v
End Webhook Response
        |
        v
Khalifa UI renders Code Review Report
```

## 2. Important Implementation Rule

This roadmap is designed for **Custom UI + n8n Webhook integration**.

The earlier n8n sample used an n8n Form Trigger and a GET request. That is useful as a learning reference, but the Custom UI implementation should use:

- **Webhook trigger**
- **HTTP Request**
- **Basic LLM Chain**
- **Groq Chat Model**
- **Respond to Webhook / End Webhook**

The browser should call the **published production webhook URL**, not a relative `/webhook/...` path unless the UI is hosted behind the same n8n domain.

Do not hardcode credentials, GitHub tokens, Groq API keys, or private repository secrets in the frontend.

---

# PHASE 1 — Create Khalifa AI Application & Visual Design System

## Phase Goal

Create the foundation of the Khalifa AI GitHub Code Reviewer application with a modern Gen-Z-friendly professional visual identity.

Use a premium **dark digital-tech theme** with:

- Deep midnight/navy background
- Electric cyan
- Violet/purple
- Mint/green success accents
- White/high-contrast text
- Glassmorphism cards
- Subtle grid/mesh background
- Soft ambient glow
- Clean rounded components
- Responsive design

The design should look professional enough for enterprise use while still feeling modern and attractive to Gen-Z users.

## Branding

Organization name:

**Khalifa**

Product name:

**Khalifa AI Code Reviewer**

Suggested tagline:

**Review code. Find risks. Ship better.**

Create an original AI-style logo using CSS/SVG or an original generated visual concept. Do not copy another company's logo.

Suggested logo concept:

- Stylized K combined with an AI circuit/scan motif
- Small glowing nodes representing AI analysis
- Compact icon suitable for favicon/mobile header
- Wordmark: `Khalifa AI`

## Copy-Paste Prompt for VS Code + Copilot

```text
Build the foundation for a professional AI GitHub Code Reviewer web application called "Khalifa AI Code Reviewer".

The application must have a premium futuristic technology aesthetic suitable for an enterprise organization while also being attractive to Gen-Z users.

Brand:
- Organization: Khalifa
- Product: Khalifa AI Code Reviewer
- Tagline: Review code. Find risks. Ship better.

Create an original AI-inspired Khalifa logo using inline SVG or CSS shapes. Do not copy an existing company logo.

Use a dark midnight/navy base with electric cyan and violet accents, plus mint/green for success states. Use high-contrast white text.

Create:
1. Full-page responsive application shell.
2. Subtle technical grid or mesh background.
3. Soft ambient glowing background elements.
4. Glassmorphism cards with restrained blur and borders.
5. Consistent CSS variables/design tokens for colors, spacing, radius, shadows and typography.
6. Responsive layout for desktop, tablet and mobile.
7. Accessible focus states and readable contrast.
8. Smooth but restrained animations.
9. Professional scrollbar styling.
10. No unnecessary external JavaScript libraries.

Keep the implementation clean and maintainable. Separate structure, styling and JavaScript where appropriate.

Do not implement the n8n API call yet. This phase is only the visual foundation.
```

## User-Facing Verification

- [ ] Application opens without console errors.
- [ ] Khalifa branding is visible.
- [ ] Logo is original and visually consistent.
- [ ] Dark technology theme is attractive but readable.
- [ ] Cards have consistent glass/tech styling.
- [ ] Desktop layout looks professional.
- [ ] Mobile layout does not overflow horizontally.
- [ ] Keyboard focus is visible.

---

# PHASE 2 — Build Professional Header, Navigation & Footer

## Phase Goal

Create a professional sticky header and footer that establish the Khalifa organization identity.

## Header Requirements

Left:

- Khalifa AI logo
- `Khalifa AI Code Reviewer`

Center/right navigation:

- Dashboard
- Code Review
- Review History
- API / Help

Right:

- `n8n Connected` status indicator
- Optional `AI Ready` indicator

The connection indicator must not falsely claim that n8n is connected. It should initially use a neutral state such as `Ready to Review` unless a real health check is implemented.

## Footer Requirements

Include:

- `© 2026 Khalifa`
- `AI-powered code review`
- Privacy / Security text if required
- Version indicator such as `v1.0`

## Copy-Paste Prompt for VS Code + Copilot

```text
Extend the Khalifa AI Code Reviewer UI by creating a professional sticky glassmorphism header and a clean responsive footer.

Header:
- Show the original Khalifa AI logo on the left.
- Display "Khalifa AI Code Reviewer".
- Show the tagline or a compact product descriptor.
- Add navigation items: Dashboard, Code Review, Review History, API / Help.
- Add a compact status area on the right.
- Do not falsely claim that n8n is connected unless the application actually verifies the connection.
- Use a neutral "Ready to Review" state initially.
- Add responsive behavior so navigation collapses cleanly on smaller screens.

Footer:
- Show "© 2026 Khalifa".
- Show "AI-powered code review".
- Include a compact application version.
- Keep the footer professional and unobtrusive.

Add accessible hover, focus and active states.

Do not implement backend/API communication in this phase.
```

## User-Facing Verification

- [ ] Header remains visible while scrolling.
- [ ] Khalifa logo and product name are clear.
- [ ] Navigation does not overlap on tablet/mobile.
- [ ] Footer is visible and responsive.
- [ ] Status badge does not falsely report backend connectivity.
- [ ] Keyboard navigation works.

---

# PHASE 3 — Build GitHub Input Dashboard & Review Controls

## Phase Goal

Create the main user workspace where the user enters a GitHub URL.

## Main Input

Label:

**GitHub Code URL**

Placeholder:

**https://github.com/owner/repository/blob/main/path/to/file.java**

Support examples such as:

```text
https://github.com/owner/repository/blob/main/src/LoginService.java
```

Optionally support repository URLs later, but the first release should prioritize a single source-code file URL because it produces a deterministic review input.

## Controls

Create:

- GitHub URL input
- `Review Code` primary button
- `Clear` secondary button
- Optional review focus selectors:
  - Correctness
  - Security
  - Performance
  - Maintainability
  - Testability
  - Code Quality

Default all review areas to selected.

## Information Cards

Add three compact cards:

- `Code Analysis`
- `AI Review`
- `Actionable Findings`

These should explain what the agent does rather than displaying fake statistics.

## Copy-Paste Prompt for VS Code + Copilot

```text
Create the main dashboard for the Khalifa AI Code Reviewer.

Build a prominent input card titled "Analyze GitHub Code".

Add:
- Label: GitHub Code URL
- Placeholder: https://github.com/owner/repository/blob/main/path/to/file.java
- Primary button: Review Code
- Secondary button: Clear

The input must accept a normal GitHub file URL.

Add optional review focus checkboxes:
- Correctness
- Security
- Performance
- Maintainability
- Testability
- Code Quality

All review areas should be selected by default.

Add a short explanatory area showing:
- Code Analysis
- AI Review
- Actionable Findings

Do not display fake review statistics.

Add client-side validation:
- Empty URL must be rejected.
- Only valid GitHub HTTP/HTTPS URLs should be accepted.
- Clearly distinguish invalid input from server/API errors.

The UI must be responsive and accessible.

Do not call n8n yet. Build the input and presentation layer first.
```

## User-Facing Verification

- [ ] Empty submission shows validation.
- [ ] Invalid non-GitHub URL is rejected.
- [ ] Valid GitHub URL is accepted.
- [ ] Review button is visually prominent.
- [ ] Clear button resets the form.
- [ ] Controls work on mobile.

---

# PHASE 4 — Build AI Review Workspace & Loading Experience

## Phase Goal

Create the area where the AI review is displayed.

The workspace must have these states:

1. Waiting
2. Validating
3. Fetching code
4. AI reviewing
5. Success
6. Error

## Waiting State

Display:

**Ready to review your code**

Example supporting text:

**Paste a GitHub source-code URL and let Khalifa AI analyze it for quality, security, maintainability and potential defects.**

## Loading State

Use an elegant AI scanning animation.

Example rotating messages:

- Validating GitHub URL...
- Fetching source code...
- Inspecting code structure...
- Checking correctness...
- Checking security risks...
- Evaluating maintainability...
- Generating recommendations...
- Preparing your review...

Do not make the UI appear stuck.

## Error State

Create a clear error card with:

- Error icon
- Short explanation
- Technical detail when appropriate
- `Try Again` button

Examples:

- Invalid GitHub URL
- GitHub file could not be retrieved
- n8n webhook unavailable
- AI review failed
- Empty response received

Never expose API keys or secrets in an error message.

## Copy-Paste Prompt for VS Code + Copilot

```text
Build the Khalifa AI Code Reviewer Review Workspace.

Create clear UI states:
- idle
- validating
- loading
- success
- error

Idle state:
Show "Ready to review your code" with a short explanation.

Loading state:
Create a polished AI scanning animation and rotate through these messages:
- Validating GitHub URL...
- Fetching source code...
- Inspecting code structure...
- Checking correctness...
- Checking security risks...
- Evaluating maintainability...
- Generating recommendations...
- Preparing your review...

While loading:
- Disable the input and Review Code button.
- Change the button text to "Reviewing..."
- Prevent duplicate submissions.
- Show a visible progress indicator.

Error state:
Show a professional error card with:
- icon
- short human-readable message
- optional technical detail
- Try Again action

Never expose secrets, credentials or API keys.

Keep animations smooth and lightweight.
```

## User-Facing Verification

- [ ] Idle state is attractive.
- [ ] Loading animation works.
- [ ] Status messages rotate.
- [ ] Form is disabled during processing.
- [ ] Error state is readable.
- [ ] Try Again resets the correct state.
- [ ] No duplicate request can be triggered.

---

# PHASE 5 — Connect Custom UI to n8n Production Webhook

## Phase Goal

Connect the frontend to the production n8n workflow.

## n8n Workflow

Recommended workflow:

```text
Webhook
   |
   v
HTTP Request
   |
   v
Basic LLM Chain
   |
   +---- Groq Chat Model
   |
   v
Respond to Webhook
```

### Webhook

Method:

**POST**

Suggested path:

```text
github-code-reviewer
```

The final production URL should be copied from the active n8n webhook.

Example payload:

```json
{
  "githubUrl": "https://github.com/owner/repository/blob/main/src/LoginService.java",
  "reviewFocus": [
    "Correctness",
    "Security",
    "Performance",
    "Maintainability",
    "Testability",
    "Code Quality"
  ]
}
```

Do not hardcode the example URL as the production endpoint.

## HTTP Request Node

The HTTP Request node should retrieve the actual GitHub source represented by the submitted URL.

Important implementation consideration:

A normal GitHub `blob` webpage is HTML, not raw source code.

Therefore the workflow should convert the submitted GitHub URL to an appropriate raw-content URL before retrieving source code.

For public GitHub files, the raw form is generally:

```text
https://raw.githubusercontent.com/{owner}/{repo}/{branch}/{path}
```

The workflow should validate the URL and safely construct the raw URL.

For private repositories, use a secure GitHub credential/token mechanism in n8n. Never put a GitHub token in the browser payload.

## Basic LLM Chain

Use a strong Groq-supported model available in the user's n8n/Groq account.

The model must receive:

- Source code
- GitHub URL
- Review focus

The LLM should be instructed not to invent code or repository facts.

## Suggested Code Review Prompt

Use the following as the starting prompt in the Basic LLM Chain:

```text
You are Khalifa AI Code Reviewer, an expert software engineering code-review assistant.

Your task is to review ONLY the source code supplied in this request.

Do not claim that you executed the code, compiled it, scanned the repository, accessed GitHub independently, or verified runtime behavior unless that information is explicitly supplied by the workflow.

Do not invent missing files, dependencies, requirements, business rules, vulnerabilities, test results, or runtime behavior.

Review the supplied source code using these areas:
1. Correctness
2. Security
3. Performance
4. Maintainability
5. Testability
6. Code Quality

For every important finding provide:
- Finding
- Severity: Critical / High / Medium / Low / Info
- Why it matters
- Evidence from the supplied code
- Recommended improvement

When suggesting corrected code, keep it concise and directly related to the supplied code.

Return the final response in Markdown using exactly this high-level structure:

# GitHub Code Review

## Review Summary
Provide a concise overall assessment.

## Key Findings
Use a Markdown table with:
| ID | Severity | Category | Finding | Recommendation |

## Detailed Findings
Explain the important findings with evidence and recommendations.

## Security Review
List security observations. If no issue is identifiable from the supplied code, explicitly say so rather than inventing one.

## Code Quality & Maintainability
Discuss readability, structure, duplication, naming, error handling and maintainability where applicable.

## Performance Review
Identify realistic performance concerns supported by the supplied code.

## Testability
Identify missing or recommended tests based only on the supplied code.

## Suggested Improvements
Provide prioritized improvements.

## Overall Assessment
Give a concise final assessment.

Important:
- Do not invent line numbers unless line numbers are actually supplied.
- Do not report speculative vulnerabilities as confirmed vulnerabilities.
- Clearly label assumptions and limitations.
- If the source code is empty, unavailable or not readable, say that the review could not be completed.
```

## n8n Response Contract

The Respond to Webhook node should return a JSON response that is easy for the frontend to consume.

Preferred response:

```json
{
  "success": true,
  "review": "# GitHub Code Review\n\n## Review Summary\n...",
  "githubUrl": "https://github.com/owner/repository/blob/main/src/LoginService.java"
}
```

Error response:

```json
{
  "success": false,
  "error": "Unable to retrieve the GitHub source code."
}
```

## Copy-Paste Prompt for VS Code + Copilot

```text
Now connect the Khalifa AI Code Reviewer frontend to an n8n production webhook.

Create one clearly named configuration value for the n8n production webhook URL instead of scattering the URL throughout the code.

The frontend must send POST JSON:

{
  "githubUrl": "<submitted GitHub URL>",
  "reviewFocus": ["Correctness", "Security", "Performance", "Maintainability", "Testability", "Code Quality"]
}

On submit:
1. Validate the URL.
2. Disable duplicate submissions.
3. Show the loading state.
4. POST the JSON payload to the configured n8n webhook.
5. Wait for the response.
6. Accept the preferred response shape:
   {
     "success": true,
     "review": "...markdown...",
     "githubUrl": "..."
   }
7. Also handle a plain-text response or nested response gracefully.
8. If success is false, display the returned error.
9. If the request times out or the server is unreachable, display a clear connection error.
10. Always restore the UI to a usable state after success or failure.

Never place Groq API keys or GitHub private tokens in frontend code.

Add safe handling for CORS/network failures and explain in the UI when the problem is connectivity rather than code-review content.

Do not use fake/mock results in production mode.
```

## n8n Configuration Checklist

### Webhook Node

- [ ] Method = POST
- [ ] Production webhook path configured
- [ ] Production workflow is active
- [ ] Response mode configured correctly
- [ ] Request body is accepted as JSON

### HTTP Request Node

- [ ] Correct GitHub URL is extracted
- [ ] GitHub blob URL is converted to raw source URL
- [ ] GET used for source retrieval
- [ ] Authentication is configured securely when private repositories are supported
- [ ] HTTP errors are handled

### Basic LLM Chain

- [ ] Prompt is configured
- [ ] GitHub source is passed into the prompt
- [ ] Review focus is passed into the prompt
- [ ] Groq model is connected

### Respond to Webhook

- [ ] JSON success response is returned
- [ ] Review Markdown is included
- [ ] Errors return a predictable JSON shape
- [ ] No credentials are returned

---

# PHASE 6 — Render AI Results, Quality UX & Production Readiness

## Phase Goal

Render the AI response as a professional code-review report and complete the production-quality experience.

## Results Layout

Display:

### Review Header

- GitHub URL
- Review completion status
- Timestamp
- Overall assessment

### Summary

Show a concise AI-generated summary.

### Findings

Render the Markdown findings table as a professional responsive table.

Suggested severity presentation:

- Critical
- High
- Medium
- Low
- Info

### Detailed Findings

Render:

- Headings
- Paragraphs
- Bullet lists
- Numbered lists
- Code blocks
- Tables
- Bold text
- Inline code

Use safe Markdown rendering.

Do not insert raw untrusted HTML directly into the DOM.

If a Markdown library is used, configure sanitization. If no library is used, implement a deliberately limited Markdown renderer rather than unsafe HTML injection.

## Copy-Paste Prompt for VS Code + Copilot

```text
Complete the Khalifa AI Code Reviewer by rendering the AI response as a polished professional report.

The backend review is Markdown.

Render safely:
- H1/H2/H3 headings
- paragraphs
- bold text
- bullet lists
- numbered lists
- inline code
- fenced code blocks
- Markdown tables
- links where appropriate

Do not inject untrusted raw HTML directly into the DOM.

Create a responsive report layout with:
- Review Complete badge
- GitHub URL
- Overall assessment
- Review Summary
- Key Findings
- Detailed Findings
- Security Review
- Code Quality & Maintainability
- Performance Review
- Testability
- Suggested Improvements
- Overall Assessment

Add:
- Copy Review button
- Copy Code button where applicable
- Download/Print Review option if practical
- Review Another File button
- Error recovery
- Smooth result fade-in

For tables, make them horizontally scrollable on small screens.

For long code blocks, provide horizontal scrolling.

Do not modify the AI content silently. Preserve the meaning of the returned review.

Make the entire application production-ready:
- responsive
- accessible
- no console errors
- no hardcoded secrets
- no fake backend status
- no duplicate submissions
- clear validation
- clear network errors
- clean source structure
```

## User-Facing Verification

- [ ] Successful review displays the complete report.
- [ ] Markdown headings render correctly.
- [ ] Findings table is readable.
- [ ] Code blocks scroll correctly.
- [ ] Mobile table does not break the page.
- [ ] Copy action works.
- [ ] Review Another File works.
- [ ] Errors can be recovered.
- [ ] No secrets appear in browser source or Network payload.
- [ ] Browser console is clean.

---

# 3. End-to-End Test Strategy

## Test 1 — Valid Public GitHub File

Input:

```text
https://github.com/testleaftrainings/Sel_May_2026/blob/main/Week5/Day2/LearnDragAndDrop.java
```

Expected:

```text
UI
 -> POST n8n
 -> retrieve source
 -> Groq review
 -> JSON response
 -> formatted report
```

Expected UI result:

- Review completes successfully.
- Source URL is shown.
- AI review is rendered.
- Findings are visible.
- User can start another review.

## Test 2 — Empty URL

Action:

Click `Review Code` without entering a URL.

Expected:

- No API request is sent.
- Validation message is displayed.
- Input is highlighted.
- User can correct the value.

## Test 3 — Invalid URL

Input:

```text
https://google.com
```

Expected:

- Client-side validation prevents the request.
- User receives a clear GitHub URL validation message.

## Test 4 — GitHub File Not Found

Use a deliberately invalid GitHub file URL.

Expected:

- n8n returns a controlled error.
- UI displays an understandable error.
- No application crash.

## Test 5 — n8n Unavailable

Temporarily use an unavailable endpoint or stop the workflow.

Expected:

- UI shows a connection error.
- UI does not freeze.
- User can retry.

## Test 6 — Slow AI Response

Expected:

- Loading state remains visible.
- Duplicate requests are prevented.
- Final response appears when available.

## Test 7 — Empty AI Response

Expected:

- UI detects missing review content.
- User sees an error instead of an empty report.

## Test 8 — Mobile

Verify at:

- Desktop
- Tablet
- Mobile portrait
- Mobile landscape

Expected:

- No horizontal page overflow.
- Input remains usable.
- Results remain readable.
- Tables/code use controlled horizontal scrolling.

---

# 4. Security Checklist

Before production submission:

- [ ] No Groq API key in frontend.
- [ ] No GitHub private token in frontend.
- [ ] No n8n credentials in frontend.
- [ ] HTTPS used for production webhook.
- [ ] Validate GitHub URL.
- [ ] Restrict source retrieval to expected GitHub domains.
- [ ] Avoid server-side URL fetching of arbitrary internal/private addresses.
- [ ] Handle private repositories through secure n8n credentials only.
- [ ] Do not expose raw backend errors containing secrets.
- [ ] Sanitize rendered Markdown.
- [ ] Avoid unsafe `innerHTML` for untrusted content.
- [ ] Configure appropriate CORS policy.
- [ ] Do not commit secrets to GitHub.

---

# 5. Suggested Project Structure

```text
khalifa-ai-code-reviewer/
│
├── index.html
├── README.md
├── LICENSE
│
├── assets/
│   └── khalifa-ai-logo.svg
│
├── css/
│   └── styles.css
│
├── js/
│   ├── app.js
│   ├── api.js
│   ├── validation.js
│   ├── markdown.js
│   └── ui.js
│
├── config/
│   └── config.example.js
│
├── n8n/
│   ├── khalifa-github-code-reviewer-workflow.json
│   └── n8n-setup.md
│
├── docs/
│   ├── Phase-wise-Implementation.md
│   ├── UI-Development-Prompt.md
│   ├── Testing-Checklist.md
│   └── Submission-Checklist.md
│
└── screenshots/
    ├── n8n-workflow.png
    ├── ui-home.png
    └── ui-review-result.png
```

For the real deployment, keep the production webhook URL in an environment/configuration mechanism appropriate to the hosting platform. Do not commit secrets.

---

# 6. Submission Package

The final submission should contain:

## A. Phase-wise Implementation File

```text
docs/Phase-wise-Implementation.md
```

This document.

## B. Complete UI Development Prompt

```text
docs/UI-Development-Prompt.md
```

Include the six copy-paste prompts from this roadmap.

## C. n8n Workflow

```text
n8n/khalifa-github-code-reviewer-workflow.json
```

Export the actual working workflow from n8n.

## D. Complete UI Source Code

Include:

```text
index.html
css/
js/
assets/
config/
```

## E. n8n Workflow Screenshot

Capture the complete visible workflow:

```text
Webhook
  -> HTTP Request
  -> Basic LLM Chain
       -> Groq Chat Model
  -> Respond to Webhook
```

## F. UI Screenshots

Capture at minimum:

1. Khalifa landing/dashboard
2. GitHub URL entered
3. Loading/review state
4. Successful code-review result
5. Error/validation state
6. Mobile responsive view

## G. GitHub Repository

The repository should contain the complete source and documentation.

Before sharing the link:

- [ ] Repository opens successfully.
- [ ] README contains setup instructions.
- [ ] No credentials are committed.
- [ ] Project structure is clean.
- [ ] Screenshots are included.
- [ ] n8n workflow export is included.
- [ ] Phase-wise document is included.

## H. Demo Video

Recommended 2–4 minute flow:

```text
1. Introduce Khalifa AI Code Reviewer
2. Show Custom UI
3. Paste GitHub code URL
4. Click Review Code
5. Show loading experience
6. Show n8n workflow briefly
7. Show AI review results
8. Demonstrate error validation
9. Demonstrate Review Another File
10. End with repository/documentation
```

---

# 7. Final Definition of Done

The application is ready for submission only when all of the following are true:

### Frontend

- [ ] Khalifa branding implemented.
- [ ] Original AI logo implemented.
- [ ] Professional Gen-Z-friendly design.
- [ ] Desktop responsive.
- [ ] Tablet responsive.
- [ ] Mobile responsive.
- [ ] Header implemented.
- [ ] Footer implemented.
- [ ] GitHub URL validation implemented.
- [ ] Loading state implemented.
- [ ] Success state implemented.
- [ ] Error state implemented.
- [ ] Review results rendered safely.

### n8n

- [ ] Production Webhook active.
- [ ] POST request accepted.
- [ ] GitHub source retrieval works.
- [ ] GitHub URL handling works.
- [ ] Basic LLM Chain works.
- [ ] Groq Chat Model connected.
- [ ] Review prompt configured.
- [ ] Respond to Webhook configured.
- [ ] Error responses are predictable.

### Integration

- [ ] UI sends the correct JSON payload.
- [ ] n8n receives the payload.
- [ ] GitHub source is retrieved.
- [ ] LLM receives the source.
- [ ] LLM generates the review.
- [ ] n8n returns the response.
- [ ] UI displays the review.
- [ ] Failed requests are handled gracefully.

### Quality

- [ ] No browser console errors.
- [ ] No hardcoded credentials.
- [ ] No fake status indicators.
- [ ] No duplicate requests.
- [ ] No unsafe HTML rendering.
- [ ] Mobile tested.
- [ ] End-to-end test completed.

---

# 8. Recommended First Release Scope

For Release 1, keep the solution focused:

**Input**
- One public GitHub source-code URL

**Review**
- Correctness
- Security
- Performance
- Maintainability
- Testability
- Code Quality

**Output**
- Summary
- Severity-based findings
- Evidence
- Recommendations
- Suggested improvements
- Overall assessment

After Release 1 works reliably, Release 2 can add:

- Repository-level review
- Multiple files
- Pull Request review
- GitHub authentication
- Review history
- Downloadable PDF/HTML report
- Jira integration
- SonarQube integration
- Static-analysis integration
- Team dashboards
- Quality trend analytics

---

# 9. Important Build Sequence

Do not ask Copilot to build everything in one step.

Use this sequence:

```text
Phase 1
   ↓
Verify visual foundation
   ↓
Phase 2
   ↓
Verify header/footer
   ↓
Phase 3
   ↓
Verify GitHub input
   ↓
Phase 4
   ↓
Verify loading/result/error states
   ↓
Phase 5
   ↓
Connect and test n8n
   ↓
Phase 6
   ↓
Complete end-to-end testing
   ↓
Prepare submission package
```

This makes troubleshooting much easier because each phase has a clear acceptance checkpoint.

---

# 10. Production Webhook Configuration

Before connecting the UI, activate the n8n workflow and copy the **Production URL** from the Webhook node.

Store that value in the application's configuration.

Example:

```text
N8N_WEBHOOK_URL=<your-production-webhook-url>
```

Do not put a real credential or secret into this documentation.

The final browser request should conceptually be:

```http
POST <N8N_PRODUCTION_WEBHOOK_URL>
Content-Type: application/json
```

Body:

```json
{
  "githubUrl": "<github-file-url>",
  "reviewFocus": [
    "Correctness",
    "Security",
    "Performance",
    "Maintainability",
    "Testability",
    "Code Quality"
  ]
}
```

---

# 11. Success Criteria

The project should be demonstrated using this exact business flow:

```text
User opens Khalifa AI Code Reviewer
        ↓
Enters GitHub source-code URL
        ↓
Clicks Review Code
        ↓
UI validates URL
        ↓
UI sends POST to n8n production webhook
        ↓
n8n retrieves source code
        ↓
Groq AI analyzes code
        ↓
n8n returns structured review
        ↓
UI renders professional report
        ↓
User reviews findings
        ↓
User can review another file
```

**Final acceptance statement:**

> The Khalifa AI Code Reviewer is considered operational only after a real GitHub source-code URL has successfully travelled through the Custom UI → n8n Webhook → GitHub source retrieval → Groq LLM → n8n response → Custom UI result-rendering path, with successful, validation, network-error and invalid-source scenarios tested.
