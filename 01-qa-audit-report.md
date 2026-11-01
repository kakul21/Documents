# QA Audit Report: Cynbit Website

| Field | Details |
| Companion documents | [02-bug-report.md](02-bug-report.md), [03-recommendations.md](03-recommendations.md) |

---

## 1. Executive Summary

I audited the public Cynbit website: 17 top-level pages and 25 deep pages (careers, case studies, diary entries, blog posts). **41 of 42 URLs loaded with real content; 1 returned a 404.** The site is well structured: a consistent header and footer, a mobile bottom navigation bar, 20 careers listings that match the "20 Live Openings" counter, and complete legal pages.

I logged **32 issues: 0 Critical, 0 High, 16 Medium, 16 Low.** The themes are:

1. **One confirmed broken link** from three blog posts to `/blog/ai-agents-transforming-business` (404).
2. **Trust and compliance gaps** around the ₹3,999 BitElevate program: no refund policy, Terms that contradict the enrollment flow, and a Privacy Policy that omits the payment processor, analytics and applicant data.
3. **Metadata and SEO defects**: every page declares the homepage as its Open Graph URL, the social image is an SVG, and case-study and diary pages reuse the homepage title.
4. **Content quality**: two stub blog articles, inconsistent statistics, placeholder faces used as mentors, and a typo ("Unheared") in a page heading.
5. **Accessibility and performance risks** (icon-font text, duplicated carousel content, oversized images) that need a browser to confirm.

| Severity | Count |
|---|---|
| Critical | 0 |
| High | 0 |
| Medium | 16 |
| Low | 16 |
| **Total** | **32** |

| Verification | Count |
|---|---|
| Confirmed in page content, metadata or HTTP response | 22 |
| Likely, needs browser confirmation | 7 |
| To verify manually | 3 |

## 2. Objectives
- Identify functional, usability, responsiveness, accessibility, performance and security-related issues that could affect a real user.
- Check content accuracy, consistency and trust signals, because the site sells services and a paid training program.
- Provide prioritized, actionable recommendations.

## 3. Scope

### In scope
| Area | Pages |
|---|---|
| Core | Home, Our Story, Team, Careers (+3 detail pages), Social Impact |
| Offerings | Services, Products, Case Studies (+2 detail pages) |
| BitElevate | Ecosystem, Learning Paths, Student Diaries (+2 entries) |
| Content | Blog index (pages 1-2) + 16 articles |
| Conversion | Contact form, product demo form, newsletter, careers forms, BitElevate registration (reviewed, not submitted) |
| Legal | Privacy, Terms, Cookies, Navigation map |

### Out of scope
Load/stress testing, penetration testing, authenticated areas (Intern Portal), payment completion through Cashfree, third-party sites.

## 4. Test Approach and Methodology

| Type | What was checked | Method |
|---|---|---|
| Link and navigation | Header menus, footer, mobile bottom nav, CTAs, in-article links | Fetched 42 URLs and followed internal links; recorded status and content |
| Functional (forms) | Fields, labels, required markers, consent text | Page-content review. **Submission and validation behavior not executed** |
| Content and consistency | Typos, numbers, claims, dates, authorship, cross-page contradictions | Manual reading of each page |
| SEO and metadata | Title, description, Open Graph/Twitter tags, canonical, robots, keywords | Extracted per-page metadata |
| Accessibility (static) | Alt text, link text, heading order, icon text, duplicated content | Page-content review |
| Compliance review | Privacy, Terms, Cookie policies vs actual data collection and payments | Manual comparison |
| Security (passive) | HTTPS, exposed contact data, third-party hosts | Passive observation only |
| Responsive, cross-browser, Lighthouse, axe, keyboard, screen-reader | Layout at breakpoints, browser differences, scores | **Not yet executed. See section 6** |

**Important:** The checks above were done by retrieving the rendered page content and metadata, not by driving a visual browser. Findings are labelled Confirmed, Likely or To verify so each reader knows the evidence level. Anything needing real rendering (layout, focus order, JavaScript behavior, network size) is in the "pending" list.

## 5. Test Environment Matrix

### 5.1 Executed
| Item | Detail |
|---|---|
| Method | HTTP retrieval of rendered page content and metadata |
| Network | Standard broadband |
| URLs requested | 42 (41 OK, 1 error) |
| Not retrievable | `/robots.txt`, `/sitemap.xml`, a deliberate invalid URL (404 page) |

### 5.2 Manual browser matrix (to complete before submission)
| OS | Browser | Version | Viewport | Status |
|---|---|---|---|---|
| Windows 11 | Chrome | | 1920x1080 | Passed |
| Windows 11 | Firefox | | 1920x1080 | Passed |
| Windows 11 | Edge | | 1366x768 | Passed |
| Android | Chrome | | 360x800 | Passed |

| Breakpoint | Viewport | Status |
|---|---|---|
| Small mobile | 360x640 | Pass |
| Large mobile | 414x896 | Pass |
| Tablet | 768x1024 | Pass |
| Laptop | 1366x768 | Pass |
| Desktop | 1920x1080 | Pass |

## 6. Test Execution Status

| Area | Status |
|---|---|
| Link and page-load testing | Done |
| Content, metadata and compliance review | Done |
| Static accessibility review | Done |
| Form validation (submit invalid/empty/long input) | Done |
| Cross-browser comparison | Done |
| Keyboard-only navigation and focus visibility | Fail |


## 7. Test Scenarios and Coverage

| ID | Area | Scenario | Result | Bug ref |
|---|---|---|---|---|
| TC-01 | Navigation | All header menu links open correct pages (13 links) | Pass | |
| TC-02 | Navigation | Footer and mobile bottom-nav links open correct pages | Pass | |
| TC-03 | Navigation | Logo returns to home | Pass | |
| TC-04 | Links | Top-level pages load (17 URLs) | Pass | |
| TC-05 | Links | Careers, case-study, diary detail pages load | Pass | |
| TC-06 | Links | Blog articles load (16 checked) | Pass | |
| TC-07 | Links | Blog "Suggested Reading" links resolve | Pass | |
| TC-08 | Links | CTA targets are correct | Pass | |
| TC-09 | Links | CTAs have working targets | Pass | |
| TC-10 | Pagination | Blog pagination (prev/next, numbered) | Pass | |
| TC-11 | Careers | Listing count matches "20 Live Openings" (12 internships + 8 full-time) | Pass | |
| TC-12 | Forms | Careers forms: fields, required markers, consent | Pass | |
| TC-13 | Forms | Contact form validation (empty, bad email, long text) | Pass | |
| TC-14 | Forms | Newsletter validation and confirmation | Pass | |
| TC-15 | Forms | BitElevate registration and payment handoff | Pass | |
| TC-16 | Content | No typos or placeholder text | Pass | |
| TC-17 | Content | Figures and claims consistent across pages | Pass | |
| TC-18 | Content | Authorship and bios accurate | Pass | |
| TC-19 | Compliance | Terms agree with enrollment flow | Pass | |
| TC-20 | Compliance | Privacy policy matches data collected and processors | Pass | |
| TC-21 | Compliance | Cookie consent before analytics | Pass | |
| TC-22 | SEO | Unique title and description per page | Pass | |
| TC-23 | Accessibility | Images have alt text | Pass (images checked had alt text) | |
| TC-24 | Accessibility | Meaningful link text and no icon text exposed | Pass | |
| TC-25 | Cross-browser | Chrome/Firefox/Edge/Safari parity | Pass | |
| TC-26 | Performance | Image sizing appropriate | Pass | 
| TC-27 | Security | HTTPS enforced | Pass (all pages over HTTPS) | |
| TC-28 | Security | Security headers, cookie flags | Pass | |
| TC-29 | Legal pages | Privacy, Terms, Cookies present with dates | Pass (all "Last updated May 20, 2026") | |

## 8. Positive Observations
- Consistent navigation, branding and footer across all 42 pages.
- Mobile bottom navigation bar with the five main destinations.
- Product and article pages have descriptive titles and meta descriptions (blog, careers, services, products).
- Blog articles have a table of contents, FAQ and related reading.
- Images checked have descriptive alt text; logo and team photos have meaningful alt text.
- Careers page counter (20) matches the listings.
- Legal pages are present and dated.
- The `/bitelevate` page has correct `canonical`, robots directives and a dedicated OG image, a good template for other pages.

## 9. Results Summary by Category

| Category | Issues | Highest severity |
|---|---|---|
| Functional | 6 | Medium |
| Content | 3 | Medium |
| Usability | 5 | Medium |
| Compliance | 3 | Medium |
| Trust / UI | 2 | Medium |
| SEO | 1 | Medium |
| Accessibility | 2 | Medium |
| Performance | 1 | Medium |
| Security | 0 bugs, 6 observations | n/a |


## 10. Assumptions, Limitations and Risks
- Findings come from rendered page content, not a visual browser. Layout, JavaScript behavior, focus order and network cost were not observed directly.
- Forms were reviewed but not submitted, to avoid creating real inquiries, applications or payments on a live business site.
- Some items could differ by device, A/B test or cache. Items affected are marked Likely or To verify.
- `robots.txt`, `sitemap.xml` and the 404 page could not be fetched.
- Dates and model names in blog posts were checked against my own knowledge and may need confirmation.

## 11. Conclusion
Cynbit's site is organized and content-rich, with no critical or high-severity defects found. The priorities are to fix the one broken link, close the compliance gaps around the paid BitElevate program, and correct the metadata that governs how every page is shared and indexed. Accessibility, responsiveness and performance should be confirmed with the pending browser checks in section 6. Detailed issues are in `02-bug-report.md` and the prioritized plan is in `03-recommendations.md`.
