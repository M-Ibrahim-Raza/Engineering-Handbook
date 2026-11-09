# UI/UX Best Practices

> **Applies to:** web apps on desktop and mobile browsers, in any framework. Examples use React 19, Tailwind CSS v4 and shadcn/ui (the stack in [react.md](react.md), [tailwind.md](tailwind.md) and [shadcn.md](shadcn.md)), or plain HTML where the browser does the job on its own.
> **Baseline:** WCAG 2.2 Level AA (also ISO/IEC 40500:2025) · EN 301 549 v4.1.1 · WAI-ARIA Authoring Practices Guide (APG) · Core Web Vitals (LCP, INP, CLS) · web platform features marked Baseline as of 2026-11, checked on webstatus.dev and MDN.
> **Last researched:** 2026-11-09, from W3C WAI, Nielsen Norman Group, the GOV.UK Design System, Baymard Institute, Material Design 3, Apple's Human Interface Guidelines, Microsoft's HAX Toolkit, Google's People + AI Guidebook, web.dev and MDN, MeasuringU, EUR-Lex, the EDPB, the FTC and other regulators (see [Sources](#23-sources)).
> **Re-check when:** WCAG 3.0 reaches Candidate Recommendation, or EN 301 549 v4.1.1 is cited in the EU Official Journal (§15); the EU publishes the Digital Fairness Act proposal, the FTC issues a new negative-option rule, or the UK subscription rules start (§19); customizable `<select>`, `dialog closedby` or `interestfor` become Baseline (§6); Google changes a Core Web Vitals threshold (§17).
> **Related:** [react.md](react.md) (accessibility basics, Actions and forms), [nextjs.md](nextjs.md) (routing, error pages, performance tooling), [tailwind.md](tailwind.md) (contrast, focus styles, touch targets, motion utilities, responsive utilities), [shadcn.md](shadcn.md) (components, forms, toasts, skeletons, empty states), [tanstack-query.md](tanstack-query.md) (optimistic updates, keeping data on screen while refetching), [timezones.md](timezones.md) (showing dates and times), [logging.md](logging.md) (request IDs, sensitive data), [feature-engineering.md](../guides/feature-engineering.md) (deciding what to build).

## How to use this file

These rules are for anyone who designs, builds or reviews a screen, human or AI agent. They describe **what users should see and be able to do**. How to build it in our stack lives in the files each rule links to. Read **Key rules** first, then the sections you need. [AGENTS.md](../AGENTS.md) explains how the files fit together.

| Keyword | Meaning |
| --- | --- |
| **MUST** / **MUST NOT** | Required or forbidden. Breaking it is a bug or blocks review. |
| **SHOULD** / **SHOULD NOT** | The default. If you deviate, add a one-line code comment saying why (or say why in the PR description if the change isn't tied to one spot). |
| **MAY** | Allowed when it helps; not required. |

**When rules conflict:** a decision recorded in the project (an ADR, the project's `CLAUDE.md` or `AGENTS.md`, a ticket) wins over this file, and a more specific file wins over a more general one ([AGENTS.md § Precedence](../AGENTS.md#precedence)). For this file that means react.md, nextjs.md, tailwind.md and shadcn.md win on **how** to build something in that stack; this file wins on **what** users experience.

More notes for this file:

- ***(our rule)*** marks a number or choice made by these best practices where research gives a range, not one answer.
- ***(human)*** marks steps that need a person, such as usability tests or screen-reader checks. An agent lists them for the developer instead of skipping them.
- **Legal notes** summarize the law for engineers as of 2026-11. They are not legal advice. Ask legal before you ship consent, pricing, subscription, cancellation or AI-disclosure flows.
- **Already covered elsewhere.** These rules live in other files. This file links to them and doesn't repeat them:

| Topic | Owning file |
| --- | --- |
| Semantic HTML, form labels, keyboard access, focus management, live regions, screen-reader tests, color contrast, status shown by color alone | [react.md §13](react.md#13-accessibility) |
| Focus ring styles, contrast of color tokens, names for icon-only buttons, touch target sizes, reduced-motion utilities, forced colors | [tailwind.md §11](tailwind.md#11-accessibility) |
| Layout shift, animating cheap properties | [tailwind.md §12](tailwind.md#12-performance) |
| Breakpoints and container queries | [tailwind.md §6](tailwind.md#6-responsive-design-and-container-queries) |
| Dark mode | [tailwind.md §7](tailwind.md#7-dark-mode), [shadcn.md §5](shadcn.md#5-theming--design-tokens) |
| Screenshot tests and automated accessibility checks (axe) | [tailwind.md §14](tailwind.md#14-testing) |
| Form layout and wiring, field error attributes, which control fits which input | [shadcn.md §8](shadcn.md#8-forms) |
| Dialog titles, focus styles and keyboard use of overlays and menus | [shadcn.md §10](shadcn.md#10-accessibility) |
| Toast setup, skeleton and spinner components, the `Empty` component | [shadcn.md §11](shadcn.md#11-feedback--loading-states) |
| Server Actions, pending state, optimistic updates | [react.md §8](react.md#8-mutations-forms-and-actions), [tanstack-query.md §11](tanstack-query.md#11-optimistic-updates) |
| Suspense fallbacks shaped like the final layout | [react.md §7.3](react.md#73-streaming-with-suspense) |
| Error and not-found pages | [nextjs.md §11](nextjs.md#11-error-handling) |
| Measuring Web Vitals | [nextjs.md §12](nextjs.md#12-performance-and-assets) |
| Showing dates, times and time zones | [timezones.md §10](timezones.md#10-showing-times-to-people) |

## Key rules

- **MUST** give every screen one primary action, and keep rarely used features one predictable step away (overflow menu, command palette, settings) instead of on the screen ([§2](#2-feature-rich-without-clutter)).
- **MUST NOT** nest progressive disclosure more than 2 levels deep, or hide information users need for the decision in front of them ([§2](#2-feature-rich-without-clutter)).
- **MUST** keep shareable view state (tab, filters, sort, search query, page) in the URL ([§3](#3-information-architecture-and-navigation)).
- **MUST** make every screen work at 320 CSS px wide and at 200% zoom without horizontal page scrolling ([§4](#4-layout-visual-hierarchy-and-typography)).
- **MUST NOT** make any action or information reachable only by hover, by dragging, or through a tooltip ([§6](#6-components-and-interaction-patterns)).
- **MUST** show feedback within 100 ms of every action, and a progress indicator for anything that can take longer than 1 s ([§7](#7-feedback-and-system-status)).
- **MUST** build loading, empty, error and success states for every view of server data, plus a no-results state for every search and filter ([§8](#8-screen-states)).
- **MUST** keep everything the user typed after a failed submit ([§9](#9-forms-and-input)).
- **MUST NOT** show a field's error while the user is still typing in that field for the first time ([§9](#9-forms-and-input)).
- **MUST** confirm irreversible actions in a dialog that names the object and the consequence, with a button that repeats the verb ([§10](#10-errors-confirmation-and-undo)).
- **MUST NOT** add single-character keyboard shortcuts that users can't turn off or remap ([§12](#12-search-command-palette-and-keyboard-shortcuts)).
- **MUST** meet WCAG 2.2 Level AA on every screen ([§15](#15-accessibility)).
- **MUST NOT** add accessibility overlay widgets ([§15](#15-accessibility)).
- **MUST** meet LCP ≤ 2.5 s, INP ≤ 200 ms and CLS ≤ 0.1 at the 75th percentile on key screens ([§17](#17-performance-as-ux)).
- **MUST** tell users when they are dealing with an AI, and get their explicit approval before an AI agent does anything irreversible or that reaches other people ([§18](#18-ai-features)).
- **MUST NOT** use dark patterns: pre-checked consent, a reject option harder than accept, cancelling harder than signing up, fake urgency, confirmshaming ([§19](#19-ethical-design-and-dark-patterns)).
- **MUST NOT** put personal data in analytics events ([§20](#20-ux-research-testing-and-metrics)).

## Contents

1. [UX principles](#1-ux-principles)
2. [Feature-rich without clutter](#2-feature-rich-without-clutter)
3. [Information architecture and navigation](#3-information-architecture-and-navigation)
4. [Layout, visual hierarchy and typography](#4-layout-visual-hierarchy-and-typography)
5. [Responsive, mobile and touch](#5-responsive-mobile-and-touch)
6. [Components and interaction patterns](#6-components-and-interaction-patterns)
7. [Feedback and system status](#7-feedback-and-system-status)
8. [Screen states](#8-screen-states)
9. [Forms and input](#9-forms-and-input)
10. [Errors, confirmation and undo](#10-errors-confirmation-and-undo)
11. [Tables, lists, filters and bulk actions](#11-tables-lists-filters-and-bulk-actions)
12. [Search, command palette and keyboard shortcuts](#12-search-command-palette-and-keyboard-shortcuts)
13. [Onboarding, discoverability and help](#13-onboarding-discoverability-and-help)
14. [Content and microcopy](#14-content-and-microcopy)
15. [Accessibility](#15-accessibility)
16. [Motion and animation](#16-motion-and-animation)
17. [Performance as UX](#17-performance-as-ux)
18. [AI features](#18-ai-features)
19. [Ethical design and dark patterns](#19-ethical-design-and-dark-patterns)
20. [UX research, testing and metrics](#20-ux-research-testing-and-metrics)
21. [Anti-patterns](#21-anti-patterns)
22. [PR checklist](#22-pr-checklist)
23. [Sources](#23-sources)

---

## 1. UX principles

Every rule in this file comes from a small set of well-tested principles: Nielsen's 10 usability heuristics and a few "laws of UX" from psychology research. Use the table in design reviews: when a screen breaks a principle, the last column says where the matching rules are.

| Principle | What to check on a screen | Rules |
| --- | --- | --- |
| **Visibility of system status** (heuristic 1) | Every action gets visible feedback; long work shows progress. | [§7](#7-feedback-and-system-status), [§8](#8-screen-states) |
| **Match between the system and the real world** (2) | Labels use the user's words, not database or code names. | [§14](#14-content-and-microcopy) |
| **User control and freedom** (3) | Undo, Cancel, Back and Esc always work. | [§10](#10-errors-confirmation-and-undo) |
| **Consistency and standards** (4) and **Jakob's law** | The same thing looks and works the same everywhere, and like other sites users know. | [§6](#6-components-and-interaction-patterns) |
| **Error prevention** (5) | Defaults, constraints and confirmations stop errors before they happen. | [§9](#9-forms-and-input), [§10](#10-errors-confirmation-and-undo) |
| **Recognition rather than recall** (6) | Options, recent items and shortcuts are shown, not memorized. | [§12](#12-search-command-palette-and-keyboard-shortcuts) |
| **Flexibility and efficiency of use** (7) | Experts get shortcuts, bulk actions and saved views; beginners never need them. | [§2](#2-feature-rich-without-clutter), [§11](#11-tables-lists-filters-and-bulk-actions), [§12](#12-search-command-palette-and-keyboard-shortcuts) |
| **Aesthetic and minimalist design** (8) | Every element on screen earns its place; rare features are one step away. | [§2](#2-feature-rich-without-clutter), [§4](#4-layout-visual-hierarchy-and-typography) |
| **Help users recognize, diagnose and recover from errors** (9) | Errors say what happened and how to fix it. | [§10](#10-errors-confirmation-and-undo), [§14](#14-content-and-microcopy) |
| **Help and documentation** (10) | Help sits where questions come up, in the same place on every page. | [§13](#13-onboarding-discoverability-and-help) |
| **Hick's law**: decision time grows with the number of choices | Few, grouped choices per decision; long menus are split or searchable. | [§2](#2-feature-rich-without-clutter), [§3](#3-information-architecture-and-navigation) |
| **Fitts's law**: big, close targets are faster to hit | Primary and frequent targets are large and near where the pointer or thumb already is. | [§5](#5-responsive-mobile-and-touch), [§6](#6-components-and-interaction-patterns) |
| **Tesler's law**: complexity can't be removed, only moved | The system absorbs complexity (defaults, inference, formatting), not the user. | [§2](#2-feature-rich-without-clutter), [§9](#9-forms-and-input) |
| **Doherty threshold**: work flows when the system answers in under 400 ms | Responses feel instant, or show progress at once. | [§7](#7-feedback-and-system-status), [§17](#17-performance-as-ux) |
| **Miller's law** (often misused) | Chunk information into groups. Don't use "7 ± 2" to cap menus: people scan menus, they don't memorize them. | [§4](#4-layout-visual-hierarchy-and-typography) |
| **Peak-end rule**: people judge an experience by its worst moment and its end | Error states and the success state at the end of a flow get real design time. | [§8](#8-screen-states), [§10](#10-errors-confirmation-and-undo) |
| **Goal-gradient effect**: people speed up as a goal gets close | Multi-step flows and setup checklists show progress. | [§9](#9-forms-and-input), [§13](#13-onboarding-discoverability-and-help) |
| **Postel's law**: accept liberally, output strictly | Accept input in many formats; show it back in one. | [§9](#9-forms-and-input) |
| **Von Restorff effect**: the item that differs is remembered | Only one element per view stands out: the primary action. | [§2](#2-feature-rich-without-clutter) |
| **Law of proximity**: items close together read as a group | Related controls sit together; spacing groups before borders do. | [§4](#4-layout-visual-hierarchy-and-typography) |
| **Aesthetic-usability effect**: polished UIs feel easier | Polish hides usability problems in tests; measure task success, not only opinions. | [§20](#20-ux-research-testing-and-metrics) |

---

## 2. Feature-rich without clutter

A product can have many features and still be easy to use. The trick is that each screen shows what most users need most of the time, and everything else is one predictable step away. "Simple" means few decisions at any moment, not few features.

### 2.1 One job per screen

- **MUST** give every screen one primary action, styled as the only primary button in view. A dialog or sheet counts as its own view. *Why:* when two primary buttons compete, users hesitate, and the eye can only be pulled to one standout element. *(our rule)*
- **MAY** skip the primary action on screens where every option is equal, such as a set of pricing plans or a list of templates.
- **SHOULD** place the remaining actions by how often users need them:

| How often users need it | Where it goes | Example |
| --- | --- | --- |
| Most visits | Visible: the primary button, or a secondary button next to it | "New task" |
| Some visits | Visible as a secondary or ghost button, a toolbar item, or an action that appears on selection | "Filter", "Export" |
| Rarely | Overflow menu (`⋯`), the command palette ([§12](#12-search-command-palette-and-keyboard-shortcuts)) or settings | "Duplicate project", "Move to workspace" |
| Once, during setup | Settings or onboarding ([§13](#13-onboarding-discoverability-and-help)) | "Default assignee" |
| Destructive | Apart from the primary action, in the destructive style, often in the overflow menu | "Delete project" |

- **SHOULD** base "how often" on usage data when you have it ([§20](#20-ux-research-testing-and-metrics)), not on how proud the team is of a feature.
- **MUST** keep every feature reachable in at least one visible place (a menu, settings or a page), even when the command palette or a shortcut also reaches it. *Why:* new users don't know accelerators exist; a feature that only an accelerator reaches doesn't exist for them.

### 2.2 Progressive disclosure

Progressive disclosure shows the common options first and moves advanced ones to a second layer the user opens on purpose.

- **SHOULD** put advanced options behind a disclosure that is closed by default and labelled with what's behind it ("Advanced options", "More filters"), not a bare "More" inside a form.
- **MUST NOT** nest disclosure more than 2 levels deep (for example, an expandable panel inside an expandable panel inside a dialog). *Why:* Nielsen Norman Group found that designs with more than two levels of disclosure have low usability, because users get lost between the levels.
- **MUST NOT** hide information users need for the decision in front of them (price, consequences, required fields, what will be shared) behind a disclosure, tooltip or "details" link. *Why:* users act on what they see; hiding the deciding facts causes errors and counts as a dark pattern ([§19](#19-ethical-design-and-dark-patterns)).
- **SHOULD** use staged disclosure (a wizard with steps) only for tasks that are linear and rare, such as first setup or a data import. Tasks users repeat belong on one page.
- **MAY** remember per user that they opened an advanced section, if they open it on most visits.

### 2.3 Defaults do the work

- **SHOULD** give every field and setting a default when one value fits most users (assignee = the current user, project = the one being viewed). *Why:* each default removes a decision; that's how Tesler's law moves complexity from the user to the system.
- **SHOULD** infer what you can (time zone from the browser, language from the `Accept-Language` header, country from the locale) and let users change it.
- Defaults that cost money, share data or subscribe the user are forbidden: see [§19](#19-ethical-design-and-dark-patterns).

### 2.4 Actions where the object is

- **SHOULD** show actions for an item next to the item (row actions, a toolbar that appears on selection, an item menu), not in a global toolbar far from it.
- Actions that appear on hover must also work with keyboard and touch: see [§6.3](#63-overlays-dialogs-sheets-popovers-and-tooltips).

### 2.5 Fast paths for frequent users

Frequent users need speed; new users need clarity. Give frequent users accelerators that new users never have to see:

- **SHOULD** offer keyboard shortcuts and a command palette ([§12](#12-search-command-palette-and-keyboard-shortcuts)), bulk actions and saved views ([§11](#11-tables-lists-filters-and-bulk-actions)), templates, and "Duplicate". Nielsen Norman Group's test for accelerators: "readily available, yet easy to ignore".
- **SHOULD** let users customize data views (visible columns, density, sort) and remember the choice per user.
- **SHOULD** offer "Reset to default" wherever users can customize a view or layout. *Why:* without it, a user who breaks a layout is stuck with it.

### 2.6 Settings

- **SHOULD** group settings by the user's task (Profile, Notifications, Members, Billing), not by the internal module that owns them.
- **SHOULD** add a search box to settings once there are more than 20 settings *(our rule)*.
- **MUST NOT** mix save models on one settings page: either every control saves when changed (and shows "Saved"), or the page has one Save button. *Why:* with both, users can't tell which changes were saved and which were lost.

### 2.7 Adding a feature without adding clutter

- **SHOULD** answer these before adding UI for a new feature, and write the answers in the ticket:
  1. Who needs it, and how often? (Use the placement table in [§2.1](#21-one-job-per-screen).)
  2. Can an existing control do it with one more option, instead of a new control?
  3. What gets demoted or removed to make room?
  The [feature-engineering guide](../guides/feature-engineering.md) covers deciding whether to build it at all.
- **SHOULD** demote or remove features with low use instead of letting screens grow, after checking usage data ([§20](#20-ux-research-testing-and-metrics)).

---

## 3. Information architecture and navigation

- **SHOULD** name navigation items with the user's nouns (Projects, Tasks, Members), in one or two words, and use the same words in the nav item, the page `<h1>` and the breadcrumbs.
- **MUST** mark where the user is: highlight the current navigation item and set `aria-current="page"` on it. *Why:* sighted users and screen-reader users both lose their place without it.
- **MUST** keep navigation that repeats across pages in the same place and order on every page. *Why:* WCAG 3.2.3 Consistent Navigation (AA); users learn positions, not labels.
- **SHOULD** use a sidebar when the app has more than 5 top-level sections, and a top bar or tabs for 5 or fewer *(our rule)*.
- **SHOULD NOT** hide the main navigation behind a menu button (hamburger) on desktop widths. *Why:* users can't use what they can't see, and visible navigation also shows what the product can do. On narrow screens a menu button or a bottom tab bar with 3–5 items is fine.
- **SHOULD** add breadcrumbs when the hierarchy is 3 or more levels deep (Workspace › Project › Task).
- Navigation must be real links, not buttons: [react.md §13](react.md#13-accessibility) and [shadcn.md §7](shadcn.md#7-component-composition).

### 3.1 Put view state in the URL

- **MUST** keep view state that users would share, bookmark, reload or come back to in the URL: the current tab, filters, sort, search query, page or cursor, and the selected item in a list-and-detail view. *Why:* otherwise reload, Back, copied links and "open in new tab" all lose the user's place.
- **MUST NOT** put secrets, tokens or personal data (emails, names, free text typed by the user other than a search query) in the URL. *Why:* URLs end up in browser history, server logs, analytics and `Referer` headers ([logging.md §1.7](logging.md#17-sensitive-data)).
- **SHOULD** add a history entry (push) for changes users expect Back to undo, such as opening an item or clicking a tab or filter, and replace the entry while the user types in a search box or drags a slider. *Why:* one Back press per keystroke traps the user on the page.
- **MUST** return users to the page they asked for after sign-in. Validate the return URL as same-origin first. *Why:* deep links that land on the dashboard after login break every shared link; an unvalidated return URL is an open redirect.

Status filters as plain links keep the URL as the source of truth, work without JavaScript, and give Back the right behavior:

```tsx
// app/(app)/tasks/status-tabs.tsx
import Link from 'next/link';

const STATUSES = [
  { value: 'open', label: 'Open' },
  { value: 'done', label: 'Done' },
  { value: 'all', label: 'All' },
] as const;

// `params` is the page's current searchParams, so sort and search survive a status change.
export function StatusTabs({ params }: { params: Record<string, string> }) {
  const current = params.status ?? 'open';

  return (
    <nav aria-label="Task status">
      {STATUSES.map(({ value, label }) => {
        const next = new URLSearchParams({ ...params, status: value });
        next.delete('cursor'); // a new filter starts at the first page
        return (
          <Link
            key={value}
            href={`/tasks?${next.toString()}`}
            aria-current={current === value ? 'page' : undefined}
            scroll={false}
          >
            {label}
          </Link>
        );
      })}
    </nav>
  );
}
```

---

## 4. Layout, visual hierarchy and typography

### 4.1 Hierarchy and grouping

- **SHOULD** make the visual order of importance match the reading order: page title, then the primary action and key content, then secondary content.
- **SHOULD** make heading sizes follow heading levels, so `h2` always looks bigger than `h3`. The heading order rule itself is in [react.md §13](react.md#13-accessibility).
- **SHOULD** group with space first: more space between groups than inside them. Add borders or backgrounds only where space isn't enough. *Why:* the law of proximity; every extra border adds visual noise.
- **SHOULD** take spacing, sizes and colors only from the design tokens ([tailwind.md §2](tailwind.md#2-design-tokens-and-the-theme)).
- **SHOULD** use a type scale of at most 6 sizes across the app *(our rule)*.
- **SHOULD** place each screen type's primary action in the same position everywhere (for example, top right of the page header, end of the form, bottom right of a dialog).
- **SHOULD** use each semantic color for one meaning only: the destructive color for destructive actions and errors, the success color for success. *Why:* if red also decorates, users stop reading it as a warning.

### 4.2 Text that is easy to read

- **SHOULD** set body text to at least 16 px (`1rem`) and the line height of paragraphs to at least 1.5 (the US Web Design System's minimum for body copy).
- **SHOULD** keep lines of running text between 45 and 75 characters (Tailwind's `max-w-prose` is 65 characters). *Why:* typographers have long treated 45–75 characters as comfortable, and Baymard's testing puts the best range at 50–75. Tables and code are exempt.
- **SHOULD** left-align text in left-to-right languages, and never justify it. *Why:* justified text makes uneven gaps between words that slow reading, worst for people with dyslexia.
- **SHOULD** use `text-wrap: balance` (Baseline since 2024) on headings, so short titles don't leave one word on the last line.
- **SHOULD NOT** write running text in all capitals. Short labels, such as a badge, are fine.

### 4.3 Zoom, reflow and text spacing

- **MUST** make every screen work at 320 CSS px wide without horizontal page scrolling. Data tables, maps and diagrams, which need two dimensions, may scroll inside their own container. *Why:* WCAG 1.4.10 Reflow (AA). A 1280 px window zoomed to 400% is 320 px wide, and low-vision users browse that way.
- **MUST** keep all content and functions usable when the browser zooms text to 200%. *Why:* WCAG 1.4.4 Resize Text (AA).
- **MUST NOT** clip or overlap text when users raise text spacing to line height 1.5, paragraph spacing 2× the font size, letter spacing 0.12× and word spacing 0.16×. Avoid fixed heights on elements that hold text. *Why:* WCAG 1.4.12 Text Spacing (AA); people with dyslexia use these overrides.

### 4.4 Density

- **SHOULD** offer a density setting (comfortable and compact) in data-heavy screens instead of designing one cramped layout for everyone. Compact suits experts scanning many rows; comfortable suits touch and occasional users.

---

## 5. Responsive, mobile and touch

Breakpoints and container queries are in [tailwind.md §6](tailwind.md#6-responsive-design-and-container-queries); touch target sizes are in [tailwind.md §11](tailwind.md#11-accessibility).

- **SHOULD** design small screens first: decide what the narrow layout shows, then add to it for wider screens.
- **SHOULD** check every new screen at 320, 375, 768, 1280 and 1920 px wide *(our rule)*.
- **SHOULD NOT** remove features from the mobile web layout that exist on desktop. Move them into menus or sheets instead. *Why:* people switch devices in the middle of a task and expect the same app.
- **SHOULD** put the primary action where a thumb reaches it on phones: a sticky bar at the bottom, or the end of a short form.
- **MUST NOT** lock the page to portrait or landscape. *Why:* WCAG 1.3.4 Orientation (AA); some users have their device mounted in one orientation.
- **MUST NOT** disable pinch zoom (`user-scalable=no` or `maximum-scale=1` in the viewport meta tag). *Why:* low-vision users zoom to read; WCAG 1.4.4 requires text to scale.
- **MUST** offer a single-tap alternative to every gesture that needs a path or more than one finger (swipe to delete, pinch to zoom a chart). *Why:* WCAG 2.5.1 Pointer Gestures (A); many users can't swipe or pinch precisely.
- **SHOULD** size full-height layouts with `dvh` or `svh` units (Baseline Widely available since 2025-06), not `100vh`. *Why:* on mobile browsers `100vh` includes the area under the address bar, so the bottom of the layout is hidden.
- **SHOULD** pad fixed bars with `env(safe-area-inset-bottom)` (and set `viewport-fit=cover`), so the home indicator and rounded corners on phones don't cover buttons.
- **SHOULD** keep sticky headers and footers short on small screens; they must never fully cover the focused element ([§15.2](#152-wcag-22-criteria-owned-by-this-file)).
- The right on-screen keyboard for each field comes from `type`, `inputmode` and `autocomplete` ([§9.2](#92-help-users-type-less)).

---

## 6. Components and interaction patterns

### 6.1 Consistency

- **SHOULD** build screens from the project's design system (components and tokens, [shadcn.md](shadcn.md)) before writing a new component. *Why:* every custom version of a known pattern is one more thing users have to learn and the team has to keep accessible.
- **MUST** give the same action the same label, icon and position everywhere ("Archive" is never "Remove" on another screen). *Why:* users assume a different word means a different action (heuristic 4).
- **SHOULD** follow web conventions users already know: the logo links home, links in running text are underlined, Esc closes overlays, Enter submits single-line forms, and Ctrl/Cmd-click opens links in a new tab.
- **SHOULD NOT** copy native platform design languages, such as Apple's Liquid Glass (2025) or Material 3 Expressive (2025), into a web app. *Why:* they are built for their own platforms (Material 3 Expressive targets Android and Wear OS, and Apple says not to use Liquid Glass in the content layer), and translucent layers easily break text contrast.

### 6.2 Buttons and disabled controls

- **SHOULD NOT** disable a submit button to signal that a form is incomplete or invalid. Keep it enabled and show the errors when it's pressed ([§9.3](#93-validation-timing)). *Why:* a disabled button doesn't say what's wrong, keyboard focus skips it, and the GOV.UK Design System notes that disabled buttons have poor contrast and confuse some users. Disabling a button while its submission is pending, to block double submits, is fine ([react.md §8](react.md#8-mutations-forms-and-actions)); the server must still reject a duplicate.
- **MUST** say why a control is unavailable when it stays unavailable for a reason the user can change or should know (missing permission, plan limit, a required earlier step), in visible text next to it or in a tooltip on a focusable element. *Why:* a silent disabled control is a dead end.
- **SHOULD** use `aria-disabled="true"` instead of the `disabled` attribute when the control must stay focusable to explain itself. *Why:* elements with `disabled` can't receive focus or hover, so their tooltip never shows.
- Button labels: [§14](#14-content-and-microcopy).

### 6.3 Overlays: dialogs, sheets, popovers and tooltips

Pick the lightest overlay that does the job:

| Need | Use |
| --- | --- |
| A decision the user must make before going on (delete, discard changes) | Modal dialog |
| A short, focused task that keeps the page context (rename, quick create) | Modal dialog or side sheet |
| A long or complex task (multi-section form, editor) | Its own page with its own URL, not a modal |
| Look at one item while keeping the list visible | Side sheet or split view, with the item in the URL |
| A few options or actions from one trigger | Popover or menu |
| A short name for an icon, or a hint | Tooltip, never essential information |
| Tell the user something happened | Toast or inline status ([§7.2](#72-choose-the-right-feedback-channel)) |

- **MUST NOT** open a dialog from inside a dialog, except a confirmation for discarding or deleting. *Why:* stacked dialogs confuse focus and Esc, and users lose track of where they are.
- **MUST NOT** open a modal the user didn't ask for (promotions, newsletter sign-ups, "rate us") when a page loads or in the middle of a task. *Why:* it blocks the task the user came to do. Session-timeout warnings ([§9.5](#95-never-lose-the-users-work)) and legally required notices are the exceptions.
- **MUST** let users close every overlay with Esc and a visible close button. *Why:* users who can't find the way out abandon the task; keyboard users have no other exit.
- **SHOULD** close popovers, menus and non-critical dialogs on a click outside them too. Dialogs holding unsaved input ask first ([§9.5](#95-never-lose-the-users-work)).
- **MUST NOT** put information or actions needed to finish a task only in a tooltip. *Why:* tooltips need hover or keyboard focus, don't exist on touch screens, and vanish.
- **MUST** make content that appears on hover or focus dismissible with Esc without moving the pointer, hoverable (the pointer can move onto it without it closing), and persistent until the user dismisses it or moves away. *Why:* WCAG 1.4.13 Content on Hover or Focus (AA). The base libraries behind shadcn do this; hand-made tooltips usually don't.
- **MUST** make actions that appear on hover (row actions, card menus) also appear on keyboard focus, and always visible or reachable from an item menu on touch screens. *Why:* touch screens have no hover, and keyboard users never trigger it.
- **SHOULD** use the browser's `popover` attribute (Baseline Widely available since 2026-10) for a custom menu or popover outside shadcn. It gives light dismiss, Esc and the top layer without JavaScript:

  ```html
  <!-- rendered by app/(app)/tasks/task-row.tsx -->
  <button type="button" popovertarget="task-42-menu">Task actions</button>
  <div id="task-42-menu" popover>
    <button type="button">Rename</button>
    <button type="button">Move to project…</button>
  </div>
  ```

- **SHOULD NOT** rely on `dialog closedby`, `popover="hint"`, customizable `<select>` (`appearance: base-select`), `interestfor` or `hidden="until-found"` without a fallback. As of 2026-11 each is missing from at least one major browser engine. Check [webstatus.dev](https://webstatus.dev) before using any feature that isn't Baseline.

### 6.4 Drag and drop, and gestures

- **MUST** provide a single-pointer alternative to every drag action that doesn't need dragging: "Move up" / "Move down" buttons or a "Move to…" menu for reordering, and number inputs or presets for resizing. *Why:* WCAG 2.5.7 Dragging Movements (AA); users with tremors, switch devices or a keyboard can't drag.
- **SHOULD** support keyboard reordering in sortable lists (Space to pick up, arrow keys to move, Space to drop, Esc to cancel), announced through a live region. Drag-and-drop libraries such as dnd-kit include this.
- **SHOULD** show where an item will land while it is dragged.

### 6.5 Choosing a selection control

Which shadcn control fits which kind of input is in [shadcn.md §8](shadcn.md#8-forms). The numbers to choose between them:

- **SHOULD** use radio buttons for 2–5 options, so all options are visible; a select for 6–15; and a combobox with search for more than 15 or for options loaded from the server *(our rule)*.
- **SHOULD** use a switch only for settings that take effect at once, and a checkbox for choices that are submitted with a form. *Why:* a switch looks like it acts immediately; users don't expect to press Save after it.

---

## 7. Feedback and system status

### 7.1 Response times

| Time from the user's action | What users feel | What to show |
| --- | --- | --- |
| Up to 0.1 s | Instant: they caused it | The result itself: pressed state, toggled switch, optimistic update |
| 0.1–1 s | A delay, but their flow holds | Usually nothing extra; the button shows it is working if a request runs |
| 1–10 s | Attention drifts | A skeleton for a full page of content; a looped indicator (spinner) for one region or button |
| More than 10 s | They switch to something else | Percent done or steps, time left if known, a Cancel button; let them leave and notify them when it's done |

The limits come from Nielsen Norman Group's response-time research: 0.1 s feels instant, 1 s is the limit for an uninterrupted flow of thought, and 10 s is the limit for keeping attention on the task.

- **MUST** show visible feedback within 100 ms of every click, tap or key press that starts an action (a pressed style, a pending state or an optimistic change). *Why:* without it users think the click missed and click again, which causes double submits.
- **MUST** show a progress indicator for every operation that can take longer than 1 s. *Why:* after about a second users notice the wait and start to wonder whether anything is happening.
- **MUST** show progress as percent done, steps or streamed partial results, with a Cancel button, for operations that can take longer than 10 s (uploads, imports, exports, AI generation). *Why:* a spinner gives no sign whether a long job is progressing or stuck; NN/g's guidance is a looped indicator for 2–9 s and a percent-done indicator from 10 s.
- **SHOULD** move work that takes longer than about 10 s to the background: let users keep working, show the job's status in a fixed place (a jobs or notifications panel), and notify them when it finishes.
- **SHOULD** update the UI optimistically for actions that almost always succeed (mark done, rename, reorder) and roll back with an error message when they fail. How: [react.md §8](react.md#8-mutations-forms-and-actions), [tanstack-query.md §11](tanstack-query.md#11-optimistic-updates).
- **SHOULD** wait about 300 ms before showing a spinner, and keep it on screen for at least about 500 ms once shown *(our rule)*. *Why:* a spinner that flashes for 100 ms looks like a glitch.
- **SHOULD** pick the loading indicator by what is loading: a skeleton for content whose layout you know ([react.md §7.3](react.md#73-streaming-with-suspense)), a spinner for an action in a button or small region, and a progress bar for long work with known size. The components are in [shadcn.md §11](shadcn.md#11-feedback--loading-states).

### 7.2 Choose the right feedback channel

| Situation | Channel | Example |
| --- | --- | --- |
| A problem with one field | Inline message under the field | "Enter a due date after the start date" |
| Result of what the user just did, reversible | Toast with Undo | "Task archived. Undo" |
| An error the user must act on | Inline alert where the action failed, with a retry | "Couldn't save your changes. Check your connection and try again. [Retry]" |
| A state that lasts across pages | Banner at the top of the app | "You're offline. Changes will sync when you reconnect." |
| A decision needed before going on | Modal dialog ([§10](#10-errors-confirmation-and-undo)) | "Delete project 'Website'?" |
| Something that happened elsewhere or later | Notification center with an unread count, plus email or push if the user opted in | "Ana assigned you a task" |
| Save state of what the user is editing | Status text that stays visible | "Saved · 2 min ago" |

- **MUST NOT** use a toast as the only place for an error the user must act on, or for information they need later. *Why:* toasts disappear on a timer, often before the user reads them. shadcn's toast rules are in [shadcn.md §11](shadcn.md#11-feedback--loading-states).
- **MUST** keep a toast that holds an action (Undo, Retry, View) on screen until dismissed or for at least 10 s, and pause its timer while it has hover or focus *(our rule for 10 s)*. *Why:* WCAG 2.2.1 Timing Adjustable (A); keyboard, screen-reader and motor-impaired users need longer to reach the button.
- **SHOULD** also offer a toast's action somewhere that doesn't expire (Undo in the item's menu or history, the archived item restorable from an Archive view).
- Status changes must reach screen readers too; the rule and how to do it are in [react.md §13](react.md#13-accessibility).

### 7.3 Saving and syncing

- **SHOULD** autosave long text (descriptions, comments in progress, documents) and show its state: "Saving…", "Saved", "Couldn't save · Retry".
- **MUST NOT** show "Saved" before the server has confirmed the save. *Why:* a false confirmation makes users close the tab and lose their work.

### 7.4 Notifications and interruptions

- **MUST NOT** ask for browser permissions (notifications, location, camera, microphone) when the page loads. Ask right after the user does something that needs the permission, with an in-app explanation first. *Why:* users deny prompts they don't understand, a denial is hard to undo, and browsers quiet the prompts of sites that users often deny.
- **MUST** let users choose which notification types they get on each channel (in-app, email, push). *Why:* users who can't tune notifications mute all of them, including the important ones.
- **MUST** include a one-click unsubscribe in marketing emails. *Why:* EU and US law require an easy opt-out, and Gmail and Yahoo require one-click unsubscribe (RFC 8058) from bulk senders since 2024.
- **SHOULD** send real-time notifications only for things that need action soon (assigned to you, mentioned, due today) and batch the rest into a digest.
- **MUST NOT** move keyboard focus or the scroll position when a toast, a notification or new data arrives. *Why:* it interrupts typing and disorients keyboard and screen-reader users (WCAG 3.2.1 and 3.2.2 forbid unexpected changes of context).
- **SHOULD** hold new items in a live list behind a bar ("3 new tasks · Show") instead of inserting them above what the user is reading. *Why:* content that jumps under the pointer causes misclicks.

---

## 8. Screen states

A screen is more than its happy path with data. Design and build each state before calling a screen done:

| State | When | What to show |
| --- | --- | --- |
| **Loading, first time** | No data yet | A skeleton shaped like the content; navigation and page chrome usable at once |
| **Loading, refresh** | Refetching, paging, filtering, sorting | The current content stays visible, with a small progress indicator; never a blank page |
| **Empty, first use** | The user has no items yet | What this area is for, a button to create the first item, and templates, sample data or import ([§13](#13-onboarding-discoverability-and-help)) |
| **Empty, no results** | Search or filters match nothing | Which query or filters caused it, a "Clear filters" button, suggestions; never the first-use message |
| **Empty, all done** | The user finished everything ("inbox zero") | A short positive message and what to do next |
| **Partial** | Some parts loaded, some failed | What loaded, plus the failed part with its own error and Retry |
| **Error** | The request failed | What happened in plain words, what to do (Retry, go back), the user's input kept, a reference ID for unexpected errors |
| **Offline** | The network is gone | A banner; cached data as read-only, or edits queued with a "will sync" status |
| **No access** | The user may not see or change this | Who can grant access, and a "Request access" button |
| **Success** | The action finished | What happened and the next step ("Project created. Invite members?") |
| **Near a limit** | Close to a plan or size limit | The limit and how to raise it, from 80% of the limit *(our rule)* |

- **MUST** build loading, empty, error and success states for every screen and widget that shows server data. *Why:* missing states show users blank screens and dead ends, and they are the states teams most often forget.
- **MUST** build a no-results state, separate from the first-use empty state, for every search and filter. *Why:* telling a user with 300 tasks that they "have no tasks yet" breaks trust in the filter.
- **MUST NOT** show an empty state ("No tasks") while the data is loading. *Why:* users believe their data is gone; Nielsen Norman Group lists it as the first empty-state mistake.
- Empty states always offer a next step: [shadcn.md §11](shadcn.md#11-feedback--loading-states).
- **MUST NOT** replace content the user is looking at with a full-page skeleton or spinner when they filter, sort, page or the data refetches. *Why:* the user loses their place and scroll position. How: [tanstack-query.md §9](tanstack-query.md#9-suspense--error-boundaries) (`startTransition`), [react.md §9](react.md#9-concurrency-and-ui-transitions).
- **MUST** keep failures local: a widget that fails shows its error and Retry in its own region, and the rest of the page keeps working. *Why:* one failed request shouldn't take down a whole dashboard. How: error boundaries per region ([react.md §10](react.md#10-error-handling), [nextjs.md §11](nextjs.md#11-error-handling)).
- **SHOULD** show "not found" instead of "no access" when the user isn't allowed to know the item exists. *Why:* "no access" confirms the item is there, which leaks information.
- **SHOULD** show a reference (the request ID) on unexpected errors, so support can find the logs ([logging.md §1.5](logging.md#15-correlation)).
- **SHOULD** cover the empty and error states in component or end-to-end tests, not only the happy path ([testing.md](testing.md)).

---

## 9. Forms and input

Form layout and wiring are in [shadcn.md §8](shadcn.md#8-forms); Server Actions, pending state and server-side validation are in [react.md §8](react.md#8-mutations-forms-and-actions). This section covers what users experience.

### 9.1 Structure

- **SHOULD** ask only for what is needed now. Remove fields you can infer, and defer the rest until they're needed. *Why:* every field is a chance to quit. Baymard's 2026 benchmark found the average US checkout shows 23.48 form elements where 12–14 would do.
- **SHOULD** lay out forms in one column, with fields in the order users think about them. Short pairs that belong together (city and postal code) may share a row. *Why:* users skip fields in multi-column layouts.
- **MUST NOT** use placeholder text as the label. *Why:* the placeholder disappears as soon as the user types, not all screen readers read it, and default placeholder styles fail WCAG contrast (GOV.UK Design System). Labels themselves are required by [react.md §13](react.md#13-accessibility).
- **SHOULD** put labels above fields; wide settings rows may put them on the left.
- **SHOULD** put format hints and examples between the label and the field, not in the placeholder.
- **SHOULD** mark both kinds of field: required fields with an asterisk explained once at the top of the form ("Fields marked * are required"), and optional fields with "(optional)" in the label. A form where every field is required needs neither. *Why:* Baymard found that when only optional fields were marked, 32% of test users missed a required field. GOV.UK forms mark only optional fields; that works for their one-question-per-page forms, not for long app forms.
- **SHOULD** size fields to their expected input (a postal code field is short, a description is wide). *Why:* the width hints at what to type.
- **MAY** let text areas grow with their content using `field-sizing: content` (Baseline since 2026-06), with a `max-height`.
- **SHOULD** split a long form into steps only when each step is a meaningful group, show progress ("Step 2 of 4"), and let users go back without losing what they entered. Forms people fill in often belong on one page.
- **MUST NOT** ask for information the user already gave in the same process; fill it in or let them pick it. *Why:* WCAG 3.3.7 Redundant Entry (A).

### 9.2 Help users type less

- **MUST** set the right `type`, `inputmode` and `autocomplete` on fields that collect information about the user, such as `autocomplete="email"`, `"name"`, `"tel"`, `"street-address"`, `"one-time-code"`, `"current-password"` and `"new-password"`. *Why:* WCAG 1.3.5 Identify Input Purpose (AA); it also brings up the right mobile keyboard and lets browsers and password managers fill the form.
- **MUST** allow pasting into every field, including passwords and verification codes. *Why:* WCAG 3.3.8 Accessible Authentication (AA); the W3C's Understanding document says a service that makes users retype a verification code by hand fails it.
- **MUST NOT** block password managers from filling login, sign-up and verification-code fields. *Why:* WCAG 3.3.8 again: password managers are what make login possible for people who can't memorize or retype a password.
- **SHOULD** collect verification codes with `type="text" inputmode="numeric" autocomplete="one-time-code"`, not `type="number"`. *Why:* number inputs drop leading zeros and show a spin button; this combination brings up the number pad and lets the browser fill the code from an SMS.
- **SHOULD** accept the formats people use and normalize them yourself: spaces and dashes in phone and card numbers, trailing spaces, upper or lower case in codes. *Why:* Postel's law; rejecting "+44 20 7946 0000" because of its spaces makes the user do the computer's job.
- **SHOULD** show password requirements before the user types, and offer a "Show password" toggle.
- **SHOULD** let users type a date as well as pick it from a calendar, and use plain text fields, not a calendar, for dates people know by heart, such as a date of birth. Times and time zones: [timezones.md §10](timezones.md#10-showing-times-to-people).

### 9.3 Validation timing

The rule of thumb is "reward early, punish late": confirm a fix at once, complain only when the user is done. The research agrees on "not while typing" but not on what comes after: Baymard recommends checking each field when the user leaves it, and the GOV.UK Design System checks only on submit. Either is fine; pick one per project and use it on every form.

- **MUST NOT** show an error while the user is still typing in a field for the first time. Check a field when the user leaves it, or when they submit. *Why:* an error after one character interrupts and accuses the user of a mistake they haven't finished making (NN/g: "avoid prematurely displaying errors").
- **SHOULD** re-check a field on every change once it is showing an error, and clear the error as soon as the value is valid. *Why:* users see at once that their fix worked.
- **MUST** show each field's error as text next to the field, not only as a red border. *Why:* color alone fails color-blind users and isn't announced by screen readers ([react.md §13](react.md#13-accessibility)). The attributes are in [shadcn.md §8](shadcn.md#8-forms).
- **MUST** show an error summary at the top of a form with more than 3 fields when a submit fails, listing every error as a link to its field, and move focus to the summary *(our rule for 3; GOV.UK shows it on every form)*. *Why:* users and screen-reader users otherwise don't learn that, or where, the submit failed (WCAG 3.3.1 Error Identification). On shorter forms, move focus to the first field with an error.
- **SHOULD** start the page `<title>` with "Error: " while a full-page form shows errors. *Why:* it's the first thing a screen reader announces after the page updates.
- Every check must also run on the server: [react.md §8](react.md#8-mutations-forms-and-actions).

```html
<!-- rendered by app/(app)/projects/new/project-form.tsx, above the fields, after a failed submit -->
<div role="alert" tabindex="-1" aria-labelledby="error-summary-title">
  <h2 id="error-summary-title">There is a problem</h2>
  <ul>
    <li><a href="#project-name">Enter a project name</a></li>
    <li><a href="#due-date">The due date must be today or later</a></li>
  </ul>
</div>
```

### 9.4 Error message wording

The wording rules and good and bad examples are in [§14](#14-content-and-microcopy). In short: say what's wrong and how to fix it, in the user's words, without blame.

### 9.5 Never lose the user's work

- **MUST** keep everything the user entered after a failed submit, including after a server error. Password and card security code fields are the exceptions. *Why:* retyping punishes the user for an error that may be ours, and it's the most common reason people give up on a form.
- In React 19, a `<form action>` resets its uncontrolled fields when the action finishes, **even when the action returns validation errors**. Return the submitted values in the error result and render them as `defaultValue`, or use a form library with controlled fields ([shadcn.md §8](shadcn.md#8-forms)).
- **MUST** warn before the user leaves a form or editor with unsaved changes (an in-app prompt for in-app navigation, `beforeunload` for closing the tab), or autosave the draft instead. *Why:* lost input can't be undone.
- **SHOULD** keep drafts of long text (comments, descriptions, AI prompts) in local storage until they are submitted. Never store passwords, payment data or other sensitive fields there.
- **MUST** warn users before a session timeout and give them at least 20 s to extend it with a simple action, unless they can turn the limit off or it is longer than 20 hours. *Why:* WCAG 2.2.1 Timing Adjustable (A).
- **MUST** keep the user's unsaved input across a forced re-login. *Why:* WCAG 2.2.5 asks for it at AAA, and losing a long form to an expired session is the worst version of losing work. *(our rule: AAA treated as required for this one)*

---

## 10. Errors, confirmation and undo

### 10.1 Prevent errors first

- **SHOULD** prevent invalid input with constraints instead of reporting it afterwards: a date picker that only offers allowed dates, a select where only fixed values are valid, a character counter that shows the limit.
- **SHOULD** state the consequence next to an action before it runs ("Archiving hides this project for all 12 members").

### 10.2 Undo or confirm?

| Action | Can it be reversed? | Pattern |
| --- | --- | --- |
| Archive, move, mark done, remove from a list, reorder | Yes | Do it at once; toast with Undo; restorable later from an Archive view |
| Delete with a trash (soft delete) | Yes, for N days | Do it at once; toast with Undo; "Restore from Trash within 30 days" |
| Delete permanently, send an email or invite, publish, pay | No | Confirmation dialog that names the object and the consequence |
| High-impact and irreversible: delete a project, workspace or account; bulk-delete more than 100 items; change to a paid plan | No | Confirmation that requires typing the object's name, or re-entering the password or passkey; list what will be lost |

- **SHOULD** make destructive actions reversible (soft delete plus Undo) rather than asking for confirmation. *Why:* users click through confirmations out of habit; undo still works after the mistake.
- **MUST** ask for confirmation before any action that can't be undone and that destroys data, spends money, or sends something to other people. *Why:* WCAG 3.3.4 Error Prevention (AA) requires a way to check, confirm or reverse such submissions, and these are the mistakes users can't fix.
- **MUST** name the object and the consequence in the confirmation, and label its confirm button with the verb ("Delete 3 tasks"), not "OK" or "Yes". *Why:* users can't judge "Are you sure?", and a specific button stops autopilot clicks.
- **SHOULD NOT** ask for confirmation for routine actions that can be undone. *Why:* needless confirmations train users to click "Yes" without reading, which makes the important confirmations useless.
- **SHOULD** require typing the object's name to confirm deleting a container (project, workspace, account) or bulk-deleting more than 100 items *(our rule)*. Keep it for these rare, most dangerous cases; NN/g warns that "if you cry wolf too many times, people will stop paying attention."
- **SHOULD** put initial focus on the least destructive button (Cancel) in a confirmation dialog for a destructive action, and never make the dangerous option the default. *Why:* an accidental Enter then does no harm (WAI-ARIA APG modal dialog pattern).

### 10.3 Error messages and error pages

- **MUST** tell users what happened and what they can do next, in plain words, without blaming them. *Why:* heuristic 9; a message like "Error 500" leaves the user stuck.
- **MUST NOT** show stack traces, SQL errors, internal codes or exception names to users. Show a reference ID instead ([§8](#8-screen-states)). *Why:* they don't help the user and they leak internals to attackers.
- **MUST** give every error page (not found, server error, no access, offline) a way forward: a link back or home, search, or a way to contact support. *Why:* an error page without links is a dead end. Next.js pages: [nextjs.md §11](nextjs.md#11-error-handling).
- **SHOULD** offer Retry for failures that may be temporary (network errors, timeouts, 503), and retry background syncs on their own with backoff ([design-patterns.md §16](design-patterns.md#16-timeouts-retries-and-stability)).

Delete with undo, in place of a confirmation dialog:

```tsx
// app/(app)/tasks/delete-task-button.tsx
'use client';

import { toast } from 'sonner'; // with Base UI, use the base's toast instead (shadcn.md §11)
import { Button } from '@/components/ui/button';
import { deleteTask, restoreTask } from './actions'; // soft delete and restore

export function DeleteTaskButton({ taskId, title }: { taskId: string; title: string }) {
  async function handleDelete() {
    const result = await deleteTask(taskId);
    if (result.status === 'error') {
      toast.error(`Couldn't delete "${title}". ${result.message}`);
      return;
    }
    toast.success(`Deleted "${title}".`, {
      duration: 10_000, // actions stay at least 10 s (§7.2)
      action: { label: 'Undo', onClick: () => restoreTask(taskId) },
    });
  }

  return (
    <Button variant="destructive" onClick={handleDelete}>
      Delete task
    </Button>
  );
}
```

---

## 11. Tables, lists, filters and bulk actions

The shadcn Data Table setup is in [shadcn.md §12](shadcn.md#12-common-app-components).

### 11.1 Tables

- **SHOULD** use a table when users compare items across 3 or more attributes, and a list or cards when they browse items or work on a narrow screen.
- **SHOULD** put the identifier users read (the task or project name) in the first column.
- **SHOULD** right-align numbers, use tabular figures (`tabular-nums`) in numeric columns, left-align text, and align each header with its column's data. *Why:* digits then line up by place value, so users can compare sizes at a glance.
- **SHOULD** keep the header row sticky while the table scrolls, and the first column sticky in wide tables.
- **MUST** show which column is sorted and in which direction, with an icon and `aria-sort` on the header cell. *Why:* users can't tell the order otherwise, and screen readers rely on `aria-sort`.
- **SHOULD** let users show, hide and reorder columns, and switch density, in tables with more than 6 columns *(our rule)*. Save the choice per user and offer "Reset to default".
- **SHOULD** show the full value of truncated text on hover and on focus, or let the cell wrap or expand. Never truncate the identifier users scan for.
- **SHOULD** make the item's name the link that opens it. Make the whole row clickable only when the row holds no other controls. *Why:* nested click targets cause accidental navigation, and screen readers need a real link.
- **SHOULD** let wide tables scroll sideways inside their own container on narrow screens, or switch to a stacked list.
- **SHOULD** paginate or virtualize lists longer than 500 rows *(our rule)*.

### 11.2 Pagination, "Load more" and infinite scroll

| Content | Pattern |
| --- | --- |
| Data users search, compare or come back to (tasks, invoices, admin tables) | Pagination, or a "Load more" button, with the position in the URL |
| A feed people browse without a goal (activity feed) | Infinite scroll is fine, if no footer content is needed |
| Search results | Pagination or "Load more" |

- **SHOULD NOT** use infinite scroll on pages with a footer or content after the list. *Why:* users can never reach it.
- **MUST** bring users back to the same position in a list (page, cursor and scroll) when they go back from an item. *Why:* losing your place in a long list after every look at an item makes the list unusable. Keep the position in the URL ([§3.1](#31-put-view-state-in-the-url)).
- **SHOULD** show the total when it's known ("1–50 of 1,284 tasks").

### 11.3 Filters and sorting

- **SHOULD** show active filters as removable chips above the results, with "Clear all".
- **SHOULD** apply a filter as soon as it changes when results come back in under 1 s, and use an "Apply" button when each change is slow, or inside a filter panel on small screens.
- **SHOULD** let users save a set of filters as a named view when the same filter sets come up again and again.
- Filters, sort and search go in the URL: [§3.1](#31-put-view-state-in-the-url).

### 11.4 Selection and bulk actions

- **SHOULD** show a bulk action bar as soon as one item is selected, with the count ("3 selected"), the actions, and "Clear selection".
- **MUST** make the scope of "select all" clear when results span more than one page: "All 50 tasks on this page are selected. Select all 1,284 tasks?" *Why:* a bulk delete that silently includes, or silently skips, items the user can't see is a data-loss bug.
- **SHOULD** support Shift-click to select a range.
- **MUST** report partial failures of a bulk action, with a way to see which items failed ("48 tasks archived. 2 couldn't be archived. View"). *Why:* a silent partial failure leaves the data in a state the user doesn't know about.
- The undo and confirmation rules in [§10](#10-errors-confirmation-and-undo) apply to bulk actions too.

### 11.5 Inline editing

- **SHOULD** allow inline editing for single fields users change often (status, assignee, due date): Enter or leaving the field saves, Esc cancels, and the field shows its saving state.
- **SHOULD** edit a whole row in a non-modal side panel rather than a modal dialog. *Why:* users can still see and compare the other rows while they edit (NN/g data tables guidance).

---

## 12. Search, command palette and keyboard shortcuts

A feature-rich app stays easy to use when users can **find** a feature instead of having to know where it lives. Search and a command palette make every feature one search away without adding anything to the screen.

### 12.1 Search

- **SHOULD** offer global search from every page in apps where users look for items by name. Show a search field, not only an icon, on wide screens.
- **SHOULD** match partial words and small typos, search the fields users remember (name, ID, assignee), and highlight the matches in the results.
- **SHOULD** show recent items and recent searches while the search box is empty.
- **SHOULD** scope search to the current context first, with a one-click way to search everywhere ("Search in Website · Search all projects").
- **SHOULD** wait about 150–300 ms after the last keystroke before searching as the user types *(our rule)*, and keep the previous results visible while new ones load ([react.md §9](react.md#9-concurrency-and-ui-transitions)).
- A search with no results shows the no-results state in [§8](#8-screen-states).

### 12.2 Command palette

- **SHOULD** add a command palette, opened with Cmd+K on macOS and Ctrl+K elsewhere, to apps with more than 20 actions *(our rule)*. It searches pages, items and actions in one list.
- **SHOULD** include every action that is in a menu, show each action's keyboard shortcut next to it, and rank recent and context-relevant actions first. *Why:* users discover deep features by searching for them, and learn the shortcuts on the way.
- **MUST** build the palette on an accessible combobox inside a dialog (shadcn's `Command` in a `Dialog`). *Why:* the palette is keyboard-first, so it must follow the APG combobox pattern for screen readers.
- The palette is an accelerator, never the only way to an action: [§2.1](#21-one-job-per-screen).

### 12.3 Keyboard shortcuts

- **MUST NOT** add single-character shortcuts (a letter, number or punctuation key without Ctrl, Cmd or Alt) unless users can turn them off or remap them, or the shortcut works only while a specific component has focus. *Why:* WCAG 2.1.4 Character Key Shortcuts (A); speech-input users trigger them by accident while dictating.
- **MUST NOT** fire shortcuts while focus is in a text field, text area or editable content, except shortcuts with Ctrl or Cmd (Cmd+Enter to send). *Why:* typing "c" in a comment would otherwise create a task.
- **MUST NOT** override the browser and OS shortcuts users rely on: copy, cut, paste, select all and undo inside text fields; new tab, close tab, address bar and reload; Tab and Shift+Tab. *Why:* users expect them to work on every site, and some assistive technology depends on them.
- **SHOULD** make shortcuts discoverable where users already look: right-aligned next to menu items, in tooltips, and next to commands in the palette. *Why:* users learn shortcuts from the menus they already use; hover-only hints miss keyboard and touch users.
- **MAY** add a list of all shortcuts (opened with `?`, which can be turned off per the first rule). NN/g calls cheat sheets a last resort, so it doesn't replace showing shortcuts in menus.
- **SHOULD** give custom shortcuts only to the most frequent app-specific commands (Apple HIG), and show the platform's modifier key: ⌘ on macOS, Ctrl elsewhere.
- **SHOULD** follow common conventions: Esc closes, Cmd/Ctrl+Enter submits multi-line input, Cmd/Ctrl+K opens the palette *(our rule; a widespread convention that no standard defines)*, `/` focuses search.

---

## 13. Onboarding, discoverability and help

- **SHOULD** let users reach the product's value before asking for anything that isn't needed yet (profile details, invites, integrations).
- **SHOULD** use first-use empty states as onboarding: what the area is for, the action that fills it, and a template, sample data or import ([§8](#8-screen-states)).
- **SHOULD NOT** open with a multi-step product tour or a carousel of tips on first login. *Why:* users skip them to get to their task and remember little of what they saw. Show a hint at the moment it's useful instead.
- **MUST** make every tour, hint, banner and checklist dismissible. *Why:* a hint that can't be closed blocks the screen it's meant to explain.
- **MUST NOT** show a hint, tour or announcement again after the user dismissed it. *Why:* repeating it is nagging ([§19](#19-ethical-design-and-dark-patterns)).
- **SHOULD** show at most one contextual hint per screen at a time *(our rule)*.
- **SHOULD** use a short setup checklist (3–5 items) with visible progress for products that need setup, and let users hide it.
- **SHOULD** announce new features with a "What's new" entry and a small "New" badge next to the feature for 2–4 weeks *(our rule)*, not with a modal.
- **SHOULD** put help where questions come up: a description under a field, a "Learn more" link next to a complex setting.
- **MUST** keep help (contact details, chat, a help center link) in the same relative place on every page that has it. *Why:* WCAG 3.2.6 Consistent Help (A).
- **SHOULD** mark sample data clearly and let users remove all of it in one action.

---

## 14. Content and microcopy

- **SHOULD** write in plain language: short sentences, common words, active voice, and "you" for the user.
- **SHOULD** use sentence case for all UI text: buttons, headings, menu items and labels.
- **SHOULD** put the key word first in labels and messages ("Archive project", not "Click here to archive this project").
- **SHOULD** use one term for one concept across the UI, docs and emails. Keep a glossary in the repo once the product has more than 20 domain terms *(our rule)*. *Why:* users assume two different words mean two different things.
- **SHOULD** label buttons with a verb and an object ("Create task", "Invite member"), not "OK", "Submit" or "Yes".
- **SHOULD** match the confirm button to the dialog title: "Delete task?" pairs with "Delete task".
- **SHOULD** write link text that says where the link goes, without "click here" or a bare "Read more". *Why:* screen-reader users often browse a list of links out of context (WCAG 2.4.4 Link Purpose).
- **SHOULD** format numbers, currencies and lists for the user's locale (`Intl.NumberFormat`, `Intl.ListFormat`). Dates and times: [timezones.md §10](timezones.md#10-showing-times-to-people).
- **SHOULD** use real plurals ("1 task", "2 tasks") with `Intl.PluralRules` or the project's i18n library, never "1 task(s)".
- **SHOULD** write the copy for empty, error and confirmation states as part of the design, not as placeholder text filled in at the end.
- **SHOULD NOT** use "please", "sorry", "oops", "invalid", "illegal" or "you forgot" in error messages (GOV.UK Design System). *Why:* they add words without helping, and some of them blame the user.

| ❌ Instead of | ✅ Write | Why |
| --- | --- | --- |
| "Invalid input" | "Enter a due date like 31/12/2026" | Says how to fix it |
| "This field is required" | "Enter your email address" | Says which information is missing |
| "Error 403" | "You don't have permission to edit this project. Ask a project admin for access." | What happened and the next step |
| "Something went wrong" (alone) | "Couldn't save your changes. Check your connection and try again. Your changes are kept." | Cause, action and reassurance |
| "You failed to enter a name" | "Enter a project name" | No blame |
| "Are you sure?" with OK and Cancel | "Delete 'Website redesign'? This deletes its 48 tasks and can't be undone." with "Delete project" and "Cancel" | Names the object, the consequence and the verb |
| "Success!" | "Invitation sent to ana@example.com" | Says what happened |
| "Oops! Our hamsters tripped 🙈" | "We couldn't load your tasks. Try again in a minute." | Jokes in errors feel dismissive when work is at stake |

---

## 15. Accessibility

Accessibility is part of usability: clear labels, keyboard paths, good contrast and forgiving forms make a feature-rich app easier for everyone, not only for disabled users. The implementation rules live in [react.md §13](react.md#13-accessibility), [tailwind.md §11](tailwind.md#11-accessibility) and [shadcn.md §10](shadcn.md#10-accessibility). This section sets the target, lists the legal baselines, and owns the WCAG 2.2 criteria the other files don't cover.

### 15.1 Target and legal baseline

- **MUST** meet WCAG 2.2 Level AA on every screen. *Why:* it includes everything in WCAG 2.1 AA, so one target covers the EU and US rules below; it is also an ISO standard (ISO/IEC 40500:2025); and it's what users with disabilities need to use the product at all.
- **SHOULD NOT** target the WCAG 3.0 draft in place of WCAG 2.2. As of 2026-11 the newest version is the W3C Working Draft of 2026-09-10, and the W3C says WCAG 3 doesn't deprecate WCAG 2.

| Where | Law | Technical standard (as of 2026-11) |
| --- | --- | --- |
| EU | European Accessibility Act (EAA), applies since 2025-06-28 to consumer services such as e-commerce, banking, e-books, transport tickets and electronic communications | EN 301 549. Version 4.1.1 (published 2026-09-02) moves web content to WCAG 2.2 A and AA. Until the EU cites it in the Official Journal, v3.2.1 (WCAG 2.1) is the cited version |
| US, state and local government | ADA Title II rule (2024) | WCAG 2.1 AA. A DOJ interim rule of 2026-04-20 moved the first deadline to 2027-04-26 for entities serving 50,000 people or more |
| US, businesses | ADA Title III (through court cases) | Courts and settlements use WCAG 2.x AA |

- **SHOULD** publish an accessibility statement that lists known issues and a way to report problems. *Why:* the EAA requires services to explain how they meet the accessibility requirements, and users need to know where to report barriers.

### 15.2 WCAG 2.2 criteria owned by this file

| Criterion | Level | What it means for us | Rule |
| --- | --- | --- | --- |
| 1.3.4 Orientation | AA | No orientation lock | [§5](#5-responsive-mobile-and-touch) |
| 1.3.5 Identify Input Purpose | AA | `autocomplete` on personal-data fields | [§9.2](#92-help-users-type-less) |
| 1.4.4 Resize Text, 1.4.10 Reflow, 1.4.12 Text Spacing | AA | Works at 200% zoom, 320 px wide and with larger text spacing | [§4.3](#43-zoom-reflow-and-text-spacing) |
| 1.4.13 Content on Hover or Focus | AA | Tooltips and popovers are dismissible, hoverable, persistent | [§6.3](#63-overlays-dialogs-sheets-popovers-and-tooltips) |
| 2.1.4 Character Key Shortcuts | A | Single-key shortcuts can be turned off | [§12.3](#123-keyboard-shortcuts) |
| 2.2.1 Timing Adjustable | A | Timeouts warn and extend; action toasts stay | [§7.2](#72-choose-the-right-feedback-channel), [§9.5](#95-never-lose-the-users-work) |
| 2.2.2 Pause, Stop, Hide | A | Moving content can be paused | [§16](#16-motion-and-animation) |
| 2.3.1 Three Flashes or Below Threshold | A | No flashing | [§16](#16-motion-and-animation) |
| 2.4.1 Bypass Blocks, 2.4.2 Page Titled | A | Skip link, landmarks, unique titles | below |
| 2.4.11 Focus Not Obscured (Minimum) | AA | Sticky bars never fully hide the focused element | below |
| 2.5.1 Pointer Gestures, 2.5.7 Dragging Movements | A, AA | Single-pointer alternatives to gestures and drags | [§5](#5-responsive-mobile-and-touch), [§6.4](#64-drag-and-drop-and-gestures) |
| 2.5.8 Target Size (Minimum) | AA | At least 24 × 24 px | [tailwind.md §11](tailwind.md#11-accessibility) |
| 3.2.6 Consistent Help | A | Help in the same place on every page | [§13](#13-onboarding-discoverability-and-help) |
| 3.3.1 Error Identification, 3.3.3 Error Suggestion | A, AA | Errors in text, with how to fix them | [§9.3](#93-validation-timing), [§14](#14-content-and-microcopy) |
| 3.3.4 Error Prevention (Legal, Financial, Data) | AA | Confirm, check or reverse important submissions | [§10.2](#102-undo-or-confirm) |
| 3.3.7 Redundant Entry | A | Don't ask twice | [§9.1](#91-structure) |
| 3.3.8 Accessible Authentication (Minimum) | AA | Paste and password managers allowed; no puzzles to log in | [§9.2](#92-help-users-type-less), below |

- **MUST** give every page a unique `<title>` that starts with the most specific part ("Website redesign · Projects · AppName"), and update it on client-side navigation. *Why:* WCAG 2.4.2 Page Titled (A); it's the first thing a screen reader announces and what tabs and history show. Next.js sets it through metadata ([nextjs.md §13](nextjs.md#13-metadata-and-seo)).
- **MUST** make "Skip to main content" the first focusable element on pages with repeated navigation, and mark the page regions with landmarks (`header`, `nav`, `main`, `footer`), with one `main` per page. *Why:* WCAG 2.4.1 Bypass Blocks (A); keyboard users otherwise tab through the whole navigation on every page.
- **MUST** keep the focused element at least partly visible when sticky headers, footers, cookie banners or chat widgets are on screen. Set `scroll-padding-top` and `scroll-padding-bottom` to the height of sticky bars. *Why:* WCAG 2.4.11 Focus Not Obscured (AA).
- **MUST NOT** make users solve a puzzle, remember a password or transcribe a code to log in, unless a way around it exists: password managers and paste ([§9.2](#92-help-users-type-less)), passkeys, or an email link. *Why:* WCAG 3.3.8 Accessible Authentication (AA).

### 15.3 No overlays, and test with people

- **MUST NOT** add accessibility overlay widgets: toolbars or scripts that claim to make a site accessible or compliant automatically. *Why:* they don't fix the underlying code and often get in the way of the screen readers disabled users already have. In 2025 the US FTC ordered accessiBe to pay $1 million (final order 2025-04-22) because its overlay did not make sites WCAG compliant as advertised, and more than 1,000 accessibility professionals have signed the Overlay Fact Sheet against overlays.
- Automated checks find only part of the problems ([tailwind.md §14](tailwind.md#14-testing)). Before release, a person checks the critical flows *(human)*:
  - **Keyboard only:** every task with Tab, Shift+Tab, Enter, Space, Esc and arrow keys; focus always visible and in a logical order; no traps.
  - **Screen reader:** VoiceOver with Safari, and NVDA with Firefox or Chrome ([react.md §13](react.md#13-accessibility)).
  - **Zoom and spacing:** 200% and 400% zoom, and a text-spacing bookmarklet.
  - **Preferences:** reduced motion, dark mode and forced colors ([tailwind.md §14](tailwind.md#14-testing)).

---

## 16. Motion and animation

The reduced-motion utilities are in [tailwind.md §11](tailwind.md#11-accessibility); which properties to animate is in [tailwind.md §12](tailwind.md#12-performance).

- **SHOULD** animate only to explain: where something came from or went (open, close, move, expand), that a state changed, or that something needs attention. Not for decoration.
- **SHOULD** keep UI transitions short: 100–150 ms for small changes (hover, toggles), 200–300 ms for panels and dialogs, at most 500 ms for full-screen changes. These sit inside Material Design 3's duration tokens (short 50–200 ms, medium 250–400 ms, long 450–600 ms).
- **SHOULD** ease out (decelerate) elements that enter, ease in (accelerate) elements that leave, and use a standard curve for elements that move on screen. Material 3's values: standard `cubic-bezier(0.2, 0, 0, 1)`, decelerate `cubic-bezier(0, 0, 0, 1)`, accelerate `cubic-bezier(0.3, 0, 1, 1)`. Put the chosen curves and durations in design tokens ([tailwind.md §2](tailwind.md#2-design-tokens-and-the-theme)).
- **MUST NOT** make users wait for an animation to finish before they can act. *Why:* motion that delays input makes the whole app feel slow.
- **MUST NOT** flash anything more than 3 times in any one second. *Why:* WCAG 2.3.1 (A); flashing can trigger seizures.
- **MUST** give content that moves, scrolls or auto-updates on its own for more than 5 s (carousels, tickers, animated illustrations) a control to pause or stop it. *Why:* WCAG 2.2.2 Pause, Stop, Hide (A).
- **SHOULD NOT** auto-advance carousels at all *(our rule)*. *Why:* users can't read slides that move on their own, and they rarely look past the first one.
- **SHOULD** replace movement with a short fade when the user prefers reduced motion, rather than removing the transition. *Why:* the change still needs to be visible; only the movement causes discomfort.
- **SHOULD NOT** use parallax, scroll-jacking or large zoom effects. *Why:* they cause motion sickness for people with vestibular disorders.
- **MAY** use view transitions for continuity between views, as an enhancement: the app must work the same without them. Same-document view transitions are Baseline since 2025-10; cross-document ones (`@view-transition`) are missing from Firefox as of 2026-11.

---

## 17. Performance as UX

Speed is part of usability: a slow or jumpy app feels hard to use even when its design is good. Measuring is in [nextjs.md §12](nextjs.md#12-performance-and-assets).

- **MUST** meet the Core Web Vitals "good" thresholds at the 75th percentile of real users, for mobile and desktop separately, on the key screens: Largest Contentful Paint (LCP) ≤ 2.5 s, Interaction to Next Paint (INP) ≤ 200 ms, Cumulative Layout Shift (CLS) ≤ 0.1. *Why:* past these limits users see the app as slow or jumpy, and Google uses them in search ranking for public pages. As of 2026-11 these are the thresholds web.dev publishes; blog posts claiming 150 ms INP or 2.0 s LCP have no Google source.
- **SHOULD** treat INP as the main metric for screens behind login, where most interactions happen after the page has loaded.
- **SHOULD** keep event handlers short: paint the feedback first, then do the work, and split JavaScript tasks longer than 50 ms. In React, mark slow updates as transitions ([react.md §9](react.md#9-concurrency-and-ui-transitions)).
- **SHOULD** make the app feel fast: render the shell and navigation at once, stream content in ([react.md §7.3](react.md#73-streaming-with-suspense)), prefetch the likely next view, and update optimistically ([§7.1](#71-response-times)).
- **SHOULD** test key flows on a throttled "Slow 4G" connection and a mid-range Android phone before release *(human)*.

---

## 18. AI features

AI features (chat, generated text or images, summaries, agents that take actions) behave differently from the rest of the UI: the output changes from run to run, it can be wrong, and it takes seconds. Users need to know what the AI is, what it can do, how far to trust it, and how to stay in control. These rules draw on Microsoft's 18 Guidelines for Human-AI Interaction (HAX), Google's People + AI Guidebook, Apple's Human Interface Guidelines and the EU AI Act.

### 18.1 Disclosure and expectations

- **MUST** tell users when they interact with an AI system (a chat assistant, an AI that writes messages or comments for them), clearly and at the latest at the first interaction, unless a reasonably well-informed user would find it obvious. *Why:* EU AI Act Article 50(1) and 50(5) require it since 2026-08-02, and the Commission reads the "obvious" exception narrowly. Users also trust the output more appropriately when they know where it came from.
- **SHOULD** mark AI-generated content in the product ("Drafted with AI" on a generated task description) until a person edits or accepts it *(our rule)*. Apple's guidance is the same: never let people believe AI-generated content was made by a person. The EU publishes optional icons for labelling AI content.
- **SHOULD** ask legal whether Article 50(2) applies when the feature generates images, audio, video or text that leaves the product. It requires machine-readable marking of generated content; systems on the market before 2026-08-02 have until 2026-12-02 *(human)*.
- **SHOULD** say what the feature can and can't do where it's used: example prompts, its scope ("Searches tasks in this project only"), and known limits (HAX guidelines 1 and 2).
- **SHOULD NOT** give the AI a human name, a face, or claims of feelings that could make users think they're talking to a person.
- **SHOULD** keep a non-AI way to do the same task where one is possible (Apple HIG). *Why:* the AI will sometimes fail or be switched off, and some users don't want it.
- **MUST** tell users, before their first use, what data the AI feature sends and to whom (for example, a third-party model provider), and whether it's used for training. *Why:* GDPR Articles 13 and 14 require it, and users decide what to paste based on it.

### 18.2 Input

- **SHOULD** show starter prompts that fit the current context when the prompt box is empty.
- **SHOULD** offer buttons for common intents ("Summarize", "Draft reply", "Make shorter") instead of making users write prompts for them.
- **MUST** keep the user's prompt when a request fails. *Why:* long prompts are work, and losing them is the same failure as losing a form ([§9.5](#95-never-lose-the-users-work)).

### 18.3 Output and waiting

- **SHOULD** stream text as it's generated. For work that can't stream, show specific steps ("Reading 12 tasks… Writing summary…"), not a generic "Processing…" ([§7.1](#71-response-times)).
- **MUST** show a Stop button while the AI is generating, and keep the partial output when the user stops it. *Why:* generation can take tens of seconds and go wrong early; stopping must be quick (HAX guideline 8).
- **SHOULD** auto-scroll new output only while the user is at the bottom of the conversation. *Why:* output that pulls the view away makes the earlier text impossible to read.
- **MUST** announce AI output to screen readers once it finishes (or in complete sentences), not token by token. *Why:* a live region that updates on every token produces an unusable stream of fragments. As of 2026-11 no W3C guidance covers streaming text directly; this follows WCAG 4.1.3 Status Messages and WAI-ARIA's `aria-busy`.
- **SHOULD** render the conversation in a `role="log"` container (W3C technique ARIA23), set `aria-busy="true"` on the response while it streams and `false` when it's done, and announce completion with a status message (the example below shows the status part).
- **MUST** sanitize model output before rendering it as HTML or Markdown. *Why:* model output is untrusted input; prompt injection can make it contain scripts or misleading links ([react.md §14.1](react.md#141-rendering)).
- **SHOULD** let users copy, edit, regenerate and rate each response, and keep earlier versions when they regenerate.

```tsx
// app/(app)/assistant/response-status.tsx
'use client';

import { Button } from '@/components/ui/button';

type Status = 'idle' | 'streaming' | 'done' | 'stopped' | 'error';

export function ResponseStatus({ status, onStop }: { status: Status; onStop: () => void }) {
  return (
    <>
      {status === 'streaming' && (
        <Button variant="outline" onClick={onStop}>
          Stop generating
        </Button>
      )}
      {/* Always rendered, so the live region exists before it changes. Announces once per response. */}
      <p role="status" className="sr-only">
        {status === 'done' && 'Response ready.'}
        {status === 'stopped' && 'Stopped. The partial response is kept.'}
        {status === 'error' && "Couldn't finish the response. Your prompt is kept."}
      </p>
    </>
  );
}
```

### 18.4 Trust: sources and uncertainty

- **SHOULD** link factual answers to their sources (the tasks, documents or pages used), so users can check them (HAX guideline 11).
- **SHOULD** express uncertainty in words, or by saying what wasn't found ("I couldn't find tasks due this week"), rather than with confidence percentages users can't interpret.
- **SHOULD** show a short, permanent note near AI output that it can be wrong, rather than a disclaimer dialog users click away.

### 18.5 Agent actions

- **MUST** get the user's explicit approval before an AI agent does anything that can't be undone, costs money, deletes data, or reaches other people (emails, comments, invites). Show exactly what will happen first ("Create 5 tasks in Website", "Email 3 people", with a preview or diff). *Why:* model mistakes and prompt injection turn into real damage the moment the agent acts; Apple's guidelines and HAX guideline 16 ("Convey the consequences of user actions") ask for the same.
- **SHOULD** let users edit a proposed action before approving it, and approve or reject the items of a batch one by one.
- **MUST** make changes made by an AI as undoable as the user's own ([§10](#10-errors-confirmation-and-undo)). *Why:* users must be able to reverse what the agent got wrong.
- **MUST** record AI-made changes in the item's history as made by the AI on the user's behalf ("Created by AI assistant for Ana"). *Why:* users and admins need to find what the agent changed.
- **SHOULD** give global controls: turn AI features off, opt out of training, and clear the AI's memory (HAX guideline 17).

### 18.6 Feedback

- **SHOULD** collect feedback on responses (thumbs up or down, with an optional reason) and say what happens with it (HAX guideline 15).
- **SHOULD** tell users when the AI's behavior changes, such as a new model or new abilities (HAX guideline 18).

---

## 19. Ethical design and dark patterns

A dark pattern is a design that gets users to do something they didn't mean to (buy, subscribe, share data), or that makes it hard to do what they want (cancel, opt out). Many are illegal, and all of them cost trust.

**Legal summary (as of 2026-11, not legal advice):**

| Law or case | Where | What it covers |
| --- | --- | --- |
| GDPR, and the CJEU's *Planet49* ruling (2019-10-01) | EU | Consent must be an active choice: a pre-ticked box isn't consent (also GDPR recital 32) |
| EDPB cookie banner taskforce report (2023-01-17) and national regulators such as the CNIL | EU | Refusing must be as easy as accepting. Most regulators treat a layer with "Accept" but no "Reject" as an infringement, and judge misleading button colors case by case |
| Digital Services Act Article 25 | EU, online platforms | Bans interfaces that deceive or manipulate users or impair their free choice. The Commission may issue guidelines on three practices: giving one choice more prominence, asking again for a choice already made, and making cancelling harder than signing up |
| Unfair Commercial Practices Directive (UCPD) | EU | Bans misleading and aggressive practices; Annex I bans false "limited time" claims outright |
| Digital Fairness Act | EU | A proposal expected to add a general ban on dark patterns and rules for subscriptions. Not published as of 2026-11-09; the European Parliament's research service expected it in Q4 2026 |
| FTC Act Section 5 and ROSCA | US | Deceptive sign-up and hard cancellation. In 2025-09 Amazon agreed to pay $2.5 billion ($1 billion penalty, $1.5 billion refunds) over Prime sign-up without clear consent and a deliberately complicated cancellation flow |
| FTC "click-to-cancel" rule | US | Vacated by the Eighth Circuit on 2025-07-08. The FTC published an advance notice of new rulemaking on 2026-03-13; no new rule as of 2026-11 |
| California Automatic Renewal Law, amended by AB 2863 | California, contracts from 2025-07-01 | Cancellation in the same medium as sign-up; express consent; an annual reminder; an online save offer must show a "click to cancel" button the whole time |
| Digital Markets, Competition and Consumers Act 2024 | UK, subscription rules expected from 2027-01 | Draft regulations on easy exit, reminders and pre-contract information |

- **MUST NOT** pre-select options that sign users up for marketing, share their data, add paid extras, or start a subscription. *Why:* a pre-ticked box isn't valid consent under GDPR (*Planet49*), and it adds charges users didn't choose.
- **MUST** make rejecting as easy as accepting in consent dialogs: "Reject all" on the same layer as "Accept all", with the same size and visual weight. *Why:* EU regulators hold that refusing must be as easy as accepting (EDPB taskforce, CNIL), and an unequal choice makes the consent invalid.
- **MUST** let users cancel a subscription or delete their account online, in the same medium they signed up in, without more steps than signing up took *(our rule for the step count)*. *Why:* the California Automatic Renewal Law requires cancellation in the same medium, the FTC's Amazon order requires it too, and making cancelling harder than signing up is one of the practices DSA Article 25 targets.
- **MAY** show one retention offer during cancellation, once, with a visible "Continue cancelling" button next to it the whole time. California requires that button, titled "click to cancel", alongside any online save offer.
- **MUST** show the total price, including mandatory fees and taxes where known, before the user commits to paying. *Why:* hidden costs added at the last step are a misleading practice under EU and US consumer law.
- **MUST** say, before collecting payment details for a free trial, when the trial turns into a paid plan, what it will cost, and how to cancel. *Why:* hidden subscriptions are what ROSCA and the California law target.
- **SHOULD** remind users before a trial ends or an annual plan renews, at least 7 days ahead *(our rule for 7 days; California requires an annual reminder)*.
- **MUST NOT** use fake urgency or scarcity (countdown timers that reset, "only 2 left" when it isn't true) or fake social proof. *Why:* false limited-time claims are banned outright in the EU (UCPD Annex I) and are deceptive under the FTC Act.
- **MUST NOT** use confirmshaming ("No thanks, I don't like saving money") or trick wording such as double negatives in opt-outs. *Why:* both manipulate the choice, which DSA Article 25 bans for platforms and the UCPD covers for everyone; the FTC's Amazon order banned exactly this kind of decline button ("No, I don't want Free Shipping").
- **MUST NOT** disguise ads or sponsored content as product content or navigation. *Why:* hidden advertising is a misleading practice under the UCPD and FTC rules.
- **MUST NOT** ask again for a choice the user already made (a declined upsell, permission or rating request) in the same session, or more than once every 30 days *(our rule for 30 days)*. *Why:* DSA Article 25 names repeated requests for a choice already made as a practice to act against, and nagging is one of the deceptive pattern types users recognize and resent.
- **SHOULD NOT** force unrelated actions to finish a task, such as signing up to see a price or sharing contacts to continue.
- **SHOULD** use the 18 types on [deceptive.design](https://www.deceptive.design/types) as the review list (for example comparison prevention, currency confusion, hidden costs, obstruction, preselection, sneaking and visual interference).
- **SHOULD** check every flow that touches money, consent, sign-up or cancellation against this section in design review, and with legal before release *(human)*.

---

## 20. UX research, testing and metrics

Deciding **what** to build is in the [feature-engineering guide](../guides/feature-engineering.md) (problem interviews in §4). This section covers checking that what you built is easy to use.

### 20.1 Review before merging

- **SHOULD** review every new screen or flow against the 10 heuristics in [§1](#1-ux-principles) and this file's [PR checklist](#22-pr-checklist) before merging. An agent can do this first pass. For large features, 3–5 reviewers should each review alone, then combine their findings (NN/g heuristic evaluation). *Why:* one reviewer finds only a fraction of the problems; a few independent reviewers find most of them.
- **SHOULD** compare the built screen with its design at 375 px and 1280 px wide, in light and dark mode, in each state from [§8](#8-screen-states) *(human)*. Screenshot tests: [tailwind.md §14](tailwind.md#14-testing).

### 20.2 Usability tests *(human)*

- **SHOULD** test each major new flow with about 5 users from the target group per round, fix what they hit, and test again. *Why:* NN/g found that 5 users find about 85% of a design's usability problems, and that three rounds of 5 beat one round of 15. With distinct user groups, use 3–4 users per group.
- **SHOULD** give participants realistic tasks ("Move all overdue tasks in Website to next week"), not a feature tour. Don't help them, and ask them to think aloud.
- **SHOULD** include keyboard-only and screen-reader users when testing public or legally covered flows ([§15](#15-accessibility)).
- **MAY** use unmoderated remote tests for quick checks, and tree tests or first-click tests to check navigation labels.

### 20.3 Metrics

| Metric | What it tells you | How to collect it | Reference point |
| --- | --- | --- | --- |
| Task success rate | Can users do it? | Share of attempts finished without help, in tests or from funnel events | Compare with the previous version |
| Time on task | How efficient is it? | Median time per task in tests or from events | Compare with the previous version |
| Error rate | Where do users stumble? | Validation errors per submit, undo use, retries | Watch for increases |
| Single Ease Question (SEQ) | How hard did one task feel? | One 7-point question right after a task | 5.3–5.6 is average (MeasuringU) |
| System Usability Scale (SUS) | How usable does the product feel overall? | 10-item questionnaire, scored 0–100 | 68 is average across 500 studies; above 80.3 is the top 10% (MeasuringU) |
| UMUX-Lite | A short stand-in for SUS | 2 items ("meets my requirements", "easy to use"), in-app | Track over time; converts to the SUS scale |
| HEART (Happiness, Engagement, Adoption, Retention, Task success) | Product-level health of a feature | Pick goals, then signals, then metrics (Google, CHI 2010) | Choose 1–2 per feature |

- **SHOULD** define one task-success signal and one adoption signal for each significant feature before release, and add the analytics events to measure them.
- **MUST NOT** put personal data (names, emails, free text the user typed) in analytics events. *Why:* analytics tools are third parties, privacy law covers the data, and events are hard to delete once sent. Redaction approach: [logging.md §1.7](logging.md#17-sensitive-data).
- **MUST** collect analytics that need consent only after the user gives it. *Why:* in the EU, non-essential tracking needs prior consent (ePrivacy Directive Article 5(3)).
- **SHOULD** name events as object and past-tense action (`task_created`, `filter_applied`), with context in properties *(our rule)*.
- **SHOULD** watch guardrail metrics (errors, support contacts, cancellations) in A/B tests, so a variant can't win by misleading users ([§19](#19-ethical-design-and-dark-patterns)).
- **MAY** use session replay or click maps to find rage clicks and dead ends, only with consent and with every input field masked.

---

## 21. Anti-patterns

| Anti-pattern | Do instead | Rule |
| --- | --- | --- |
| Every feature as a visible button | Place by how often it's used; overflow menu and command palette for the rest | [§2](#2-feature-rich-without-clutter) |
| Panels inside panels inside panels | At most 2 levels of disclosure | [§2.2](#22-progressive-disclosure) |
| Filters lost on Back or reload | View state in the URL | [§3.1](#31-put-view-state-in-the-url) |
| Actions that only appear on hover | Also on focus, and visible or in a menu on touch | [§6.3](#63-overlays-dialogs-sheets-popovers-and-tooltips) |
| Submit button disabled until the form is valid | Keep it enabled; show errors on submit | [§6.2](#62-buttons-and-disabled-controls) |
| Promo modal on page load | Contextual hints, a "What's new" entry | [§6.3](#63-overlays-dialogs-sheets-popovers-and-tooltips), [§13](#13-onboarding-discoverability-and-help) |
| An error shown only in a toast | Inline alert where the action failed | [§7.2](#72-choose-the-right-feedback-channel) |
| Full-page spinner on every filter change | Keep content visible, show progress | [§8](#8-screen-states) |
| "No tasks yet" when a filter matches nothing | A separate no-results state | [§8](#8-screen-states) |
| Placeholder text as the label | A visible label above the field | [§9.1](#91-structure) |
| An error after the first keystroke | Check on leaving the field or on submit | [§9.3](#93-validation-timing) |
| A cleared form after a failed submit | Keep everything the user typed | [§9.5](#95-never-lose-the-users-work) |
| "Are you sure?" on every action | Undo for reversible actions; a specific confirmation for irreversible ones | [§10.2](#102-undo-or-confirm) |
| Infinite scroll above a footer | Pagination or "Load more" | [§11.2](#112-pagination-load-more-and-infinite-scroll) |
| "Select all" that silently covers hidden pages | State the scope and offer to select everything | [§11.4](#114-selection-and-bulk-actions) |
| Single-key shortcuts that can't be turned off | Modifier keys, or a setting to turn them off | [§12.3](#123-keyboard-shortcuts) |
| A five-step product tour on first login | First-use empty states and hints at the moment of need | [§13](#13-onboarding-discoverability-and-help) |
| An accessibility overlay widget | Fix the code | [§15.3](#153-no-overlays-and-test-with-people) |
| An AI agent that acts without asking | Preview and explicit approval | [§18.5](#185-agent-actions) |
| Pre-checked marketing consent, cancel by phone only | Unchecked boxes; cancel online as easily as signing up | [§19](#19-ethical-design-and-dark-patterns) |

---

## 22. PR checklist

- [ ] Each screen has one primary action; every feature is reachable from a visible place; disclosure is at most 2 levels deep and hides nothing needed for the current decision; one save model per settings page ([§2](#2-feature-rich-without-clutter)).
- [ ] The current page is marked with `aria-current`; repeated navigation keeps its order; shareable view state is in the URL; no secrets or personal data in URLs; sign-in returns to the requested page ([§3](#3-information-architecture-and-navigation)).
- [ ] Screens work at 320 CSS px wide, at 200% zoom and with WCAG text spacing ([§4.3](#43-zoom-reflow-and-text-spacing)).
- [ ] No orientation lock, pinch zoom not disabled, single-tap alternatives to gestures ([§5](#5-responsive-mobile-and-touch)).
- [ ] Same action, same label everywhere; unavailable controls say why; no dialog opened from a dialog except a confirmation; no unrequested modals; overlays close with Esc and a close button; nothing essential only in a tooltip; hover content is dismissible, hoverable and persistent; hover actions work on focus and touch; every drag has a non-drag alternative ([§6](#6-components-and-interaction-patterns)).
- [ ] Feedback within 100 ms; progress after 1 s; determinate progress and Cancel after 10 s; no action-needed error only in a toast; action toasts stay at least 10 s and pause on hover and focus; "Saved" only after the server confirms; permissions asked in context; per-type notification settings; one-click unsubscribe in marketing email; no focus or scroll jumps when content arrives ([§7](#7-feedback-and-system-status)).
- [ ] Loading, empty, error, success and no-results states built; no empty state while data loads; refetching keeps content visible; failures stay local ([§8](#8-screen-states)).
- [ ] Forms: no placeholder labels; no redundant entry; `type`, `inputmode` and `autocomplete` set; paste and password managers allowed; no errors while first typing; errors in text next to fields; error summary on forms with more than 3 fields; input kept after a failed submit; unsaved-changes warning; session timeout warning and input kept across re-login ([§9](#9-forms-and-input)).
- [ ] Irreversible actions that destroy data, spend money or reach other people are confirmed, naming the object and with a verb on the button; error messages say what happened and what to do; no stack traces or internal codes; error pages have a way forward ([§10](#10-errors-confirmation-and-undo)).
- [ ] Sorted columns show their sort; Back restores the list position; "select all" states its scope; partial bulk failures are reported ([§11](#11-tables-lists-filters-and-bulk-actions)).
- [ ] The command palette is an accessible combobox; single-key shortcuts can be turned off; shortcuts don't fire in text fields; browser and OS shortcuts aren't overridden ([§12](#12-search-command-palette-and-keyboard-shortcuts)).
- [ ] Hints, tours and checklists are dismissible and stay dismissed; help is in the same place on every page ([§13](#13-onboarding-discoverability-and-help)).
- [ ] WCAG 2.2 AA; unique page titles; skip link and landmarks; focus never fully hidden by sticky bars; no puzzles to log in; no overlay widgets ([§15](#15-accessibility)).
- [ ] No waiting on animations; nothing flashes more than 3 times a second; moving content can be paused ([§16](#16-motion-and-animation)).
- [ ] Key screens meet LCP ≤ 2.5 s, INP ≤ 200 ms and CLS ≤ 0.1 at p75 ([§17](#17-performance-as-ux)).
- [ ] AI is disclosed; data use is explained before first use; prompts kept on failure; Stop button; completion announced once; output sanitized; approval before irreversible or outward-facing agent actions; AI changes undoable and attributed ([§18](#18-ai-features)).
- [ ] No pre-selected consent or paid extras; reject as easy as accept; cancellation online and no harder than sign-up; total price and trial terms before payment; no fake urgency, confirmshaming, disguised ads or repeated asks ([§19](#19-ethical-design-and-dark-patterns)).
- [ ] No personal data in analytics events; consent-dependent analytics wait for consent ([§20](#20-ux-research-testing-and-metrics)).

---

## 23. Sources

All checked on 2026-11-09.

**Usability research and design systems**

- Nielsen Norman Group, 10 Usability Heuristics for User Interface Design: https://www.nngroup.com/articles/ten-usability-heuristics/
- Nielsen Norman Group, Response Times: The 3 Important Limits: https://www.nngroup.com/articles/response-times-3-important-limits/
- Nielsen Norman Group, Progress Indicators Make a Slow System Less Insufferable: https://www.nngroup.com/articles/progress-indicators/
- Nielsen Norman Group, Skeleton Screens 101: https://www.nngroup.com/articles/skeleton-screens/
- Nielsen Norman Group, Progressive Disclosure: https://www.nngroup.com/articles/progressive-disclosure/
- Nielsen Norman Group, Designing Empty States in Complex Applications: https://www.nngroup.com/articles/empty-state-interface-design/
- Nielsen Norman Group, Infinite Scrolling: When to Use It, When to Avoid It: https://www.nngroup.com/articles/infinite-scrolling-tips/
- Nielsen Norman Group, Data Tables: Four Major User Tasks: https://www.nngroup.com/articles/data-tables/
- Nielsen Norman Group, Modal and Nonmodal Dialogs: https://www.nngroup.com/articles/modal-nonmodal-dialog/
- Nielsen Norman Group, Tooltip Guidelines: https://www.nngroup.com/articles/tooltip-guidelines/
- Nielsen Norman Group, Confirmation Dialogs Can Prevent User Errors: https://www.nngroup.com/articles/confirmation-dialog/
- Nielsen Norman Group, Error-Message Guidelines: https://www.nngroup.com/articles/error-message-guidelines/
- Nielsen Norman Group, Marking Required Fields in Forms: https://www.nngroup.com/articles/required-fields/
- Nielsen Norman Group, UI Accelerators: https://www.nngroup.com/articles/ui-accelerators/
- Nielsen Norman Group, How to Conduct a Heuristic Evaluation: https://www.nngroup.com/articles/how-to-conduct-a-heuristic-evaluation/
- Nielsen Norman Group, Why You Only Need to Test with 5 Users: https://www.nngroup.com/articles/why-you-only-need-to-test-with-5-users/
- GOV.UK Design System, Error summary: https://design-system.service.gov.uk/components/error-summary/
- GOV.UK Design System, Error message: https://design-system.service.gov.uk/components/error-message/
- GOV.UK Design System, Validation pattern: https://design-system.service.gov.uk/patterns/validation/
- GOV.UK Design System, Button (disabled buttons): https://design-system.service.gov.uk/components/button/
- GOV.UK Design System, Question pages: https://design-system.service.gov.uk/patterns/question-pages/
- GOV.UK Design System, Text input (placeholders, widths): https://design-system.service.gov.uk/components/text-input/
- Baymard Institute, Usability Testing of Inline Form Validation: https://baymard.com/research-articles/inline-form-validation
- Baymard Institute, Mark both required and optional fields: https://baymard.com/blog/required-optional-form-fields
- Baymard Institute, UX statistics (checkout form elements): https://baymard.com/blog/ux-statistics
- Baymard Institute, Line length readability: https://baymard.com/blog/line-length-readability
- Laws of UX: https://lawsofux.com/
- U.S. Web Design System, Typography: https://designsystem.digital.gov/components/typography/
- The Elements of Typographic Style Applied to the Web, line length: http://webtypography.net/2.1.2
- Material Design 3 motion tokens (Material Web v0.192 token file): https://github.com/material-components/material-web/blob/main/tokens/versions/v0_192/_md-sys-motion.scss
- Google, Material 3 Expressive launch (Android and Wear OS): https://blog.google/products/android/material-3-expressive-android-wearos-launch/
- Apple Human Interface Guidelines, Materials (Liquid Glass): https://developer.apple.com/design/human-interface-guidelines/materials
- Apple Human Interface Guidelines, Keyboards: https://developer.apple.com/design/human-interface-guidelines/keyboards
- Visual Studio Code, Command Palette UX guidelines: https://code.visualstudio.com/api/ux-guidelines/command-palette

**Accessibility**

- W3C, Web Content Accessibility Guidelines (WCAG) 2.2: https://www.w3.org/TR/WCAG22/
- W3C, Understanding 3.3.8 Accessible Authentication (Minimum): https://www.w3.org/WAI/WCAG22/Understanding/accessible-authentication-minimum.html
- W3C WAI, WCAG 2.2 approved as ISO/IEC 40500:2025: https://www.w3.org/WAI/news/2025-10-21/wcag22-iso
- W3C, WCAG 3.0 Working Draft (2026-09-10) and history: https://www.w3.org/TR/wcag-3.0/ and https://www.w3.org/standards/history/wcag-3.0/
- W3C, WAI-ARIA Authoring Practices Guide, patterns: https://www.w3.org/WAI/ARIA/apg/patterns/
- W3C, WAI-ARIA 1.2 (`aria-busy`): https://www.w3.org/TR/wai-aria-1.2/
- W3C, Technique ARIA23 (`role="log"`): https://www.w3.org/WAI/WCAG22/Techniques/aria/ARIA23
- ETSI, EN 301 549 releases: https://www.etsi.org/deliver/etsi_en/301500_301599/301549/
- AccessibleEU, EN 301 549 has been updated (2026-09-07): https://accessible-eu-centre.ec.europa.eu/content-corner/news/european-accessibility-standard-en-301-549-has-been-updated-2026-09-07_en
- European Accessibility Act, Directive (EU) 2019/882: https://eur-lex.europa.eu/eli/dir/2019/882/oj/eng
- US Department of Justice, ADA Title II web rule and the interim rule extending its deadlines (Federal Register, 2026-04-20): https://www.federalregister.gov/
- FTC, final order against accessiBe (2025-04-22): https://www.ftc.gov/news-events/news/press-releases/2025/04/ftc-approves-final-order-requiring-accessibe-pay-1-million
- Overlay Fact Sheet: https://overlayfactsheet.com/en/

**Performance and the web platform**

- web.dev, Web Vitals: https://web.dev/articles/vitals
- web.dev, Interaction to Next Paint (INP): https://web.dev/articles/inp
- Chrome UX Report release notes: https://developer.chrome.com/docs/crux/release-notes
- web.dev, Interop 2026: https://web.dev/blog/interop-2026
- Web platform status (Baseline): https://webstatus.dev
- MDN, `popover` global attribute: https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Global_attributes/popover
- MDN, `autocomplete` attribute: https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Attributes/autocomplete
- MDN, `inputmode` attribute: https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Global_attributes/inputmode
- web.dev, SMS OTP form best practices: https://web.dev/articles/sms-otp-form

**AI features**

- Amershi et al., Guidelines for Human-AI Interaction (CHI 2019): https://doi.org/10.1145/3290605.3300233
- Microsoft HAX Toolkit, guidelines and design library: https://www.microsoft.com/en-us/haxtoolkit/ai-guidelines/ and https://www.microsoft.com/en-us/haxtoolkit/library/
- Google PAIR, People + AI Guidebook: https://pair.withgoogle.com/guidebook-v2/chapters
- Apple Human Interface Guidelines, Generative AI: https://developer.apple.com/design/human-interface-guidelines/generative-ai
- EU AI Act, Regulation (EU) 2024/1689: https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=OJ:L_202401689
- Digital Omnibus on AI, Regulation (EU) 2026/1744: https://eur-lex.europa.eu/eli/reg/2026/1744/oj/eng
- European Commission, FAQ on Article 50 transparency obligations: https://digital-strategy.ec.europa.eu/en/faqs/transparency-obligations-under-article-50-ai-act
- European Commission, EU icons for labelling AI-generated content: https://digital-strategy.ec.europa.eu/en/policies/eu-icons-labelling-ai-generated-content

**Dark patterns and consumer law**

- Deceptive Patterns, types: https://www.deceptive.design/types
- Digital Services Act, Regulation (EU) 2022/2065: https://eur-lex.europa.eu/eli/reg/2022/2065/oj/eng
- EDPB, Guidelines 03/2022 on deceptive design patterns in social media platform interfaces: https://www.edpb.europa.eu/our-work-tools/our-documents/guidelines/guidelines-032022-deceptive-design-patterns-social-media_en
- EDPB, Cookie Banner Taskforce report (2023-01-17): https://www.edpb.europa.eu/system/files/2023-01/edpb_20230118_report_cookie_banner_taskforce_en.pdf
- CNIL, Cookies FAQ: https://www.cnil.fr/fr/cookies-et-autres-traceurs/regles/cookies/FAQ
- CJEU, *Planet49*, C-673/17: https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX:62017CJ0673
- European Parliament Research Service, Digital Fairness Act briefing (2026-10): https://eprs.europarl.europa.eu/contents/publications/EPRS/2026/10/EPRS_BRI(2026)791529.html
- FTC, Amazon settlement (2025-09-25): https://www.ftc.gov/news-events/news/press-releases/2025/09/ftc-secures-historic-25-billion-settlement-against-amazon
- FTC, Negative Option Rule: https://www.ftc.gov/legal-library/browse/rules/negative-option-rule
- FTC, advance notice of proposed rulemaking on negative options (2026-03): https://www.ftc.gov/news-events/news/press-releases/2026/03/ftc-seeks-public-comment-response-advance-notice-proposed-rulemaking-regarding-negative-option
- Latham & Watkins, California automatic renewal law amendments (AB 2863): https://lw.com/admin/upload/SiteAttachments/In-California-New-Automatic-Renewal-and-Continuous-Service-Requirements-Signed-Into-Law.pdf
- UK government, subscription rules from January 2027: https://www.gov.uk/government/news/pm-starts-roll-out-of-everyday-fixes-on-the-cost-of-living-ending-rip-off-discounts-and-subscription-traps

**UX research and metrics**

- MeasuringU, System Usability Scale: https://measuringu.com/sus/
- MeasuringU, Single Ease Question: https://measuringu.com/seq10/
- MeasuringU, UMUX-Lite: https://measuringu.com/umux-lite/
- Rodden, Hutchinson and Fu, Measuring the User Experience on a Large Scale (HEART, CHI 2010): https://research.google/pubs/measuring-the-user-experience-on-a-large-scale-user-centered-metrics-for-web-applications/
