# Work Breakdown Structure: Slack Bot Governance Policy

**Project Name:** Slack Bot Governance Policy
**Organization:** Martell Media
**Project Lead:** Alfred Loh
**Version:** 1.0
**Date:** March 16, 2026
**Source:** Project Charter v1.0, PRD v1.0

---

## Overview

This is a policy rollout plan, not a software development plan. All policy documents (charter, PRD) and Notion deliverables are complete. The remaining work is review, communication, onboarding, and ongoing governance.

### Completed Deliverables

| Deliverable | Location | Status |
|-------------|----------|--------|
| Project Charter v1.0 | `docs/core/project_charter.md` | Complete |
| PRD v1.0 | `docs/core/prd.md` | Complete |
| Slack Bot Policy (team-facing) | [Notion — Automation Team](https://www.notion.so/Slack-Bot-Policy-325c61f3f77e8117a962edf853987a55) | Published |
| Bot Approval — Approver Reference | [Notion — Automation Team](https://www.notion.so/Bot-Approval-Approver-Reference-325c61f3f77e81f3a96cfcdf76be83b8) | Published |
| Git repository | [Martell-Media/slackbot-governance](https://github.com/Martell-Media/slackbot-governance) | Active |

---

## Phase 1: Tod Review & Sign-Off

**Target:** Week of March 17, 2026
**Effort:** ~30 minutes (Alfred) + Tod's async review time

| # | Task | Owner | Dependencies | Est. |
|---|------|-------|-------------|------|
| 1.1 | Restrict Approver Reference page in Notion to Alfred + Justine only | Alfred | None | 5 min |
| 1.2 | Send Tod the Slack Bot Policy Notion link for async review | Alfred | 1.1 | 5 min |
| 1.3 | Include brief message: what this is, why it matters, what you need from him (sign-off) | Alfred | 1.2 | 10 min |
| 1.4 | Tod reviews and provides feedback or sign-off | Tod | 1.3 | Tod's timeline |
| 1.5 | Incorporate any feedback from Tod | Alfred | 1.4 | 15 min |
| 1.6 | Confirm effective date (date of Tod's sign-off) | Alfred | 1.4 | 5 min |
| 1.7 | Update Notion policy page callout with effective date | Alfred | 1.6 | 2 min |

**Exit criteria:** Tod has signed off. Effective date is set.

---

## Phase 2: Stakeholder Briefings

**Target:** Within 2 days of Tod sign-off
**Effort:** ~1 hour (Alfred)

| # | Task | Owner | Dependencies | Est. |
|---|------|-------|-------------|------|
| 2.1 | Brief Dan Martell privately (DM or in-person) — behavioral standards that apply to Kai and grandfathered bots | Alfred | Phase 1 complete | 15 min |
| 2.2 | Brief Etienne de Bruin privately — behavioral standards that apply to Tony Apex | Alfred | Phase 1 complete | 15 min |
| 2.3 | Brief Marcel Petitpas privately — behavioral standards that apply to Phil | Alfred | Phase 1 complete | 15 min |
| 2.4 | Brief Sam Gaudet privately — he raised the original noise complaint AND owns Ultron (Tier 4). He's an ally but deserves the same courtesy. | Alfred | Phase 1 complete | 5 min |
| 2.5 | Walk Justine through the Approver Reference page and quick reference card | Alfred | Phase 1 complete | 20 min |
| 2.6 | Practice run with Justine: review a past request together (e.g., Jake's JAX) | Alfred | 2.5 | 10 min |
| 2.7 | Confirm Justine is comfortable acting independently on Tiers 1-3 | Alfred | 2.6 | 5 min |

**Exit criteria:** Dan, Etienne, Marcel, and Sam are aware of the policy before public announcement. Justine can process Tier 1-3 requests independently.

---

## Phase 3: Workspace Announcement

**Target:** Within 1 day of completing Phase 2
**Effort:** ~15 minutes (Alfred)

| # | Task | Owner | Dependencies | Est. |
|---|------|-------|-------------|------|
| 3.1 | Draft Slack announcement message — short, links to Notion policy page | Alfred | Phase 2 complete | 10 min |
| 3.2 | Post announcement in #mm-general | Alfred | 3.1 | 2 min |
| 3.3 | Pin the announcement or link the Notion page in channel bookmarks | Alfred | 3.2 | 2 min |
| 3.4 | Configure Slack app request settings to include policy link — so requesters see the rules before submitting (Settings & Administration → Manage Apps → Settings) | Alfred | 3.1 | 5 min |

**Exit criteria:** All Martell Media team members have been notified and can access the policy.

---

## Phase 4: Active Governance

**Target:** Ongoing from announcement date
**Effort:** ~5-10 minutes per request

| # | Task | Owner | Dependencies | Est. |
|---|------|-------|-------------|------|
| 4.1 | Apply 4-step checklist to every new Slack app request | Alfred / Justine | Phase 3 complete | Per request |
| 4.2 | For restrictions: DM requester with what to change and why | Alfred / Justine | 4.1 | 5 min |
| 4.3 | For Tier 3: DM requester for justification before approving | Alfred / Justine | 4.1 | 10 min |
| 4.4 | For Tier 4: DM Justine with assessment, she gets Tod's sign-off | Alfred → Justine → Tod | 4.1 | Varies |
| 4.5 | Handle violations per escalation process (Level 1/2/3) | Alfred | Ongoing | As needed |

**Exit criteria:** None — this is ongoing. Effectiveness checked at 30-day review.

---

## Phase 5: 30-Day Review

**Target:** April 15, 2026
**Effort:** ~3 hours (Alfred)

| # | Task | Owner | Dependencies | Est. |
|---|------|-------|-------------|------|
| 5.1 | Review 13 AI agent bots — behavioral compliance (5 questions per bot) | Alfred | 30 days elapsed | 2 hrs |
| 5.2 | Review 14 custom bots — tier categorization (3 questions per bot) | Alfred | 30 days elapsed | 1 hr |
| 5.3 | Resolve TBD owners: Apex - Janel, Apex Assistant, Sammy - Apex Support | Alfred | 5.1 | Included |
| 5.4 | Investigate Kai "Deleted" anomaly (posting but shows deleted in admin) | Alfred | 5.1 | Included |
| 5.5 | Policy effectiveness check — 3 questions with Justine | Alfred + Justine | 5.1, 5.2 | 15 min |
| 5.6 | Update charter Section 8 grandfathering table with findings | Alfred | 5.1, 5.2, 5.3 | 30 min |
| 5.7 | Write recommendation on Option B (monitoring bot) — yes/no with rationale. Send to Tod via Justine for async decision. | Alfred → Justine → Tod | 5.5 | 15 min |
| 5.8 | Flag Tony Apex bot identity for discussion with Etienne | Alfred | 5.1 | 5 min |

**Exit criteria:** Charter Section 8 updated. All TBD owners resolved. Policy effectiveness assessed. Option B decision made.

---

## Phase 6: Quarterly Reviews

**Target:** July 2026, October 2026, January 2027, ongoing
**Effort:** ~2 hours per review (Alfred)

| # | Task | Owner | Dependencies | Est. |
|---|------|-------|-------------|------|
| 6.1 | Review any new bots approved since last review | Alfred | Quarterly | 30 min |
| 6.2 | Check for behavioral standard violations | Alfred | Quarterly | 30 min |
| 6.3 | Update scope mapping if Slack has added new scopes | Alfred | Quarterly | 15 min |
| 6.4 | Policy effectiveness check with Justine | Alfred + Justine | Quarterly | 15 min |
| 6.5 | Update Notion pages with any policy changes | Alfred | 6.1-6.4 | 15 min |
| 6.6 | Reassess Option B (monitoring bot) if policy alone is insufficient | Alfred → Justine → Tod | 6.4 | 15 min |
| 6.7 | Formalize new employee onboarding — ensure new team members are directed to the policy page as part of workspace onboarding (first quarterly review) | Alfred + Justine | 6.4 | 15 min |

**Exit criteria:** Policy remains current. Notion pages updated. Quarterly cadence maintained.

---

## Timeline Summary

```
March 15-16    Charter, PRD, Notion pages complete ✓
March 17-19    Phase 1: Tod async review & sign-off
March 19-21    Phase 2: Private briefings + Justine onboarding
March 21-22    Phase 3: Workspace announcement
March 22+      Phase 4: Active governance begins
April 15       Phase 5: 30-day review
July 2026      Phase 6: First quarterly review
```

---

## Risk Mitigation

| Risk | Mitigation | Phase |
|------|-----------|-------|
| Tod delays sign-off | Follow up via Justine after 3 business days | Phase 1 |
| Dan/Etienne pushback at briefing | Policy is universal, bots are grandfathered. Escalate to Tod if needed. | Phase 2 |
| Justine not confident as co-approver | Practice run on real past request. Start with Tier 1-2 only. | Phase 2 |
| Bot requests flood in before announcement | Apply the checklist immediately after Tod signs off, even before public announcement. | Phase 3-4 |
| 30-day review slips | Calendar-block April 15 now. 3 hours is manageable. | Phase 5 |

---

## Total Effort Estimate

| Phase | Effort (Alfred) | Effort (Others) |
|-------|-----------------|-----------------|
| Phase 1: Tod Review | 30 min | Tod: async review |
| Phase 2: Briefings | 1 hr | Justine: 30 min |
| Phase 3: Announcement | 15 min | — |
| Phase 4: Active Governance | 5-10 min/request | Justine: shared |
| Phase 5: 30-Day Review | 3 hrs | Justine: 15 min |
| Phase 6: Quarterly Reviews | 2 hrs/quarter | Justine: 15 min/quarter |
| **Total rollout (Phases 1-3)** | **~2 hours** | |
