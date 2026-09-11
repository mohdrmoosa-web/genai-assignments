# Khalifa AI Code Reviewer

Khalifa AI Code Reviewer is a responsive GitHub source-code review interface with a Canada-inspired visual identity and n8n webhook integration.

## Current Features

- Khalifa AI branding and original SVG logo
- Canada-inspired red and white visual theme with maple leaf motif
- Responsive glassmorphism application shell
- Sticky header and responsive navigation
- Footer with version and organization information
- GitHub source-file URL validation
- Review focus selection:
  - Correctness
  - Security
  - Performance
  - Maintainability
  - Testability
  - Code Quality
- Loading, success, timeout, network-error, and retry states
- POST integration with the configured n8n webhook
- Safe Markdown report rendering without raw HTML injection
- Responsive tables and horizontally scrollable code blocks
- Copy, download, print, and review-another-file actions
- Reduced-motion support and visible keyboard focus states

## Project Structure

```text
.
├── index.html
├── README.md
├── assets/
│   ├── khalifa-ai-logo.svg
│   └── maple-leaf.svg
├── config/
│   └── config.js
├── css/
│   └── styles.css
└── js/
    ├── app.js
    └── markdown.js
```

## Run Locally

From the project directory, start a static server:

```powershell
python -m http.server 4173
```

Open:

```text
http://localhost:4173
```

The browser must be able to reach the configured n8n instance when a review is submitted.

## Webhook Configuration

The frontend reads the webhook URL from `config/config.js`:

```js
window.KHALIFA_CONFIG = Object.freeze({
  N8N_WEBHOOK_URL: 'http://localhost:5678/webhook/github-code-reviewer'
});
```

The browser sends:

```json
{
  "githubUrl": "https://github.com/owner/repository/blob/main/path/to/file.java",
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

The preferred n8n success response is:

```json
{
  "success": true,
  "review": "# GitHub Code Review\n\n## Review Summary\n...",
  "githubUrl": "https://github.com/owner/repository/blob/main/path/to/file.java"
}
```

The frontend also handles plain-text responses and common nested response fields. Backend errors should use:

```json
{
  "success": false,
  "error": "Unable to retrieve the GitHub source code."
}
```

## Security Notes

- Do not put GitHub tokens, Groq API keys, or n8n credentials in frontend files.
- Configure private repository authentication inside n8n.
- Use HTTPS for production deployments.
- Configure n8n CORS to allow the hosted frontend origin.
- Keep Markdown rendering limited to supported text constructs. The renderer does not use `innerHTML`.
- Restrict GitHub source retrieval to approved GitHub domains in the n8n workflow.

## Validation

The following checks pass:

```powershell
node --check .\js\app.js
node --check .\js\markdown.js
node --check .\config\config.js
```

Editor diagnostics report no errors for the HTML, CSS, JavaScript, SVG, and configuration files.

## Scope Note

The UI is implemented through the six requested phases. The actual n8n workflow export is not included because no workflow JSON was supplied or exported from n8n in this workspace. The webhook URL is configured and ready for the active local n8n workflow.
