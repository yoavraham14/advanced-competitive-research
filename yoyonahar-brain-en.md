# Shared Brain — `yoyonahar` Project

> Version: 1.0
> Last updated: 2026-09-09
> Owners: Yoav Avraham and Nahar
> Source location: private `Git` repository, single file. This is the source of truth. No competing copy is kept anywhere else.

---

## About this file

This document holds the full context of the project and is used by both partners across two separate `Claude` accounts. It is attached as project knowledge in each account through the `GitHub` integration, so a change made here reaches both sides after a sync.

Maintenance rules, mandatory:

- Every material update is written into this file, not left in a chat. Chats close; the file stays.
- Every change gets a line in the changelog at the bottom, with a date and the name of whoever made it.
- Every factual claim about the market or a competitor is written with a source and a date. A claim without a source is explicitly marked as an estimate.
- The version number at the top increments on every content update.

---

## Who we are

Yoav Avraham — `DevOps` and `Platform` engineer with roughly 4.5 years of experience, based in Israel. Owns infrastructure, architecture, the stack, and running the system.

Yoav's stack: `Linux`, `Bash`, `Python`, `FastAPI`, `Docker`, `Kubernetes`, `OpenShift`, `Ansible`, `ArgoCD`, `GitLab`, `GitHub Actions`, `Prometheus`, and `Grafana`. `GCP` services, and `Cloud Run` in particular, are the foundation of this product.

Nahar — full business partner. He built the existing prototype, and he is currently employed at the company whose cloud infrastructure the prototype runs on. That fact is the source of the central legal constraint described below.

---

## What the project is

We are building a service and a product for competitor research, aimed at Israeli `B2B SaaS` companies.

The core idea is continuous market memory. Not one-off research, but competitor profiles that update over time, dated change logs, and the client's own internal context — vision, positioning, and ideal customer profile. All of it queryable through a language model.

The differentiator is not the technology. The differentiator is continuity over time, data quality, the client's organizational context, and focus on the Israeli-European market.

---

## Business model

The chosen model is service first, product later.

In the first phase we perform the research ourselves as freelancers, with our tool as the internal engine. The first clients are design partners at cost price, and we shape the methodology with them. Only after we have proof and repeatable deliverables do we turn this into a standalone product.

Target pricing for the commercial phase is roughly 10,000 shekels for onboarding, plus roughly 5,000 shekels per month. The comparison baseline is the fully loaded employer cost of a `PMM` or a junior analyst in Israel, which runs around 30,000 to 40,000 shekels per month.

---

## Two-year vision

The goal is at least 50 clients, each personally handled by a dedicated account rep, with headcount scaled to the client base.

Two parallel tracks are planned:

- A standalone self-serve product companies can buy without the service layer.
- A boutique service for companies that want personal attention.

A hard prerequisite for that scale is efficient client onboarding. The intent is a mechanism where the client supplies raw material — decks, site content, internal documents — and the engine derives the structured context files from it on its own. Without automating that step, the model does not scale.

One open item in the vision is the permissions and access mechanism separating clients from one another. It has not been decided whether the answer is architectural or contractual through a confidentiality document.

---

## Current state

A working prototype exists, known by its historical name `SynCloud Recon`. Its architecture:

- Compute layer: a `FastAPI` application on `Python 3.12`, running on `Cloud Run` in two environments — `syncloud-recon` for production and `syncloud-recon-staging` for testing.
- Intelligence layer: `Vertex AI` with `gemini-3.1-pro-preview` through the `google-genai SDK`, in two modes — with `grounding` for live search, and without it for local `function-calling`.
- Data layer: a `GCS` bucket named `gs://syncloud-recon-data`, holding `companies.json`, plus `profiles/*.md`, `diffs/*.md`, `insights/*.md`, `context/*.md`, and `history/*.json`.
- Main modules: `main.py` for routing and `HTTP Basic Auth`, plus `recon.py`, `briefing.py`, `pm_query.py`, `filters.py`, `gemini_client.py`, `data_loader.py`, `gcs_storage.py`, `history.py`, and `pdf_export.py`.
- `CI/CD` layer: `GitHub Actions` workflows using `Workload Identity Federation`, with no long-lived `JSON` keys. Current repository: `NaharMagar/competitor-synscloud-app`.

Three usage modes exist in the system:

- `Deep Recon` — full research with live search.
- `Quick Briefing` — a short briefing ahead of a call.
- `Ask a Question` — querying local data only.

What is missing: the automated multi-source collection layer. That part has not been built yet.

Where it currently sits: the prototype runs on the cloud infrastructure of the company Nahar works at, not on resources we own. A rebuild on our own independent `GCP` account is planned in the near term. Until that happens we have no environment we can show a paying client or build a business on, and this is blocking task number one.

---

## Settled decisions

Do not reopen these without a new and material reason:

- Focus on competitive intelligence, not prospect briefing. `Quick Briefing` mode stays a secondary feature only, because that field is taken by `Aomni` and others.
- Manual service first, product later. First design partners at cost price.
- The target audience is Israeli `B2B SaaS` companies from `pre-seed` and `seed` through `Series C`. Enterprise and large companies are not the target audience and are not a future goal.
- The main buying signal is an open `PMM` role at the target company. The job posting itself is the signal.
- The current working name is `yoyonahar`. `SynCloud Recon` remains only as the prototype's historical name and is not for external use, because it is tied to the company Nahar works at.
- No `MCP` server exists today and none is planned in the near term, because we are the only users of the engine. On the merits too, `MCP` is not a differentiator — `Crayon` launched such a server back in September 2025, followed by `Similarweb`, `Klue`, and `Valona`.

---

## Hard constraints

These must not be forgotten in any conversation on this project:

- **Legal risk in data sources.** `G2` explicitly prohibits automated scraping in its terms as of July 2026, including for publicly accessible content. `Reddit` has blocked unauthenticated endpoints since May 2026, charges roughly 12,000 dollars per month on the commercial tier, and actively sues scrapers. Any design of the collection layer must rest on licensed data providers or an official `API`, not on direct scraping.
- **Israeli anti-spam law.** Outbound outreach must be personal and targeted, not identical mass mailing. Keep the wording individual for every recipient.
- **Accuracy above all.** Every factual claim about a competitor — a date, a funding round, a pivot, a price — must come with a source and a date. If there is no source, say there is none. `Klue` already markets an entire campaign around model hallucinations on this exact point, and this is the breaking point of client trust.
- **IP separation from the employer.** The prototype was built on an employer's infrastructure. Until a clean separation of code, repository, data, and name is complete, we must not charge a client and must not present the tool as our asset. This is not a technical problem of moving servers, and it must be treated as a business blocker.
- **`AWS` is not Yoav's specialty.** Do not describe it as such in any material, deck, or phrasing.

---

## Competitive landscape

The five meaningful threats, in order:

1. Deep research from language models at roughly 20 dollars per month. This is the central threat, not any single company. The only answer is continuity over time plus organizational context.
2. `IntelCue` — competitive intelligence inside `Claude` and `ChatGPT` through `MCP`, at a flat 8.99 dollars per month.
3. `IndustryLens` — `MCP-first`, between 59 and 149 euros per month, targeting exactly the same segment.
4. `RivalSense` — Riga-based, between 45 and 223 dollars per month, same founder-and-manager audience. Poor execution quality according to reviews.
5. `Parano.ai` — priced by number of competitors with unlimited users, from roughly 89 dollars per month.

In the enterprise tier sit `Klue`, at roughly 20 to 40 thousand dollars per year with about 500 clients, roughly 78 percent of them in the United States, and `Crayon`, acquired for roughly 1.34 billion dollars in May 2025. They set the narrative in the market but do not compete in our price range.

A key figure worth remembering: roughly 40 percent of competitive intelligence tool deployments are abandoned within 12 months, mainly due to alert overload and the absence of an internal owner. Our service has to solve that up front, not after the fact.

Note on data validity: the numbers in this section came from market research conducted before September 2026. Re-verify every price and funding figure before using it in front of a client.

---

## Open questions

- Exactly who to approach at the target company. The current assumption is the `CTO`, but the budget holder for a `PMM` hire is usually the `VP Marketing`, the `CMO`, or the CEO. Test both paths in parallel and measure who responds.
- What the right delivery cadence is in the managed service. The current proposal is a weekly briefing, a monthly change log, and a quarterly review.
- Which licensed data sources to choose for the first collection layer.
- How to execute the separation from the employer cleanly. The open questions are whether a from-scratch rewrite on our own resources is required, and what Nahar's personal agreement with the company says about intellectual property clauses.
- What the final commercial name will be. The working name `yoyonahar` is internal and has not been settled as an external name.
- How to separate different clients' data from one another — an architectural solution or a confidentiality document.

---

## How to work with us

Language: reason in Hebrew, and always deliver the final answer to Yoav in Hebrew, regardless of the language of the question, the language of this file, or the language of any material pasted into the conversation. For Nahar, match the language he writes in. This file is written in English only because it is shared infrastructure; it is not an instruction to reply in English.

Hebrew writing rules, mandatory when the reply is in Hebrew:

- Start every paragraph, sentence, and bullet with a Hebrew word. Never with an English word and never with a number.
- Wrap English technical terms and product names in backticks.
- Do not mix English and Hebrew in the same sentence beyond wrapped terms. Explain in Hebrew; put code and commands on a separate line.
- End sentences with a Hebrew word so punctuation falls correctly in `RTL`.
- Deliver long Hebrew documents as an `HTML` artifact with `dir="rtl"`, unless another format was requested for copying.

Yoav's personal rule: when Yoav pastes output or a report from `Claude Code`, the first explanation must be in completely plain language, as if to someone with no technical background at all, using everyday analogies and with no unexplained technical term. Only after that plain explanation come a short technical summary and the next prompt. This is a permanent, binding preference.

General style rules:

- Write concisely and technically. No motivation, no compliments, no repeated summaries of what is already known.
- Challenge our assumptions when they look wrong, including decisions already made. Better to hear it now than to discover it in front of a client.
- When citing a market or competitor figure, give a source and a date. If the source is a vendor's own blog, say so explicitly.
- Propose one concrete next step at the end, not an open list of options.

---

## Changelog

- 2026-09-09 — Yoav — Version 1.0 of the shared file created. Merged the original project instructions with accumulated conclusions: working name `yoyonahar`, target audience widened to `pre-seed` and `seed` with enterprise excluded, `MCP` removed from the near-term scope, the two-year vision, and the mechanism for deriving context from client raw material.
