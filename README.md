# Bible-Translation Decision-Support System — Research & Design Suite

A research-grounded plan for a system that captures Bible-translation decisions as **structured, anchored, interlinked, reviewable objects** — with dependency tracking, precedent retrieval, deterministic prioritization, and AI assistance that can never overrule a human.

**One-paragraph thesis.** Translation decisions are not isolated: they share source-language senses, key terms, discourse features, target-language characteristics, and cultural questions; they constrain one another; and reviewed decisions can serve as precedents for later ones. If decisions are anchored to shared identifier spaces (tokens, senses, terms, typological features), those connections become *computable*: relevance is anchor intersection, dependencies propagate deterministically, and prior work becomes retrievable with an explanation of why it is relevant. This suite turns that thesis into a domain model, an architecture, an MVP, and a plan to test it with professional translators — including the criteria under which it should be judged to have failed.

## Contents

| Doc | Section |
|---|---|
| [01](docs/01-executive-synthesis.md) | Executive synthesis — the concept, its strongest insight, key refinements, falsifiers |
| [02](docs/02-terminology-and-conceptual-model.md) | Terminology and conceptual model — Issues, Decisions, anchors, treatments; the legal analogy audited |
| [03](docs/03-research-and-prior-art-landscape.md) | Research and prior-art landscape — tools, standards, datasets, licenses (incl. discourse-analysis representation, §3.9) |
| [04](docs/04-domain-model.md) | Domain model — entities, relationships, lifecycle state machines, provenance |
| [05](docs/05-relevance-and-precedent-framework.md) | Relevance and precedent framework — why the R-scale is rejected; relevance profiles; cross-project precedent |
| [06](docs/06-dependency-and-prioritization.md) | Dependencies and prioritization — fulfillment rules, cycles, change propagation, leverage |
| [07](docs/07-user-experience-and-workflows.md) | UX and workflows — views, roles, journeys, progressive disclosure |
| [08](docs/08-ai-and-deterministic-boundaries.md) | AI and deterministic boundaries — responsibility matrix and safeguards |
| [09](docs/09-technical-architecture.md) | Technical architecture — recommendation + alternatives, data model, JSON examples, APIs, pseudocode |
| [10](docs/10-mvp-definition.md) | MVP definition — scope, exclusions, data, screens, demo scenario (Jonah) |
| [11](docs/11-phased-roadmap.md) | Phased roadmap — discovery through production, with exit criteria and gates |
| [12](docs/12-efficacy-study.md) | Efficacy study — measures, designs, baselines, pre-registered success/failure criteria |
| [13](docs/13-risks-and-open-questions.md) | Risks and unresolved questions — adoption, linguistic, theological, governance, licensing, technical |
| [14](docs/14-next-actions.md) | Recommended next actions — prioritized sequence from concept to prototype |
| [15](docs/15-worked-example.md) | Worked example — ἀμνὸς τοῦ θεοῦ (John 1:29) end to end, plus the 1 Corinthians slogan cluster |
| [16](docs/16-sil-contribution-map.md) | SIL contribution map — the SIL GitHub orgs and repos to build on and contribute to |

**Reading shortcuts:** in a hurry → [15](docs/15-worked-example.md), then [05](docs/05-relevance-and-precedent-framework.md)–[06](docs/06-dependency-and-prioritization.md), then [10](docs/10-mvp-definition.md). Talking to partners → [01](docs/01-executive-synthesis.md) + [15](docs/15-worked-example.md). Building → [04](docs/04-domain-model.md) + [09](docs/09-technical-architecture.md) + [10](docs/10-mvp-definition.md).

## Relationship to the ChatGPT documents

The repository root contains two independently produced, ChatGPT-authored designs (`CHATGPT_*.md`). This suite was written without reference to them and converges with them on the major verdicts (reject a scalar relevance score, IBIS-style decomposition, deterministic dependency state, advisory-never-binding precedent, validate retrieval before building ambitious AI). After a comparative review, eleven specific items from those documents were folded into this suite and are credited inline where they appear — notably the `waived` dependency state (docs 04/06), the retrieval false-positive taxonomy and asymmetric dependency-error costs (doc 12), the per-call context manifest with recorded exclusions (docs 08/09), role-specific review briefings and visible omissions (doc 07), negative-context retrieval (doc 05), the structure-vs-AI ablation arms (doc 12), Web Annotation and Scripture Forge in the landscape (doc 03), two organizational risks (doc 13), and the 1 Corinthians slogan cluster as a second worked scenario (doc 15). Deliberate divergences, retained after considering their approach: no stored numeric relevance or confidence values, no `partially satisfied` dependency state (derived display only), offline-first rather than server-first architecture, and the Context Engine adopted as concepts rather than as a subsystem.

## Method and evidence standards

Prior-art claims were researched against primary sources (official documentation, specifications, repositories, peer-reviewed papers), with all URLs accessed **2026-07-29**; claims that could not be verified are marked `[unverified]` inline. Throughout the suite, **established research**, **reasonable inference**, and **original proposal** are distinguished, speculative ideas are marked, and licensing is identified before any reuse recommendation. No translation philosophy is presented as universally correct, and the worked example explicitly does not endorse a rendering.
