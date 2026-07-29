# 8. AI and Deterministic-System Boundaries

> The responsibility matrix, the AI-assistance model, and safeguards. Principle inherited from the whole design: **AI proposes; rules compute; humans decide.** No AI output ever becomes authoritative content without a recorded human promotion event.

## 8.1 Responsibility matrix

| Function | Humans | Conventional software | Rules / graph algorithms | Retrieval systems | Generative AI |
|---|---|---|---|---|---|
| Make/approve/revise a Decision | **sole authority** | record it | — | — | never |
| Confirm dependencies, treatments, anchors | **sole authority** | record it | — | — | suggest only |
| Dependency state (satisfied/invalidated), blocking, readiness | override with audit trail | — | **sole authority** ([§6.2](06-dependency-and-prioritization.md)) | — | never |
| Leverage / prioritization scores | override, pin, re-rank | display components | **compute** (reach, frontier, SCC) | — | never |
| Invalidation cascade & re-examination queue | disposition each item | notify | **compute** | — | pre-sort with labeled assessments |
| Text/lemma/sense anchoring of a new Issue | confirm | token lookup (MACULA) | deterministic from cursor position | — | suggest refinements (sense, category) |
| Precedent candidate surfacing | accept/dismiss/treat | — | **hard-rule tier** (principles in scope, co-dependents) | **ranked tier** (facet score; embeddings as lowest-tier facet) | explain *why a candidate may or may not apply* — labeled draft |
| Consistency surveillance (same sense, conflicting renderings, no distinguishing rationale) | adjudicate | — | **detect** (graph query) | — | draft the explanation of the conflict |
| Schema/citation validation | — | **JSON Schema at write time**; resource-registry resolution | graph invariants (e.g., superseded ⇒ successor exists) | — | never validates; its citations are what get validated |
| Drafting (rationale summaries, decision-record structure from informal notes, comparison tables, review summaries) | edit/accept/reject | — | — | supply sources | **primary drafter** — always labeled, always sourced |
| Research assistance (find lexicon entries, parallel passages, typological matches) | evaluate | — | — | **retrieve from registered resources only** | summarize retrieved content with citations |
| Missing-dependency / unresolved-assumption detection | confirm | — | pattern rules where deterministic (e.g., key-term anchor without key-term decision) | — | flag from free text — `hypothesized` only |
| Audit log | read | **append-only store** | — | — | is a subject of it, never a writer to it |

## 8.2 The AI-assistance model

All generative capabilities from the brief's §10 are implemented as **retrieval-augmented, provenance-stamped suggestion services**:

1. **Grounding rule.** Generative output shown to users must carry citations resolvable against the project's registered resources (or explicit "no source — model knowledge" labeling, which is barred from Evidence). Retrieval scope is the project's corpus + licensed resources — never other projects' private data (isolation, §8.3).
2. **Provenance stamp — a per-call context manifest.** Every suggestion records: model ID + version, parameters, prompt template + hash of the full prompt, timestamp, and a **manifest** of the context assembled for the call — each included item with its selection reason, *and each deliberately excluded candidate with its reason* (superseded, permission-denied, unlicensed, suppressed) (concept adapted from the companion ChatGPT context-engineering document). Recording exclusions is what makes an AI answer auditable rather than merely attributed: a reviewer can see not just what the model saw, but what it was prevented from seeing and why. Full prompt/response bodies go to the audit store ([§4.6](04-domain-model.md), [§9.6](09-technical-architecture.md)).
3. **Origin-class segregation.** `ai-suggested` content is visually distinct everywhere (color/badging), excluded from official exports by default, and inert: it satisfies no dependency, blocks nothing, approves nothing.
4. **Promotion event.** Accepting a suggestion is an auditable act by a named human; the record shows both the AI origin and the human promoter — the PROV qualified-role pattern (`SoftwareAgent` suggested, `Person` accepted, [§3.8](03-research-and-prior-art-landscape.md)).
5. **Feedback capture.** Accept/edit/reject (with optional reasons) is logged as tuning signal for retrieval weights ([§5.8](05-relevance-and-precedent-framework.md)) and as data for the efficacy study's automation-bias measures.

## 8.3 Safeguards mapped to the brief's risk list

| Risk | Safeguard |
|---|---|
| Hallucinated sources | Citation-resolution gate: Evidence must resolve to a resource-registry entry; unresolved citations are flagged and cannot support approval. Spot-audit sampling of AI citations in the reviewer queue. |
| Invented linguistic facts | Linguistic claims in suggestions must cite retrieved content (lexicon entry, treebank node, typology datapoint); "model knowledge" assertions are labeled and barred from Evidence. |
| Overconfidence | Suggestions carry no numeric confidence theater; uncertainty phrasing is enforced in prompt templates; the UI language is "candidate/draft," never "answer." |
| Automation bias | Distinct visual class; mandatory one-line human rationale on every Decision (the deliberate-consideration forcing function the HITL literature suggests, [§3.6](03-research-and-prior-art-landscape.md)); acceptance-rate monitoring per user surfaced to team leads; periodic "blind" mode in studies ([§12](12-efficacy-study.md)). |
| Theological / translation-philosophy bias | AI never ranks Options on theological criteria; comparison drafts must present all Options symmetrically against the *project's own* stated principles; philosophy-neutrality is a prompt-template invariant reviewed with pilot partners. Residual risk is real and is monitored via dissent records ([§13](13-risks-and-open-questions.md)). |
| Cross-language overgeneralization | Cross-project suggestions must display the typological/genealogical basis (which facets matched) and the distinguishing facets; imported precedents are informative-only ([§5.7](05-relevance-and-precedent-framework.md)). |
| Circular reasoning (AI citing AI) | Origin class is carried through retrieval: `ai-suggested` content is excluded from AI retrieval corpora; only human-approved records are retrievable as precedent. |
| Stale precedents | Citator-style health flags: superseded/invalidated records surface with warnings; retrieval down-ranks superseded versions and always links the current one. |
| Hidden changes to approved decisions | Approved content is immutable; change requires the supersession workflow with review; the append-only event log makes silent mutation structurally impossible ([§9.6](09-technical-architecture.md)). |
| Sensitive data leaking between projects | Hard project isolation at the storage and retrieval layers; sharing only via explicit publication + snapshot import; no cross-project model fine-tuning on private data; embeddings computed and stored per-project. |
| Authority laundering (an AI summary makes a low-authority claim sound official) | Every summarized or restated claim carries forward the authority label and origin class of its sources; a summary of a provisional observation can never display as project policy; summaries are always paired with links to the unsummarized sources. (Named risk adapted from the companion ChatGPT context-engineering document.) |
| Instruction injection via imported content | Imported notes, external precedents, fetched resources, and community feedback are **data, never instructions**: retrieval outputs are structurally segregated from system prompts, tool results are labeled untrusted text, and nothing retrieved can modify prompt templates, policies, or retrieval scope. |

## 8.4 Where generative AI must NOT be used

Restating the deterministic boundary as a checklist (the brief's "recommend where conventional code… should be used instead"):

- Dependency satisfaction, blocking, readiness → **rules** over explicit data.
- Anything counted or ranked for prioritization → **graph algorithms** with displayed components.
- Anchor identity (which token, which verse, which sense ID) → **deterministic lookup**; AI only proposes *refinements* a human confirms.
- Record validity → **JSON Schema / DB constraints / graph invariants**.
- Consistency detection over structured data (same sense + different rendering + no exception) → **graph queries**; AI only *narrates* findings.
- Approval state, role authority, access control → **conventional authorization code**.
- The audit log → **append-only storage**; nothing generative anywhere near it.
- Versification mapping, term-occurrence lookup, alignment display → **data + deterministic code** (Copenhagen mappings, MACULA, alignments).

The heuristic behind the whole table: **if the output must be *correct*, it is code or rules; if it must be *useful*, it may be AI — behind a human gate.**
