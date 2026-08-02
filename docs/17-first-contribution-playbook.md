# 17. First-Contribution Playbook: Targets and the Human/AI Division of Labor

> Two things in one operational document: (A) live reconnaissance of the SIL repositories where trust-building contributions should start — including a **correction** to doc 16's web-xforge assessment — and (B) a step-by-step decomposition of the contribution process (following the Open-Source Contribution Protocol field guide) classifying which steps AI can perform and which are irreducibly human. Recon data verified against the repositories on **2026-08-02**; assignment states and issue lists go stale — re-verify before claiming anything ([protocol §2.7]).

## 17.1 Reconnaissance results

### Cross-cutting findings

- **Legal friction: none found.** Every surveyed repo is MIT; no CLA and no DCO were found anywhere in the surveyed sillsdev repos.
- **AI-contribution policy: none exists, anywhere in sillsdev** — no contributor-facing policy in any surveyed repo, and the org-wide [`sillsdev/.github`](https://github.com/sillsdev/.github) repo is a stub (FUNDING.yml only). More telling: the org *institutionalizes AI-assisted development internally* — [`machine.py` ships an in-repo Claude Code skill](https://github.com/sillsdev/machine.py/blob/main/.claude/skills/port-pr/SKILL.md) for porting C# PRs to Python, and [FieldWorks' CONTRIBUTING](https://github.com/sillsdev/FieldWorks/blob/main/Docs/CONTRIBUTING.md) directs core developers using Copilot/Claude Code to a documented AI-PR workflow. This is the opposite of the curl/Ghostty environment. Default conduct where policy is absent: disclose material AI assistance, own every line.

### Per-repo assessment

| Repo | Contribution docs | Newcomer entry | Local setup | Verdict |
|---|---|---|---|---|
| **web-xforge** (Scripture Forge) | [CONTRIBUTING.md](https://github.com/sillsdev/web-xforge/blob/master/CONTRIBUTING.md) covers only commit style (`SF-xxxx` Jira-ID prefix, imperative, squash-merge); real dev docs in the wiki | **None (correction to doc 16).** `good first issue` and `help wanted` queries return **zero** open issues; the public tracker is essentially empty; work is driven from internal Jira; all 15 most recent merged PRs are core-team; the wiki gates developer secrets on "ask another developer" | Heavy: .NET 8 + Node 22 + MongoDB 8 + Mercurial + FFmpeg (maintained devcontainer exists) | **No cold entry.** The only channel is maintainer-confirmed work: email help@scriptureforge.org or go through the SIL collaboration contact. Do not cold-PR |
| **machine.py** | No CONTRIBUTING.md; conventions inferable: Poetry, `.flake8`/`.pylintrc`, `local_check.sh` (one-command format+lint+typecheck+test), devcontainer | **12 open issues, all unassigned** at survey time — see 17.2 | Light: Python + Poetry; ML-heavy tests exist but docs/USFM work runs anywhere | **Primary target** |
| **languageforge-lexbox** | No CONTRIBUTING.md; per-OS developer guides in README | Best label hygiene surveyed: **4 unassigned `good first issue`s**, incl. [#2305](https://github.com/sillsdev/languageforge-lexbox/issues/2305) "Minor improvement to BulkCreateEntries" (May 2026, fresh); also #1875, #1095, #1094 | Heavy: Docker + Tilt + k8s-enabled Compose + .NET 8 + Node 20 + Postgres + Mercurial | Second track — worth the setup cost only if lexicon integration matters near-term |
| **TheCombine** | No CONTRIBUTING file but high-quality per-OS README, style guides, test/coverage docs | 3 unassigned frontend `Size:S` `good first issue`s ([#3602](https://github.com/sillsdev/TheCombine/issues/3602), [#3545](https://github.com/sillsdev/TheCombine/issues/3545), [#2580](https://github.com/sillsdev/TheCombine/issues/2580)) — **labels 1.5–3 years stale**; verify currency before claiming | Moderate-heavy: Node 22 + Python 3.12 + .NET 8 + MongoDB + FFmpeg | Fallback only — least relevant domain |
| **FieldWorks** | [Docs/CONTRIBUTING.md](https://github.com/sillsdev/FieldWorks/blob/main/Docs/CONTRIBUTING.md): fork → branch → NUnit → PR; documented AI-PR workflow (framed for core developers) | Not surveyed for issues | **Windows required** for builds/tests | Only viable on Windows |

External-PR treatment: recent merged PRs in web-xforge and machine.py are all core-team — a *neutral* signal (no evidence of external PRs being rejected; simply few outsiders). This ecosystem runs on small institutional teams; the protocol's small-project adaptation applies (direct communication, low ceremony, reduce review burden aggressively).

## 17.2 Recommended starting sequence (machine.py first)

Why machine.py wins on the protocol's own suitability dimensions: legal clarity 4/4 (MIT, no CLA), local feasibility 4/4 (lightest setup, one-command validation), work alignment 3–4/4 (live unassigned issues), and **personal value 4/4 — this library is the decision-support system's own critical path** (USFM parsing + word alignment = the machinery of precedent retrieval). Contributions here build trust with the exact team (Serval/machine) that matters for the later proposal, while building competence in code the project will depend on.

Candidate ladder (protocol Phase 10 rungs, with live issue numbers from the survey):

1. **Baseline first, no PR** — clone, devcontainer, `local_check.sh`, record the baseline (protocol Phase 3).
2. **[#165](https://github.com/sillsdev/machine.py/issues/165) "Update README to include conda Dev"** — rung 3 (docs), done properly: *execute* the instructions, fix what's actually wrong, verify by running them.
3. **[#174](https://github.com/sillsdev/machine.py/issues/174) "Add deuterocanon tests"** — rung 5: tests-only, no behavior change.
4. **[#149](https://github.com/sillsdev/machine.py/issues/149) "No error thrown when handling unknown BookNameForms"** — rung 6: small confirmed defect, natural single-regression-test scope.
5. **[#337](https://github.com/sillsdev/machine.py/issues/337) (port a USFM fix from C#)** — best technical fit, but it is exactly the shape of the team's own `port-pr` AI skill: **ask availability first** (protocol template 9.1); it may be earmarked for their internal workflow. The ask itself opens working contact with the team.

Parallel: route the web-xforge conversation through email/SIL contacts ("what would the team welcome from an outside contributor?") — treat any answer as discovery channel #1, which outranks all scoring.

## 17.3 The division of labor: model

Every step below is classified into one of four lanes:

| Lane | Meaning |
|---|---|
| **AI** | AI can execute end-to-end; human spot-checks the output |
| **AI→H** | AI drafts or executes; the human must verify, edit, and **own** the result before it goes anywhere |
| **H+AI** | Human leads and must be genuinely engaged; AI assists (advises, checks, accelerates) |
| **H** | Human only. Non-delegable — delegating it either breaks an attestation, breaks a relationship, or defeats the purpose of the exercise |

Two principles govern the classification — both come from the protocol itself:

1. **The ownership warranty.** "Never submit anything you cannot personally explain, debug, and defend line by line" is the consensus rule of the 2026 landscape. Any step whose output carries your attestation (a PR, a claim of reproduction, a disclosure statement, a security judgment) can be AI-*assisted* but must be human-*warranted* — and the warranty is only real if the human did enough of the step to survive a probing question in review.
2. **The purpose test.** This whole exercise exists to *prove your competence and build trust*. Steps that generate the trust signal — authored communication, demonstrated understanding, judgment about maintainer priorities — cannot be delegated without hollowing out the thing being built. Steps that are mechanical evidence-gathering or convention-mirroring generate no trust signal by being done by hand; delegate them freely and spend the saved hours on the steps that do.

A corollary worth stating: the split is not static. **Early rungs: do more yourself than capability requires** — maintainers probe newcomers, and the machine.py team can smell a port-pr-shaped PR from someone who can't answer questions about it. **Later rungs: shift mechanical load to AI as credibility and understanding accumulate** — which is exactly the end-state the machine.py team itself models: AI as tooling inside a codified, human-owned workflow.

## 17.4 The process, step by step

### Phase 1–2: Selection and reconnaissance

| Step | Lane | Notes |
|---|---|---|
| Define intent: why this project, time budget, willingness to maintain after merge | **H** | These are your commitments; an AI can prompt the questions, not answer them |
| Gather project health data (commit/PR/issue flow, release cadence) | **AI** | Exhaustive data collection is where AI beats a human |
| Judge maintainer *tone* and community safety from that data | **H+AI** | AI surfaces the threads; reading tone is human judgment |
| Extract contributor infrastructure (CONTRIBUTING, templates, ownership files) | **AI** | With citations |
| Read the AI policy and code of conduct | **H** | You will be held to these; read the primary text yourself, not a summary |
| Read recent merged and rejected PRs for hidden conventions | **AI→H** | AI does breadth across dozens; you personally read 2–3 exemplars to calibrate voice and tone |
| Fill the suitability scorecard | **AI→H** | AI drafts scores with evidence; the protocol itself says the score is "a prompt for judgment, not a verdict" — the judgment is yours |
| Go/no-go decision | **H** | Every stop condition is a human decision |

### Phase 3: Baseline

| Step | Lane | Notes |
|---|---|---|
| Environment setup, build, full validation run, log capture, baseline template | **AI** | Agents do this well and document it better than humans bother to |
| **Replay the key commands yourself once** | **H** | Non-negotiable: during review a maintainer will ask "can you check X?" — if only the AI can drive the validation loop, you can't respond competently. The baseline attestation is yours |
| Confirm no secrets, no production resources | **H** (after AI scan) | Safety-critical confirmations are dual-checked, human-final |
| Compare local results with CI runs | **AI** | |

### Phase 4–5: Exploration and testing assessment

| Step | Lane | Notes |
|---|---|---|
| Structural inventory / repository map | **AI** | |
| Execution-path trace of one vertical slice | **H+AI** | **The map is not the deliverable; your understanding is.** AI can produce a perfect trace document while you learn nothing. Drive the debugger yourself at least once; use AI as the guide, not the substitute |
| Change-history analysis (blame, related PRs, reverts) | **AI** | |
| Risk-signal search (TODOs, skips, empty catches) | **AI** | Every hit is a lead, not a finding |
| The four-question context check per hit (intentional? reachable? current? consequential?) | **AI→H** | AI argues each side; you rule |
| Verified-unknowns list; blocking/non-blocking calls | **AI→H** | AI drafts; you decide what blocks |
| Test inventory, CI-actually-runs-what verification, maturity-rubric placement | **AI** | |
| The one-of-ten scaffolding decision | **H+AI** | Binds future review burden — a judgment call; the decision tree advises, you decide |

### Phase 6–8: Discovery, validation, prioritization

| Step | Lane | Notes |
|---|---|---|
| Candidate generation from issues/history | **AI** | |
| Duplicate/in-flight-PR search | **AI** | Another exhaustiveness win |
| Reproduction | **AI→H with a human witness run** | AI can find and script the reproduction; **you must run it at least once** — the evidence package implicitly attests "I reproduced this" |
| Falsification checklist (docs check, upstream cause, flags, platform) | **AI→H** | AI executes the checks; you own the conclusion "this is a real bug" |
| Security-sensitivity screening | **H** (AI may only *raise* suspicion) | Asymmetric by design: AI flagging something as possibly sensitive is safe; AI *clearing* something as non-sensitive is the curl failure mode. Exploitability is verified by a human against the code as written — never on a tool's assertion |
| Channel decision (issue vs. discussion vs. PR vs. private) | **H+AI** | Social consequences; protocol table advises |
| Alignment gate ("is this wanted?") | **H** | It is a judgment about other humans' priorities; AI can only summarize signals |
| Scoring + decision record | **AI→H** | AI computes; you sign |

### Phase 9: Maintainer engagement

| Step | Lane | Notes |
|---|---|---|
| Availability asks, clarification requests, proposals, follow-ups, disagreement, withdrawal | **H** | **The trust currency is minted here and nowhere else.** Your own words, your own voice — some projects ban AI prose outright; even where SIL doesn't, detectably generated communication burns exactly the credibility this exercise exists to build |
| Pre-send check of your draft (facts, links, tone-against-etiquette, "did I include an easy way to say no?") | **AI** | AI as editor/checker of your writing — never author |
| Cadence tracking (when a follow-up is due per project norms) | **AI** | |

### Phase 10–11: Implementation

| Step | Lane | Notes |
|---|---|---|
| Branch mechanics, rebase, convention conformance | **AI** | |
| Write the failing regression/characterization test | **AI→H** | AI drafts; **you verify it fails for the right reason** (watch the red→green transition yourself; mutate the fix and watch it go red again) |
| The fix itself | **AI→H** — with an early-rung caveat | At SIL, AI-drafted code with full human ownership is culturally normal (they do it themselves). But on rungs 3–6, write more of it yourself than capability requires: the review conversation *is* the competence exam, and you sit it alone |
| Line-by-line self-review as a hostile reviewer | **H** (after an AI review pass) | Checklist 10.8's last item is explicit: every line understood and defensible — *including AI-generated lines*. An AI review pass before yours catches mechanical issues; it does not substitute for your read |
| Diff hygiene scan (secrets, debug output, unrelated changes, lockfile noise) | **AI→H** | AI scans exhaustively; you confirm — secrets are safety-critical |
| Commit messages in project convention | **AI→H** | AI drafts to the observed convention; you approve |
| PR description | **H+AI** | AI assembles the evidence sections (reproduction, validation results — they're records of real runs); the problem statement, scope rationale, and risks are your voice and your claims |
| **AI-assistance disclosure statement** | **H** | A first-person attestation ("I have reviewed, tested, and can defend every line"). Having an AI write your attestation of human oversight is the exact failure the 2026 landscape is defending against |
| CI monitoring, fixing introduced failures | **AI→H** | AI watches and drafts fixes; you own what gets pushed |

### Phase 12: Review and post-merge

| Step | Lane | Notes |
|---|---|---|
| Summarize a long review thread; check your planned response addresses every point | **AI** | |
| Responding to review feedback | **H** | Same rule as Phase 9 — this is where the maintainer decides whether you are someone they want around |
| Deciding to narrow, defer, or withdraw | **H** | Sunk-cost resistance is a human discipline |
| Post-merge verification runs; watching subsequent releases | **AI** | |
| Reading maintainer signals to pick the next contribution | **H+AI** | AI aggregates the signals; you interpret the relationship |

## 17.5 The irreducibly human core

Compressing the table: eight things can never move out of the **H** lane, regardless of how good the tooling gets — the first four because they are *attestations*, the last four because they are *relationships and judgment about people*:

1. **Ownership attestations** — the claim behind every submission that a human stands behind every line, has reproduced what they say they reproduced, and can defend it under questioning.
2. **The disclosure statement** — a first-person certification of human oversight.
3. **Security clearance decisions** — clearing something as non-sensitive, and verifying exploitability. (AI may raise suspicion; only a human may lower it.)
4. **Secrets/production-safety confirmations** — final human check, always.
5. **Authored communication with maintainers** — every message that builds or spends trust.
6. **The alignment judgment** — reading what other humans want, need, and mean by silence.
7. **Go/stop decisions at every gate** — including resisting sunk cost and respecting a "no."
8. **Intent and commitment** — why you're contributing and whether you'll still be there after the merge.

And one thing that is technically delegable but strategically shouldn't be, early: **enough of the implementation and tracing work to make your understanding real** — because in the trust-building phase, the review conversation is an oral exam, and the protocol's own governing rule ("never submit anything you cannot personally explain") is graded there.

## 17.6 SIL-specific calibration

- **No AI policy anywhere in sillsdev** → default to the protocol's conservative posture: disclose material assistance (template 9.11), own every line. Disclosure here is low-cost precisely because the teams use these tools themselves.
- **The machine.py `port-pr` skill is the target end-state**: AI executing a codified workflow inside human review — proof that "AI-heavy but human-owned" is this team's native culture. Reaching the point where *your* AI-assisted throughput is trusted the way they trust their own is what the ladder is for.
- **web-xforge and Scripture Forge**: relationship-first, not PR-first (17.1). The human-only lane does all the work there initially.
- Re-verify everything in 17.1–17.2 at time of use: issue states were live on 2026-08-02 and several GitHub pages rendered partially during the survey.
