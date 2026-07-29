# 10. MVP Definition

> The smallest prototype that can demonstrate the core thesis — *translation decisions form an interconnected, reviewable, reusable body of precedent* — without attempting to solve Bible translation.

## 10.1 The thesis test, restated

The MVP succeeds if it can show, with real translators on a real text: (a) capturing a decision costs ≤ ~1 minute beyond the thinking the translator was doing anyway; (b) when a later Issue arises, the system surfaces the relevant earlier Decision with a correct, legible explanation — and translators judge that useful; (c) resolving a key upstream Issue visibly unblocks downstream work. Everything not needed for (a)–(c) is out.

## 10.2 In scope

- Single project, single language, **one book: Jonah** (see 10.5), plus a handful of cross-book issues to exercise canon-wide anchors (Lamb/ḥesed-class key terms).
- Decision Records at Tier 1 + partial Tier 2 ([§7.4](07-user-experience-and-workflows.md)): Issue, Options, Decision with rationale + confidence, Reviews (single consultant role), Observations/Evidence with citations.
- Anchors: text (org versification), lemma/sense (MACULA + SDBH/SDGNT), key-term, category. Cultural-concept and target-feature anchors as free-tag vocabulary (no curated scheme yet).
- Dependencies: `requires-decision` and `requires-completion`, hard/soft, instance + project-wide-by-key-term; deterministic state machine; frontier + reach + newly-unblocked notifications.
- Precedent retrieval: Tier-1 hard surfacing + Tier-2 anchor-intersection ranking with facet explanations; treatments (followed/adapted/distinguished/rejected); suppression on dismissal. **No embeddings** in v1 (test whether anchors alone carry retrieval — that is itself a research question).
- Views: Scripture-first, Decision-first, dependency graph, key-term view, reviewer queue, basic audit log.
- Supersession + invalidation cascade + re-examination queue (this is the thesis's "connected decisions" payoff — it must be in).
- AI, minimal and optional: anchor/category suggestion at Issue creation; drafting a structured record from an informal note; both label-segregated per [§8](08-ai-and-deterministic-boundaries.md). (Cheap to build once the gateway exists, and needed to test the capture-cost claim.)
- Local single-user operation + naive hub sync (append-only event exchange between 2–3 clients; conflict = surfaced merge Issue). Offline function is core to credibility with field partners.

## 10.3 Explicitly excluded

- Cross-project/cross-language precedent (governance-gated; Phase 6 in [§11](11-phased-roadmap.md)) — but the *data model* fields for it (origin class `imported-external`, sharing policy stubs) exist so nothing needs migration.
- Discourse-annotation vocabulary beyond a placeholder tag set; full Levinsohn-seeded SKOS scheme is Phase 2 work with SIL engagement.
- Prioritization beyond reach + frontier (no WSJF scoring, no deadlines/effort).
- Paratext live integration (import of a Biblical Terms XML + USFM text is in; live P10 extension is Phase 2 — MVP is a standalone Electron/web app built on the platform-agnostic core).
- Oral/sign media attachments; multilingual UI; embeddings/vector search; rules beyond the dependency/consistency set; dashboards.
- Any production hardening (auth beyond basic accounts, RLS, backup tooling).

## 10.4 Required data

| Data | Source | License note |
|---|---|---|
| Hebrew text + tokens for Jonah | OSHB/WLC via MACULA Hebrew | PD / CC BY 4.0 |
| Lemma/sense/domain layer | MACULA + SDBH | CC BY / CC BY-SA (registry-tracked) |
| Versification | Copenhagen "org" + mappings | Apache 2.0 / CC BY-SA |
| Key-term seed list for Jonah | Paratext Major Biblical Terms subset (or hand-built ~30-term list if licensing unclear) | verify; fallback is trivial at this scale |
| A working draft translation | pilot partner's real draft, or a constructed scenario using an open English text (WEB) for demos | partner data under project agreement |
| Seed decision corpus | ~40–80 Decision Records entered from the pilot team's actual past decisions (workshop exercise) | partner consent |

## 10.5 Why Jonah

48 verses — enterable in full within a workshop; narrative with dialogue (participant-reference and speech-act issues); theologically loaded key terms recurring at decision-critical points (*ḥesed* 2:8/4:2, *gadol* motif, divine names YHWH/Elohim patterning — a classic discourse-signal problem); the Jonah 2 psalm gives one genre shift; rich cultural-concept content (sacrifice, vows, fasting, lots). It generates every MVP-relevant issue category without requiring OT-wide analysis. (Ruth is the comparable alternative; Jonah's divine-name patterning and psalm make it slightly richer for dependency demonstrations.)

## 10.6 Suggested technology

TypeScript end-to-end (matches Platform.Bible for Phase 2): Electron + React UI; app core as a pure TS library (rules, graph, validation — unit-testable headless); SQLite via better-sqlite3 with FTS5; JSON Schema validation (ajv); Node hub with Postgres for sync demo; AI gateway calling a hosted model API with the audit store from day one. Nothing exotic; the research risk is in the model and UX, not the stack.

## 10.7 Prototype screens

1. Scripture view (Jonah) with badges + selection → "raise question" (the 20-second capture).
2. Issue/Decision Record page: progressive-disclosure card with candidate panels (ruled + ranked, facet chips) and treatment recording.
3. Decision-first worklist with filters incl. "ready" and "leverage."
4. Dependency graph (interactive, SCC clusters, reach on hover).
5. Key-term view (term → senses → decisions → verses).
6. Reviewer queue incl. re-examination items after a supersession.
7. Audit trail pane on any record (events + AI suggestions with provenance).

## 10.8 Human review workflow (MVP-scale)

One consultant role, one policy: key-term and culture-category Decisions require consultant approval; others auto-approve on team-lead review. Batch review mode for a pericope. Dissent recording enabled (it costs nothing and demonstrates the philosophy).

## 10.9 Demonstration scenario (scripted, ~25 minutes)

1. Translator hits *ḥesed* in Jonah 2:8 → raises Issue in ~20s; system auto-anchors, shows no precedent yet; team creates project-wide key-term dependency.
2. Dependency graph shows KT-ḥesed blocking 2:8 and 4:2; leverage list ranks it top with the explanation.
3. Team resolves KT-ḥesed (options, evidence from SDBH, rationale, consultant approves) → both Issues unblock with notifications; 4:2 opens with the precedent surfaced — *same sense, same key term; differing facet: 4:2 sits in the divine-attributes formula (Exod 34:6 intertext)* → translator records `adapted` treatment.
4. New evidence forces revision: KT-ḥesed superseded → cascade flags 2:8 and 4:2 with reasons; team dispositions one `unaffected`, reopens the other.
5. Audit pane replays the whole history, including the AI's anchor suggestions and who promoted them.
6. Consultant-facing payoff: reviewer queue + decision trail as consultant-check preparation ("the rationale is already written down").

Success criteria for the demo audience (professional translators): they can follow every explanation without training; at least one says "I wanted this during my last consultant check." Formal measures come later ([§12](12-efficacy-study.md)); the MVP demo is for credibility and partner recruitment.
