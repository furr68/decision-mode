# Decision Mode

Decision Mode is a workflow for turning ambiguous requests into a **clear decision** and an **immediately actionable plan**, while maintaining durable, auditable project state in Markdown files.

It is designed for situations where chat discussion is useful, but **files—not chat—must remain the source of truth**.

---

## What Decision Mode Produces

Decision Mode enforces two durable artifacts:

1. **Current plan (authoritative)**

   * `plan.md` or `docs/plan.md`
   * This is the single “current state” document that should always reflect the latest agreed direction.

2. **Decision log (history)**

   * One Markdown file per decision under:

     * `decisions/` if the plan is `plan.md`
     * `docs/decisions/` if the plan is `docs/plan.md`
   * Each decision is timestamped and self-contained.

---

## Core Principles

* **Plan is authoritative:** `plan.md` (or `docs/plan.md`) represents the latest agreed plan.
* **Coherence matters:** applying a decision requires updating *all impacted plan sections* so the plan remains internally consistent.
* **History is explicit:** every decision is recorded as its own Markdown file in the decisions folder.
* **Chat is non-authoritative:** anything that matters must be captured in the plan and decision log.
* **Patch-first outputs:** once a decision is made, the update is delivered as a **diff-style patch** that updates the plan and adds the decision log file.
* **No code generation:** do not output code or pseudocode as part of this mode.
* **No unstable commitments:** refuse timelines/commitments when inputs are missing or assumptions are unstable—capture missing inputs in the plan instead.

---

## File Conventions

### Plan file selection (Step 0)

1. Use `docs/plan.md` **if it exists**
2. Otherwise use `plan.md`
3. If neither exists, default to creating `plan.md` unless the user specifies otherwise

### Decision log location

* If plan is `docs/plan.md` → decision logs go in `docs/decisions/`
* If plan is `plan.md` → decision logs go in `decisions/`

### Decision log filename format

`YYYY-MM-DD_HHMM-<short-title>.md`

* Local time
* 24h clock
* Use a short, descriptive, kebab-case title

### Decision log first line (required)

The first line must be a top-level heading including day-of-week, date, and time, e.g.:

* `# Decision - Thu 2026-01-07 13:45`

---

## Operating Procedure

### Step 1 — Convert ambiguity into options

When a request is ambiguous or underspecified, Decision Mode responds using a fixed structure:

* **Options (exactly 3)**

  * Name (short label)
  * Rating: `X/10 (verbal strength)`
  * Suggested improvements (what would raise confidence)

* **Ask For Info (missing details)**

  * Only request inputs that materially affect the decision
  * Prefer precise prompts with constrained answer formats (e.g., “Pick one: A/B/C”)

* **Risks (top only)**

  * List the primary risks for the leading choices
  * Include the most direct mitigation or validation step for each

* **Rationale**

  * Explain why the leading option wins *given current info*
  * If you cannot choose responsibly, say so plainly and identify the minimum required info

* **Decision Framework**

  * Rate options against:

    * Meets goal (business/user value)
    * Feasibility (technical/operational)
    * Risk reduction (unknowns/dependencies)
    * Time-to-impact (speed to results)
    * Cost/effort (resources required)

* **Close (always)**

  * End with a direct next step: the user confirms Option 1/2/3, or asks you to propose a specific plan.

### Step 2 — After the user chooses, patch plan + add decision log

Once the user explicitly selects an option (or asks you to propose a specific plan), produce **only** a diff-style patch that:

* Updates the plan file to reflect the chosen decision as the new current truth
* Adds exactly one new decision log file with the required naming and header format
* Performs a “ripple check” to update all impacted plan sections (scope, assumptions, dependencies, risks, acceptance criteria, terminology, open questions, etc.)
* Ensures both files are self-contained and do not rely on chat text for key details

---

## When to Refuse Timelines

If the user requests delivery dates, milestones, or commitments but required inputs are missing or assumptions are unstable:

* **Refuse the timeline**
* Patch the plan to include a **Dependencies / Inputs Needed** section instead
* Capture what must be resolved before scheduling is responsible

---

## What “Good” Looks Like

A Decision Mode interaction is successful when:

* The user can open `plan.md` and understand exactly what is happening *now*
* The decision log provides an audit trail answering:

  * What was decided?
  * What options were considered?
  * Why was this chosen?
  * What risks remain and how will they be mitigated?
  * What references informed the decision?

---

## Quick Start

1. Ensure your repository has either `plan.md` or `docs/plan.md`.
2. When you have an ambiguous request, use Decision Mode to force a three-option decision.
3. After selecting an option, apply the generated diff patch so:

   * The plan becomes the updated source of truth
   * A new timestamped decision log is added under the correct folder

---

## Non-Goals

Decision Mode is not intended to:

* Replace normal brainstorming (it structures and concludes it)
* Generate implementation code
* Guess timelines in the absence of stable inputs

---

If you want, I can also rewrite this README for a specific repository layout (e.g., if you store modes under `prompts/` or `docs/modes/`) and include a brief “Folder Layout” section aligned to your actual structure.
