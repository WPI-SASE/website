# AGENTS.md

Guidance for future agents working in this repository.

## Non-Negotiable Engineering Standard

This project prioritizes code quality over speed.

- Do not introduce hacks, monkey patches, duct-tape fixes, or fragile local workarounds.
- If a requested feature exposes a weak design, fix the underlying design cleanly.
- Backwards compatibility is not important here; this site is not in production.
- Prefer clarity, correctness, maintainability, and simple robust abstractions over preserving flawed behavior.
- If the repo lacks support for a request, say so honestly or add that support properly.
- After changes, report any uncertainty or anything that could reasonably be considered fragile.

## Project Purpose

This is the website for the WPI student chapter of the Society of Asian Scientists and Engineers (SASE).

The audience is potential sponsors, companies, and external partners. It is not primarily for current students. Student-facing communication happens through Instagram, so this site should present the chapter as a professional, sponsor-worthy organization.

The current design direction is image-first: the site should feel like a sponsor-facing photo essay. Photos should carry the story; text should be short, supplementary, and used only where it adds necessary context.

## Tech Stack

- Astro static site using Vite.
- Node requirement: `>=22.12.0`.
- No framework beyond Astro is currently used.
- No added runtime dependencies beyond `astro`.
- Images live in `public/images`.
- Pages live in `src/pages`.
- Shared components live in `src/components`.
- Shared global layout and styling live in `src/layouts/Layout.astro`.

Useful commands:

```powershell
npm run dev
npm run build
npm run preview
```

Run `npm run build` before considering layout or component work complete.

## Current Site Structure

Routes:

- `/` - visual homepage with a large chapter-photo hero and image-led navigation tiles.
- `/about` - chapter/national context, pillars, executive board.
- `/programs` - Mentor Mentee and Senior Banquet as large visual feature sections.
- `/conferences` - National and Regional Conference visual sections.
- `/events` - event photo mosaic plus compact event category labels.
- `/sponsorship` - sponsor-facing value proposition and compact impact stats.
- `/get-involved` - leadership paths with an image-led hero.

Important shared files:

- `src/components/ImagePlaceholder.astro` is the shared media component. Despite the name, it now handles real image folders, static images, carousels, sizing variants, and pending-photo fallback states.
- `src/layouts/Layout.astro` defines global tokens, nav, page layout primitives, `.media-card`, `.media-caption`, `.hero`, grids, and responsive rules.
- `src/components/Footer.astro` contains the footer and social links.
- `src/data/exec.json` contains executive board data and intended portrait paths.

## Image System

Use `ImagePlaceholder.astro` for image presentation instead of hand-rolling image markup.

Supported props:

- `label`: required display/fallback label.
- `folder`: required image folder path. Accepts paths like `/public/images/events/general` or `/images/events/general`.
- `src`: optional static image path, mainly used for exec portraits.
- `alt`: optional alt text.
- `tone`: `blue`, `green`, or `gold`.
- `carousel`: defaults to `true`; set `false` for static images like portraits.
- `variant`: `hero`, `feature`, `tile`, or `portrait`.
- `aspect`: `wide`, `square`, `portrait`, or `panorama`.

Behavior:

- The component scans the target folder at build time for `.jpg`, `.jpeg`, `.png`, `.webp`, `.gif`, and `.avif`.
- If a folder has images and `carousel` is enabled, it renders a carousel.
- If no carousel images exist but a valid `src` exists, it renders the static image.
- If neither exists, it renders an intentional placeholder with “Photo pending.”
- Do not fake missing images or hardcode nonexistent paths.

Known asset reality:

- Populated: `public/images/events/general`, `public/images/programs/mentor-mentee`, `public/images/programs/senior-banquet`, `public/images/conferences/conference-one`.
- Currently pending/mostly empty: `public/images/about/national`, `public/images/about/exec`, `public/images/sponsorship/impact`, `public/images/conferences/conference-two`.
- `src/data/exec.json` references portrait filenames that are not currently present in `public/images/about/exec`; the placeholder behavior is intentional until real portraits are added.

## Design Principles

- Images should be much larger than text blocks.
- Text should act as captions, short context, labels, or calls to action.
- Avoid text-heavy cards when an image can carry the page.
- Use `.media-card` and `.media-caption` for image-led tiles/features.
- Keep card radius at `8px` or less, matching the existing design.
- Avoid decorative gradient/orb-heavy treatments; use real chapter imagery.
- Make layouts responsive with stable dimensions, `min-width: 0`, aspect ratios, and grid constraints.
- Do not hide overflow as a shortcut. If overflow happens, fix the underlying sizing rule.
- Keep copy sponsor-facing: professional, concise, and focused on student opportunity, leadership, community, and partner value.

## Implementation Notes

- Global `.hero` layout is intentionally image-biased: compact text column, large media column.
- `.hero.media-first` supports the homepage’s full visual hero.
- `.media-caption` is overlaid on desktop; global mobile behavior stacks captions below media, except the homepage keeps its hero title over the image for first-viewport brand visibility.
- Carousel controls are built into `ImagePlaceholder.astro`; do not duplicate carousel logic per page.
- The carousel image order is shuffled at build/render time by the component.
- For empty folders, placeholders are part of the product behavior, not a bug.

## Verification Checklist

Before handing off visual/layout changes:

- Run `npm run build`.
- Check all routes at desktop and mobile widths:
  - `/`
  - `/about`
  - `/programs`
  - `/conferences`
  - `/events`
  - `/sponsorship`
  - `/get-involved`
- Confirm there is no horizontal overflow.
- Confirm captions do not overlap incoherently or exceed the viewport.
- Confirm large images render and carousels remain usable.
- Confirm pending-photo folders show intentional placeholders.
- Confirm mobile stacking still makes the site image-first rather than text-first.

## Content Voice

Tone should be polished, direct, and sponsor-aware.

Good copy is short and concrete:

- “Partner with students you can see growing.”
- “Career readiness and belonging in the same room.”
- “Structured mentorship and annual traditions.”

Avoid long student-club explanations, internal process details, and Instagram-style announcements.

