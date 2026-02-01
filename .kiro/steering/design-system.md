# Design System Steering

This document defines the professional design standards for the "So Life Dev Log" Astro blog. Kiro should follow these standards for all UI/UX and styling tasks.

## 1. Visual Identity
- **Vibe**: Clean, Professional, Developer-centric, Minimalist.
- **Theme**: Dark Mode only.
- **Primary Color**: `#1a1a1a` (Background).
- **Secondary Color**: `#242424` (Header, Footer, Sidebar).
- **Accent Color**: `#00d4d4` (Cyan) - use for links, buttons, and highlights.
- **Card Background**: `#2a2a2a`.
- **Border Color**: `#3a3a3a`.
- **Text Primary**: `#e5e5e5`.
- **Text Secondary**: `#a0a0a0`.

## 2. Typography
- **Primary Font**: 'Pretendard', -apple-system, BlinkMacSystemFont, sans-serif.
- **Body Text**: `18px`, line-height `1.7`.
- **Headings**:
  - H1: Bold, `2.5rem`, low letter-spacing.
  - H2: Semi-bold, `2rem`.
  - H3: Semi-bold, `1.5rem`.
- **Code**: Monospace font, background matching `--bg-secondary`, rounded corners.

## 3. Layout Standards
- **Container**: Max-width `1100px`, centered.
- **Sidebar (Left)**:
  - Width: `240px` fixed on desktop.
  - Position: Sticky (80px from top).
  - Responsive: Moves to top/bottom or hidden on mobile (< 720px).
- **Grid**: Use CSS Grid `auto-fill` with `minmax(300px, 1fr)` for post lists.
- **Spacing**: Consistent 8px scale (padding: 1em, 1.5em, 2em).

## 4. Component Patterns
- **Post Cards**:
  - 1px solid border.
  - 12px border-radius.
  - `transition: transform 0.2s, box-shadow 0.2s`.
  - Hover: `translateY(-4px)` and stronger box-shadow.
- **Search**:
  - Minimalist input.
  - Results dropdown must have high z-index and shadow.
- **Buttons/Links**:
  - Hover should slightly brighten or change to accent color.
  - No underlines by default.

## 5. Implementation Rules
- **No Emoji Overload**: Use emojis only for structural icons (e.g., Search icon, Logo). Avoid them in body text unless requested.
- **A11y**: Maintain high contrast ratios for readability.
- **Responsive First**: Always ensure the layout works on mobile (1 column) and tablet.
- **CSS Variables**: Use the defined CSS variables in `global.css` for consistency.
