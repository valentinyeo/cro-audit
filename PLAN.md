# CRO Audit Tool - Project Plan

## Original Requirements

> This is a CRO conversion script. CRO auditing script. Which basically means I have clients from Upwork that essentially asked me to do a CRO audit on their website. And the way I do it is I basically go to their website and take screenshots, and then I go to biggest competitor websites I research which the competitors are and which are adjacent websites. This is kinda like a deeper research task.
>
> And then I take screenshots of their home page, product page, and category page or whatever page is suitable in comparing to the original page. And then essentially, I highlight different things that the website should be doing based on sort of best practices that the other companies are doing. And the client then kind of gets a report with initiatives. So that he knows how what to fix on his website. And, I probably also make a loom video explaining the issues.
>
> And I would like to use Claude to essentially help me with the researching and arrangement of the results. And the snagging and grabbing of the screenshots I need, for example, usually the HD screenshots or full HD resolution of the full website. So this is a long website but also of different sections. And of the hero section. And then the same for mobile. And then the same for all the competitors. So I have them side by side.
>
> And ideally, this should then be resulting in a kind of like HTML report. And ideally, it would also be possible to highlight things on the screenshots. So for example, if I want to show the hero claim, then you would have, like, three or four screenshots next to each other. And each would have the hero claim highlighted so that the customer can see his arrangement versus the other person's arrangement or the other website's arrangement.
>
> So then there should be an explanation of the principle that I'm trying to teach them, and they would then get a URL on crolab.org, slash report name, and they could access that with a password probably. And, just review the findings.

---

## Project Overview

A Playwright-based tool for automating CRO (Conversion Rate Optimization) audits. The tool captures screenshots of client websites and competitors, generates comparison reports with highlighted areas, and serves them via password-protected URLs.

---

## Features

### 1. Screenshot Automation (Playwright)

- **Full-page screenshots** at HD/Full HD resolution (1920x1080)
- **Section-specific screenshots**: hero, navigation, product grid, footer, etc.
- **Mobile viewport screenshots** (375px, 414px widths)
- **Batch processing** for multiple competitor sites
- **Configurable wait times** for dynamic content loading

### 2. Image Highlighting

- Draw rectangles around specific areas
- Add labels/annotations
- Arrow pointers to call out specific elements
- Color-coded highlights (client vs competitors)

### 3. HTML Report Generator

- **Side-by-side comparisons** (3-4 screenshots per row)
- **Sections for each principle/finding**:
  - Screenshot comparison
  - Highlighted areas
  - Explanation of the principle
  - Actionable recommendations
- **Table of contents** with jump links
- **Professional styling** suitable for client delivery
- **Responsive design** for viewing on any device

### 4. Password-Protected Hosting

- Simple Express server with basic auth
- Static HTML output option for other hosting
- URL structure: `crolab.org/reports/{client-name}`

---

## Technical Stack

| Component | Technology |
|-----------|------------|
| Screenshots | Playwright |
| Runtime | Node.js |
| Image Processing | Sharp |
| Report Output | HTML/CSS (Tailwind) |
| Server | Express.js |
| Auth | Basic HTTP Auth / Simple password |

---

## Project Structure

```
cro-audit/
├── src/
│   ├── screenshot/
│   │   ├── capture.js          # Playwright screenshot logic
│   │   ├── viewports.js        # Viewport configurations
│   │   └── selectors.js        # Common section selectors
│   ├── highlight/
│   │   ├── annotate.js         # Add highlights to images
│   │   └── shapes.js           # Rectangle, arrow, label utilities
│   ├── report/
│   │   ├── generator.js        # HTML report builder
│   │   ├── templates/          # HTML templates
│   │   └── styles/             # CSS styles
│   └── server/
│       └── index.js            # Express server with auth
├── audits/
│   └── {client-name}/
│       ├── config.json         # Audit configuration
│       ├── screenshots/        # Raw screenshots
│       ├── annotated/          # Screenshots with highlights
│       └── report/             # Generated HTML report
├── config/
│   ├── viewports.json          # Screen size presets
│   └── defaults.json           # Default settings
├── package.json
├── PLAN.md
└── README.md
```

---

## Workflow

### Step 1: Configure Audit

Create `audits/{client-name}/config.json`:

```json
{
  "client": {
    "name": "Client Brand",
    "url": "https://clientsite.com",
    "pages": [
      { "name": "home", "path": "/" },
      { "name": "product", "path": "/products/example" },
      { "name": "category", "path": "/collections/main" }
    ]
  },
  "competitors": [
    {
      "name": "Competitor A",
      "url": "https://competitor-a.com",
      "pages": [
        { "name": "home", "path": "/" },
        { "name": "product", "path": "/product/similar" }
      ]
    },
    {
      "name": "Competitor B",
      "url": "https://competitor-b.com",
      "pages": [
        { "name": "home", "path": "/" }
      ]
    }
  ],
  "viewports": ["desktop", "mobile"],
  "sections": ["hero", "navigation", "full-page"]
}
```

### Step 2: Capture Screenshots

```bash
npm run capture -- --audit={client-name}
```

This will:
- Visit each URL
- Capture full-page and section screenshots
- Save to `audits/{client-name}/screenshots/`

### Step 3: Define Highlights

Create `audits/{client-name}/highlights.json`:

```json
{
  "findings": [
    {
      "id": "hero-claim",
      "title": "Hero Value Proposition",
      "principle": "The hero section should immediately communicate what you offer and why it matters.",
      "recommendation": "Add a clear, benefit-focused headline above the fold.",
      "screenshots": [
        {
          "file": "client-home-hero-desktop.png",
          "highlights": [
            { "type": "rectangle", "x": 100, "y": 200, "width": 400, "height": 80, "label": "Weak claim" }
          ]
        },
        {
          "file": "competitor-a-home-hero-desktop.png",
          "highlights": [
            { "type": "rectangle", "x": 100, "y": 180, "width": 500, "height": 100, "label": "Strong value prop" }
          ]
        }
      ]
    }
  ]
}
```

### Step 4: Generate Report

```bash
npm run report -- --audit={client-name}
```

This will:
- Process screenshots with highlights
- Generate HTML report
- Output to `audits/{client-name}/report/`

### Step 5: Serve Report

```bash
npm run serve -- --audit={client-name} --password={secret}
```

Access at: `http://localhost:3000/reports/{client-name}`

---

## Implementation Phases

### Phase 1: Core Screenshot Engine
- [ ] Set up Playwright with TypeScript/JavaScript
- [ ] Implement full-page screenshot capture
- [ ] Add viewport configurations (desktop/mobile)
- [ ] Add section-specific capture (hero, nav, etc.)
- [ ] Implement batch processing for multiple URLs

### Phase 2: Image Annotation
- [ ] Integrate Sharp for image processing
- [ ] Implement rectangle overlay
- [ ] Add text labels
- [ ] Add arrow/pointer annotations
- [ ] Color coding system

### Phase 3: Report Generation
- [ ] Create HTML template structure
- [ ] Implement side-by-side comparison layout
- [ ] Add findings sections with explanations
- [ ] Style with Tailwind CSS
- [ ] Generate table of contents

### Phase 4: Server & Auth
- [ ] Set up Express server
- [ ] Implement basic password protection
- [ ] Add static file serving
- [ ] Configure for production deployment

### Phase 5: CLI & Polish
- [ ] Create CLI commands for each step
- [ ] Add progress indicators
- [ ] Error handling and validation
- [ ] Documentation and examples

---

## VPS Deployment

### Prerequisites
- Node.js 18+
- npm or yarn

### Installation
```bash
git clone https://github.com/{username}/cro-audit.git
cd cro-audit
npm install
npx playwright install chromium
```

### Running
```bash
# Capture screenshots for an audit
npm run capture -- --audit=example-client

# Generate report
npm run report -- --audit=example-client

# Serve with password protection
npm run serve -- --port=3000
```

### Nginx Configuration (for crolab.org)
```nginx
server {
    listen 80;
    server_name crolab.org;

    location /reports/ {
        proxy_pass http://localhost:3000/;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
    }
}
```

---

## Future Enhancements

- **GUI for highlighting**: Web interface to draw highlights on screenshots
- **AI-powered analysis**: Use Claude to suggest improvements automatically
- **Competitor research**: Automated competitor discovery
- **PDF export**: Generate PDF reports for offline viewing
- **Loom integration**: Embed Loom videos in reports
- **A/B test tracking**: Track implementation results

---

## Notes

- Screenshots are captured at 2x DPI for retina clarity
- Mobile viewports simulate real device sizes
- Reports are self-contained HTML (images embedded or relative paths)
- Password protection uses bcrypt for hashing
