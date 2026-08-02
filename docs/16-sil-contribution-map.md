# 16. SIL Contribution Map

> A contributor-oriented survey of SIL's open-source translation software on GitHub, oriented to two goals: (a) where the decision-support system integrates, and (b) where to contribute to build credibility for the SIL collaboration. All repository claims verified against the repos on **2026-08-02**; items that could not be confirmed are marked `[unverified]`. Note this survey covers SIL's *language-software* organizations — the separate `sil-org` organization is SIL Global's IT/infrastructure org (identity/SSO, Terraform, backups) and is relevant only to deployment operations ([§9](09-technical-architecture.md), roadmap Phase 8).

## 16.1 The three SIL organizations that matter

| Org | What it is | Size |
|---|---|---|
| [`sillsdev`](https://github.com/sillsdev) | SIL Language Software Development — the main org: Scripture Forge, FLEx, silnlp, machine libraries, lexbox | ~305 public repos |
| [`paranext`](https://github.com/paranext) | Platform.Bible / Paratext 10 Studio — the extensible successor platform (MIT, © SIL Global + UBS jointly) | 19 repos |
| [`sil-ai`](https://github.com/sil-ai) | SIL AI — AQuA quality assessment and alignment research | ~30 repos |

Boundary notes for partner conversations: [`ubsicap`](https://github.com/ubsicap) (USFM/USX specs, versification) is UBS+SIL shared; [`usfm-bible/tcdocs`](https://github.com/usfm-bible/tcdocs) is the USFM/X Technical Committee; [`Clear-Bible`](https://github.com/Clear-Bible) (MACULA, Alignments) is Biblica; [`BibleNLP`](https://github.com/BibleNLP) is a consortium; [`unfoldingWord`](https://github.com/unfoldingWord) is a separate organization. Useful partners — not SIL repos.

## 16.2 Priority repositories

| Repo | What it is | Relevance to this project | Contribution reality |
|---|---|---|---|
| [`paranext/paranext-core`](https://github.com/paranext/paranext-core) | The Platform.Bible extension host: TS/React/Electron + .NET 8, MIT; actively developed daily (4,069 commits; 109 open PRs at survey time) | **The platform the decision-support extension ships on** ([§9.1](09-technical-architecture.md)) | Code fully open, but **issue creation is restricted and GitHub Discussions is disabled** — work is tracked internally. Build freely; propose through relationship channels, not GitHub issues |
| [`paranext/paranext-extension-template`](https://github.com/paranext/paranext-extension-template/wiki/Your-First-Extension) | Extension tutorial wiki: PAPI, WebViews, commands, Scripture data providers; plus [multi-extension template](https://github.com/paranext/paranext-multi-extension-template) and API docs at [paranext.github.io/paranext-core](https://paranext.github.io/paranext-core/) | Exactly how to build, structure, and publish the extension | Fully public; no permission needed to start today |
| [`paranext/paratext-bible-extensions`](https://github.com/paranext/paratext-bible-extensions) | Official Paratext.Bible extensions monorepo: per-extension manifest + typed inter-extension APIs via PAPI | Reference for exposing the decision-support tool's data to other extensions and consuming Scripture providers | Small, watchable; the structural reference |
| [`sillsdev/web-xforge`](https://github.com/sillsdev/web-xforge) (Scripture Forge) | Community checking + AI drafting web app synced with Paratext (Angular/C#, MongoDB, MIT; 4,313 commits) | Closest existing product to the review loop; its community-checking flow is the pattern to integrate with or learn from ([§3.1](03-research-and-prior-art-landscape.md)) | **Best entry point in the ecosystem**: CONTRIBUTING.md + `good first issue` and `help wanted` labels — the only surveyed repo with an explicit newcomer on-ramp |
| [`sillsdev/machine.py`](https://github.com/sillsdev/machine.py) | Python NLP library: USFM/Paratext parsing, verse-aligned corpora, word alignment (MIT, 512 commits) | The core of precedent retrieval as a library — "where else was this term rendered, and how" | Small focused surface; best option for meaningful library PRs in Python |
| [`sillsdev/serval`](https://github.com/sillsdev/serval) + [`machine`](https://github.com/sillsdev/machine) | Production MT/alignment REST API behind Scripture Forge drafting (C#/.NET, MongoDB, MIT) and its engine library (SMT, IBM 1–4/HMM/FastAlign alignment) | Alignment-as-a-service for the retrieval layer instead of rebuilding it | Team-coordinated; engage the Serval team before PRs |
| [`sil-ai/aqua-api`](https://github.com/sil-ai/aqua-api) + [`paranext/aqua-extension`](https://github.com/paranext/aqua-extension) | AQuA quality-assessment API (Python/FastAPI/Postgres, MIT, active) and its Platform.Bible extension | AQuA flags become Issues in the decision-support model ([§3.6](03-research-and-prior-art-landscape.md)); aqua-extension is the **direct precedent** for a service-backed analysis extension | Active (58 open issues); no formal contributor process — coordinate with the team |
| [`sillsdev/languageforge-lexbox`](https://github.com/sillsdev/languageforge-lexbox) | Cloud lexicon hub for FLEx data ("Language Depot" successor): .NET 8 + SvelteKit, GraphQL API; very active (211 open issues, 21 PRs) | The modern API for linking Decisions to target-language lexicon entries without touching desktop FLEx ([§9.8](09-technical-architecture.md) LIFT row) | Visible backlog, cross-platform setup docs |
| [`sillsdev/FieldWorks`](https://github.com/sillsdev/FieldWorks) + [`libpalaso`](https://github.com/sillsdev/libpalaso) | FLEx itself (largest community footprint of the surveyed repos: 110★) and shared .NET libraries incl. **`SIL.Lift`** (lexicon interchange) and **`SIL.Scripture`** (verse references / versification) | Integrate via LIFT; reuse `SIL.Scripture` for text anchoring in any C# component rather than reimplementing versification | FieldWorks has CONTRIBUTING.md and dev-setup docs; libpalaso publishes NuGet per commit |
| [`sillsdev/transcelerator`](https://github.com/sillsdev/transcelerator) | Comprehension-question tool (Paratext plugin, **Platform.Bible extension in progress**) | Closest existing "structured checking content authored alongside translation"; a natural ally for the community-testing evidence flow ([§7](07-user-experience-and-workflows.md)) | Maintainer contact published in the repo |

Two findings worth stating explicitly:

1. **[`sillsdev/scripture-forge-platform-extensions`](https://github.com/sillsdev/scripture-forge-platform-extensions)** — an SIL product team shipping its features as Platform.Bible extensions from its own repo, generated from the public template. This is the exact shape the decision-support extension takes: your repo, their platform, template-synced.
2. **The niche is open.** Nothing in any surveyed org resembles a structured translation-decision tracker — the closest artifacts are Scripture Forge's community-checking Q&A and Transcelerator's questions. This corroborates the gap finding of [§3.0](03-research-and-prior-art-landscape.md) from inside SIL's own codebase inventory.

## 16.3 Recommended contribution sequence

1. **Build the extension now** against `paranext-extension-template` + the PAPI docs — fully public, MIT, no permission required. Study `aqua-extension` and `scripture-forge-platform-extensions` as the two service-backed precedents.
2. **Earn credibility in `web-xforge`** — the one repo with an explicit newcomer on-ramp, and its community-checking code is directly relevant to the review workflows of [§7](07-user-experience-and-workflows.md).
3. **PR into `machine.py`** (Python) — small library, directly on the critical path for precedent retrieval (USFM corpora + alignment).
4. **Route the big proposal through people, not GitHub.** `paranext` deliberately does not accept inbound GitHub issues; the Platform.Bible developer forum (support.bible) and direct SIL contacts are the channel. Ecosystem-wide star counts are tiny (3–110): this ecosystem runs on institutional teams, so contribution success tracks relationships with the specific team (Scripture Forge, Serval, Platform.Bible) more than OSS process.

Licensing across the surveyed repos is almost uniformly **MIT** (FieldWorks and ptx2pdf licenses unconfirmed from repo pages `[unverified]`); no CLA was encountered anywhere in the fetched material.

## 16.4 Integration summary (one line per architecture component)

`paranext-core` = host · extension template + PAPI docs = how to build · `serval` + `machine.py` = alignment/MT services for retrieval · `web-xforge` = community-review pattern · `lexbox` / FLEx (`SIL.Lift`, `SIL.Scripture`) = lexicon and versification linkage · `aqua-api` + `aqua-extension` = QA-signal ingestion precedent · `ubsicap` USFM/USX/versification + `usfm-bible/tcdocs` = interchange anchors · `sil-org` (separate IT org) = deployment-phase identity (SAML stack) and backup patterns only.
