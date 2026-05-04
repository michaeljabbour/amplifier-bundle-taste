# Bento 2.0 — The Motion-Engine Bento Paradigm

When generating modern SaaS dashboards or feature sections, use the "Bento 2.0" architecture below. This goes beyond static cards and enforces a "Vercel-core meets Dribbble-clean" aesthetic heavily reliant on perpetual physics.

This document captures Section 9 of the base `taste-skill` SKILL.md.

---

## A. Core design philosophy

- **Aesthetic:** High-end, minimal, and functional.
- **Palette:** Background `#f9fafb`. Cards are pure white (`#ffffff`) with a 1px border of `border-slate-200/50`.
- **Surfaces:** `rounded-[2.5rem]` for all major containers. Apply a "diffusion shadow" — a very light, wide-spreading shadow, e.g., `shadow-[0_20px_40px_-15px_rgba(0,0,0,0.05)]` — to create depth without clutter.
- **Typography:** Strict `Geist`, `Satoshi`, or `Cabinet Grotesk` font stack. Subtle tracking (`tracking-tight`) for headers.
- **Labels:** Titles and descriptions sit **outside and below** the cards, gallery-style. Do not embed labels inside cards.
- **Pixel-perfection:** Generous `p-8` or `p-10` padding inside cards.

---

## B. The Animation Engine — perpetual motion specs

All cards must contain **"Perpetual Micro-Interactions."** Use the following Framer Motion principles:

- **Spring physics:** No linear easing. Use `type: "spring", stiffness: 100, damping: 20` for a premium, weighty feel.
- **Layout transitions:** Heavily utilize the `layout` and `layoutId` props for smooth re-ordering, resizing, and shared element state transitions.
- **Infinite loops:** Every card has an "Active State" that loops infinitely — Pulse, Typewriter, Float, or Carousel — so the dashboard feels "alive."
- **AnimatePresence requirement:** Wrap dynamic lists and any element that mounts/unmounts in `<AnimatePresence>` so exit animations actually run.
- **60fps target:** Optimize the entire grid for 60fps. If a card drops frames during its perpetual loop, it goes back to the bench.

### The "Perpetual Micro-Animation" Concept

Every Bento card has a *resting state* that is still moving. It is never inert. The motion is small enough that it does not steal focus from the user's task — a status dot pulsing, an avatar floating ±2px, a list item swapping rank — but consistent enough that the page feels staffed by an active system rather than a static screenshot.

The discipline of the concept is: motion *signals liveness, not novelty*. Avoid attention-grabbing animations on resting components. Reserve big motion for state changes the user actually triggered.

---

## C. The 5 Card Archetypes

Use these specific archetypes when constructing Bento grids. A typical grid is **Row 1: 3 cols | Row 2: 2 cols split 70/30**.

### 1. The Intelligent List
A vertical stack of items with an infinite auto-sorting loop. Items swap positions using `layoutId`, simulating an AI prioritizing tasks in real time.

- **When to use:** "AI prioritizes your inbox," activity feeds, ranked task lists, suggestion stacks.
- **Anti-patterns:** Don't fake the sort with CSS-only transitions — use `layoutId` so React reconciles position changes correctly. Don't use `useState` to drive the swap loop in the parent (causes re-render of every sibling card); isolate the loop in the leaf list itself.

### 2. The Command Input
A search/AI bar with a multi-step Typewriter Effect. Cycles through complex prompts with a blinking cursor and a "processing" state with a shimmering loading gradient.

- **When to use:** "Ask anything" hero panels, AI command palette demos, search-driven product features.
- **Anti-patterns:** Don't restart the typewriter loop on every parent re-render — memoize. Don't use a real `<input>` element if the bar is illustrative only; otherwise screen readers announce a phantom textbox.

### 3. The Live Status
A scheduling interface with "breathing" status indicators. Includes a pop-up notification badge that emerges with an "Overshoot" spring effect, stays for 3 seconds, then vanishes.

- **When to use:** Real-time dashboards, status pages, calendar/scheduling product moments, monitoring overviews.
- **Anti-patterns:** Don't loop the notification badge so tightly that it feels like a flicker — leave 3+ seconds between cycles. Don't use a green "all systems go" dot if there is no actual signal behind it; users read these as truth.

### 4. The Wide Data Stream
A horizontal "Infinite Carousel" of data cards or metrics. The loop must be seamless using `x: ["0%", "-100%"]` with a speed that feels effortless (not laggy, not racing).

- **When to use:** Logo bars, metric tickers, "loved by these companies" strips, full-width data scrubs.
- **Anti-patterns:** Don't use a real scroll container — `overflow-x-auto` will leak horizontal scrollbars. Drive the loop with `transform: translateX` only. Pause on hover for accessibility.

### 5. The Contextual UI (Focus Mode)
A document view that animates a staggered highlight of a text block, followed by a "Float-in" of a floating action toolbar with micro-icons.

- **When to use:** "AI rewrites your doc" demos, focus-mode editor showcases, contextual menu reveals.
- **Anti-patterns:** Don't let the highlight + toolbar fire on every render — gate the animation with `whileInView` so it plays once when the card scrolls into the viewport, then loops on a long interval.

---

## D. Performance isolation rules

This is the difference between Bento 2.0 looking premium and looking janky on mobile.

- **Memoize every perpetual leaf.** Each archetype above is wrapped in `React.memo` so a parent re-render does not restart its loop or recompute its motion values.
- **Each leaf is its own Client Component.** `'use client'` at the top of the file. The parent grid layout stays a Server Component (or a static Client Component) whose children are these isolated leaves.
- **Do not re-render parents from inside loops.** Never call `setState` in the parent from within an infinite animation. State for the loop lives in the leaf.
- **Use Framer Motion's motion values for continuous animation.** `useMotionValue` / `useTransform` run outside the React render cycle. `useState` for hover positions or animation frames will collapse mobile performance.
- **AnimatePresence wraps the dynamic list, not the whole grid.** Wrapping the grid causes every sibling to re-evaluate exit animations on any list change.
- **Pause when off-screen.** Use `whileInView` or IntersectionObserver to pause infinite loops for cards not in the viewport. Five idle infinite loops on a 60fps target burn battery for nothing.

If a Bento card violates one of the above isolation rules, the grid will look fine on a desktop preview and stutter on a mid-range Android. Do not ship without verifying the isolation.

---

## E. When NOT to use Bento 2.0

- Brutalist or editorial archetypes — Bento 2.0's diffusion shadow + 2.5rem radius is incompatible with brutalist's 90° corners and editorial's flatter type-led layouts.
- `VISUAL_DENSITY > 7` — at cockpit density, cards are banned (use `border-t` / `divide-y`); Bento 2.0 expects breathable cards.
- Pages where the perpetual motion would compete with a primary scroll-driven narrative — pick one source of motion energy per viewport.
