# 7. User Experience and Workflows

> Major views, user journeys, and human review processes. Governing constraint: the design-rationale **capture problem** ([§3.3](03-research-and-prior-art-landscape.md)) — rationale capture succeeds only as a by-product of work translators are already doing. Every workflow below is designed against the question "what is the minimum the user must do, and what does the system do for them?"

## 7.1 Roles

| Role | Primary activities | Authority |
|---|---|---|
| **Translator** (mother-tongue) | draft, raise Issues, record Observations, propose Options, make provisional Decisions | decides at `working` confidence; cannot self-approve gated categories |
| **Team lead / project manager** | prioritize, assign, configure review policy, manage dependencies | approves low-risk categories; manages workflow |
| **Translation consultant** | review Decisions, distinguish precedents, attest procedural completions | approval authority for gated categories (per FOBAI-style practice) |
| **Exegetical / language advisor** | Observations, Evidence, discourse annotations, target-feature profile | advisory; attests analyses |
| **Community reviewer** | comprehension/naturalness verdicts, cultural-concept input | approval voice per project policy; community testing results enter as Evidence |
| **Administrator** | access control, sharing policy, vocabulary management, imports | no translation authority |

A person may hold several roles; authority is per-project and per-category (configured in the project's review policy, [§4.3.8](04-domain-model.md)).

## 7.2 The two primary views

### Scripture-first view

The default working surface — because translators think in text, not in databases. A book/pericope/verse navigator (versification-aware) where each verse shows **badges**: open Issues, approved Decisions, applicable Ruling Principles, key terms present, discourse features annotated, review status, and (on demand) cross-project comparisons. Selecting a span shows everything anchored to it, plus everything anchored to the same lemmas/senses/terms elsewhere ("this word has 3 decided renderings in this project"). Creating an Issue from here auto-fills the text anchor and suggested lemma/sense/term anchors from the token under the cursor (MACULA-backed) — the 20-second capture path.

### Decision-first view

A filterable, sortable worklist over Issues/Decision Records. Filter/group/sort by: status, category, assignee, book/reference, risk, confidence, review state, date, title (alphabetical), **dependency count / % satisfied**, **downstream reach** (the computed importance/impact measure), deadline, "relevant to my current passage." Saved filters cover the brief's browsing requirements (e.g., "key-term Issues, unresolved, reach > 5, ordered by leverage"). Every row answers "why is this here" on hover (leverage components, [§6.4](06-dependency-and-prioritization.md)).

### Secondary views (evaluated per the brief)

| View | Verdict | Notes |
|---|---|---|
| Dependency graph | **MVP** — the thesis is visible here | Interactive DAG (condensed SCC clusters shown as joint-resolution groups); click-through to Issues |
| Key-term view | **MVP** | Term → senses → decisions → verses; Paratext Biblical Terms import/export lives here |
| Reviewer queue | **MVP** | Per-reviewer worklist: Decisions awaiting their role, re-examination queue items ([§6.5](06-dependency-and-prioritization.md)), AI suggestions pending triage |
| Precedent network | post-MVP | Graph of treatment edges; valuable once enough treatments exist |
| Discourse-feature view | post-MVP (roadmap Phase 5) | Spans by discourse anchor; needs the vocabulary to be in use first |
| Language-feature view | post-MVP | Issues/Decisions grouped by target-feature anchor (honorifics, clusivity…) |
| Risk & uncertainty dashboard | post-MVP | Low-confidence approved Decisions, stale precedents, unresolved dissent |
| Cross-project comparison | roadmap Phase 6 (gated by [§5.7](05-relevance-and-precedent-framework.md) governance) | Side-by-side treatment of the same anchor across consenting projects |
| AI activity & audit log | **MVP (basic)** | Every suggestion, retrieval, and acceptance/rejection; filterable by model/version ([§8](08-ai-and-deterministic-boundaries.md)) |

## 7.3 Core workflows

Each workflow lists the *mandatory* user actions; everything else is optional or automatic.

**1. Create a translation question (Issue).** From Scripture view: select span → "raise question" → type title. *Mandatory: title.* Auto: text anchor, suggested lemma/sense/key-term anchors (accept/reject chips), category suggestion, immediate display of rule-surfaced items (governing Principles, co-dependents) and top relevance candidates — capture and retrieval in one gesture.

**2. Record observations and evidence.** Free-text note + optional source picker (lexicon entry, parallel passage, community report, resource registry). AI can draft a citation from a pasted quote; unresolved citations are flagged and cannot silently support approval ([§4.3.5](04-domain-model.md)). A project-level **capture inbox** accepts unanchored quick notes (voice or text — important for oral-preference teams); AI proposes triage into Issues/Observations, humans confirm.

**3. Compare alternatives.** Options listed side by side with per-Option arguments, evidence, and (where drafted) back-translations. AI may add a comparison draft — labeled, sourced, dismissible.

**4. Make a provisional decision.** Pick Option → one-line rationale (*mandatory*) → confidence. Rationale-type tags (RT-derived taxonomy, [§3.9](03-research-and-prior-art-landscape.md)) offered as chips, optional. If the Decision conflicts with an in-scope Principle, the UI requires either changing course or recording an `excepts` edge with a distinguishing reason.

**5. Request review / approve.** Submitting for review routes by category policy to the reviewer queue. The reviewer opens an automatically assembled **briefing** rather than reconstructing a thread (concept adapted from the companion ChatGPT context-engineering document): what changed since their last review, the rationale and evidence, alternatives considered, recorded dissent, AI involvement (what was suggested, what was accepted, by whom), unfulfilled or waived dependencies, and downstream impact. Verdicts: approve / request changes / dissent-but-approve; dissent persists visibly ([§4.3.8](04-domain-model.md)). Consultant checking sessions get a batch mode: briefings for all pending Decisions in a pericope, canonical order — mirroring how consultant checks actually run. Community reviewers get a purpose-built plain-language briefing (question, alternatives, response form — never a simplified dump of the consultant view).

**6. Link precedents.** From the candidate panel: Use → choose treatment (followed/adapted/distinguished/rejected); distinguishing requires stating the differing characteristic (anchor-chip picker + free text). "Not relevant" is one click ([§5.5](05-relevance-and-precedent-framework.md)). The panel also surfaces **negative results** — options already investigated and rejected, precedents previously distinguished on this anchor — and states **visible omissions** ("2 candidates hidden: 1 unlicensed resource, 1 from a project you lack permission to view"), so an apparently complete panel never creates false confidence.

**7. Create / fulfill dependencies.** "This waits on…" picker (search Issues, or create the prerequisite inline). AI-suggested dependencies appear as `hypothesized` chips to confirm/reject. Fulfillment is automatic when rule-evaluated criteria are met; human-attested completions are a one-click attestation by the required role. Resolution notifications: "approving KT-ḥesed unblocked 2 issues assigned to you."

**8. Revise an approved decision.** "Propose revision" → new draft version → normal review → on approval, supersession + invalidation cascade + re-examination queue ([§6.5](06-dependency-and-prioritization.md)). The UI shows projected downstream impact *before* submission ("this will flag 12 records for re-examination").

**9. Import an external precedent.** Browse/search consenting projects (Tier-0 filtered) → import snapshot (distinct visual class, full attribution) → treat locally like any candidate. Upstream changes notify; nothing propagates automatically.

**10. Review AI output.** Every AI artifact (suggestion, draft, flag) sits in a visually distinct "proposed" state with its provenance (model, date, sources). Actions: accept (promotes with your name on the promotion event), edit-then-accept, reject (with optional reason — tuning signal). Nothing AI-produced ever changes a record's authoritative content without a human promotion event ([§8](08-ai-and-deterministic-boundaries.md)).

## 7.4 Progressive disclosure — the anti-form principle

The full Decision Record schema ([§4](04-domain-model.md)) would be an overwhelming form. The interface never presents it as one:

- **Tier 1 (everyone, always):** title, anchor, chosen rendering, one-line rationale, confidence. This alone yields a searchable, anchored, precedent-capable record.
- **Tier 2 (on demand):** options, arguments, evidence, dependencies, discourse annotations.
- **Tier 3 (specialists):** Toulmin structure, typological features, vocabulary management.

AI assistance is aimed at back-filling Tier 2 from Tier 1 + context (drafting structure from a translator's informal note), with human confirmation — the system, not the translator, pays the structuring cost.

## 7.5 Field and modality constraints

- **Offline-first:** all workflows function disconnected; sync on reconnect ([§9](09-technical-architecture.md)). Review round-trips tolerate days of latency (consultants are often remote).
- **Oral and sign-language projects:** renderings and community feedback may be audio/video artifacts; Options and Evidence accept media attachments with time-coded anchors [speculative — needs field validation in discovery phase]; the record structure is modality-neutral.
- **Low-spec hardware:** target mid-range laptops/Android; no server dependency for daily work.
- **Language:** UI and controlled vocabularies must carry multilingual labels (SKOS `prefLabel` per language) — consultant-facing English labels with national-language equivalents at minimum.
