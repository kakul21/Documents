# Improvement Recommendations: Cynbit Website

Tester: Kakul | Date: 31-10-2026 | Related: [01-qa-audit-report.md](01-qa-audit-report.md), [02-bug-report.md](02-bug-report.md)

Effort: **S** under half a day, **M** 1-3 days, **L** over 3 days.

## 1. Prioritized Fixes for the Development Team

### P1: Fix immediately
| # | Recommendation | Bugs | Effort | Impact |
|---|---|---|---|---|
| 1 | Correct the "Suggested Reading" slug `ai-agents-transforming-business` to `ai-agents-business-operations` in three posts (or add a 301 redirect). | BUG-001 | S | Removes the only confirmed dead link |
| 2 | Publish a refund, cancellation and pricing/tax policy and link it from Terms and the BitElevate payment box. | BUG-019 | S-M | Legal and consumer trust for a paid program |

### P2: Next release
| # | Recommendation | Bugs | Effort | Impact |
|---|---|---|---|---|
| 3 | Make Terms §4 match reality (instant enrollment vs "competitive selection"). | BUG-020 | S | Removes a contradiction users can cite |
| 4 | Update the Privacy Policy: Cashfree, Google Analytics, careers/applicant data (DOB, address, gender), retention period, grievance contact. Add a privacy-specific mailbox. | BUG-021, BUG-023 | M | Compliance (India DPDP / GDPR) |
| 5 | Add a cookie-consent banner and load analytics only after consent. | BUG-022 | M | Compliance |
| 6 | Generate per-page metadata: unique title/description for `/case-studies`, `/diaries` and their detail pages; a canonical per page; `og:url` set to the page's own URL; a PNG/JPG 1200x630 `og:image`. | BUG-025, BUG-026, BUG-028 | M | Search results and social previews |
| 7 | Fix the BitElevate syllabus link (point it to the on-page curriculum section) and make sure all FAQ answers are in the HTML and keyboard-operable. | BUG-002, BUG-005 | S | Conversion on the paid page |
| 8 | Test every CTA: confirm each has a target (link, modal or anchor), including `#inquiry`. | BUG-006 | S | Conversion |
| 9 | Replace pravatar and Unsplash placeholders with real mentor photos and real project visuals, or label them as illustrative. | BUG-024 | M | Credibility |
| 10 | Complete or unpublish the two stub articles; verify authorship. | BUG-007 | S-M | Content quality |
| 11 | Standardize careers forms: one required-field convention, drop DOB / permanent address / notice period for internships, shorten the form, link consent to a privacy section covering applicants. | BUG-018 | M | Fewer drop-offs, less personal data held |
| 12 | Mark icon elements `aria-hidden="true"` (or use inline SVG with titles) so icon names are not announced. | BUG-029 | S | Accessibility |
| 13 | Add the `sizes` attribute to Next.js images and cap generated widths. | BUG-032 | S-M | Faster loads on mobile |

### P3: Backlog
| # | Recommendation | Bugs | Effort | Impact |
|---|---|---|---|---|
| 14 | Mark duplicated marquee/testimonial clones `aria-hidden` and `inert`; or use CSS-only looping. | BUG-030 | S | Accessibility, DOM size |
| 15 | Add a source or footnote to every statistic and unify values (99.9% vs 99.99%, growth figures, subscriber counts); remove unlabeled metrics. | BUG-009 | S | Credibility |
| 16 | Fix "60-Day (8 Weeks)" wording; align BitElevate messaging across Home, Impact, Ecosystem and Paths. | BUG-010, BUG-011 | S | Clarity |
| 17 | Fix the typo "Unheared" and the "2025" label. | BUG-008 | S | Polish |
| 18 | Align blog category filter with real categories; use full dates; hide "Previous" on page 1. | BUG-004, BUG-012, BUG-013 | S | Usability |
| 19 | Fix the "Our impact" link; reorganize footer columns; review Team bios and role labels. | BUG-003, BUG-014, BUG-017 | S | Navigation |
| 20 | Clean title templates (single brand suffix, under 60 characters) and remove copied meta keywords. | BUG-027 | S | SEO |
| 21 | Review model names and publication dates in AI-related articles. | BUG-015 | S | Accuracy |
| 22 | Label decorative widgets as illustrative or connect them to real data. | BUG-016 | S | Honesty of UI |
| 23 | Give LinkedIn links a descriptive accessible name ("Mohak Saini on LinkedIn"). | BUG-031 | S | Accessibility |

## 2. Quick Wins (under an hour each)
- Slug fix for the broken link (BUG-001).
- Typo fix (BUG-008).
- `og:url` and `og:image` template change (BUG-025) fixes all pages at once.
- `aria-hidden` on icon spans (BUG-029) fixes all pages at once.
- Link target for "Our impact" (BUG-003).

## 3. Area-wise Recommendations

| Area | Recommendation |
|---|---|
| Functionality | Run an automated crawler on every release; ensure forms show clear success/error states; confirm every CTA has an action. |
| UI / UX | Keep one pattern for required fields and for date formats; avoid presenting static numbers as live status. |
| Content | Add an editorial checklist: no stub posts, sources for numbers, verified bylines and dates. |
| Compliance | Treat Terms, Privacy and Cookie pages as part of the paid-product release checklist. |
| Responsiveness | Verify the five breakpoints, especially the long careers forms, the marquees, and the BitElevate pricing card on 360px screens. |
| Accessibility (WCAG 2.1 AA) | Fix icon text, duplicated content and link names; test keyboard order and focus visibility in the nav dropdowns and FAQ accordions; check contrast on gradient and glass-style sections. |
| Performance | Right-size images, lazy-load below-the-fold marquees, run Lighthouse on Home, BitElevate and Blog. |
| Security | Add security headers (CSP, HSTS, X-Content-Type-Options, frame protection); confirm cookie flags; rate-limit and validate public forms; sanitize user-submitted diary entries. |

## 4. Testing Enhancements

| # | Suggestion | Benefit |
|---|---|---|
| 1 | Add a **link-checker** test (Playwright or a crawler) that visits every internal URL and every "Suggested Reading" link on each deploy. | Would have caught BUG-001 |
| 2 | Add **metadata assertions** per route: unique title, description, canonical, `og:url` equal to the page URL, `og:image` type. | Prevents BUG-025, 026, 028 |
| 3 | Add **axe** checks in CI and fail on serious violations. | Continuous accessibility coverage |
| 4 | Add **Lighthouse CI** budgets (performance, accessibility, SEO). | Prevents regressions |
| 5 | Add form tests: empty, invalid email, very long input, special characters, double-submit; test with a sandbox payment key. | Protects revenue flows |
| 6 | Maintain a **content checklist** (typos, number consistency, date accuracy) reviewed before publishing. | Fewer content defects |
| 7 | Use **BrowserStack or similar** for Safari and real mobile devices. | Wider coverage |
| 8 | Add visual regression snapshots at 360, 768 and 1366px. | Catches layout breaks |
| 9 | Define severity rules and a bug template for triage. | Consistent prioritization |

## 5. Suggested Roadmap

| Phase | Focus | Timeline |
|---|---|---|
| 1 | P1 fixes and quick wins (1, 2, 3, templated metadata, icon aria-hidden) | Week 1 |
| 2 | Remaining P2: privacy and cookie compliance, CTA audit, careers forms, placeholder images, image sizing | Weeks 2-3 |
| 3 | P3 content and accessibility cleanup; add link-checker, metadata tests, axe and Lighthouse CI | Weeks 4-5 |
| 4 | Full manual regression across browsers and devices | Ongoing per release |

## 6. Conclusion
The site's foundation is solid. The highest-value changes are a short list: fix the 404 link, publish refund terms, align Terms and Privacy with how the BitElevate program actually works, and correct page-level metadata. Those four items remove the main legal, trust and discoverability risks. The automated checks in section 4 would keep them from returning.
