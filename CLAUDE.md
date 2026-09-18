# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a college portfolio website for Aadhavan Sivakumar built with vanilla HTML, CSS, and JavaScript. It's a static single-page site showcasing academic achievements, athletics, volunteer work, internships, awards, projects, and skills for college applications.

## Technology Stack

- Vanilla HTML5, CSS3, JavaScript (ES6+) — no build tools, no `package.json`, no dependencies
- Fonts: Inter (Google Fonts), Font Awesome icons (both loaded via CDN `<link>` tags in `index.html`)
- Deployment: static hosting (GitHub Pages, Netlify, or any traditional host)

## Development

There is no build/lint/test process. To work on the site, open `index.html` directly in a browser, or serve it locally:

```
python -m http.server 8000
```

Since everything is one HTML file, one CSS file, and one JS file, verify changes by reloading the page in a browser — there is no compiler to catch mistakes.

## File Structure

```
/
├── index.html                              # All page content, section by section
├── styles.css                              # All styling (~1080 lines)
├── script.js                               # All behavior (~300 lines)
├── images/
│   ├── academics/ athletics/ volunteering/ internship/   # gallery photos, referenced by filename in index.html
│   └── profile.jpeg                        # hero/profile photo
└── documents/
    └── aadhavan-sivakumar-resume.pdf       # linked from the resume download button
```

## Architecture

### `index.html` — sections
The page is one long document broken into `<section id="...">` blocks that the nav bar links to and the scroll-spy highlights: `about`, `academics`, `athletics`, `volunteering`, `awards`, `internships`, `projects`, `skills`, `contact`. Each gallery-style section (academics/athletics/volunteering/internships) follows the same pattern: a grid of `.gallery-item` elements wrapping an `<img>`, wired up in `script.js` to open in the lightbox modal.

### `script.js` — behavior
Single `DOMContentLoaded` handler wiring up independent features:
- Mobile hamburger nav + smooth-scroll nav links (offset for the fixed header)
- Scroll-based navbar styling and active-section highlighting
- Image modal/lightbox for gallery photos (click to expand)
- `IntersectionObserver` fade-in animation applied to gallery items and cards as they scroll into view
- Typing animation for the hero `<h1>`
- Contact form handler (client-side only — no backend; submission just shows a confirmation state)
- `window.portfolioFunctions` — a small console-driven API for editing content without touching HTML: `addAcademicPhoto/addAthleticPhoto/addVolunteerPhoto(src, caption)` append a gallery item; `updateProfile({name, tagline, bio, email})` and `updateStats({gpa, volunteerHours, leadership})` rewrite text via `querySelector`. These select elements by class/order (e.g. first three `.stat-number` nodes), so reordering the stats markup in `index.html` will break them.

### `styles.css`
No CSS custom properties/`:root` variables exist — colors are hardcoded hex literals repeated throughout the file (`#3498db` primary blue, `#9b59b6` purple accent, `#2c3e50` text, `#f8f9fa` light background). To change the color scheme, do a project-wide find/replace of these hex values rather than editing a single variable block.

## Content Editing

Since there's no CMS, all content lives directly in `index.html`:
- Contact info (email/phone/location): `.contact-item` spans inside `<section id="contact">`
- Hero tagline/bio: `.tagline` and `.bio` paragraphs near the top of `<section id="about">`
- Stats (GPA, volunteer hours, etc.): `.stat-number`/`.stat-label` pairs inside `.stats`

When adding photos, drop the image in the matching `images/<category>/` subfolder and add a matching `.gallery-item` block in the relevant section of `index.html` (don't rely on `portfolioFunctions.addXPhoto`, which only mutates the live DOM and doesn't persist to the file).
