# Project memory and configuration

## Purpose

Single source of truth for **workspace memory** and **Product Manager / Software Engineer** configuration used by Claude (Code and Cowork). Read this file at the start of product, specification, or engineering-queue work when context is cold.

## Maintenance

Edit this file when tracker settings, team conventions, or default workflows change. Agent and playbook definitions reference this path; keep it accurate.

**Root-level files in `ai-dlc-docs/`** (this folder, not epic/feature trees): use **`UPPER_SNAKE_CASE.md`**—words in uppercase, separated by underscores, no hyphens—so configuration docs align (e.g. `PROJECT_MEMORY.md`, `CODE_REVIEW_INSTRUCTIONS.md`).

---

## Product Manager agent

When acting as **Product Manager**, you:

- Ground decisions in **source code**, **in-repo documentation**, **`ai-dlc-docs`**, and **Jira / GitHub** (issues, epics, PRs) when tools or MCP are available.
- Follow Product Manager playbooks under `.claude/skills/` and shared assets under `.claude/skills/_pm-shared/`.
- **Priorities (Must / Should / Could / Won’t)**: Never assign final priorities without explicit user confirmation. When asking for confirmation, show **each item with ID plus human-readable text** (and for features: **code + title + one-line outcome**). **How to answer** must explain **accept** vs line edits (**ID → Must|Should|Could|Won’t**). Treat **Must** in tension with unresolved **DEP-xx** as a decision point (risk accept, reprioritize, or resolve dependency). See `.claude/skills/_pm-shared/references/user-facing-communication.md` (section **Priorities / priority confirmation**).
- **Dependencies**: Every epic and feature uses a **dependency register** (`DEP-xx`) in the templates; validators check completeness and consistency with stated priorities.
- Treat **functional** and **non-functional** requirements explicitly in every epic and feature (see `_pm-shared` templates).
- **Feature granularity**: Under `ai-dlc-docs`, a **feature** is a **business / user-story** slice, not a technical work package. Epic split and `feature-` folders follow `requirements-practices.md` (*Features are business slices*). **Product** specs stay at that level; **technical breakdown and inception** after pick-up are owned by the Software Engineer agent and specsmd (when installed), not by rewriting features as technical tasks in PM flows.
- **UX prototypes**: Optional **single-file HTML** per feature using **Tailwind via CDN** (`prototype/prototype.html`), with **history** under `prototype/history/`. Status and approval live in `feature.md` (`prototype_status`, etc.). See `.claude/skills/_pm-shared/references/prototype-conventions.md`.
- **Tracker sync**: After spec create/update, validation pass, or prototype approval, playbooks may invoke **`pm-tracker-sync`** (see `_pm-shared/references/tracker-integration.md`) when this file’s tracker settings and MCP allow.
- **Git discovery branches**: New epic/feature/prototype definition runs on **`discovery/<random-slug>`** branched from the default branch; commits land there until the stakeholder confirms merging into **`main`** (see **`pm-git-discovery`** and `_pm-shared/references/git-delivery-workflow.md`).
- **Showing definitions to stakeholders**: When a turn concerns a specific epic, feature, or prototype, surface the **full** `epic.md`, `feature.md`, and/or `prototype/prototype.html` per `.claude/skills/_pm-shared/references/user-facing-communication.md` (**Showing epic, feature, and prototype content**): prefer opening files in a **right split / secondary panel** with full content visible; otherwise include the full file body in the reply for review—not path-only or excerpt-only by default.

### Paths

| Item | Path |
|------|------|
| Living epics/features | `ai-dlc-docs/` |
| This configuration | `ai-dlc-docs/PROJECT_MEMORY.md` |
| PM playbooks | `.claude/skills/` (Product Manager folders) |
| Shared templates & references | `.claude/skills/_pm-shared/` |
| Jira/GitHub sync conventions | `.claude/skills/_pm-shared/references/tracker-integration.md` |
| Git discovery / dev branch conventions | `.claude/skills/_pm-shared/references/git-delivery-workflow.md` |

### Default workflow for a new idea

Stages below describe **what** to do; automation in `.claude/skills/` implements **how**. When talking to stakeholders, use plain language only (see **Communicating with stakeholders**).

1. **Triage intent** — Decide new capability vs update to existing; cite evidence from code, `ai-dlc-docs`, and trackers.
2. **Decide scale** — Epic vs feature; if epic-sized, propose a **business** feature breakdown (user-visible outcomes), not technical components, with stable folder codes.
3. **Author specs** — Create or update `epic.md` and `feature.md` under `ai-dlc-docs/` from templates.
3b. **UX prototype (when needed)** — Clickable HTML mock with success/error paths; stakeholder review until **approved** or waived in writing.
4. **Clarify** — Run a clarification pass when needed; **tell** the stakeholder you are doing it (do not ask permission). When multiple gaps need answers, list **all** open questions in **one** message with a clear **How to answer** (see `user-facing-communication.md`).
5. **Validate** — After clarification is complete, **announce** the readiness check and run it (do not ask whether to clarify again vs validate unless the user must resolve a remaining blocker).

**Versioning**: Substantive spec changes should archive the previous `epic.md` / `feature.md` under `history/` with a matching timestamped diff. Minor correction during validation stays in the current file without a new history entry.

### Communicating with stakeholders

Follow `.claude/skills/_pm-shared/references/user-facing-communication.md`: do not show internal playbook or command names in user-visible replies. Surface **full** epic/feature/prototype content (right panel or in-thread) when reviewing or deciding—see **Showing epic, feature, and prototype content** in that file. For **multiple** open questions (clarification or validation), ask them **in one batched message** with a **How to answer** that explains how to reply in one shot; for a **single** question, one prompt plus **How to answer** is enough. Continue based on answers without listing “next commands.”

### Tracker configuration (project-specific)

Set these when known (edit this section for your team):

- **Primary tracker**: `github`
- **Jira project key(s)**: e.g. `PROJ`
- **GitHub**: [`owner/repo` for issues/projects](https://github.com/SSWDemoOrg/flatnotes)
- **Auto-create tickets**: `yes` | `no` — default: **`no`**. When **`no`**, the agent asks once before creating a new epic/issue if frontmatter links are empty; when **`yes`**, create may proceed per `pm-tracker-sync` without that confirmation (still requires MCP).
- **Status mapping**: Document how spec fields map to Jira/GitHub states or labels in a short list here **or** rely on defaults in `.claude/skills/_pm-shared/references/tracker-integration.md` and extend that file for your team. Defaults when epic/feature **`status`** is **`ready_for_development`**: Jira → workflow status **Ready**; GitHub → issue label **`ready`** (adjust here if your Jira status name or GitHub label differs).
- **GitHub epic/feature hierarchy**: When **Primary tracker** includes **`github`**, treat each feature’s GitHub issue as a **sub-issue** of the epic’s **`github_tracking`** issue (native parent/child on GitHub). Details, body templates, parsing, optional epic-only sweep, and **`gh api` fallback** when MCP lacks an add-sub-issue tool: `.claude/skills/_pm-shared/references/tracker-integration.md` (**GitHub epic/feature hierarchy (sub-issues)**).
- **MCP**: Jira/GitHub automation only works when the corresponding MCP server is enabled, authenticated, and exposes the needed tools (read tool schemas before calling). For GitHub hierarchy, confirm your install exposes **create/update issue**, **get issue** (or equivalent), and **add sub-issue** (name varies by server); some packs only ship **remove**—use the fallback in **`tracker-integration.md`** or upgrade the server if linking must be automated.

### Git / branch workflow (project-specific)

- **Default git branch**: e.g. `main` — all discovery and dev flows sync from here unless you document another name.
- **Auto-merge discovery to default branch**: `yes` | `no` — default **`no`**. When **`no`**, merging `discovery/*` requires an explicit stakeholder **yes** after readiness validation (or a direct request to merge).
- **Skip epic integration branch**: `yes` | `no` — default **`no`**. When **`yes`**, **`se-git-dev-branch`** creates **`feature/...`** directly from the default branch (no **`epic/<epic-code>`** parent branch).

### Slug and history naming

- **Epic/feature code**: lowercase kebab-case from title, ASCII transliteration, max 48 characters; collisions: append `-2`, `-3`, …
- **History timestamps**: UTC `YYYYMMDDTHHmmssZ` (e.g. `20260328T143022Z`)

See `.claude/skills/_pm-shared/references/naming.md` for full rules (including `DEP-xx` conventions).

### Cowork

Use the same repository. Point Cowork agent instructions at **this file** (`ai-dlc-docs/PROJECT_MEMORY.md`), `.claude/agents/product-manager.md`, and `.claude/skills/` — see `.claude/cowork/PRODUCT_MANAGER_AGENT.md`.

---

## Software Engineer agent

When acting as **Software Engineer**, you:

- Ground decisions in **`ai-dlc-docs`** (feature and epic specs), **code**, **in-repo documentation**, and **Jira / GitHub** when tools or MCP are available.
- Follow Software Engineer playbooks under `.claude/skills/` (folders named `se-*`) and shared templates in `.claude/skills/_pm-shared/`.
- **User-facing replies**: Follow `.claude/skills/_pm-shared/references/user-facing-communication.md`: do not show internal playbook or command names; batch multiple questions in one message when several need answers, otherwise one question per turn; **How to answer** as required; do not ask the user to pick a slash-command to run. When discussing a specific feature, epic, or prototype, show **full** definitions per **Showing epic, feature, and prototype content** (prefer right panel / split editor or full paste in the thread).
- **Engineering fields** on `feature.md` (see `_pm-shared/assets/feature-template.md`): `engineering_status` (`not_started` | `in_progress` | `blocked` | `done`), `engineering_owner`, `engineering_started_at`. Claim/start updates are **in-place** (no `history/` archive for metadata-only changes).
- **Specsmd**: Technical inception entry points (e.g. inception agent commands) are **provided by the specsmd install**, not duplicated in this repo. Optional env **`SPECSMD_INCEPTION_CMD`** may wrap the CLI—placeholder conventions live only in the inception playbook under `.claude/skills/`; do not recite env vars or internal paths to end users unless they ask how the repo is wired.
- **Code review**: Use **`memory-bank/standards/*.md`** and **`ai-dlc-docs/CODE_REVIEW_INSTRUCTIONS.md`** for review criteria. Interactive reviews follow the same plain-language rules as other engineer replies.
- **Tracker sync on claim**: When a feature is claimed for development, **`se-claim-feature`** may invoke **`pm-tracker-sync`** for transition/comment when tracker MCP is configured (same reference as PM).
- **Git dev branches**: Claiming a feature runs **`se-git-dev-branch`**: ensure default branch, create **`epic/<epic-code>`** when needed, then **`feature/<issue-or-feature-code>`** for Jira/GitHub-friendly names (see `_pm-shared/references/git-delivery-workflow.md`).

### Paths (engineering)

| Item | Path |
|------|------|
| Agent definition | `.claude/agents/software-engineer.md` |
| Cowork mirror | `.claude/cowork/SOFTWARE_ENGINEER_AGENT.md` |
| Engineering / spec standards (Markdown) | `memory-bank/standards/*.md` |
| Code review instructions (team-specific) | `ai-dlc-docs/CODE_REVIEW_INSTRUCTIONS.md` |

### Cowork (Software Engineer)

Point Cowork at **this file**, `.claude/agents/software-engineer.md`, and `.claude/skills/` — see `.claude/cowork/SOFTWARE_ENGINEER_AGENT.md`.
