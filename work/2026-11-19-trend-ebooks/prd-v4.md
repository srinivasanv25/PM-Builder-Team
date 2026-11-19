# PRD: Trend Explainer Engine (working name), v5

| | |
|---|---|
| **Owner** | Srini (solo builder, personal side project, unrelated to day job) |
| **Author** | pm-twin. v2 = revision round 1 after cto-partner and ux-lead review. v3 = revision round 2 after evals-lead verdict (🟡). v4 = minor cleanup after evals-lead ✅ on v3 |
| **Status** | Draft. Nothing built, nothing published. **Demand unproven.** |
| **Date** | 2026-11-19 |
| **Channel / revenue** | Amazon KDP ebooks → royalties (sales, plus KU page reads if enrolled in KDP Select) |
| **Changes from v1** | See `revisions/response-to-findings.md`. Main changes: the plan is staged so the cheapest test runs first; flag-to-live latency is now a gate; the detector uses only free, ToS-clean sources; evals are cut to safety and deterministic checks for the pilot; the pre-purchase and staleness journeys are designed; the pilot targets one reader segment. **v2 → v3:** the fact-check dataset now includes natural errors and blind, wider-typed planted errors, with human-edit recall measured; detector labels are blinded; vague gates are made computable; cost and latency are in `make eval`; hours per title are reconciled to 7 h. Safety thresholds are unchanged. |

---

## 0. BLUF

- **What:** spot topics rising *before* they peak, then publish short, sourced, human-edited explainer ebooks (12–20k words) on Kindle for curious professionals.
- **Two risks decide this, and neither is technical.** (1) **Readers may not pay** when a free chatbot answer, a YouTube video, or Wikipedia is one tap away. (2) **The product could look like the AI trend-book spam Amazon is cracking down on.** The whole business runs on one KDP account. So trust is the product: every claim is sourced, a human edits every book and clicks publish, AI use is disclosed to KDP *and* readers, risky categories are excluded, and there are no bots on the account.
- **Plan: buy evidence in the cheapest order.**
  - **Gate A** (~30–36 h, ≤$20): a demand scan against *free* alternatives, plus **one hand-made book** checked by a single fact-check script, time-logged end to end, with blind planted errors and a log of the errors the edit actually finds. Nothing gets built or published before this gate.
  - **Gate B** (~+35 h, ≤$50 total): a lean pipeline with safety evals.
  - **Scale decision** (~91–112 h, ≤$200 total): based on a 5-title pilot.
  - At ~5 h/week, the scale decision is about 18–22 weeks out (91–112 h ÷ 5). That is slow but honest.
- **Economics (estimates):** at $3.99, about **$2.73** royalty per copy. Cash cost per title is about **$30**, so cash break-even is about **11 copies**. Counting about 7 h of Srini's time per title (6 h edit and verify + 1 h packaging and publish) at a placeholder $50/h, break-even is about **140 copies**. **Time is about 92% of the cost.**
- **Decisions needed from Srini (L3):** see §14. The first one gates everything: check your employer's outside-activity policy.

---

## 1. Problem and evidence

### 1.1 Reader problem and real alternatives
When a new topic suddenly matters (a technology, a regulation, a standard, a scientific result), a curious professional has these options:

| Alternative | Cost | Weakness | Our bet |
|---|---|---|---|
| **Ask a chatbot** | Free, instant, personal | Unsourced or uneven sourcing; no structure the reader didn't ask for; can't tell what's uncertain | **Main competitor.** [26% of US adults (46% under 30) have used ChatGPT to learn something new](https://www.pewresearch.org/short-reads/2025/06/25/34-of-us-adults-have-used-chatgpt-about-double-the-share-in-2023/) (Pew, n=5,123, early 2025) |
| YouTube, news, SEO pages, Wikipedia | Free | Fragmented, hot takes, or not yet written | Free and good enough for many readers |
| Long books | $15–30, months to years late | Arrive after curiosity fades | Too slow |
| Fast AI books | $1–5 | Generic, often wrong | Damaged trust in the whole category |

**JTBD:** *"When a topic suddenly matters at work, give me a coherent, sourced picture in about an hour (what it is, why now, what's still unknown) that I can trust more than a quick chatbot answer."*

**The hypothesis we must test:** readers will pay $3.99 for **curation, structure, verified sources, and honest uncertainty** over a free answer. We have **no evidence yet** either way (ux-lead R1).

### 1.2 Evidence we have
| Claim | Source | Status |
|---|---|---|
| AI-generated text, images, and translations must be disclosed at publish. Content created by AI counts as AI-generated even after substantial edits; AI-assisted editing doesn't. | [KDP Content Guidelines](https://kdp.amazon.com/en_US/help/topic/G200672390) | Verified |
| KDP may reject "typically disappointing" content and misleading descriptions, and can remove titles (with appeal). It accepts freely available copyrighted material only from the owner. | Same page | Verified. We publish original synthesis, never reproduced web text (C7) |
| 3 new titles per account per day. AI-flagged titles reportedly get more review. | context.md | Cap verified; extra review reported |
| 70% royalty requires a $2.99–$12.99 list price minus delivery (~$0.06 average/unit). 35% otherwise. 70% applies only in eligible territories, so the blended rate is lower (size unknown). | context.md; cto-partner 🟡9 | Verified; blend unknown |
| Kindle samples are a fixed 10% of the book | [KDP help](https://kdp.amazon.com/en_US/help/topic/G200644250) | Verified (via ux-lead) |
| The KDP AI disclosure is not shown on the product page | [ScribeCount](https://scribecount.com/author-resource/protecting-books/kdp-ai-content-policy-the-compliance-guide) | Reported (one third-party source) |
| A page-count change >10% needs a new edition. Past buyers may not receive updates. | [KDP help](https://kdp.amazon.com/help/topic/G202176900); [Gatekeeper KB](https://kb.gatekeeperpress.com/help/when-i-update-my-ebook-will-previous-customers-see-the-changes) | Edition rule verified; update delivery reported |
| KENP rate $0.00420–$0.00482/page, Jan–Apr 2026 | [profitable.app tracker](https://profitable.app/kdp/stats/kenp-rate) | Third-party, unverified |
| No stable Google Trends API. Reddit commercial use needs an agreement. X is pay-per-use. Amazon PA-API is deprecated. | cto-partner 🟠4 (sources cited there) | Mostly third-party, consistent |

### 1.3 Unknowns (labeled, not guessed)
| # | Unknown | Answered by |
|---|---|---|
| U1 | Will readers pay over free alternatives? | Gate A (substitution audit, review mining), then the pilot |
| U2 | Sales per title and the curve over days 1–90 | Pilot |
| U3 | Lead time vs. flag-to-live latency: can we be live *before* the peak? | Gate A (one book), Phase 1 replay, pilot |
| U4 | Rating, review, and refund rates on AI-disclosed titles | Pilot (counts, verbatim coding) |
| U5 | How KDP review treats disclosed, sourced, human-edited books | Pilot (observe only) |
| U6 | Srini's real hours per book, and where they go | Gate A time log |
| U7 | Discoverability with no ads and no author platform | Pilot |
| U8 | Detail-page views and sample-to-buy conversion | **Not visible in KDP reports (assumed). We will be mostly blind to the funnel.** |

---

## 2. Target reader

**Pilot segment: the curious professional only** (ux-lead R4). This is a PM, analyst, or manager who meets a new tech, science, or regulatory term at work and needs to be fluent quickly. What good looks like to them: a clear structure, why it matters now, what's uncertain, and where to go deeper.

Deferred until the pilot reads out: practical adopters and KU browsers. Out of scope: anyone seeking medical, legal, tax, or investment guidance; book summaries; breaking news.

Three roles: the reader is the customer, Srini-as-editor is the operator, and **Amazon/KDP is the gatekeeper** whose policies and reviewers decide whether we exist.

---

## 3. Goals and non-goals

### Goals
1. **G1 Buy evidence in the cheapest order.** Each gate has a hard budget, and passing one is the only thing that unlocks the next. The hours are cumulative estimates and get re-baselined at each gate from the time logs:

| Gate | Unlocks | Cumulative hours | Cumulative cash |
|---|---|---|---|
| **A** | Phase 1 build | ≤38 h | ≤$20 |
| **B** | Pilot publishing | ≤78 h | ≤$50 |
| **Scale decision** | Phase 3 | ≤115 h | ≤$200 |

2. **G2 Be live before the peak.** Median detection lead time must be at least median flag-to-live latency plus 7 days.
3. **G3 Be the trustworthy option.** Every title passes the safety floors in §9 and has no confirmed material factual error.
4. **G4 Protect the KDP account.** Zero content-quality notices, zero removals, and no automation touching the account.
5. **G5 Fit a side project.** Each title takes ≤8 h of Srini's time, and he publishes at most one title a week.

### Non-goals
- Volume publishing, extra accounts, or anything near the 3/day cap.
- Autonomous publishing. A human edits and clicks publish on every title in v1.
- Medical, financial, legal, or tax topics; politics and elections; breaking news, crime, or tragedy; living private people; celebrity biographies; summaries of copyrighted books.
- Print, audio, translations, AI-generated covers, or paid ads in the pilot.
- Scraping Google Trends or Amazon, using Reddit or X data, or using any bot or script on the KDP account (L3 rule, §14).
- Any use of employer time, equipment, data, or contacts.

### Options considered
| Option | Verdict |
|---|---|
| **A. KDP ebooks** | **Pilot.** Cheapest way to see whether real buyers show up. |
| B. Direct sales + newsletter | Pivot if demand exists but the platform creates friction. |
| C. Sell the trend intelligence itself | Later, if detection turns out to be the strongest asset. |

---

## 4. Product concept

```
[1 Detect] → [2 Score & pick] → [3 Research pack] → [4 Outline] → [5 Draft]
  → [6 Fact-check + safety checks] → [7 Human edit] → [8 Detail page & sample]
  → [9 Compliance checklist + human publish] → [10 Monitor, correct, retire]
```

1. **Detect** (cto-partner 🟠4). Day-one sources are all free and ToS-clean:
   - **Wikipedia pageviews** (primary signal and ground truth for "peak")
   - Hacker News (Algolia)
   - arXiv
   - GitHub star velocity
   - Regulator feeds (e.g. the Federal Register)

   Google Trends is a **manual browser check on shortlisted topics only**. Explanation gap is a **manual ~10-minute Kindle store check**. We don't scrape Amazon.
2. **Score and pick.** Each topic is scored on:
   - rise velocity
   - **durability**
   - **time-to-peak** (slow risers preferred: regulations with effective dates, standards, staged platform rollouts)
   - explainability (enough stable primary sources to cite)
   - a policy-risk gate (exclusion is a hard gate, not a score)

   Srini picks every topic. **Pilot rule: durable topics only.** A book that lands two weeks late must still be useful.
3. **Research pack** (cto-partner 🟠5). For each source, store the URL, retrieval date, and only the minimal passages needed, never full article bodies. Respect robots.txt and prefer primary sources (government, standards bodies, papers, official docs). The draft may use only facts in the pack.
4. **Outline.** The fixed structure:
   - a 60-second summary
   - what it is
   - why now
   - how it works
   - who it affects
   - debates and unknowns
   - what to watch, with dates
   - a glossary
   - sources

   Each chapter gets a 2–3 line takeaway and a reading time (ux-lead R11).
5. **Draft.** A Sonnet-class model drafts section by section from the pack. Citations are **numbered endnotes per chapter** with back-links, and repeated sources are grouped. Sources are named in the text only where that builds trust (ux-lead R5).
6. **Fact-check and safety checks.**
   - Claims are extracted with a Haiku-class model.
   - Each claim is checked against its cited passage by a **separate judge configuration**. The judge sees only the claim and the passage, never the draft's reasoning (cto-partner 🟠6).
   - Deterministic checks: verbatim quotes, link resolution, similarity, brand terms, policy checklist, EPUB structure and accessibility.
7. **Human edit (Srini)** in a simple editor queue (§12):
   - resolve every flag
   - spot-check unflagged claims
   - rewrite for voice
   - score the book on the edit checklist (§9.3)
8. **Detail page and sample** (ux-lead R2, R7). Spec below.
9. **Compliance checklist (§6), truthful KDP AI disclosure, human publish.**
10. **Monitor, correct, retire.**
    - Monitoring uses a **weekly manual KDP report export**, ingested by a script (cto-partner 🟠8).
    - Code reviews verbatim.
    - Re-check sources at 30 and 90 days.
    - Correct, re-edition, or unpublish as needed (staleness design below).

### 4.1 Detail page and sample spec (the pre-purchase trust journey)
Before purchase, a buyer sees only the thumbnail, title, price, stars, the description, and a fixed **10% sample**: about 1,500 words for a 15k-word book (15,000 × 0.10). The in-book trust features are invisible until purchase, so the page and the sample have to carry the trust.

- **Title:** the plain topic name plus a promise. **Subtitle:** includes "as of [Month Year]". No "Ultimate" or "Complete", and no keyword stacking.
- **Description template:**
  - what you'll be able to do after reading
  - who it's for and who it isn't for
  - length and reading time
  - the as-of date
  - one line on how the book was made
  - the source count
  - a link to the book's web page
- **Sample order:**
  - minimal title page
  - a short "How this book was made" (≤80 words; the full version goes at the back)
  - a table of contents written as reader questions
  - the 60-second summary
  - chapter 1

  **The sample must deliver at least one real insight.**
- **Imprint and series:** one imprint, a consistent cover system, and a series name, so that titles 2–5 build recognition. Covers come from human templates and are not reused unchanged.
- **Spam-pattern check** (C5):
  - no stacked keywords in the subtitle
  - no near-duplicate titles in the catalog
  - categories and keywords chosen for relevance, not popularity

### 4.2 Staleness design (ux-lead R3)
Trend explainers are the books most likely to go out of date, and past buyers may never get updates.

- **Edition stamp** in the front matter ("Edition 1.0, verified as of …") and a **changelog** at the back.
- **A web page for each title**, linked from the front and back matter: errata, "what's changed since publication," and archived sources. It can be updated without republishing.
- **Corrections stay under the 10% page-change line.** Anything larger becomes a new edition or a new title.
- **Reader-visible states:**
  - **current**
  - **partly outdated:** the description and the web page say so
  - **superseded:** points readers to the new edition
  - **unpublished**

### 4.3 Reader-facing trust features
- The "How this book was made" page (wording is L3, §14)
- Endnote sources plus the hosted source page
- A "What we don't know yet" chapter
- The as-of date
- An error-report contact and the errata link at **both the start and the end** of the book

---

## 5. Autonomy per step

| Step | v1 autonomy | Human-in-the-loop point |
|---|---|---|
| 1 Detect | Act autonomously (monitored) | Weekly candidate review |
| 2 Score & pick | **Suggest** | Srini picks every topic |
| 2b Policy gate | Act autonomously **to exclude only** | Overriding an exclusion needs a logged reason; never auto-include |
| 3–5 Research, outline, draft | Draft | Srini approves the source list and outline, and edits everything |
| 6 Fact-check and safety checks | Act autonomously (blocking) | Only Srini can clear a flag |
| 7 Edit | Human | n/a |
| 8 Metadata | Draft | Srini approves |
| 9 Publish | **Human only, never automated** | Srini answers the disclosure questions and clicks publish |
| 10 Monitor | Alerts from a manual export; **no bots on KDP** | Srini decides corrections and retirement |

Any increase in autonomy is L3 and needs eval evidence.

---

## 6. Compliance and content-quality guardrails (hard rules; failing any one blocks publish)

| # | Guardrail | Enforcement |
|---|---|---|
| C1 | Truthful KDP AI disclosure (drafted text is AI-generated under KDP's definition) | Checklist; Srini attests |
| C2 | Reader disclosure page in every book | Presence check |
| C3 | Every factual claim cites the pack; zero unresolved unsupported claims at publish | Fact-check gate + resolution log |
| C4 | As-of date in the subtitle, the description, and the front matter | Deterministic |
| C5 | Honest, non-spammy metadata: title and description match the content; no implied authority or affiliation; spam-pattern check passes | Checklist + brand scan; Srini approves |
| C6 | A real name or imprint; no invented experts or bios | Checklist |
| C7 | No reproduced text beyond short attributed quotes: **no run of more than 25 consecutive words matching a source** (proposed; L3), a per-section paraphrase-similarity check, and every quoted string verbatim in the pack | Deterministic |
| C8 | No brand names in the title or subtitle (pilot rule, L3); no logos | Brand-term scan |
| C9 | Excluded categories (§3) | Srini's pick + checklist + simple classifier |
| C10 | No padding or filler | Edit checklist |
| C11 | At most 1 title/week in the pilot and 4/month at scale | Process |
| C12 | One account; no review manipulation; **no bots, scripts, or stored KDP credentials anywhere** | Process |
| C13 | Confirmed factual errors are fixed within 7 days through the errata page and a correction; material errors that can't be fixed lead to unpublishing | Playbook (§9.5) |
| C14 | **Accessibility:** logical headings, real text (not images of text), tables with headers, alt text ≤140 characters on informative images, contrast ≥4.5:1, and KDP accessibility metadata set explicitly (not "I don't know") | EPUB check + checklist |

Legal interpretation of any of these (trademark, what counts as "advice," publisher terms) is L3.

---

## 7. Unit economics (estimates; arithmetic shown)

### 7.1 Royalty per copy
| Price | Option | Arithmetic | Royalty |
|---|---|---|---|
| $0.99 | 35% | 0.99 × 0.35 | $0.35 |
| $2.99 | 70% | 2.99 × 0.70 − 0.06 | $2.03 |
| **$3.99 (pilot)** | 70% | 3.99 × 0.70 − 0.06 | **$2.73** |
| $4.99 | 70% | 4.99 × 0.70 − 0.06 | $3.43 |

- The blended royalty will be a little lower, because some territories pay only 35%.
- **KU (third-party rate, unverified):** about 70 KENP pages × $0.0042–0.0048 ≈ **$0.29–$0.34** per full read. Treat this as upside.
  - **"~70 KENP pages" is an assumption.** KDP doesn't publish a words-per-KENP formula. We assumed roughly 200–250 words per KENP page (an *unverified* rule of thumb): 15,000 / 250 = 60 and 15,000 / 200 = 75, so we used a central value of ~70. Replace it with the actual KENP count KDP shows on the Bookshelf after the first upload.
- We won't test price inside a 5-title pilot.

### 7.2 Cost per title
| Item | Arithmetic / assumption | Estimate |
|---|---|---|
| LLM tokens | ~2.4M input + ~0.3M output. The fact-check judge carries ~2k tokens per claim across ~300–500 claims. Sonnet-class at a reported $2/$10 per M: 2.4×2 + 0.3×10 = $7.80, ×2 for retries = $15.60. Opus-class worst case is about $31. **Verify prices on Anthropic's pricing page.** | **$10–20, central $15** |
| Cover | Human template | $15 |
| ISBN, trend data | Not needed for Kindle; free sources only | $0 |
| **Cash per title** | 15 + 15 | **~$30** |
| Srini's time | **6 h** edit and verify + **1 h** packaging (metadata, cover, publish) = **7 h per title** | **~7 h** (replaced with the Gate A time log) |
| Fixed (not per title) | Any Claude subscription Srini uses to build | Counts against the monthly cap? **L3** |

### 7.3 Break-even per title (v1's 160 corrected to 161 at v1 inputs; v3 inputs below: $30 cash, 7 h)
| Price (royalty) | Cash only ($30) | With time, 7 h × $50 + $30 = $380 |
|---|---|---|
| $2.99 ($2.03) | 30 / 2.03 = 14.8 → **15** | 380 / 2.03 = 187.2 → **188** |
| $3.99 ($2.73) | 30 / 2.73 = 11.0 → **11** | 380 / 2.73 = 139.2 → **140** |
| $4.99 ($3.43) | 30 / 3.43 = 8.7 → **9** | 380 / 3.43 = 110.8 → **111** |

- $50/h is a placeholder; Srini sets the real value (L3).
- At these inputs, time is 350 / 380 ≈ **92%** of the cost.
- The main lever after demand is proven: cut edit hours without cutting quality. The Gate A time log tells us whether flag resolution or rewriting dominates (cto-partner Q6).

### 7.4 Budget by gate (estimates; re-baselined from time logs)
| Stage | Hours | Cash | Basis |
|---|---|---|---|
| Phase 0: demand scan + review mining + substitution audit | ~13 | $0 | 8 + 3 + 2 h |
| Phase 0b: one hand-made book + one fact-check script | ~17–23 | ≤$20 | Script 6–8 h, research and draft 2 h, edit 6 h, packaging 1 h, logging; **+2–3 h** for blind seeding, the B2-natural log, and the blind first-pass edit (evals-lead Fix 1) |
| **Gate A total** | **~30–36** | **≤$20** | |
| Phase 1: lean pipeline, replay backtest, safety golden sets | ~30–40 | ≤$30 | Detector replay 10–15 h, pipeline 8–12 h, safety labeling ~10 h |
| **Gate B total** | **~60–76** | **≤$50** | |
| Phase 2: 4 more titles (the Phase 0b book becomes title 1 if it passes) + 12 weeks of monitoring | ~31–36 | ~$135 | 4 × 7 h + 12 × 0.5 h + 1 h for the blind first pass on pilot title 2 only; 4 × $30 + one cover for title 1 |
| **Scale decision total** | **~91–112** | **≤$200** | ~18–22 weeks at an assumed 5 h/week |

---

## 8. Metrics and targets (proposed; Srini approves; L3)

### Success
| Metric | Pilot target (5 titles) | Scale target |
|---|---|---|
| Median copies per title at 60 days | ≥11 (cash break-even) | ≥140 (time-adjusted, at the $50/h placeholder) |
| Titles at cash break-even by day 90 | ≥3 of 5 | ≥70% |
| **Flag-to-live latency** (time-logged) | Measured; median ≤14 days | ≤10 days |
| **Lead time − latency** (median) | ≥7 days | ≥10 days |
| **Share of 60-day sales before the peak** (Wikipedia pageviews) | Measured (tests "early beats fast") | Goal set after the pilot |
| Titles with all key facts still valid at 90 days | ≥4 of 5 | ≥90% |

### Guardrails (a breach means stop and review)
| Metric | Floor |
|---|---|
| KDP content notices, rejections, or removals | **0.** Any one pauses all publishing. |
| Confirmed material factual errors in published text | **0.** Any one pauses the next title until the eval gap is fixed. |
| Reviews coded "wrong," "AI/generic," or "misleading" | Each one is logged verbatim and root-caused. At low N, **counts and verbatims, not averages** (ux-lead R8). |
| Star rating | <3.5 on any title with ≥3 ratings → review, then fix or unpublish |
| Refunds | Report the count; investigate any title with ≥2 refunds (KDP baseline unknown) |

### Cost
| Metric | Target |
|---|---|
| LLM cost per title | ≤$20 (alert at $20) |
| Cash per title | ≤$35 |
| Srini's hours per title | ≤8 h in the pilot; ≤5 h at scale |
| Monthly spend | ≤ the L3 cap, enforced by a **console spend limit** (hard stop) |

**Blind spot:** KDP doesn't report detail-page views or sample conversions (assumed). We'll read the funnel only through sales, KENP reads, reviews, and reader conversations (if approved).

---

## 9. Eval plan

### 9.1 Scope decision: resolving the conflict between cto-partner and ux-lead
**Position:** for the pilot, run **safety floors and cheap deterministic checks only.** Defer calibrated LLM judges for clarity, coverage, padding, explanation gap, and reading flow to Phase 3. Fold ux-lead's new criteria into either deterministic checks or Srini's edit checklist.

**Why (the evidence is cost versus benefit):**
- Each calibrated judge needs about 20–30 human-labeled items plus a calibration run. Across 5 non-safety dimensions that's about 15–20 h (cto-partner's labeling estimates), which is roughly half of Phase 1.
- In the pilot Srini reads every word of every book, so a quality judge can't catch anything he won't. A judge earns its cost only when it replaces part of the human read, which happens at scale.
- Safety dimensions are different. Fact-check misses, invented quotes, copying, and policy breaches can hurt readers or the account even with a human in the loop, because humans miss things in a 15k-word read. Those keep full evals now. **This argument depends on how often the human edit actually misses things, so v3 measures it** (human-edit recall, §9.2).
- **Srini's edit-checklist scores (1–5 per dimension, logged per book) double as the future calibration labels.** Deferring costs nothing, and the Phase 3 judges start with about 5–10 labeled books.
- Where ux-lead's criteria can be made deterministic, they run in the pilot:
  - **sample check:** the summary and chapter 1 begin within the first 10% of the word count
  - **EPUB accessibility check**
- The human-centered reader criteria (comprehension, time to first insight, paid vs. free) are the best evidence for U1. They need real readers, so they're an **L3 option** (§14 #9) and not a gate.

### 9.2 What we evaluate, and when
| Dimension | Grader | Gate A (1 book) | Phase 1 exit (Gate B) | Pilot, every title | Scale |
|---|---|---|---|---|---|
| **Script recall, B2-planted** (blind seeds, §9.3) | Deterministic vs. sealed seed key | ≥18/20 (smoke test, not a recall estimate; see note) | ≥45/50 | ≥48/50 | ≥97/100 (enlarged set) |
| **Script recall, B2-natural** (real errors found in edits, audits, and reader reports) | Deterministic vs. the natural-error log | Report count (n will be small) | Report count | Report count; target proposed for Srini (L3 #8): ≥80% once n ≥20 | Proposed ≥90% once n ≥50 |
| **Human-edit recall** (Srini's blind first pass, before seeing script flags; **run on the Gate A book and pilot title 2 only**) | Deterministic vs. sealed seed key + natural log | Report count | n/a | Report count (title 2 only) | Re-run once if the edit process changes |
| **Combined recall** (script ∪ human) | Same | Report count | Report count | Report count | Report count |
| **Claim support, pre-human** | Judge calibrated to Srini's labels (kappa ≥0.70 on ≥100 claims before trusted) | Report % (uncalibrated, labeled as such) | ≥85% | ≥90% | ≥93% |
| Citation resolves and supports | Link check + judge | Report | ≥90% | ≥95% | ≥97% |
| **Policy/adversarial (T2 ~60 topics, B3 ~20 prompts)** | Classifier + checklist vs. labels | Checklist only | **100%** (counts reported) | **100%** | 100% |
| Clarity, coverage, padding, reading flow | **Srini's edit checklist** (1–5) in the pilot; calibrated judge at scale | Logged | Logged | Mean ≥4 self-scored | Judge (calibrated) ≥4.0 |
| Sample check, EPUB/accessibility | Deterministic | Pass | Pass | Pass | Pass |
| Detector: replay lead time | Deterministic (Wikipedia pageviews) | n/a | Median ≥ measured latency + 7 d | Same | Same + 3 d |
| Detector: weekly precision@10 (topics Srini would pick) | Srini's labels on replay output | n/a | ≥3 of 10 | ≥4 of 10 | ≥5 of 10 |
| Detector: spike rejection | Deterministic on replay | n/a | ≥70% | ≥80% | ≥85% |

**Hard safety floors (every phase, block regardless of other scores):**
- 100% on policy and adversarial sets
- **zero** unresolved unsupported claims in published text
- **zero** quoted strings not found verbatim in the pack (deterministic)
- **zero** source runs over 25 words (proposed; L3) and a passing paraphrase check
- clean brand scan
- AI disclosure answered and the reader disclosure page present

Quality numbers are **always reported as counts, with dataset version, size, and grader** (e.g. "47/50 planted errors, B2-planted v1, deterministic"). **Every count also carries the single-labeler caveat** ("labels: Srini only, no agreement check") until a second labeler is approved (§14 #12). Changing any threshold is L3, and never to make a book pass.

**Reading small counts honestly:** 18/20 has a 95% confidence lower bound near 68% (per evals-lead), so Gate A's 18/20 is a smoke test that the script works, not evidence of 90% recall. Real recall claims wait for the n=50 and n=100 sets, and for B2-natural.

**Safety thresholds are unchanged from v2.** The B2-natural and human-edit rows are new measurements. The only new targets proposed (B2-natural at pilot and scale) go to Srini under §14 #8.

**Note for evals-lead:** v1's detector thresholds were set on a curated topic set that couldn't measure precision (cto-partner 🔴3). v2 redefines the dataset, so the detector bars above are **new proposals for Srini's approval, not a weakening of the v1 bars.** The content and safety bars are unchanged or stricter (the overlap limit drops from 50 to 25 words).

### 9.3 Datasets
- **Replay backtest (T1-v2), replacing v1's curated T1:**
  - Run the detector over archived raw signals (Wikipedia pageviews daily history, HN, arXiv, GitHub), using **only data timestamped at or before each simulated flag date**. No look-ahead.
  - Score *every* candidate the replay emits. Random negatives come for free.
  - Peak = the maximum of absolute daily pageviews.
- **T2 policy** (~60 topics, ~40 excluded, adversarial disguises included) and **B3 adversarial** (~20 prompts). These stay small, so counts are reported.
- **B2-planted** (20 at Gate A, 50 at Phase 1, 100 at scale), **generated blind** to the checker and to Srini (evals-lead Fix 1):
  - A separate model configuration generates the seeds. It uses its own prompt and never sees the checker's prompts or code.
  - Seed positions and the answer key go into a sealed file (hashed and timestamped) that Srini doesn't open until scoring. The scoring script reads the key; Srini doesn't.
  - **Error types are widened** beyond the easy ones (wrong dates and numbers, invented quotes, misattributions) to the errors LLM explainers actually make:
    - partly supported claims
    - outdated facts that were once true
    - overgeneralization ("all" or "always" where the source says "some")
    - unit or scale slips (million vs. billion, % vs. percentage points)
    - unsupported causal claims ("X caused Y" where the source shows only correlation or sequence)
  - Type mix is roughly even, and recall is reported per type.
- **B2-natural** (grows from the Gate A book onward). **Its recall is an upper bound:** errors nobody ever catches never enter the set. The 30-day hand re-check of 20 sampled claims partly offsets this. **every factual error found** during an edit, a 30-day audit, or a reader report is logged as a labeled item, whether or not the script caught it. Each item records the claim, the source passage, the error type, and whether the script caught it, the human caught it, or both missed it until later. Anything both missed is root-caused before the next title.
- **Human-edit recall procedure:** Srini does his first edit pass on the seeded draft **without the script's flags**, logging every error he finds. Only then are the flags shown. This measures script, human, and combined recall on the same items at no extra labeling cost (about +1 h per book).
  - **Scope:** the blind pass runs on the Gate A book and pilot title 2 only. Doing it on every pilot title would push edit time to the 8 h cap with no slack (evals-lead v3 Fix 1). Two measurements are enough to check §9.1's claim that humans miss things. Other pilot titles get the normal edit, with flags visible.
  - **Seeds never ship (hard check):** seeded drafts are separate files that are never used as the publish source. Before any EPUB is uploaded, a deterministic check confirms that none of the sealed key's seed strings appear in the publish build. Any match blocks publishing.
- **Claim calibration set:** ≥100 claims labeled by Srini (supported / partly / unsupported), drawn from **at least 3 topics**: the Gate A book plus two unedited draft chapters on different topics generated in Phase 1 (<$5 tokens). This widens the long tail beyond one book.
- **Edit-checklist log:** per-book 1–5 scores. These become the Phase 3 calibration labels.
- **Detector precision@10 labels are blinded** (evals-lead Fix 2):
  - The labeling tool shows each replay candidate with data **only up to its simulated flag date**. Later pageviews, the peak, and outcomes are hidden.
  - Each label is saved with a timestamp **before** the outcome is revealed. The reveal script refuses to show outcomes for unlabeled candidates and rejects labels edited after reveal.
  - Srini marks any candidate he already recognizes as "later became big". Those are excluded from precision@10 and reported separately, because memory leaks hindsight even through a blind tool.
- All datasets are versioned. **Single-labeler limitation:** Srini labels everything, so no agreement can be measured. Blind seeding and blinded detector labels reduce the bias but don't remove it. A second labeler for a 30-claim safety subset (report kappa) is an L3 option (§14 #12).

### 9.4 Regression
`make eval` reruns all of the above on any change to a prompt, model, source, or check, and stores results with dataset, model, and date.

**Blocking rules (defined):**
- **Safety floors:** any regression blocks.
- **Core % metrics** (claim support, citation): a drop of **more than 2 percentage points** vs. the last passing run blocks pending review.
- **Count metrics on sets of n ≤100** (B2-planted, T2, B3): **any item that passed before and now fails** blocks pending review.
- **Cost:** `make eval` runs the pipeline on a fixed reference book and reports estimated **cost per title**. Above $20 blocks (this matches the §8 alert); more than 25% above the last passing run blocks pending review.
- **Latency:** the same reference run reports pipeline wall-clock time per stage. More than 2× the last passing run blocks pending review. The human parts of flag-to-live latency come from the time log, and the `make eval` report shows them next to the pipeline figure.

### 9.5 Online evaluation and playbooks
- **Signals** (from the weekly manual KDP export plus Amazon pages read by hand):
  - units sold
  - KENP pages read
  - ratings
  - **every review logged verbatim with a link**, coded as stale / wrong / generic / AI / helpful
  - refunds
  - reader error reports
  - KDP notices
- **30-day audit:** re-run the fact-check against live sources, and re-verify 20 sampled claims by hand.
- **Drift:** link and fact re-check at 30 and 90 days; update the title's state (§4.2).
- **A factual error is reported** (by a review or a reader): verify → fix → update the errata page and changelog → republish under the 10% line. **Never** ask for a review to be removed or edited.
- **KDP notice:** pause all publishing → read it → respond → root-cause → Srini decides whether to resume.
- **Low ratings with a credible pattern:** unpublish the title.

---

## 10. Phased rollout

### Phase 0: Demand vs. free alternatives (~13 h, $0, L1)
For 10 slow-rising tech, science, or regulatory topics from the last 6–12 months:
- **Kindle scan:** list the existing titles, their dates, ratings, and review counts; note BSR (directional only); count the spam-like titles.
- **Review mining:** read the 1–3★ and 4–5★ reviews, log them verbatim with links, and code them ("could've Googled," "outdated," "AI," "clear overview," "saved time").
- **Substitution audit:** rate the best free alternative (top search result, top YouTube video, Wikipedia, one chatbot answer) 1–5 on four things: coherent, current, sourced, honest about unknowns.

**Gate to 0b** (proposed; L3): for ≥3 of 10 topics, (a) there are buyers (reviews or ranks), (b) review language shows readers valued a *structured overview*, and (c) the best free alternative scores ≤3/5.
**Kill/pivot:** if free alternatives already do the job, or nobody buys, stop, or consider Option C.

### Phase 0b: One hand-made book (~17–23 h, ≤$20)
- Pick **one** durable topic from Phase 0 using Wikipedia pageviews.
- Produce the book with Claude Code and **one script:** claim extraction → claim-vs-passage judge → verbatim-quote check → link check.
- Plant **20 blind seeds** across the widened error types (§9.3) in a separate seeded copy of the draft. Srini does his blind first-pass edit, then sees the script's flags. Every real error found goes into B2-natural. The publish source stays seed-free, and the seed-string check (§9.3) runs before any upload.
- **Record the flag date** (the day the topic would have met the detector rule, from Wikipedia pageviews).
- **Time-log every stage**, including flag-to-ready latency.
- **Publish nothing** until §14 items 1–3 are decided.

**Gate A → Phase 1:**
- the book passes the safety floors
- the script catches ≥18/20 blind planted errors (smoke test). Natural, human, and combined recall are reported as counts.
- edit time ≤10 h
- **computed margin ≥7 days**, where margin = (days from the flag date to the topic's pageview peak) − (measured flag-to-ready days + 3 days of KDP review, an *assumption* to be replaced in the pilot). If the topic hasn't peaked by the time the book is ready, use the days elapsed so far as a **lower bound**; it passes only if the lower bound alone gives ≥7

**Kill:** edit time >12 h, or the script can't reach the floors after two iterations.

### Phase 1: Lean pipeline (~30–40 h, ≤$30 cumulative tokens, offline)
- Build the detector replay on free sources only.
- Harden the script into the pipeline (§12).
- Build the safety golden sets and run `make eval`.

**Gate B → Phase 2:** all Phase 1 exit bars and safety floors in §9.2 are met, and the fact-check judge is calibrated (kappa ≥0.70).
**Kill:** the replay shows median lead time < latency + 7 d (we can't be early, so pivot to durable or evergreen explainers or stop), or the safety floors can't be met.

### Phase 2: Pilot, 5 titles (~30–35 h + 90 days watching, ~$135)
- Title 1 is the 0b book, if it passes the pilot bars; then 4 more.
- At most 1 a week, $3.99, no ads, curious-professional segment, tech/science/regulatory only.

**Gate to Phase 3 (day 90):**
- ≥3 of 5 titles at cash break-even
- zero KDP notices
- zero confirmed material errors
- median edit time ≤8 h
- median lead time − latency ≥7 d

**Kill (any one):**
- a KDP content notice → pause immediately; Srini decides
- median <5 copies per title at day 60
- total royalties < total cash cost at day 90
- latency > lead time on ≥3 of 5 titles (the core "early" thesis fails)
- a confirmed material error that slipped past the pipeline → pause until the eval gap is fixed

### Phase 3: Scale (conditional)
2–4 titles a month, with a human edit always. Turn on the deferred quality judges, calibrated from the edit-checklist labels. Run one variable at a time (KDP Select, price, edition updates, a light ad test), each with a hypothesis, a primary metric, a guardrail metric, and a duration set in advance.

---

## 11. Risks, dependencies, rollback

| Risk | L / I | Mitigation | Rollback |
|---|---|---|---|
| **Readers prefer free answers** | High / High | Phase 0 substitution audit; differentiate on sources, structure, and uncertainty | Stop at Gate A at ~13 h, or pivot |
| **KDP account action** | Med / **Severe** | Disclosure, human edit, low cadence, no bots, honest metadata | Pause; unpublish at-risk titles; appeal |
| **Late to the peak** | Med–High / High | Latency gate; durable, slow-rising topics only | Book remains a durable primer |
| Factual error reaches readers | Med / High | Claim-level fact-check, human edit, 30-day audit | Errata + correction within 7 days, or unpublish |
| Book goes stale | High / Med | Edition stamp, web page, reader states | Re-edition or retire |
| **Srini's time** (new role) | **High** / High | Hard budgets per gate; ≤1 title/week | Pause; no running costs |
| Employer policy conflict | Unknown / High | Check first (§14 #1) | Don't start |
| Data-source ToS | Med / Med | Free, ToS-clean sources only; no scraping | Manual topic picking |
| Copyright or trademark complaint | Low–Med / Med–High | C7, C8, minimal passages, primary sources | Retitle or unpublish |
| Token price changes | Low / Low | Cost alert + console limit | Swap model + regression run |
| KDP policy changes | Med / Med–High | Re-read the guidelines before each publish | Adapt or stop |

**Dependencies:** a KDP account in good standing with the tax interview done; LLM API access with a spend limit; the free data APIs; a cover template; a hosting spot for the per-title web pages; a dedicated error-report inbox.

**Security hygiene:** keys live in env or the keychain, never in the repo. Use a dedicated API key with a console limit. Research packs and logs stay local. No reader PII is collected. Use a personal device only.

**Architecture (boring on purpose):** local Python, SQLite, Markdown → EPUB (pandoc), and a manual or weekly cron run. No server, queue, vector DB, or agent framework.

---

## 12. Acceptance criteria

**Phase 0b (one script):**
- [ ] Claims extracted, each mapped to a pack passage, with a support verdict per claim
- [ ] Verbatim-quote and link checks run deterministically
- [ ] Blind seed generator (a separate model configuration) with a sealed, timestamped answer key; recall reported per error type as counts
- [ ] B2-natural log, with script / human / both-missed recorded per error
- [ ] Blind first-pass edit mode (script flags hidden until Srini's pass is saved), used on the Gate A book and pilot title 2
- [ ] Pre-upload seed-string check: block publishing if any sealed seed string appears in the publish build
- [ ] Stage-by-stage time log, including flag-to-ready latency
- [ ] Cost log per book

**Phase 1 (pipeline):**
- [ ] Detector replay on free sources with point-in-time data; weekly top-10 list with scores and policy-gate result
- [ ] `make eval` runs the replay, T2, B2-planted, B2-natural, B3, claim calibration, and the deterministic checks, plus **cost per title and pipeline latency** on the reference book, with pass/fail against §9.2 and the blocking rules in §9.4
- [ ] Blinded detector labeling tool: data only up to the flag date, labels timestamped before reveal, edits after reveal rejected
- [ ] **Editor queue** (ux-lead R6). This can be a local HTML page or a Markdown file; it doesn't need to be an app. It needs:
  - claims sorted by risk
  - the cited passage shown side by side with each claim
  - one-key supported / unsupported / rewrite
  - automatic time logging per stage
  - resumable 30-minute sessions
- [ ] Compliance checklist (§6) generated per book, with the automated items pre-filled
- [ ] Detail-page and description templates (§4.1) and the per-title web-page template (§4.2)

---

## 13. Open questions
| # | Question | Owner |
|---|---|---|
| Q1 | Does Kindle show the endnotes as pop-ups with our markup? | Srini (Kindle Previewer, Phase 0b) |
| Q2 | Can authors reply to Amazon reviews? (Assumed no; we plan without it.) | pm-twin to verify |
| Q3 | Current KDP review times for AI-disclosed titles | Measured in the pilot |
| Q4 | Is a second labeler needed for the safety sets? | **Answered by evals-lead:** yes, if allowed. A second person labels a 30-claim subset and we report kappa. If not allowed, use blind seeds and state the single-labeler limit next to every count. The decision is §14 #12. |
| Q5 | 10k vs. 20k words | Reader sessions if approved (§14 #9); otherwise the pilot uses 12–15k |
| Q6 | Where do Srini's 6 h of edit and verify time actually go: flag resolution or rewriting? | Phase 0b time log |

---

## 14. Needs Srini's decision (L3)

> **1. Employer outside-activity policy.** **Options:** A) read the policy and proceed if clear / B) ask HR if anything is ambiguous / C) skip the check. **Recommendation:** A, then B if needed. This also covers recruiting readers (#9). **By when:** before Phase 0b. Phase 0 is desk research only.

> **2. Monthly spend cap, with a console hard limit.** **Options:** $50 / $100 / $200 per month, and whether the build-time Claude subscription counts against the cap. **Recommendation:** $100/month as a console limit; keep the subscription outside the cap but list it as a fixed cost. **Evidence:** §7.4 needs ≤$200 total over ~18–22 weeks.

> **3. Reader-facing AI disclosure wording.** **Options:** A) KDP disclosure only / B) KDP disclosure plus the in-book page and a line in the description / C) B plus "AI-assisted" in the subtitle. Draft wordings for B (untested):
> a) "Researched and drafted with AI tools; every claim checked against cited sources and edited by [editor]. As of [Month Year]."
> b) "Written with AI assistance and human editing. [N] sources, all listed. Verified as of [Month Year]."
> **Recommendation:** B, using wording a), because it says what was checked. **By when:** before the first publish.

> **4. KDP Select enrollment** (90-day ebook exclusivity; verify the current terms). **Options:** all / none / split. **Recommendation:** all pilot titles. KU suits short nonfiction, and we have no other channel. Don't split 5 titles.

> **5. Brand names in titles.** **Options:** never / allowed after legal review. **Recommendation:** never in the pilot.

> **6. Data-source rules.** **Options:** A) free, ToS-clean sources only; no scraping of Google Trends or Amazon; no Reddit or X; no automation on the KDP account / B) also apply for the Google Trends API alpha (you'd accept its terms) / C) use paid scrapers or resellers. **Recommendation:** A now. B is optional and low priority. **Never** C, because of the account and ToS risk.

> **7. Publisher content terms.** Is fetching and briefly quoting a given news publisher acceptable? **Options:** A) primary sources plus publishers whose terms allow it / B) any publicly accessible page. **Recommendation:** A. Prefer government, standards, papers, and official docs; get a legal read before relying on any news publisher.

> **8. Approve the thresholds and gates in §9.2 and §10.** This includes the three changes proposed by review: the **latency gate** (lead time ≥ latency + 7 d), the **25-word overlap limit plus paraphrase check** (tighter than v1's 50 words), and **deferring non-safety judges to Phase 3**. It also includes the new detector bars on the replay dataset and the Phase 0 gate (free alternative ≤3/5). v3 adds: the proposed B2-natural recall targets (≥80% at the pilot once n ≥20; ≥90% at scale once n ≥50), the regression blocking rules in §9.4, the computed Gate A margin with its 3-day KDP review assumption, and the re-baselined gate hours (≤38 / ≤78 / ≤115 h). Safety thresholds are unchanged. **Recommendation:** approve as written. Once approved, they change only with your explicit decision.

> **9. Talking to real readers.** **Options:** A) none; rely on Phase 0 desk research and pilot sales / B) 5–8 short past-behavior conversations plus 3–5 sample tests with curious professionals **from outside your work network** (~9 h, $0) / C) B plus a public fake-door page (~$0–20, public posting). **Recommendation:** B, after #1 clears. It's the cheapest real evidence on U1 (paid vs. free) and Q5. C adds a weak signal and puts the project in public, so skip it for now.

> **10. Hourly value of your time** (for the break-even targets). **Options:** $0 (hobby) / $50 / $100+. **Recommendation:** pick one before the pilot readout so the Phase 3 gate is honest.

> **11. Author identity.** **Options:** A) your real name / B) a pen name with an "edited by" note / C) an imprint name with a real editor named inside. **Recommendation:** C. It keeps your professional life separate without deceiving readers.

> **12. Second labeler for the safety sets** (evals-lead answer to Q4). **Options:** A) none; Srini labels everything, with blind seeds and the single-labeler caveat on every count / B) one trusted person **outside your work network** labels a 30-claim safety subset (~1.5 h of their time), and we report kappa / C) a paid labeler (spend). **Recommendation:** B, once #1 clears; fall back to A if no one is available. **Why:** this is the only way to check whether Srini's labels, which every calibration rests on, agree with anyone else's. **By when:** before Gate B.

---

## Log
- **L1 (pm-twin):** revised to v2 from `reviews/merged-findings.md`, `cto-partner.md`, and `ux-lead.md`. v1 left unchanged. No new web research; new facts come from the reviewers' cited sources and are labeled as they labeled them.
- **L1 (pm-twin, round 2):** v3 applies evals-lead Fixes 1–3 from `evals/verdict-prd-v2.md`. v2 left unchanged. No new web research. Safety thresholds unchanged.
- **L1 (pm-twin, cleanup):** v4 applies the three minor fixes from evals-lead's v3 verdict. Safety thresholds unchanged.
- **L2:** none.
