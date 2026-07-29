# 13. Risks and Unresolved Questions

> Ordered within each category by (impact × likelihood), highest first. "Mitigation" means *designed response*, not elimination. Items marked **[open]** have no adequate answer yet and are honest research questions.

## 13.1 Adoption risks (the most likely failure mode)

- **R-A1: The capture problem.** Forty years of design-rationale tools failed on capture overhead ([§3.3](03-research-and-prior-art-landscape.md)). If recording a decision costs more than translators believe retrieval returns, the corpus never reaches critical mass and retrieval can't prove itself — a cold-start death spiral. *Mitigation:* Tier-1 minimalism, AI back-filling, capture-cost as a pre-registered kill criterion ([§12.7](12-efficacy-study.md)), seeding the corpus from existing Paratext notes/Terms data. *Residual:* real.
- **R-A2: Tool fatigue and ecosystem loyalty.** Field teams already run Paratext + FLEx + spreadsheets; another app is a hard sell. *Mitigation:* Platform.Bible extension path; import-don't-replace posture toward Biblical Terms/notes.
- **R-A3: Champion dependence.** Pilot success via one enthusiast doesn't generalize. *Mitigation:* multi-team pilots; measure adoption breadth, not just depth.

## 13.2 Linguistic and modeling risks

- **R-L1: Anchor sparsity/misfit for target-language and cultural dimensions.** Source-side anchors ride on MACULA/SDBH; but D9/D10 (target-feature, cultural-concept) — the dimensions that matter most for cross-language precedent — have no pre-existing ID-spaces and depend on per-project tagging discipline. **[open]** Whether free-tag vocabularies converge enough to support retrieval is an empirical question the MVP must answer.
- **R-L2: False precision in discourse and rationale encoding.** Interpretive judgments (focus structure, illocution, RT rationales) recorded as data can launder opinion into fact. *Mitigation:* attributed-claim-with-confidence representation everywhere ([§3.9](03-research-and-prior-art-landscape.md)); disagreement preserved. *Residual:* users may still read stored claims as facts.
- **R-L3: Misleading similarity.** High-overlap pairs where following the precedent is wrong (same lemma, different sense; same term, different discourse role). *Mitigation:* sense-over-lemma weighting, mandatory distinguishing-facet display, divergence monitoring ([§5.8](05-relevance-and-precedent-framework.md)).
- **R-L4: Versification and text-base mismatches** corrupting cross-project anchor comparison. *Mitigation:* scheme+version stamped on every anchor; Copenhagen mappings; mismatch detection before comparison ([§9.8](09-technical-architecture.md)).

## 13.3 Theological and philosophical risks

- **R-T1: Philosophy capture.** The system could silently privilege one translation philosophy (e.g., treating formal-equivalence rationales as default categories). *Mitigation:* project-defined principles are the only normative layer; rationale taxonomy reviewed across philosophy traditions; no built-in "correctness." *Residual:* the choice of what is easy to record is itself a bias — monitor via pilot diversity.
- **R-T2: Precedent ossification / false authority.** A reviewed corpus can read as "the right answers," chilling fresh exegesis — the stare-decisis failure mode the model explicitly rejects ([§2.4](02-terminology-and-conceptual-model.md)). *Mitigation:* informative-never-authoritative framing in every UI surface; distinguishing made cheap and visible; health flags on stale records. **[open]** Whether framing survives real social dynamics (junior translator vs. cited consultant precedent) needs field observation.
- **R-T3: Doctrinal conflict surfacing.** Recording dissent makes disagreement durable and visible; some teams/orgs may find this threatening rather than healthy. *Mitigation:* project-configurable visibility of dissent records; discovery-phase conversations.

## 13.4 AI risks

Covered in depth in [§8.3](08-ai-and-deterministic-boundaries.md); the residuals worth restating: automation bias despite labeling (measured, with a kill criterion); citation-gate evasion via plausible-but-wrong resource references (spot-audits); model-version drift changing suggestion behavior mid-project (version pinning per project, change logs).

## 13.5 Privacy, governance, and ethics

- **R-P1: Community data sovereignty.** Decision records encode a community's theological/cultural judgments; cross-project sharing is secondary reuse of arguably Indigenous data (CARE, [§3.6](03-research-and-prior-art-landscape.md)). *Mitigation:* Phase-6 hard gate — no live cross-project features before a co-developed governance framework; default-private; revocable publication; consent recorded on the policy object. **[open]** Revocation semantics after import (snapshot persists locally by design) is an unresolved tension between provenance integrity and the right to withdraw.
- **R-P2: Anonymization is weak for tiny communities.** "Anonymized" decisions from a 2,000-speaker language identify the community regardless. *Mitigation:* treat anonymization as one tier, never as sufficient consent; metadata-only sharing tier.
- **R-P3: Security-sensitive projects.** Some projects operate where exposure endangers people. *Mitigation:* Paratext-style confidential tier excluded from all sharing and all aggregate statistics; self-hosted hub option; this constraint is architectural (project isolation), not policy-only.

## 13.6 Licensing risks

- **R-C1: ShareAlike contagion** (SDBH/SDGNT CC BY-SA) into derived lexical layers; **NC exclusion** (BHSA); proprietary quotation exposure (BDAG/HALOT extracts, licensed model translations inside Evidence). *Mitigation:* per-attribute license tracking in the resource registry (MACULA's LICENSE.md as model); citation-pointer pattern for gated resources; legal review before bundling.
- **R-C2: Paratext data terms.** Biblical Terms list redistribution status unclear [unverified]; renderings are project-private. *Mitigation:* round-trip only within the user's own licensed Paratext context; fallback term lists.

## 13.7 Organizational and sustainability risks

- **R-O1: Org-political positioning.** The tool touches consultant authority, agency workflows, and inter-agency data; building without UBS/SIL/unfoldingWord-world engagement invites polite death. *Mitigation:* Phase-0 partnership-first strategy; ship as extension to their platform rather than competitor.
- **R-O2: Funding horizon vs. slow validation — and donor pressure for premature speed claims.** The honest efficacy timeline (Phases 4–8) outlasts typical grant cycles, and funders will want headline numbers before the guarded claims structure can support them. *Mitigation:* stage results (retrieval bench study lands early and is publishable); align with ETEN-style innovation funding; the pre-registered criteria of [§12.7](12-efficacy-study.md) are the standing defense against being talked into a speed claim the data can't carry.
- **R-O4: Metrics coercion.** Leverage scores, capture counts, and time-to-resolution exist to prioritize *work*; a project manager or funder can trivially repurpose them to evaluate *translators* — which would poison both the data (gaming) and team trust (risk named in the companion ChatGPT document). *Mitigation:* no per-person productivity dashboards in the product; reporting is aggregate-only by default; pilot data-use agreements state explicitly that system telemetry is not a personnel-evaluation instrument; per-user metrics exist only in the research context of [§12](12-efficacy-study.md) under informed consent.
- **R-O3: Maintainer bus-factor and preservation.** Field tools outlive their startups or don't matter. *Mitigation:* open source from Phase 2; preservation exports (Burrito + JSONL + PROV) verified restorable ([§11 Phase 8](11-phased-roadmap.md)).

## 13.8 Technical risks

- **R-X1: Platform.Bible pre-1.0 churn** (verified: heavy active development, no stable release seen). *Mitigation:* platform-agnostic core; extension is a thin adapter.
- **R-X2: Sync-tooling churn** (ElectricSQL pivot; Kùzu death). *Mitigation:* own the append-only exchange; vendors behind abstractions.
- **R-X3: Event-schema evolution over years.** *Mitigation:* versioned event catalog from Phase 1; upcasting discipline; the scope restraint of event-sourcing only the decision log.

## 13.9 The questions that decide the project **[all open]**

1. Does anchor-only retrieval (no embeddings) reach useful precision on a real corpus? (MVP question; falsifies the anchor thesis if no.)
2. Is sub-90-second capture achievable for real questions raised mid-draft? (Kill criterion.)
3. Do target-feature/cultural-concept vocabularies self-organize per project, or do they need curated schemes before cross-project retrieval works? (Gates Phase 6 value.)
4. Can the informative-vs-authoritative distinction survive contact with real team hierarchies? (Ethnographic question, Phase 4.)
5. What are acceptable revocation semantics for shared precedents? (Governance question, Phase 6 gate.)
6. Will SIL-tradition practitioners co-own a machine-readable discourse vocabulary? (Without them it lacks legitimacy; Phase 5.)
