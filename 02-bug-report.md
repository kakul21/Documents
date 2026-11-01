# Bug & Issue Report: Cynbit Website (https://www.cynbit.in/)

| Total issues | 32 (0 Critical, 0 High, 16 Medium, 16 Low) |
| Companion docs | [01-qa-audit-report.md](01-qa-audit-report.md), [03-recommendations.md](03-recommendations.md) |

## How to read this report

**Severity:** Critical (core function broken or serious security exposure), High (major feature broken, no workaround), Medium (feature partly broken, trust/compliance/accessibility impact, or workaround exists), Low (cosmetic or minor).
**Priority:** P1 fix immediately, P2 next release, P3 backlog.

**Verification status** (how sure the finding is):
- **Confirmed:** directly observed in the page content, page metadata, or an HTTP error response.
- **Likely:** strongly implied by the page content, but behavior (rendering, ARIA, network) must be checked in a real browser.
- **To verify:** the page content raised a question that only manual interaction can answer.

Evidence files go in `evidence/` as `BUG-0XX.png`. See `evidence/README.md` for what to capture.

---

## Summary table

| ID | Title | Category | Page(s) | Sev | Pri | Status |
|---|---|---|---|---|---|---|
| BUG-001 | Broken "Suggested Reading" link returns 404 | Functional | 3 blog posts | Medium | P1 | Confirmed |
| BUG-002 | "View full 60-day syllabus" links to a page with no syllabus | Functional | /bitelevate | Medium | P2 | Confirmed |
| BUG-003 | "Our impact" CTA goes to Case Studies, not Impact | Functional | /contact | Low | P3 | Confirmed |
| BUG-004 | "Previous Page" on page 1 links to `?page=0` | Functional | /blog | Low | P3 | Likely |
| BUG-005 | 6 of 7 BitElevate FAQ answers absent from page content | Functional / SEO | /bitelevate | Medium | P2 | To verify |
| BUG-006 | Several CTAs show no link target | Functional | /services, /contact, /bitelevate/paths, /products | Medium | P2 | To verify |
| BUG-007 | Two blog posts are truncated stubs labelled 8-10 min reads | Content | 2 blog posts | Medium | P2 | Confirmed |
| BUG-008 | Typo "Unheared" and stale "2025" label | Content | /diaries | Low | P3 | Confirmed |
| BUG-009 | Metrics and claims inconsistent across pages | Content | Home, case studies, impact, BitElevate | Low | P3 | Confirmed |
| BUG-010 | "60-Day (8 Weeks)" does not add up | Content | /bitelevate | Low | P3 | Confirmed |
| BUG-011 | BitElevate described in contradictory ways | Content | Home, /bitelevate, /bitelevate/paths | Low | P3 | Confirmed |
| BUG-012 | Blog category filter does not match post categories | Usability | /blog | Low | P3 | Confirmed |
| BUG-013 | Ambiguous blog date format ("Apr 26", "Dec 25") | Usability | /blog | Low | P3 | Confirmed |
| BUG-014 | Team bios appear mismatched with job titles | Content | /team | Low | P3 | Likely |
| BUG-015 | Outdated or incorrect AI model references in posts | Content | 3 blog posts | Low | P3 | Likely |
| BUG-016 | Static values shown as live status | UI / Trust | Home | Low | P3 | Likely |
| BUG-017 | Footer groups unrelated links under "Impact" | Usability | All pages | Low | P3 | Confirmed |
| BUG-018 | Careers application forms inconsistent and over-collect data | Usability | /careers/* | Medium | P2 | Confirmed |
| BUG-019 | No refund/cancellation policy for a paid program | Compliance | /terms, /bitelevate | Medium | P1 | Confirmed |
| BUG-020 | Terms say selection is competitive; enrollment is instant on payment | Compliance | /terms, /bitelevate | Medium | P2 | Confirmed |
| BUG-021 | Privacy policy omits payment processor, analytics, applicant data | Compliance | /privacy | Medium | P2 | Confirmed |
| BUG-022 | No cookie consent mechanism observed | Compliance | All pages | Medium | P2 | To verify |
| BUG-023 | One generic mailbox used for contact and privacy requests | Usability | Footer, /privacy | Low | P3 | Confirmed |
| BUG-024 | Stock/placeholder images presented as real mentors and client work | Trust | /bitelevate/paths, case studies | Medium | P2 | Confirmed |
| BUG-025 | `og:url` always the homepage; `og:image` is an SVG logo | SEO | All pages | Medium | P2 | Confirmed |
| BUG-026 | No canonical tags (except /bitelevate); www vs non-www mismatch | SEO | All pages | Low | P3 | Confirmed |
| BUG-027 | Page titles repeat the brand and are over-long; keywords copied | SEO | Many pages | Low | P3 | Confirmed |
| BUG-028 | Case-study and diary pages reuse the homepage title/description | SEO | /case-studies, /diaries, detail pages | Medium | P2 | Confirmed |
| BUG-029 | Icon-font names exposed as text (screen readers, font failure) | Accessibility | All pages | Medium | P2 | Likely |
| BUG-030 | Duplicated marquee/testimonial content in the DOM | Accessibility / Perf | Home, /bitelevate | Medium | P3 | Likely |
| BUG-031 | Team LinkedIn links use raw URLs as link text | Accessibility | /team | Low | P3 | Confirmed |
| BUG-032 | Images requested at 3840px width, including small avatars | Performance | Multiple | Medium | P2 | Likely |

---

## Detailed bug entries

### BUG-001: Broken "Suggested Reading" link returns 404
| Field | Details |
|---|---|
| Category / Sev / Pri | Functional / Medium / P1 |
| Status | Confirmed |
| Pages | `/blog/ai-automation-trends-2026`, `/blog/why-startups-need-custom-dashboards`, `/blog/ai-chatbots-vs-human-support` |
| Environment | HTTP request to the target URL |

**Steps:**
1. Open any of the three pages above and scroll to "Suggested Reading".
2. Click "How AI Agents Are Transforming Business Operations" (`/blog/ai-agents-transforming-business`).

**Expected:** The article opens.
**Actual:** The server returns a client error (404). The real article lives at `/blog/ai-agents-business-operations`, which other posts link to correctly.
**Impact:** Readers hit a dead end from three articles, and the dead link hurts SEO.
**Suggested fix:** Change the slug in those three posts, or add a redirect.

### BUG-002: "View full 60-day syllabus" links to a page with no syllabus
| Field | Details |
|---|---|
| Category / Sev / Pri | Functional / Medium / P2 |
| Status | Confirmed |
| Page | `/bitelevate` (Full Stack lab card) |

**Steps:** Open `/bitelevate`, choose the Full Stack track, click "View full 60-day syllabus".
**Expected:** A 60-day syllabus.
**Actual:** Opens `/bitelevate/paths`, a "Design your own career" page about 16 topic areas with no syllabus. The 60-day roadmap is actually a section on `/bitelevate` itself.
**Impact:** A paying prospect cannot find the curriculum they were promised.

### BUG-003: "Our impact" CTA goes to Case Studies
| Field | Details |
|---|---|
| Category / Sev / Pri | Functional / Low / P3 |
| Status | Confirmed |
| Page | `/contact` (bottom CTA) |

**Steps:** Open `/contact`, scroll to "Ready to construct?", click "Our impact".
**Expected:** `/impact` (Social Impact).
**Actual:** Goes to `/case-studies`.

### BUG-004: "Previous Page" on page 1 links to `?page=0`
| Field | Details |
|---|---|
| Category / Sev / Pri | Functional / Low / P3 |
| Status | Likely (seen in page content; confirm in browser) |
| Page | `/blog` |

**Steps:** Open `/blog` (page 1) and inspect the pagination.
**Expected:** "Previous Page" is disabled or hidden on the first page.
**Actual:** It links to `/blog?page=0`, which is not a valid page. Page 2 correctly links back to `?page=1`.

### BUG-005: 6 of 7 BitElevate FAQ answers absent from page content
| Field | Details |
|---|---|
| Category / Sev / Pri | Functional, SEO / Medium / P2 |
| Status | To verify |
| Page | `/bitelevate` |

**Steps:** Open `/bitelevate`, go to "Frequently Asked Questions", expand each question.
**Expected:** Every question reveals an answer, and the answers exist in the HTML for search engines and assistive tech.
**Actual:** Only the first answer appears in the page content; the other six questions (college credits, daily time, exam leave, PR reviews, repo ownership, beginners) show no answer text. They may render only on click.
**To check:** Click each question; view page source for the answers.

### BUG-006: Several CTAs show no link target
| Field | Details |
|---|---|
| Category / Sev / Pri | Functional / Medium / P2 |
| Status | To verify |

CTAs that appeared as plain text rather than links in the page content:
- `/services`: "Start discovery", "Inquire" (x3), "Technical roadmap"
- `/contact`: "Write us", "Secure Voice Line"
- `/bitelevate/paths`: "Generate My Custom Path"
- `/diaries`: "Share my entry"
- `/products`: product CTAs point to `#inquiry`; confirm that anchor exists and preselects the product

**Steps:** Click each. **Expected:** navigates, opens a form, or scrolls. **Actual:** unknown until tested in a browser. Record any that do nothing.

### BUG-007: Two blog posts are truncated stubs
| Field | Details |
|---|---|
| Category / Sev / Pri | Content / Medium / P2 |
| Status | Confirmed |
| Pages | `/blog/data-privacy-advantage`, `/blog/ai-first-learning` |

**Actual:** "Data Privacy as a Competitive Advantage" has two sentences ending in an ellipsis but is labelled "8 Min Read". "The Rise of AI-First Learning Ecosystems" is a single paragraph labelled "10 Min Read". Every other post has 9-10 sections with a table of contents and FAQ. The first post is bylined to "Julian Quince, Head of Growth", who is not on the Team page.
**Impact:** Looks unfinished and is thin content for SEO.

### BUG-008: Typo "Unheared" and stale year label
| Field | Details |
|---|---|
| Category / Sev / Pri | Content / Low / P3 |
| Status | Confirmed |
| Page | `/diaries` |

**Actual:** The heading reads "Student Diaries: Unheared Stories" (should be "Unheard"), and the eyebrow says "Student diaries 2025" while the site and entries are dated 2026.

### BUG-009: Metrics and claims inconsistent across pages
| Field | Details |
|---|---|
| Category / Sev / Pri | Content / Low / P3 |
| Status | Confirmed |

Examples: uptime is "99.9%" on Home, Story and Services but "99.99%" on Case Studies; growth appears as "+140%", "2.4x" and "+40%"; "1000+ Certified Architects" (Home) vs "1,000+ Interns Graduated" (BitElevate); "25k+" vs "25,000+" subscribers; "6+ Global Reach" has no unit; "98% Retention", "3x ROI Uplift" and "88% Placement Advancement" have no source.
**Impact:** Weakens credibility. Unsubstantiated placement and ROI figures can also be a consumer-protection risk.

### BUG-010: "60-Day (8 Weeks)" does not add up
| Field | Details |
|---|---|
| Category / Sev / Pri | Content / Low / P3 |
| Status | Confirmed |
| Page | `/bitelevate` |

**Actual:** The sprint is called "60-Day Standard Sprint (8 Weeks)" with "60 daily PRs", but 8 weeks is 56 days. The roadmap phases also end at week 8.

### BUG-011: BitElevate described in contradictory ways
| Field | Details |
|---|---|
| Category / Sev / Pri | Content / Low / P3 |
| Status | Confirmed |

Home calls it "a globally recognized training program" with "Global Certification"; `/bitelevate/paths` says "We've demolished the rigid curriculum"; `/bitelevate` sells a fixed 60-day, 6-lab internship for ₹3,999; `/impact` describes it as workshops plus mental-wellness phases. A visitor cannot tell what the product is.

### BUG-012: Blog category filter does not match post categories
| Field | Details |
|---|---|
| Category / Sev / Pri | Usability / Low / P3 |
| Status | Confirmed |
| Page | `/blog` |

**Actual:** The filter offers IT Strategy, Marketing, Education, Cloud Architecture, Community. Posts use many others (AI Agents, AI Automation, AI Chatbots, Business Automation, Data Analytics, SaaS Development, Software Development, Web Development). Some posts look miscategorised, for example "Best AI Tools for Startups" and "AI in Hiring" under Education.

### BUG-013: Ambiguous blog date format
| Field | Details |
|---|---|
| Category / Sev / Pri | Usability / Low / P3 |
| Status | Confirmed |
| Page | `/blog` |

**Actual:** Dates read "Apr 26", "Dec 25", "Jan 26", which looks like a day of the month. They mean April 2026 and so on. The article pages use full dates ("May 20, 2026").

### BUG-014: Team bios appear mismatched with titles
| Field | Details |
|---|---|
| Category / Sev / Pri | Content / Low / P3 |
| Status | Likely |
| Page | `/team` |

**Actual:** "Co Founder & CTO" Dhawan Nama's bio is about "strategic partnerships and ecosystem development", while the "Co Founder" Ashutosh Upadhyay's bio is about architecture and technical integrity. They look swapped. The CEO's role is also labelled "Founder & CEO" on one page and "CEO, Cynbit" on a blog byline.

### BUG-015: Outdated or incorrect AI model references in posts
| Field | Details |
|---|---|
| Category / Sev / Pri | Content / Low / P3 |
| Status | Likely |
| Pages | `/blog/best-ai-tools-startups`, `/blog/future-saas-development`, `/blog/best-automation-tools-2026` |

**Actual:** A post dated May 29, 2025 recommends "Claude 3.5 Opus / GPT-5", and a June 25, 2025 post mentions GPT-5 and Llama 4. To my knowledge neither GPT-5 nor a "Claude 3.5 Opus" existed at those dates, so publication dates or model names look wrong. A post dated May 2026 calls "Claude 3.5 Sonnet / GPT-4o" the primary reasoning engines, which is dated by that time.
**Action:** Verify against model release dates before citing this in the final submission.

### BUG-016: Static values shown as live status
| Field | Details |
|---|---|
| Category / Sev / Pri | UI / Trust / Low / P3 |
| Status | Likely |
| Page | Home hero and Execution card |

**Actual:** Widgets show "Efficiency 95%", "Runtime: Active now", "System Stability: 99.9%". They appear to be decorative constants styled as live telemetry. Confirm in the browser whether they update.

### BUG-017: Footer groups unrelated links under "Impact"
| Field | Details |
|---|---|
| Category / Sev / Pri | Usability / Low / P3 |
| Status | Confirmed |

**Actual:** "Expert Blog", "Privacy Policy" and "Contact Support" sit under the heading "Impact". "Contact Support" goes to the sales contact form. The sitemap page is labelled "Navigation".

### BUG-018: Careers application forms inconsistent and over-collect data
| Field | Details |
|---|---|
| Category / Sev / Pri | Usability / Medium / P2 |
| Status | Confirmed |
| Pages | `/careers/qa-testing-intern`, `/careers/hr-talent-acquisition-intern`, `/careers/full-stack-developer` |

**Actual:**
- Internship forms mark required fields with `*` and require Date of Birth, Gender, a separate WhatsApp number, Permanent Address and Notice Period. The full-time form shows no asterisks, makes WhatsApp, Graduation Year and Notice Period optional, and says "Complete the form below".
- A four-step form with about 20 fields for an unpaid internship; a notice period makes little sense for students.
- The consent checkbox points to Terms, which say nothing about applicant data (see BUG-021).
**Impact:** Drop-offs, privacy exposure, and confusing validation rules.

### BUG-019: No refund/cancellation policy for a paid program
| Field | Details |
|---|---|
| Category / Sev / Pri | Compliance / Medium / P1 |
| Status | Confirmed |
| Pages | `/terms`, `/bitelevate` |

**Actual:** BitElevate charges ₹3,999 via Cashfree. The Terms have five short sections (agreement, IP, inquiries, BitElevate selection, liability) and no refund, cancellation, pricing/tax or certificate-validity terms.

### BUG-020: Terms say selection is competitive; enrollment is instant on payment
| Field | Details |
|---|---|
| Category / Sev / Pri | Compliance / Medium / P2 |
| Status | Confirmed |

**Actual:** Terms §4 says BitElevate selection "is competitive and at the sole discretion" of Cynbit. The `/bitelevate` page offers "Immediate Activation" and credentials emailed on payment. The two statements conflict.

### BUG-021: Privacy policy omits processors and applicant data
| Field | Details |
|---|---|
| Category / Sev / Pri | Compliance / Medium / P2 |
| Status | Confirmed |
| Page | `/privacy` |

**Actual:** Third parties named are only Resend and Prisma. Missing: Cashfree (payments), Google Analytics (named on the Cookies page), and the sensitive applicant data collected on Careers forms (date of birth, address, gender). No retention period, grievance officer, or reference to India's data-protection law.

### BUG-022: No cookie consent mechanism observed
| Field | Details |
|---|---|
| Category / Sev / Pri | Compliance / Medium / P2 |
| Status | To verify |

**Actual:** The Cookie Policy says analytics cookies (Google Analytics) are used, but no consent banner or preference control appears in the page content. A banner injected by JavaScript would not show there.
**To check:** Open the site in a fresh incognito window and see whether a banner appears and whether analytics cookies are set before consent.

### BUG-023: One generic mailbox for contact and privacy requests
| Field | Details |
|---|---|
| Category / Sev / Pri | Usability / Low / P3 |
| Status | Confirmed |

**Actual:** The footer and Privacy page give `opportunities@cynbit.in` (a recruiting-style address) for all queries and data-deletion requests. `support@cynbit.in` appears only on the Contact page.

### BUG-024: Stock/placeholder images presented as real people and client work
| Field | Details |
|---|---|
| Category / Sev / Pri | Trust / Medium / P2 |
| Status | Confirmed |
| Pages | `/bitelevate/paths`, `/case-studies/*`, `/impact` |

**Actual:** The three "Cynbit expert mentor" avatars are loaded from `i.pravatar.cc`, a placeholder-face service. Case-study hero images and the Impact image are Unsplash stock photos. Presenting stock faces as mentors and stock photos as client projects undermines credibility.

### BUG-025: `og:url` always the homepage; `og:image` is an SVG logo
| Field | Details |
|---|---|
| Category / Sev / Pri | SEO / Medium / P2 |
| Status | Confirmed |
| Pages | All except `/bitelevate` |

**Actual:** `og:url` is `https://cynbit.in` on every page, and `og:image` / `twitter:image` is `logo.svg`. Most social platforms do not render SVG images, so shared links show no preview image, and the wrong canonical URL is declared.

### BUG-026: No canonical tags; www vs non-www mismatch
| Field | Details |
|---|---|
| Category / Sev / Pri | SEO / Low / P3 |
| Status | Confirmed |

**Actual:** Only `/bitelevate` declares a canonical. Pages are served on `www.cynbit.in` while metadata points at `cynbit.in`, which risks duplicate-content signals. Only `/bitelevate` has explicit robots directives.

### BUG-027: Titles repeat the brand and are over-long
| Field | Details |
|---|---|
| Category / Sev / Pri | SEO / Low / P3 |
| Status | Confirmed |

**Actual:** Titles end "| Cynbit Technologies | Cynbit Technologies" on Home, Contact, Team, Careers, Blog, Privacy, Terms, Cookies and Navigation. Home is 80+ characters. The same meta keywords (AI automation Jaipur…) are copied onto Careers, Blog and legal pages.

### BUG-028: Case-study and diary pages reuse the homepage title/description
| Field | Details |
|---|---|
| Category / Sev / Pri | SEO / Medium / P2 |
| Status | Confirmed |
| Pages | `/case-studies`, `/case-studies/*`, `/diaries`, `/diaries/*` |

**Actual:** They share the homepage's title and description. Blog and careers pages, by contrast, have page-specific titles, which shows the gap is fixable. Tabs, bookmarks and search results will show identical titles.

### BUG-029: Icon-font names exposed as text
| Field | Details |
|---|---|
| Category / Sev / Pri | Accessibility / Medium / P2 |
| Status | Likely |

**Actual:** Icon names appear as text inside links and buttons ("expand_more", "arrow_forward", "east", "send", "history_edu"), for example "Join the Newslettereast" and "Subscribesend". Unless the icon elements are `aria-hidden`, screen readers will read the words aloud, and if the font is blocked or slow, users see the raw words.
**To check:** Disable web fonts in DevTools; run a screen reader or axe on the footer.

### BUG-030: Duplicated marquee/testimonial content in the DOM
| Field | Details |
|---|---|
| Category / Sev / Pri | Accessibility, Performance / Medium / P3 |
| Status | Likely |

**Actual:** The client-logo strip on Home repeats the same nine names four to five times, and `/bitelevate` renders its testimonials (17 entries) twice. Screen readers will repeat them, and the page carries unnecessary DOM weight.

### BUG-031: Team LinkedIn links use raw URLs as link text
| Field | Details |
|---|---|
| Category / Sev / Pri | Accessibility / Low / P3 |
| Status | Confirmed |
| Page | `/team` |

**Actual:** Link text is `https://www.linkedin.com/in/...`, not "Mohak Saini on LinkedIn".

### BUG-032: Images requested at 3840px width
| Field | Details |
|---|---|
| Category / Sev / Pri | Performance / Medium / P2 |
| Status | Likely |

**Actual:** Next.js image URLs use `w=3840` for team photos, testimonial avatars and case-study heroes, and `w=1920` for blog heroes, suggesting missing `sizes` attributes. Small avatars may download multi-megabyte variants.
**To check:** Network tab, filter by Img, compare transferred size vs displayed size.


