# Feature: Automated Scope Analyzer

**Status:** Documented — not yet built
**Priority:** Future (Option B from charter)
**Date:** March 18, 2026

---

## Problem

Approvers (Alfred and Justine) receive Slack app requests via Slackbot, which is read-only. They must manually read the scopes, mentally run the 4-step checklist, and determine the tier. This works but doesn't scale if request volume increases post-Apex V1, and Justine must memorize or reference the checklist for every request.

## Proposed Solution

A lightweight FastAPI bot that listens for Slack's `app_requested` event and automatically posts a recommendation to both approvers.

## How It Works

```
Someone requests an app
        ↓
Slack fires app_requested event
        ↓
Governance bot receives it automatically
        ↓
Bot parses scopes → runs the 4-step checklist
        ↓
Bot posts recommendation to a shared channel or DMs both Alfred and Justine
        ↓
Approver reviews recommendation → clicks Approve/Restrict in Slackbot
```

## What the Recommendation Would Include

- App name, requester, reason
- Auto-decline scopes found (if any) — with a clear "RESTRICT" recommendation
- Determined tier (1-4)
- Risk flags triggered
- Recommended action (Approve / Restrict / Escalate to Tod)
- For restrictions: which scopes to remove and why

## Technical Approach

- FastAPI endpoint to receive Slack `app_requested` event
- Scope analysis function implementing the 4-step checklist from PRD Section 4.1
- Slack message posting to a shared approver channel or DMs to both Alfred and Justine
- Scope-to-tier mapping from PRD Section 4.2 (65 scopes)

## Decision Point

Evaluate at the 30-day review (April 15, 2026) or when request volume makes manual analysis unsustainable. See WBS Phase 5, task 5.7.
