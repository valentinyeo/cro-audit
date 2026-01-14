# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

CRO Audit Tool - A Playwright-based tool for automating Conversion Rate Optimization audits. Captures screenshots of client websites and competitors, generates comparison reports with highlighted areas, and serves them via password-protected URLs.

**Status**: Project is in planning phase. See PLAN.md for implementation phases.

## Planned Commands

```bash
# Install dependencies
npm install
npx playwright install chromium

# Capture screenshots for an audit
npm run capture -- --audit={client-name}

# Generate HTML report with annotations
npm run report -- --audit={client-name}

# Serve reports with password protection
npm run serve -- --audit={client-name} --password={secret}
```

## Architecture

### Tech Stack
- **Screenshots**: Playwright
- **Image Processing**: Sharp (for annotations/highlights)
- **Report Output**: HTML/CSS with Tailwind
- **Server**: Express.js with basic HTTP auth

### Directory Structure
- `src/screenshot/` - Playwright capture logic, viewport configs, section selectors
- `src/highlight/` - Image annotation (rectangles, arrows, labels)
- `src/report/` - HTML report generator and templates
- `src/server/` - Express server with auth
- `audits/{client-name}/` - Per-client audit data:
  - `config.json` - URLs, competitors, pages to capture
  - `highlights.json` - Annotation definitions for findings
  - `screenshots/` - Raw captured images
  - `annotated/` - Images with highlights applied
  - `report/` - Generated HTML output

### Workflow
1. Configure audit in `audits/{client-name}/config.json`
2. Run screenshot capture for client + competitors
3. Define highlights in `highlights.json` with coordinates and labels
4. Generate HTML report with side-by-side comparisons
5. Serve via Express with password protection

## Key Specifications
- Screenshots at 2x DPI for retina clarity
- Desktop viewport: 1920x1080
- Mobile viewports: 375px, 414px widths
- Reports are self-contained HTML (relative image paths)
- Reports must be fully responsive/mobile-friendly
- Support embedding Loom videos in reports
- Reports should be downloadable as PDF
