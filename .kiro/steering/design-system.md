# Professional Design & Engineering Standards (So Life Dev Log)

This document combines advanced UI/UX principles and frontend engineering best practices to guide Kiro in building and maintaining this Astro blog.

## 1. Core Visual Identity & Design System
- **Vibe**: Clean, Professional, Minimalist, Developer-centric.
- **Theme**: Dark Mode only.
- **Color Palette (HSL/HEX)**:
  - Background: `#1a1a1a` (Primary) / `#242424` (Secondary/Header/Footer/Sidebar).
  - Cards: `#2a2a2a`.
  - Accent: `#00d4d4` (Cyan) for links, active states, and primary actions.
  - Border: `#3a3a3a`.
  - Text: `#e5e5e5` (Primary) / `#a0a0a0` (Secondary).
- **Spacing**: Follow a strict **8px grid scale**. Whitespace is a first-class citizen—give content room to breathe.
- **Borders & Corners**: Consistent **12px border-radius** for cards and main containers. 1px solid borders using `--border-color`.

## 2. Typography Standards
- **Primary Font**: 'Pretendard', -apple-system, BlinkMacSystemFont, sans-serif.
- **Readability**: 
  - Body: `18px`, line-height `1.7`.
  - Prose (Article) Max-width: **720px** for optimal reading experience.
- **Headings**: Semantic use of H1-H6. 
  - H1: Bold, `2.5rem`.
  - H2: Semi-bold, `2rem`.

## 3. Advanced UI/UX Patterns
- **Glassmorphism**: Use backdrop filters and translucent layers sparingly for modern accents (e.g., sticky header).
- **Bento Grid**: Use for dashboard-like layouts or post grids where appropriate.
- **Micro-animations**: Interaction feedback (hover, click, focus) must be subtle (200-300ms transitions).
  - Hover on Cards: `translateY(-4px)` with enhanced shadow.
- **Consistency**: Always use CSS variables. No hardcoded colors or spacing.

## 4. Frontend Engineering Principles
- **Semantic HTML**: Mandatory use of `<nav>`, `<aside>`, `<main>`, `<article>`, `<footer>`.
- **Component Strategy**: One component per file. Use PascalCase for components and kebab-case for filenames.
- **Performance**: Target FCP < 1s. Avoid heavy custom JS; prefer Astro's zero-JS-by-default approach.
- **Imports**: Use absolute paths (`@/components/...`) if configured, otherwise stay consistent.

## 5. Layout & Composition (Priority Fix)
- **Container**: Max-width `1100px`, centered.
- **Composition**:
  - Desktop: Sidebar (`240px`) + Main Content (`flex: 1`). Gap: `2.5em`.
  - Sidebar should be `position: sticky` (top: 80px).
  - Mobile (< 720px): Stack vertically. Sidebar content should be re-ordered or simplified.
- **Search UI**: Minimalist input. Dropdown results must have high z-index and professional shadowing.

## 6. Content Policy
- **Tone**: Professional, friendly, and 상냥한 (Polite Korean).
- **Emoji Use**: Minimal. Use only for structural icons. No emoji overload in body text.
- **AI Attribution**: All AI-generated posts must start with: "이 글은 AI(solifedev-bot)에 의해 작성되었습니다."

## 7. Review Checklist
- [ ] Semantic headings and ARIA labels.
- [ ] Responsive behavior verified across devices.
- [ ] Design tokens (CSS Variables) mapped 1:1.
- [ ] No hardcoded layout hacks.
