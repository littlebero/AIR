# AIR — DECISIONS LOG

This document records structural decisions that must not drift over time.
If a decision changes, it must be appended — never silently replaced.

---

## 2026-02-13 — Git as Absolute SSOT

Decision:
GitHub repository is the Single Source of Truth.
Chat is working memory only.

Reason:
Prevents architectural drift between sessions.
Prevents memory dependency on AI conversation history.

Impact:
All structural changes must be committed.
No undocumented state allowed.

---

## 2026-02-13 — Single Model Routing (Opus 4.6)

Decision:
Default model fixed to:
anthropic/claude-opus-4-6

No multi-model routing enabled.

Reason:
Stability over complexity.
Avoid premature optimization.
Routing expansion intentionally paused until structure is mature.

Impact:
No critic/writer split.
No fallback chain.
All LLM tasks use single primary model.

---

## 2026-02-13 — Generated Output Ignored

Decision:
ssot/research_out/ is ignored via .gitignore.

Reason:
Generated artifacts should not pollute SSOT.
Only reproducible logic and documentation belong in Git.

Impact:
Research outputs are ephemeral.
Scripts that generate them are tracked.

---

## 2026-02-13 — Runbooks Are Structural Assets

Decision:
Operational scripts (rss_fetch.py, rss_run.sh) moved to:
ssot/runbooks/rss/

Reason:
Runbooks represent reproducible system behavior.
They must be versioned.

Impact:
Infrastructure recovery is reproducible.

---

## Open Decisions (Pending)

- Routing v1 design
- Stage tracker definition
- Automated document pipeline

---

## 2026-02-13 — Stage 3 Started + Routing v1 Declared (Single-Model)

Decision:
Stage 3 is explicitly started for defining Routing v1.

Routing v1 constraints:
- Still single-model only: anthropic/claude-opus-4-6
- No multi-model routing
- No fallback chain
- Role separation is protocol-level (prompt/format), not model-level

Reason:
We need deterministic behavior and reproducible outputs while keeping infra stable.
Multi-model routing and fallback chains are deferred until later in Stage 3.

Impact:
- Routing rules are now documented in docs/architecture/ROUTING_V1.md
- Operational behavior must follow Routing v1 protocol
- Any later expansion must be appended here as new decisions

---

## 2026-02-13 — Canonical Paths and Commands Locked in SSOT

Decision:
Add docs/architecture/PATHS_AND_COMMANDS.md as the canonical reference for:
- Repo path
- OpenClaw paths (config, state, sessions)
- Safe copy-paste operational commands
- Shell rule: run multi-line scripts with bash to avoid zsh parsing issues

Reason:
Avoid repeated path discovery and command re-derivation across chats.
Prevent recurring zsh parse errors from copy-paste scripts.

Impact:
All future operations should reference PATHS_AND_COMMANDS.md first.
No secrets/tokens may be added to SSOT.

---

## 2026-02-13 — Constitution enforced via workspace SOUL.md

Decision:
Routing v1 protocol is enforced through the agent constitution injected via ~/.openclaw/workspace/SOUL.md,
because OpenClaw assembles/injects a system prompt per agent run and workspace “soul” files are part of that prompt.

Reason:
OpenClaw routing rules (channel→agent bindings) are not the mechanism for WRITER/CRITIC/EDITOR enforcement.
agents set-identity only sets metadata, not instructions.

Impact:
- CONSTITUTION.md becomes SSOT for behavioral protocol.
- SOUL.md contains a managed “AIR_ROUTING_V1” block synced from SSOT.

---

## 2026-02-13 — Workspace constitution deployment is official (SSOT-driven)

Decision:
The AIR Routing v1 constitution is enforced by a managed block inside:
~/.openclaw/workspace/SOUL.md

SSOT stores:
- docs/architecture/AIR_ROUTING_V1_MANAGED_BLOCK.md (source of truth)
- ssot/runbooks/openclaw/deploy_constitution_to_workspace.sh (deploy mechanism)

Reason:
Git is SSOT, but OpenClaw workspace files live outside the repo.
We must make local workspace enforcement reproducible and re-applicable via a runbook.

Impact:
- Any constitution changes must update the SSOT block file.
- Deploy runbook must be re-run after updates.

---

## 2026-02-13 — Control UI remains local-only (trusted proxies warning accepted)

Decision:
Keep OpenClaw Control UI local-only (loopback). Do not expose via reverse proxy by default.

Reason:
If not exposed, gateway.trustedProxies is irrelevant and leaving it empty reduces complexity.

Impact:
- security audit WARN "trusted_proxies_missing" is accepted under this policy.
- If we later expose the Control UI, we must set gateway.trustedProxies and record the change here.

---

## 2026-02-13 — Routing v1 constitution compacted (v1.1)

Decision:
Compact the managed constitution block to reduce token burn while preserving enforcement.

Reason:
The constitution block is injected frequently; keeping it short reduces overhead.

Impact:
- docs/architecture/AIR_ROUTING_V1_MANAGED_BLOCK.md is now COMPACT v1.1
- Re-deploy to workspace required after this change

---

## 2026-02-13 — Deploy runbook must not use inline perl substitution

Decision:
Managed block replacement in workspace files must use deterministic Python-based logic.
Inline perl -e substitution is prohibited for multiline managed content.

Reason:
Perl inline substitution caused unsafe interpolation errors and non-deterministic failures.

Impact:
All deploy scripts must follow the Python-based block replacement pattern.

---

## 2026-02-13 — Constitution v1.4: Strict 6-heading enforcement (merged structure)

Decision:
Upgrade constitution from v1.3 to v1.4 with mandatory 6-heading research summary structure.

Headings (exact, in order):
1) Scope & Executive Summary
2) Key Findings
3) Evidence & Sources
4) Caveats & Risks
5) Implications & Recommendations
6) Next Action

Design:
- Merged RESEARCH_SUMMARY_V1 (v1.0) headings with operator-specified v1.4 headings
- "Scope" + "Executive Snapshot" combined into one section to avoid redundancy
- "Caveats" + "Risks" combined — both address limitations
- "Verification Status" absorbed into footer (Verified/Unverified already present)
- Override keywords remain active but do NOT exempt heading requirement

Enforcement:
- Each section must contain ≥1 meaningful sentence
- Placeholders (N/A, TBD, done, 완료, 끝, 없음) forbidden
- Abbreviated endings (EDITOR: done, 완료) forbidden
- Self-check before output: if any heading missing/modified/empty → rewrite

Reason:
EDITOR was not strictly enforcing required headings (known issue logged in HANDOFF_STAGE3_READY).
Operator requested enforcement upgrade.

Impact:
- AIR_ROUTING_V1_MANAGED_BLOCK.md updated to v1.4
- RESEARCH_SUMMARY_V1.md synced to match
- CURRENT_STATE.md updated to reflect Stage 3 active
- Deploy to workspace SOUL.md required
