# 5. Relevance and Precedent Framework

> Answers the brief's §3 ("a decision-relevance model", the proposed R-scale) and §2/§7 (precedent taxonomy, cross-project precedent). Builds on the anchor model of [§4](04-domain-model.md).

## 5.1 Purpose of the relevance model

When a translator opens or creates an Issue, the system should surface prior Decision Records (and Ruling Principles) that could inform it — ranked, *explained*, and cheap to dismiss. The relevance model exists for exactly four functions:

1. **Retrieval** — find candidate precedents for a current Issue.
2. **Explanation** — say *why* each candidate is relevant, in terms a translator can verify or reject in seconds.
3. **Consistency surveillance** — detect Decisions that share strong relevance but conflict (same sense, same context class, different renderings, no distinguishing rationale).
4. **Cross-project scoping** — determine which external records are even candidates, before any ranking.

It is **not** a measure of whether a precedent *should be followed*. Applicability is a human judgment recorded as a treatment (followed/adapted/distinguished/rejected). The model proposes; the treatment disposes.

## 5.2 Verdict on the R-scale

**Recommendation: do not build a scalar R-scale, and retire the name.** Reasoning:

1. **Relevance here is multi-dimensional and the dimensions are incommensurable.** Two records can share a lemma but differ in discourse function; two can share no source-language material yet share a decisive target-language feature (honorific register). Any collapse to one number either hides which dimension fired (destroying explanation, the model's second purpose) or requires weights that pretend cross-dimension trade-offs are stable when they demonstrably vary by issue category — for a key-term issue, shared-sense dominates; for a participant-reference issue, shared-discourse-function dominates.
2. **The E-scale analogy fails structurally.** The missiological E-scale (Winter's E-0…E-3) expresses distance along *one* conceptual axis (cultural distance from the evangelist). Decision relevance has no single axis. A closer analogy from the research record is legal CBR — HYPO's *dimensions* and CATO's *factor hierarchies* (Ashley; Aleven — see §3) — which deliberately represent case similarity as a **profile of shared and distinguishing factors**, not a scalar, precisely so that arguments about applicability remain inspectable.
3. **Scalar scores invite automation bias.** "R = 0.87" reads as authority. "Shares sense 'sacrificial lamb' (LN 4.24) and cultural-concept 'animal sacrifice'; differs: target-feature honorific register" reads as a checkable claim. In a high-stakes, trust-sensitive domain, the interface must exhibit the second form (§8; automation-bias literature in §3).

**Replacement:** the **relevance profile** — a typed set of facet matches — plus a transient **retrieval score** used only to rank the candidate list. The score is never stored on records, never shown as a bare number (shown, if at all, as ordering plus facet badges), and never used by any deterministic rule.

## 5.3 Relevance dimensions and their representation

Each dimension below is computed from anchor intersection ([§4.3.3](04-domain-model.md)) or project/profile metadata. "Specificity" means matches deeper in a refinement hierarchy count more (same *sense* ≫ same *lemma*; same *subdomain* ≫ same *domain*).

| # | Dimension | Computed from | Representation | Hard rule or weighted indicator? |
|---|---|---|---|---|
| D1 | Same lemma / same sense | lemma & sense anchors (lexicon-ID space) | exact ID match; sense refines lemma | weighted; sense-match weighted far above lemma-match (Strong's-style lemma identity alone is a known false-positive source — §3.2) |
| D2 | Same semantic domain | domain anchors (Louw–Nida / SDBH) | hierarchy-aware match (subdomain > domain) | weighted |
| D3 | Same grammatical construction | construction anchors | controlled-vocabulary match | weighted |
| D4 | Same discourse function | discourse anchors (§3.9 vocabulary) | controlled-vocabulary match | weighted |
| D5 | Same genre / text type | derived from text anchors + a canonical genre map (per-pericope) | categorical match | weighted, low default weight |
| D6 | Same speaker / participant / referent | referent anchors (participant registry per book/corpus) | entity-ID match | weighted; high weight for honorifics/participant-reference categories |
| D7 | Intertextual link | text anchors joined through a curated intertext dataset (quotation/allusion pairs) | path-through-intertext match | weighted; surfaced with the intertext edge shown |
| D8 | Same theological concept / key term | key-term anchors | ID match | weighted, high for key-term category |
| D9 | Same target-language feature | target-feature anchors | ID match (project profile / Grambank-style feature) | weighted; **the** dominant dimension for cross-project retrieval |
| D10 | Same cultural/sociolinguistic issue | cultural-concept anchors | ID match | weighted |
| D11 | Same Ruling Principle implicated | principle scope patterns | rule match | **hard for surfacing** (an in-scope adopted Principle is always shown) — not a similarity weight |
| D12 | Same audience/medium | project metadata | categorical | cross-project filter more than a weight |
| D13 | Language relatedness | Glottolog genealogy distance | ordinal (same language > same subgroup > same family > unrelated) | weighted **cap/filter for cross-project**, never sufficient alone — typological match (D9) may legitimately beat genealogical proximity, per the brief's own caution |
| D14 | Typological similarity | shared Grambank-style feature values relevant to the issue category | per-feature match | weighted |
| D15 | Same unresolved dependency | dependency edges to a common prerequisite | graph fact | **hard**: co-dependents are always mutually visible ("these 7 issues all wait on the ḥesed key-term decision") |
| D16 | Same issue category | category field | categorical | acts as the *weight-profile selector*, not a scored dimension (see 5.4) |
| D17 | Lexical/semantic text similarity | embeddings over issue title/description/rationale | vector similarity | weighted, lowest tier; a recall net for records with sparse anchors, always labeled as "text similarity" in explanations |

## 5.4 Scoring: hard rules vs. weighted indicators

Three tiers, evaluated in order:

- **Tier 0 — filters (deterministic).** Scope eligibility: same project, or external projects passing the sharing policy + consent + trust checks (§5.7). Nothing outside the filter is ever scored.
- **Tier 1 — hard surfacing rules (deterministic).** D11 (in-scope Principle), D15 (shared unresolved dependency), explicit human links. These appear in a distinct "governing/related by rule" panel — not ranked among similarity candidates, because they are facts, not similarities.
- **Tier 2 — ranked candidates (weighted).** Score = Σ wᵢ(category) · fᵢ · specificityᵢ over D1–D14, D17, where the weight vector is selected by issue category (D16). Initial weights are hand-set per category from translator-consultant workshops; the tuning loop (5.8) refines them from treatment outcomes. This is deliberately a *linear, per-facet-explainable* model — not a learned black box — so every ranked item can display exactly which facets contributed. **[proposal; the linear-explainable constraint is a design commitment, not a claim that it maximizes retrieval accuracy]**

## 5.5 Explanation and override

Every surfaced candidate shows: facet badges (shared anchors, with their labels), distinguishing facets already known (anchors the current Issue has that the candidate lacks, and vice versa), and provenance (whose decision, when, what approval state, which project). Translator actions, all one click:

- **Use** → opens treatment recording (followed/adapted/distinguished) — creating the Precedent link.
- **Not relevant** → records a rejection with optional reason; the pair is suppressed for this Issue and logged as tuning signal.
- **Wrong anchor** → the deeper correction: if a match fired on a bad anchor (e.g., an AI-suggested sense link that's wrong), the translator fixes the *anchor*, improving every future retrieval — this makes correction cumulative rather than per-query.

Overrides never fight the user: a suppressed candidate stays suppressed for that Issue even if its score rises.

**Negative context is retrieved, not just stored** (framing adapted from the companion ChatGPT context-engineering document). The corpus already contains dead ends — Options investigated and rejected, precedents `distinguished` or `rejected` with reasons, suppressed candidate pairs, community-testing failures. Retrieval surfaces these as first-class results on matching anchors: "a rendering like this was tested and rejected in 3:9 — see why." Preventing a team (or an AI assistant) from unknowingly reopening a settled dead end is one of the highest-value retrieval outcomes, and it costs nothing beyond ranking treatment-bearing negative records alongside positive ones with distinct badging.

## 5.6 Precedent treatments (the human layer)

Adapted from legal citator signals (§2.4):

| Treatment | Meaning | Required extras |
|---|---|---|
| `followed` | Reasoning and rendering strategy adopted | — |
| `adapted` | Reasoning adopted, execution altered for local factors | note which facets differed |
| `distinguished` | Considered and set aside as materially different | distinguishing characteristics, ideally as anchor references |
| `rejected` | Considered and judged wrong or inapplicable in principle | reason |

Treatments are always human-authored. They serve three audiences: the current team (rationale trail), future retrieval (a heavily-followed record ranks up *within explanation*: "followed 6× in this project"), and the tuning loop (5.8).

## 5.7 Cross-project and cross-language precedent

Mechanism: **explicit publication, snapshot import, local treatment.**

1. **Publication.** A project marks specific approved Decision Records (or distilled Ruling Principles) shareable under a chosen policy: full / anonymized (no personal attributions, optionally no draft text — sharing *the reasoning* without the rendering) / metadata-only. Community consent is a publication precondition recorded on the policy (CARE-principles alignment, §3.6); default is **not shared**.
2. **Scoping.** External candidates pass Tier-0 filters: sharing policy, requesting project's trust configuration (which orgs/projects it accepts precedents from), and version compatibility of anchor schemes.
3. **Snapshot import.** Using an external record creates a local immutable copy with full attribution and source-version stamp. Upstream supersession *notifies*, never auto-propagates.
4. **Status.** External records are **informative, never authoritative** — a distinct visual class (`imported-external`), and no external record can satisfy a local dependency. Local teams distinguish or reject them with exactly the same treatment machinery, and the treatment stays local.
5. **Relatedness basis.** Retrieval across projects leans on D9/D14 (target-feature and typological match) and D8/D10 (key term, cultural concept) — not genealogy alone. A Bantu-language solution to honorific participant reference may be the best precedent for an unrelated Southeast Asian language sharing the register system.

Open governance questions (licensing of shared records, inter-org trust registries, anonymization robustness for tiny language communities where "anonymous" is a fiction) are unresolved and flagged in [§13](13-risks-and-open-questions.md) — they gate the cross-project phase of the roadmap, not the MVP.

## 5.8 Empirical validation and tuning

- **Ground truth from use:** every treatment (followed/adapted/distinguished) and every "not relevant" dismissal is a labeled relevance judgment on a (query-issue, candidate) pair — the system's normal operation generates its own evaluation corpus.
- **Offline benchmark:** before any live tuning, build a judged set — sample ~100–200 issues from the pilot corpus, have 2–3 consultants mark relevant prior records, measure precision@5, recall, MRR; report inter-annotator agreement (Cohen's κ / Krippendorff's α) because "relevant precedent" is itself an expert judgment ([§12](12-efficacy-study.md) specifies the full protocol and baselines — including a "TM-style fuzzy text match" baseline and a "same-lemma-only" baseline that the profile model must beat to justify its complexity).
- **Weight tuning:** per-category logistic regression (or simple grid search at MVP scale) over the facet features against treatment outcomes; weights remain inspectable and are versioned like code.
- **Failure-mode monitoring:** false positives (dismissal rate per dimension — a dimension that mostly fires wrongly gets down-weighted or demoted to display-only), false negatives (periodic consultant audits: "what should have been surfaced?"), and **misleading similarity** — the worst case: high-facet-overlap pairs where following would be wrong (e.g., same lemma, different sense). Mitigations: sense-over-lemma weighting, distinguishing-facet display (the UI always shows differences, not just matches), and a consistency-checker that flags followed-precedent pairs whose outcomes later diverge.

## 5.9 Name recommendation

Retire **"R-scale."** It imports the E-scale's single-axis intuition, which is the exact misconception the model must avoid, and it implies a stored per-pair number, which invites misuse in rules and UI. Recommended vocabulary: **relevance profile** (the explanation object), **retrieval score** (transient ranking value), **treatment** (the human applicability judgment). If a memorable brand is wanted for the whole mechanism, name it after what it does — e.g., "precedent finder" — rather than after a scale.
