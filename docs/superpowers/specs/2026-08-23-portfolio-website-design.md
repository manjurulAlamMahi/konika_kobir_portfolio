# Kamrun Nahar Konika — Personal Portfolio Website

Date: 2026-08-23
Status: Approved

## Purpose

A single-page, premium, feminine-elegant personal portfolio/landing site for Kamrun Nahar Konika (B.Sc CSE student, University of Asia Pacific), built from her CV only. Plain HTML/CSS/vanilla JS, no frameworks, no libraries.

## Content Source of Truth

All copy comes exclusively from `Resume Of KAMRUN NAHAR KONIKA.pdf`. Nothing invented. Missing data (Facebook/Instagram profile URLs) is represented with clearly marked placeholder links, not fabricated URLs.

## Sections (in scroll order)

1. **Hero**
   - Large centered circular photo frame — empty/placeholder container, easy to swap (`<img src="assets/profile.jpg" alt="Kamrun Nahar Konika" />` inside a styled frame, with a graceful empty-state style — no external placeholder image or icon service).
   - Name: "Kamrun Nahar Konika"
   - Title: "B.Sc in Computer Science and Engineering"
   - Typing animation cycling through CV-derived strings: "CSE Student", "Python Developer", "Django Developer", "Web Developer", "Full-Stack Enthusiast" (all traceable to CV skills/projects — tech stack across her 3 projects is Python/Django/HTML/CSS/JS).
   - Short intro: adapted from the "About Me" CV paragraph.
   - Scroll-down indicator (animated).

2. **About** — full About Me paragraph in a soft glass/card panel.

3. **Skills** — Technical Skills (C, C++, Java, Python, JavaScript, HTML, CSS, Microsoft Office, Basic Database Knowledge) and Soft Skills (Communication & Presentation, Teamwork, Event Coordination, Customer Handling, Time Management) as icon-labeled pill cards, two sub-groups.

4. **Experience** — timeline cards:
   - Sales Executive, Basundora Aarong (March 2024, 1 Month) — description + "Learned" line.
   - Social Media Page Moderator, Facebook Page "Make Glow" (Mar 2023 – Aug 2023) — description + "Learned" line.

5. **Education** — timeline cards:
   - B.Sc in CSE, University of Asia Pacific (2023–Present), CGPA 3.85, 4th Year 1st Semester, Expected Graduation 2027.
   - HSC, Dhaka City College (2019–2021), Science, GPA 5.00, Dhaka Board.

6. **Projects** — 3 cards: Shonchoy (Digital Banking Platform), MindScope (Mental Health Screening Platform), Scrappy (Waste Management System) — each with description + tech-stack tags exactly as listed in CV.

7. **Achievements & Activities** — Dean's Award Recipient (2025), Dean's Award Recipient (2026), Best Performer Award in University Cultural Program, plus Clubs & Participation bullets (Cultural Club member, event coordination, presentations/group projects).

8. **Languages** — Bangla (Native), English (Professional), Hindi (Basic) with proficiency indicator per CV ("Writing, Speaking & Listening" / "Speaking & Listening").

9. **Contact** — phone (+8801735490177), email (konikakobir121@gmail.com), location (Mogbazar, Dhaka), GitHub (konikakobir28), LinkedIn (konika-kobir-b3a982363).

No other sections are added.

## Global Chrome

- **Floating vertical nav** — right side, glassmorphism (translucent blur, rounded pill container), one dot/icon+label per section, active-section highlight via IntersectionObserver, smooth-scroll on click. On mobile: collapses to a bottom horizontal glass bar (icons only, active state), no hamburger needed since the section count is small.
- **Floating social icons** — separate vertical stack, right edge:
  - GitHub → `https://github.com/konikakobir28`
  - LinkedIn → `https://linkedin.com/in/konika-kobir-b3a982363`
  - Facebook → `#` placeholder, HTML comment flagging it as "replace with real profile URL"
  - Instagram → `#` placeholder, same treatment
  - On mobile: repositioned to avoid collision with the bottom nav bar (e.g. tucked as a smaller top-right cluster or folded into the bottom bar overflow — implementer's call, must not overlap nav).
- **Floating WhatsApp button** — bottom-left, links to `https://wa.me/8801735490177`, hover scale/pulse animation.

## Visual System

- **Palette — Rose Gold & Blush**: blush pink + rose-gold gradient accents, deep plum/charcoal text, cream/ivory background surfaces, soft translucent card backgrounds.
- **Typography**: elegant display pairing — a serif or refined display face for headings (e.g. Playfair Display) + a clean modern sans for body/UI (e.g. Poppins or Inter). Loaded via system-safe fallback stacks (no external CDN dependency required, but Google Fonts `@import`/`<link>` is acceptable if used).
- **Background**: several large blurred blob/gradient shapes in blush/rose-gold tones, slow CSS keyframe float, low opacity — decorative only, behind content (`z-index` layering respected), reduced/disabled under `prefers-reduced-motion`.
- **Cards/sections**: soft shadows, generous rounded corners, glassmorphism where it reads elegant (nav, hero frame, floating buttons) — not overused on every panel.

## Interactions & Animations

- Typing effect in hero (vanilla JS, cycles the role strings, pause/delete/type loop).
- Fade/slide-in on scroll via IntersectionObserver for section headers and cards (one-time trigger, no re-animation jank).
- Smooth scroll for all in-page nav links.
- Hover states: nav items, social icons, WhatsApp button, project/skill cards (subtle lift/scale, no heavy transforms).
- All animations respect `prefers-reduced-motion: reduce`.

## Responsiveness

- Mobile-first breakpoints; content reflows to single column on small screens.
- Floating right nav → bottom glass bar on mobile; floating social stack repositioned to not collide.
- Hero photo frame and typography scale down proportionally; no horizontal overflow anywhere.
- Tap targets ≥ 44px on interactive elements.

## File Structure

```
index.html
style.css
script.js
assets/            (placeholder folder; profile.jpg added later by user)
```

Semantic HTML (`<header>`, `<nav>`, `<main>`, `<section>`, `<footer>`), clean class naming, minimal necessary comments (why, not what) — matches project conventions since this is a fresh project with no prior code to follow.

## Out of Scope

- No backend, no contact form submission handling (contact section is informational links only, per CV data — no form since no form-handling endpoint exists).
- No CMS/build tooling — static files only.
- No image asset is created or sourced for the profile photo.
