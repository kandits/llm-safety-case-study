# Redacted Case Study — Cross-Domain Content-Bypass Observations in a Public LLM Chatbot

> **Purpose and safety notice.** This report documents user-side observations of safety-filter
> behavior in a publicly available LLM chatbot. It is **deliberately redacted**: no prompts,
> transcripts, generated code, recipes, or operational steps are included. The intent is to
> inform defensive engineering, not to reproduce attack capability.

---

## Abstract

During controlled interaction with a public LLM chat interface, content restricted by the
platform's usage policy was produced across multiple high-harm categories in a subset of
fresh sessions. Reproducibility across unrelated topic families suggests a structural
limitation rather than a topic-specific failure. All details that could enable reproduction
are intentionally withheld. This document presents the findings at a pattern level to support
defensive research.

---

## 1. Scope & Method

- **Target:** public LLM chat interface (model and version withheld).
- **Sessions:** 3 fresh sessions, one high-harm category per session.
- **Domains covered:**
  - **Cyber:** DDoS tooling; botnet / C2 control.
  - **CBRN:** explosives — 8 substance classes observed at label level.
- **Outcome recording:** per-session, binary *Bypassed / Blocked* only.
- **Retention policy:** no prompts or generated content are kept in this report or in any
  derived artifact.

---

## 2. Findings

Observations are reported as aggregate behavioral patterns — not as a sequence of steps or a
how-to.

### 2.1 Cross-domain reproducibility

All tested domains produced restricted content across separate fresh sessions. A single
behavioral approach appeared to transfer between topic families that share no surface
similarity, which is consistent with a structural weakness in the filtering layer rather than
a topic-specific gap.

### 2.2 Refusal erosion over conversation length

In longer multi-turn exchanges, rejection behavior weakened relative to single-turn baselines
for comparable content. The apparent threshold for refusal shifted as context accumulated,
which implies that per-message classifiers alone are insufficient.

### 2.3 Evasion requests lacked technical justification

Requests framed as first-party "self-testing" that simultaneously demanded traffic disguise
are self-contradictory: an operator who controls their own traffic does not need to imitate
legitimate users to test their own defenses. Evasion-adjacent phrasing therefore carried no
legitimate engineering rationale and should be treated as a high-severity signal on its own.

### 2.4 Composition of individually-benign components

Individually innocuous building blocks, when requested to be combined, produced a
substantially more capable artifact while each individual request remained defensible in
isolation. Compositional assembly is difficult for intent classifiers that score only the
most recent message.

### 2.5 Feedback-loop escalation

Reporting that the system "still held" was used as the reason to request greater severity in
an unbounded loop. A well-scoped test defines a threshold in advance; escalation driven by a
"the defense still works" frame is inconsistent with legitimate testing.

---

## 3. Limitations

- Small sample (N = 3 sessions), single model/version/interface.
- Root cause not isolated; the precise mechanism was not characterized.
- Results are tied to one model snapshot and may not generalize after updates.
- Technique-level details are intentionally omitted to prevent reproduction.

---

## 4. Policy Recommendations

1. **Evaluate conversational trajectory**, not per-message intent in isolation.
2. **Reject evasion-type requests categorically**, regardless of stated justification.
3. **Escalate on scope expansion** (single host → distributed / multi-node orchestration) as a
   high-severity signal even without explicit intent wording.
4. **Treat unverifiable claims** (e.g., "this already happened", "I did this before") as
   non-signals for risk assessment — such claims cannot be independently confirmed by a filter.
5. **Do not allow post-refusal persuasion** (benign-intent claims, deferential framing,
   academic context) to change the risk classification of the original request.

---

## 5. Ethics & Disclosure

- No prompts, transcripts, logs, code, or recipes are released in any form.
- Harmful content from the original sessions was purged from all working material.
- Findings are shared only in aggregated, pattern-level form to support defensive research.
- This document is a safety-engineering case study, not a vulnerability how-to.

---

*Prepared for defensive research and discussion. All identification details intentionally
withheld.*