# 2. Terminology and Conceptual Model

> **Status of this document.** Terms defined here are used consistently across the whole suite. Where the original concept brief used a different label, the change is flagged and justified. Labels marked **[proposal]** are original to this project; labels marked **[established]** are borrowed from existing research with citations in [§3 Research Landscape](03-research-and-prior-art-landscape.md).

## 2.1 The central conceptual move

The brief's core idea — "capture each translation decision as a structured object" — is right, but *decision* is doing too much work in it. What translators actually produce during a working session is a mixture of:

- questions they cannot yet answer,
- observations about the source text or the target language,
- candidate renderings,
- arguments for and against those candidates,
- provisional choices,
- and, eventually, reviewed and approved choices.

Modeling all of that as one "decision object" forces either a bloated record (most fields empty most of the time) or premature closure (you cannot record a question without pretending it is already a decision). The conceptual model therefore decomposes the brief's "decision" into a small **typed subgraph** built around an *issue*, following the IBIS tradition of issue-based information systems (Kunz & Rittel 1970) and the lifecycle conventions of Architecture Decision Records (Nygard 2011). See §2.3.

The second conceptual move is that **decisions are connected primarily through shared anchors, not through manually drawn pairwise links**. A decision about ἀμνός in John 1:29 and a decision about ἀρνίον in Revelation 5:6 are related because both anchor to the same key-term concept and overlapping semantic domains — facts that can be computed from the anchors, not facts someone must remember to enter. Manual links exist too, but they are the minority. This is what makes precedent retrieval feasible at scale (§5).

## 2.2 Glossary of core terms

### Units of work and knowledge

| Term | Definition | Replaces / relates to brief's term |
|---|---|---|
| **Issue** [established: IBIS] | A specific translation question that needs resolving, anchored to one or more anchors (see below). E.g., "How should ἀμνὸς τοῦ θεοῦ be rendered given that the community has no sheep-herding tradition?" | "Translation question" / part of "decision" |
| **Observation** [proposal] | A recorded fact or judgment that does not by itself resolve anything: an exegetical note, a target-language datum, a community reaction. Has an author (human or AI, always distinguished) and evidence links. | "Observations" |
| **Option** [established: IBIS "position"] | A candidate resolution of an Issue: a rendering, a strategy, a principle. | "Alternative renderings considered" |
| **Argument** [established: IBIS] | A reason supporting or opposing an Option, linked to Evidence. Toulmin-style warrant structure is available but optional (§4.3.6). | "Rationale" (partially) |
| **Evidence** [proposal] | A citation-bearing link from an Observation or Argument to a source: a lexicon entry, a grammar, a parallel passage, a community-testing report, an AI retrieval result. Evidence always identifies its origin class (§2.5). | "Relevant source-language evidence", "Supporting resources" |
| **Decision** | The *resolution event* of an Issue: which Option was selected, by whom, with what rationale, at what confidence, under which project principles. Narrowed from the brief's usage: a Decision cannot exist without an Issue it resolves. | "Decision" (narrowed) |
| **Decision Record** [established: ADR, adapted] | The retrievable bundle: an Issue plus its Options, Arguments, Evidence, Decision(s), Reviews, and links. This is the "case" in the case-based-reasoning sense — the unit that precedent retrieval returns. | "Structured decision object" |
| **Review** | A structured evaluation of a Decision by a person in a review role (consultant, exegete, community reviewer), producing an approval state and possibly dissent. | "Human reviewers and approval status" |
| **Ruling Principle** [proposal] | A project-level or team-level policy extracted from one or more Decisions ("we render covenant loyalty with X except in poetic texts"). Principles are first-class objects because they are what actually generalizes; individual Decisions are instances. | "Translation philosophy or project-specific principles" (made concrete) |

### Anchors — the join points

| Term | Definition |
|---|---|
| **Anchor** [proposal] | A typed reference from a Decision Record to a stable identifier in a shared reference system. Anchors are what make two records comparable without anyone linking them by hand. |
| **Text anchor** | A Scripture span: book/chapter/verse(s) plus optional token range, expressed against an explicit versification scheme. |
| **Lemma anchor** | A source-language lexeme, ideally by lexicon ID (e.g., a MACULA/Louw–Nida-keyed ID), not merely a Strong's number (see §3 on Strong's limitations). Optionally refined to a *sense* anchor. |
| **Domain anchor** | A semantic-domain ID (Louw–Nida domain/subdomain for Greek; SDBH domain for Hebrew). |
| **Construction anchor** | A grammatical or syntactic construction type (e.g., genitive of source, historical present), from a controlled project vocabulary seeded with treebank categories. |
| **Discourse anchor** | A discourse feature from the controlled vocabulary recommended in §3.9 (e.g., participant-reference default violation, thematic prominence marker). |
| **Key-term anchor** | An entry in the project's key-terms list (seeded from UBS/Paratext Biblical Terms). |
| **Target-feature anchor** | A feature of the target language relevant to the issue (e.g., honorific register system, clusivity distinction, evidentiality), drawn from a project lexicon/typology profile (Grambank-style feature IDs where applicable). |
| **Cultural-concept anchor** | A cultural or sociolinguistic concept ("animal sacrifice", "shepherding economy", "kin-avoidance registers") from an extensible project vocabulary. |

### Relationships

| Term | Definition |
|---|---|
| **Dependency** | A directed, typed edge stating that one item (Issue, Decision, or external task) must reach a specified state before another can properly reach its target state. Dependencies carry hardness, blocking behavior, scope, and confirmation status (§6). |
| **Precedent link** [established: legal informatics, adapted] | A directed edge recording that a Decision Record was *consulted* in resolving another Issue, with a treatment: **followed / adapted / distinguished / rejected**. Treatments are modeled on legal citator signals (Shepard's/KeyCite) but carry no binding force (§5.6). |
| **Relevance profile** [proposal] | The computed, multi-faceted description of *why* two Decision Records are related: which anchors they share, at what specificity. Replaces the proposed scalar "R-scale" — see §5 for the full argument. |
| **Supersession** | A versioning relation: a new Decision replaces a prior Decision on the same Issue. The old Decision is never deleted; it is marked superseded with provenance (§4.6, §9.6). |

### States and provenance

| Term | Definition |
|---|---|
| **Origin class** | Every node and edge carries exactly one origin class: `human-authored`, `human-approved`, `ai-suggested`, `rule-derived` (deterministic computation), or `imported-external`. Nothing is ever silently promoted between classes; promotion (e.g., accepting an AI suggestion) is an event with an actor (§8, §9.6). |
| **Lifecycle state** | Issues: `draft → open → in-analysis → decided → approved → superseded` (plus `deferred`, `withdrawn`). Dependencies: `hypothesized → confirmed → satisfied → invalidated → reopened`, plus `waived` (authorized, recorded, revocable override — never displayed as satisfied). Full state machines in §4. |
| **Provenance event** | An append-only record of who/what changed anything, when, and on what basis, structured after W3C PROV (Entity–Activity–Agent) (§4.6, §9.6). |

## 2.3 Why a subgraph, not a single record

The brief asked whether a decision should be "a single record, an event, an argument graph, a knowledge-graph subgraph, or some combination." The answer defended throughout this suite is: **a combination, layered**:

1. **Storage layer — events.** Every mutation is an append-only event (event sourcing). This gives auditability and supersession for free.
2. **Logical layer — a typed subgraph.** Issue, Options, Arguments, Evidence, Decision, Reviews, plus anchors and typed edges. This is what queries and precedent retrieval operate on.
3. **Presentation layer — a record.** Users mostly see a Decision Record rendered as one card/page. The decomposition should be invisible until it is needed (a translator jotting a quick question creates an Issue with one text anchor and nothing else — a 20-second interaction).

The argument-graph machinery (Arguments, Toulmin warrants) is deliberately **optional and progressive**: teams that want only "question → chosen rendering → one-paragraph rationale" can work that way, and the system still gains anchors, dependencies, and retrieval. Mandatory argumentation formalism is a documented adoption killer in design-rationale research (the "capture problem" — see §3.3) and this design treats it as such.

## 2.4 The legal-precedent analogy, audited

The brief proposed legal precedent as the guiding analogy and asked for an honest audit. Verdict: **keep the retrieval-and-treatment machinery, discard the authority machinery.**

| Legal concept | Verdict for translation | Reasoning |
|---|---|---|
| Applicability by shared characteristics | **Adopt.** | Exactly the CBR/HYPO factor-comparison model; becomes the relevance profile (§5). |
| Distinguishing a precedent | **Adopt.** | "Same lemma, but here the referent is divine, so the honorific decision doesn't transfer" is a distinguishing move. Recording *why a precedent was not followed* is as valuable as recording that one was. |
| Treatment signals (followed/distinguished/overruled) | **Adopt, renamed.** | Citator-style treatments become `followed / adapted / distinguished / rejected / superseded`. |
| Binding authority / stare decisis | **Reject.** | No translation decision binds a later one. The community and translation team hold authority; a precedent is at most *strongly informative*. Encoding bindingness would also poison cross-project sharing (an external project's choice must never appear authoritative). |
| Jurisdiction hierarchy | **Reject as hierarchy, keep as scope.** | There is no appellate structure, but there *is* scope: a project-wide Ruling Principle outranks nothing — it merely applies more widely than a verse-specific Decision. Scope is a filter, not a rank. |
| Precedent supersession (overruling) | **Adopt within a project.** | A revised key-term Decision supersedes its predecessor and triggers downstream re-examination (§6.5). Across projects, supersession never propagates automatically. |
| Adversarial parties | **Discard.** | Translation review is collaborative. Dissent is recorded, but the model has no plaintiff/defendant structure. |

A second analogy quietly does as much work as the legal one: **case-based reasoning** (retrieve → reuse → revise → retain, Aamodt & Plaza 1994). CBR contributes the retrieval cycle; legal informatics contributes the treatment vocabulary and the discipline of distinguishing. Both are documented in §3.

## 2.5 Distinctions the brief requested

The brief listed fourteen candidate unit types (questions, observations, hypotheses, constraints, options, recommendations, decisions, rationales, dependencies, evidence, reviews, corrections, precedents, exceptions). Mapping to this model:

| Brief's term | Modeled as |
|---|---|
| Translation question | **Issue** (node) |
| Observation | **Observation** (node) |
| Hypothesis | An **Observation** with `epistemic-status: hypothesis`, or a **hypothesized Dependency** — hypotheses are a status, not a separate type |
| Constraint | A **Ruling Principle** (if policy) or a **Dependency** of type `constrains` (if inter-decision); orthographic/format constraints are Principles with `category: constraint` |
| Option | **Option** (node) |
| Recommendation | An **Option** carrying an `ai-suggested` or reviewer endorsement edge — recommendations are attributed stances toward Options, not a type |
| Decision | **Decision** (event-node) |
| Rationale | **Argument**(s) attached to the Decision, plus free-text summary on the Decision itself |
| Dependency | **Dependency** (edge with state) |
| Evidence | **Evidence** (edge/node hybrid: a link with citation payload) |
| Review | **Review** (node) |
| Correction | A new Decision that **supersedes**, plus the provenance event trail — corrections are lifecycle transitions, not a type |
| Precedent | A **Precedent link** (edge with treatment) — precedent-hood is relational, not intrinsic; no record "is a precedent" until another record treats it as one |
| Exception | A Decision with an `excepts` edge to a Ruling Principle — exceptions are typed relations to the principle they carve out of |

Two of the brief's implicit assumptions are corrected here:

1. **"Precedent" is not a property of a record.** A record becomes a precedent only when cited. What the system can compute in advance is *candidate relevance*, not precedent-hood. This keeps retrieval honest: the system surfaces candidates; humans create precedent by treating them.
2. **Importance/risk/impact are mostly derived, not entered.** Downstream impact = computed from the dependency graph. Recurrence = computed from anchor-sharing counts. Only *risk* (e.g., theologically sensitive, community-sensitive) and *confidence* genuinely require human entry. Every field a translator must fill in by hand is adoption cost; the model minimizes them (§7, §13).

## 2.6 Terms deliberately not used

- **"R-scale"** — retired; see §5.9 for the full argument. The replacement vocabulary is *relevance profile* (the explanation) and *retrieval score* (the transient ranking number).
- **"Knowledge base of truth"** — the corpus is a body of *reviewed, situated decisions*, not a repository of correct renderings. All user-facing language must preserve this distinction; several failure modes in §13 follow from blurring it.
- **"AI decision"** — does not exist in this system. AI produces suggestions, drafts, retrievals, and flags; only humans produce Decisions (§8).
