# Product Requirements Document: Slack Bot Governance Policy

**Project Name:** Slack Bot Governance Policy
**Organization:** Martell Media
**Project Lead:** Alfred Loh (Automation Developer & AI Strategist)
**Sponsor:** Tod Melnyk (CEO, Martell Media)
**Version:** 1.0
**Date:** March 15, 2026
**Source:** Project Charter v1.0

---

## 1. Product Overview

This PRD operationalizes the Slack Bot Governance Policy defined in the project charter. It provides the detailed tools, mappings, and processes that approvers (Alfred Loh and Justine Rockwood) need to evaluate bot requests, enforce behavioral standards, and conduct the 30-day review.

This is a **policy operations document** — it defines how the policy is applied in practice. There is no software to build.

### Deliverables

| # | Deliverable | Priority | Status |
|---|------------|----------|--------|
| 1 | Slack scope-to-tier mapping | P0 | Complete |
| 2 | Approval checklist (4-step decision tree) | P0 | Complete |
| 3 | Bot identity naming standard | P1 | Complete |
| 4 | 30-day review plan | P1 | Complete |
| 5 | ~~Sensitive channel registry~~ | Dropped | Replaced with one-line guidance |
| 6 | ~~Formal audit trail~~ | Dropped | Slack admin panel is sufficient |

---

## 2. User Personas

### Approver — Alfred Loh

- **Role:** Primary Slack app approver, policy author and maintainer
- **Goal:** Evaluate bot requests quickly and consistently using the tiered framework
- **Pain point:** Ad-hoc decisions with no written criteria lead to pushback ("PLEASE ALFRED") and repeat submissions
- **Technical capability:** High — understands Slack scopes, bot architectures, and risk assessment
- **Tools:** Slackbot DM notifications, Slack admin panel

### Approver — Justine Rockwood

- **Role:** Co-approver for Tiers 1-3, Tier 4 handoff to Tod
- **Goal:** Process bot requests independently when Alfred is unavailable, route Tier 4 requests to Tod
- **Pain point:** Needs a clear, simple framework — not deeply technical
- **Technical capability:** Medium — can follow a checklist but may not understand all scopes intuitively
- **Tools:** Slackbot DM notifications, DM with Tod

### Sponsor — Tod Melnyk

- **Role:** Final authority on Tier 4 approvals and policy enforcement
- **Goal:** Keep the workspace under control without creating bureaucracy. Wants it "simple like when Apple sends an update."
- **Pain point:** Too many bots, too much noise, but also wants the team to adopt Apex
- **Technical capability:** Low-medium — understands the business impact, not the scope details
- **Tools:** DMs from Justine with Alfred's assessment

### Bot Owner — Team Member

- **Role:** Any team member requesting a Slack app/bot installation
- **Goal:** Get their bot approved and working
- **Pain point:** Unclear what's allowed, why requests get rejected, and what to change
- **Technical capability:** Varies widely — from Etienne (built the platform) to Jake (just wants his bot in Slack)
- **Tools:** Slack app request flow, DMs from approvers

---

## 3. User Journeys

### Journey 1: Standard Approval (Tiers 1-2)

1. Team member submits app install request via Slack
2. Slackbot sends notification to Alfred and Justine with app name, requester, reason, and scopes
3. Approver scans scopes against the checklist (Step 1: instant reject check)
4. No auto-decline scopes found → determines tier (Step 2)
5. Tier 1 or 2 → clicks **Approve for Workspace**
6. Done. No documentation needed.

### Journey 2: Restriction (Problematic Scopes)

1. Team member submits app install request
2. Approver scans scopes → finds auto-decline scope (e.g., `chat:write.customize`)
3. Clicks **Restrict for Workspace**
4. DMs requester: "Your request was restricted because [scope] allows [risk]. Please resubmit without [scope]."
5. Requester resubmits with reduced scopes → re-enters Journey 1

### Journey 3: Tier 3 Approval (Needs Justification)

1. Team member submits app install request
2. Approver scans scopes → finds `assistant:write` or `app_mentions:read` + `chat:write`
3. Determines Tier 3
4. DMs requester: "This bot wants to talk in conversations. What's the use case? Which channels?"
5. Requester provides justification
6. If justified → **Approve for Workspace**
7. If not justified → **Restrict for Workspace** with explanation

### Journey 4: Tier 4 Escalation (Tod Required)

1. Team member submits app install request
2. Approver scans scopes → finds `im:write`, `channels:manage`, or other Tier 4 scope
3. Determines Tier 4
4. Alfred DMs Justine: "This is a Tier 4 request — [app name] from [requester]. Needs Tod's approval. Here's my assessment: [summary]."
5. Justine gets Tod's sign-off (DM, in-person, or meeting)
6. Justine clicks **Approve for Workspace** or **Restrict for Workspace** based on Tod's decision

### Journey 5: Violation Escalation

1. Team member or approver observes a bot violating a behavioral standard
2. **Level 1 (Low):** Alfred DMs bot owner informally. Owner fixes it.
3. **Level 2 (Medium):** Alfred formally DMs bot owner citing the policy. 48 hours to correct. If not corrected, approver may restrict the bot. Notify Tod first if the bot is owned by senior leadership.
4. **Level 3 (Critical):** Alfred suspends bot access immediately. Notifies Tod and bot owner.

---

## 4. Functional Requirements

### 4.1 Approval Checklist — 4-Step Decision Tree

This is the mental model approvers use for every request. It is a quick reference, not a form.

#### Step 1: Instant Reject Check

Scan the scope list for auto-decline scopes. If any are present, click **Restrict** and DM the requester.

**Also check for user token scopes.** Slack requests show two sections: "On behalf of the app" (bot scopes) and "On behalf of the user" (user scopes). User-scope `chat:write` lets the bot post as the actual human — same impersonation risk as `chat:write.customize`. Always decline.

| Scope | Description | Action |
|-------|------------|--------|
| `chat:write.customize` | Send messages with any name and avatar — impersonation risk | Restrict. No exceptions. |
| `chat:write.public` | Post to channels the app isn't a member of — bypasses channel membership | Restrict. No exceptions. |
| `channels:join` | Join any public channel without invitation — violates behavioral standard #2 | Restrict. No exceptions. |
| `users:read.email` | View email addresses of all workspace members — email harvesting | Restrict. Unless written justification + Tod approval. |
| `users.profile:write` | Edit user profile information — could modify other people's profiles | Restrict. No exceptions. |
| `team.billing:read` | View workspace billing information — sensitive financial data | Restrict. No exceptions. |
| User-scope `chat:write` | Send messages as the actual user, not the bot — impersonation by design | Restrict. Same risk as `chat:write.customize`. |

#### Step 2: Determine Tier

Based on the highest-risk scope in the request:

| If the highest-risk scope is... | Tier | Action |
|--------------------------------|------|--------|
| Read-only scopes only | Tier 1 | Approve |
| `chat:write`, `reactions:write`, `commands`, or other Tier 2 write scopes | Tier 2 | Approve. Ask: which channels? |
| `assistant:write`, `app_mentions:read`, `files:write`, or other Tier 3 scopes | Tier 3 | DM requester for justification |
| `im:write`, `mpim:write`, `channels:manage`, `groups:write`, `usergroups:write`, `conversations.connect:manage` | Tier 4 | Escalate to Justine → Tod |

**Tie-breaking rule:** The highest-risk scope determines the tier. One Tier 3 scope in a request full of Tier 1 scopes = Tier 3.

**Unknown scope rule:** If a scope is not listed in the mapping (Section 4.2), treat it as Tier 3 (require justification from the requester) and flag it for Alfred to categorize.

#### Step 3: Risk Flags

Quick gut-check before approving:

- **"Note: this app is not approved by Slack"** — custom/unknown app. Extra scrutiny.
- **`files:write` present?** — Includes delete capability. Ask: what does the bot need to upload?
- **`im:history`, `groups:history`, or `mpim:history` present?** — Can read private conversations (DMs, private channels, group DMs). Appropriate for this bot?
- **Weak or missing "Reason for requesting"?** — "PLEASE ALFRED" or "I want it in Slack" = red flag. Ask for a real justification.
- **User token scopes present ("On behalf of the user")?** — Check what's listed. `identify` alone is low risk (legacy). `identify` + `chat:write` = bot posts as the human. Restrict.
- **Requester resubmitted identical scopes after restriction?** — Restrict again. Point to the policy.

#### Step 4: Act

| Tier | Action |
|------|--------|
| 1 | Click **Approve for Workspace** |
| 2 | Click **Approve for Workspace**. If `files:write` present, DM requester: "Approved — upload only, no file deletion." |
| 3 | DM requester for justification. If justified, **Approve**. If not, **Restrict** with explanation. |
| 4 | DM Justine with assessment. She gets Tod's sign-off and clicks **Approve** or **Restrict**. |
| Restrict | DM requester: which scopes are the problem and what to resubmit without. |

---

### 4.2 Complete Slack Scope-to-Tier Mapping

#### Always Decline (7 scopes)

| Scope | Description | Exception |
|-------|------------|-----------|
| `chat:write.customize` | Send messages with any name and avatar — impersonation | None |
| `chat:write.public` | Post to channels the app isn't a member of | None |
| `channels:join` | Join any public channel without being invited | None |
| `users:read.email` | View email addresses of everyone in the workspace | Written justification + Tod approval |
| `users.profile:write` | Edit user profiles — could modify other people's profiles | None |
| `team.billing:read` | View workspace billing and financial information | None |
| User-scope `chat:write` | Send messages as the actual human, not the bot — impersonation by design | None |

#### Tier 1 — Observer (30 scopes)

| Scope | Description | Notes |
|-------|------------|-------|
| `channels:read` | View basic info about public channels (names, topics) | |
| `channels:history` | View messages in public channels the app is added to | |
| `groups:read` | View basic info about private channels the app is added to | |
| `groups:history` | View messages in private channels the app is added to | Can read private channels — check Step 3 risk flags |
| `im:read` | View basic info about DMs the app is added to | |
| `im:history` | View messages in DMs the app is added to | Can read private DMs — check Step 3 risk flags |
| `mpim:read` | View basic info about group DMs the app is added to | |
| `mpim:history` | View messages in group DMs the app is added to | Can read group DMs — check Step 3 risk flags |
| `files:read` | View files shared in channels the app is added to | |
| `users:read` | View people in the workspace (names, display names) | |
| `users.profile:read` | View detailed user profiles (title, phone, status) | |
| `reactions:read` | View emoji reactions on messages | |
| `links:read` | View URLs in messages | |
| `pins:read` | View pinned messages in channels | |
| `bookmarks:read` | View channel bookmarks | |
| `usergroups:read` | View user groups (e.g., @design-team) | |
| `team:read` | View workspace name, domain, and icon | |
| `team.preferences:read` | View workspace preference settings | |
| `dnd:read` | View Do Not Disturb settings for workspace members | |
| `emoji:read` | View custom emoji in the workspace | |
| `reminders:read` | View reminders created by the app | |
| `calls:read` | View info about Slack Huddles and calls | |
| `canvases:read` | View Slack canvases | |
| `lists:read` | View Slack lists | |
| `metadata.message:read` | View message metadata (hidden structured data on messages) | |
| `connections:write` | WebSocket connections for Socket Mode (app infrastructure) | |
| `remote_files:read` | View remote files added by the app | |
| `search:read` | Search messages and files across the workspace | |
| `triggers:read` | View automation triggers | |
| `workflows.templates:read` | View workflow templates | |

#### Tier 2 — Reporter (16 scopes)

| Scope | Description | Notes |
|-------|------------|-------|
| `chat:write` | Send messages as the app in channels it's a member of | Must declare target channels |
| `reactions:write` | Add and edit emoji reactions | |
| `links:write` | Show URL previews in messages | |
| `links.embed:write` | Embed video player URLs in messages | |
| `incoming-webhook` | Post messages to a specific channel via webhook | Limited to one channel — low risk |
| `commands` | Register slash commands for the app | Users must explicitly invoke |
| `bookmarks:write` | Create, edit, and remove channel bookmarks | |
| `channels:write.topic` | Set the topic/description of public channels | |
| `groups:write.topic` | Set the topic/description of private channels | |
| `canvases:write` | Create and edit Slack canvases | |
| `lists:write` | Create and edit Slack lists | |
| `reminders:write` | Create and manage reminders | |
| `users:write` | Set presence status for the app | |
| `remote_files:write` | Add, edit, and delete remote files | |
| `remote_files:share` | Share remote files on the app's behalf | |
| `triggers:write` | Create and manage automation triggers | |

#### Tier 3 — Responder (7 scopes)

| Scope | Description | Notes |
|-------|------------|-------|
| `app_mentions:read` | View messages that @mention the app | Triggers bot responses when combined with `chat:write` |
| `assistant:write` | Act as an AI assistant in conversations | Requires written justification |
| `files:write` | Upload, edit, **and delete** files as the app | Includes delete — no granular control. Requester must justify. |
| `pins:write` | Pin and unpin messages in channels | Can pin own content or unpin others' |
| `channels:write.invites` | Invite users to public channels | Bot can pull people into channels |
| `groups:write.invites` | Invite users to private channels | Bot can pull people into private channels |
| `calls:write` | Start and manage Slack Huddles/calls | Bot can initiate calls |

#### Tier 4 — Autonomous Agent (6 scopes)

| Scope | Description | Notes |
|-------|------------|-------|
| `im:write` | Start direct messages with anyone in the workspace | Cold-DM risk — unsolicited contact |
| `mpim:write` | Start group direct messages with multiple people | Same risk as `im:write` for group DMs |
| `channels:manage` | Create, archive, and rename public channels | Destructive — bot could archive channels |
| `groups:write` | Manage private channels (create, archive, invite) | Destructive — affects private spaces |
| `usergroups:write` | Create and modify user groups | Could alter @here/@channel membership |
| `conversations.connect:manage` | Manage Slack Connect channels with external orgs | External org access — high risk |

#### User Token Scopes

Slack requests may include scopes under "On behalf of the user" — these let the app act as the human who authorized it, not as the bot. The key distinction:

- **Bot-scope `chat:write`** — posts as "@FreaketonMV" or "@Apex - Jaryd" (clearly a bot)
- **User-scope `chat:write`** — posts as "Jaryd" or "Nic" (indistinguishable from the real person)

| Scope | Description | Action |
|-------|------------|--------|
| User-scope `identify` | View the authorizing user's identity (name, email, user ID) | Low risk alone. Legacy scope. |
| User-scope `chat:write` | Send messages as the actual human, not the bot | **Always decline.** Same impersonation risk as `chat:write.customize`. |
| User-scope `identify` + `chat:write` | Full impersonation — bot knows who it is and can post as them | **Always decline.** |
| User-scope `users:read` | View workspace members on behalf of user | Low risk. Redundant with bot-scope `users:read`. |

**Rule of thumb:** If the "On behalf of the user" section contains `chat:write`, restrict and ask the requester to remove it. The bot should post as itself.

**Unknown user scopes:** For any user token scope not listed above, apply the unknown scope rule — treat as Tier 3 (require justification from the requester) and flag for Alfred to categorize.

---

### 4.3 Bot Identity Standard

**Requirement:** All new Tier 3-4 AI agent bots must use a display name that clearly identifies them as a bot or AI agent.

**Good examples:** "Apex - Kai (Dan's Twin)", "Apex - Marcel", "Sammy - Apex Support"
**Borderline:** "Tony Apex" (contains "Apex" but reads like a human name)
**Violation:** "Tony", "Sam's Assistant", or any name indistinguishable from a human team member

**"On behalf of" footer:** Encouraged but not required. The display name is the primary identity mechanism.

**Retroactive enforcement:** Not applied to grandfathered bots. Borderline cases (e.g., Tony Apex) flagged for discussion at the 30-day review.

---

### 4.4 Sensitive Channel Guidance

The following channels contain automated PII flows and should not have bots added without approver confirmation:

- **`#mm-elite-sales`** — Customer names, emails, phone numbers, payment amounts (Stripe via Relay.app)
- **`#ps-sales`** — Customer names, emails, phone numbers, amounts, sales rep info (Pink Skirt Bot/Relay.app)
- **`#mm-revenue`** — Revenue figures, client emails, phone numbers, wire transfer details, sales rep performance

This is enforced through behavioral standard #5 ("No access to sensitive channels without approval"), not through a maintained registry. If new channels with automated PII flows are created, approvers should add them to this list.

---

## 5. 30-Day Review Plan

**Target date:** April 15, 2026
**Estimated effort:** ~3 hours
**Output:** Updated charter Section 8 (Grandfathering table)

### AI Agent Bots (13 bots) — Behavioral Compliance Check (~2 hours)

For each bot, answer:

1. **Is the bot still active?** Check recent channel activity. If inactive, note and move on.
2. **Does it violate any behavioral standard?** (Bot-to-bot chat, unsolicited replies, duplicate content, channel sprawl, missing identity)
3. **Is the owner confirmed?** Resolve TBDs: Apex - Janel, Apex Assistant, Sammy - Apex Support.
4. **Is the tier assignment correct?** Does actual behavior match the assigned tier?
5. **Should grandfathered status continue?** Any reason to revoke?

### Custom Bots (14 bots) — Tier Categorization (~1 hour)

For each bot, answer:

1. **What does it do?** Check installed scopes in Slack admin panel.
2. **What tier does it fall into?** Map scopes to tier using the checklist.
3. **Any concerns?** Unexpected scopes, unclear purpose, inactive?

### Policy Effectiveness Check

1. Have all new bot requests since the policy launched gone through tiered approval?
2. Have there been any bot-related complaints from team members?
3. Is the checklist working, or does it need adjustment?

### Review Output

- Update charter Section 8 grandfathering table (confirm owners, update tiers, remove inactive bots)
- Flag any behavioral violations found (enter escalation process per charter Section 10)
- Decision: is Option B (monitoring bot) needed, or is policy alone sufficient?

---

## 6. Non-Functional Requirements

| Requirement | Criteria |
|-------------|----------|
| **Simplicity** | Tod's directive: "simple like when Apple sends an update." The checklist must be usable without training. |
| **Speed** | Tier 1-2 approvals should take under 2 minutes. Tier 3 approvals under 10 minutes (including DM exchange). |
| **Independence** | Either approver (Alfred or Justine) can process Tier 1-3 requests independently. |
| **Maintainability** | Policy documents live in Notion (Automation Team workspace) and the Git repo. Updates happen quarterly. |
| **No new tooling** | No software, databases, dashboards, or Notion tables to maintain. The checklist is a reference card. |

---

## 7. Constraints

| Constraint | Impact |
|-----------|--------|
| **Slack's approval is binary.** Approve or Restrict — no conditional approval, no scope modification. | Approvers cannot trim scopes. Must restrict and ask requester to resubmit. |
| **Slackbot notifications are not reply-able.** One-way notification only. | Cannot document decisions inline with the request. |
| **Approval is workspace-wide.** No per-channel approval in Slack's built-in flow. | Channel restrictions are policy-based, not technically enforced. |
| **Solo developer + EA.** Alfred and Justine are the only approvers. | Must be lightweight enough to sustain without dedicated governance staff. |
| **Political dynamics.** Dan Martell (CEO, SaaS Academy) owns the most active bots and is pushing Apex adoption. | Policy must be universal with no named exemptions. Existing bots are grandfathered. |
| **Apex V1 launched March 16.** Bot request volume expected to increase significantly. | Tier 4 requiring Tod's approval acts as a natural rate limiter. |

---

## 8. Assumptions

1. Tod will sign off on the charter and policy before the workspace announcement.
2. Dan, Etienne, and Marcel will be briefed privately before the public announcement and will not actively resist the policy.
3. Justine is willing and able to serve as co-approver for Tiers 1-3.
4. The Slack admin panel's built-in logging is sufficient as an audit trail — no separate documentation system is needed.
5. Bot owners will correct behavioral violations when notified — the escalation process will rarely need to go beyond Level 1.
6. Tod will be available to approve Tier 4 requests within a reasonable timeframe (1-2 business days).
7. The volume of Tier 4 requests will not overwhelm Tod — monitored at the 30-day review.

---

## 9. Edge Cases

| Scenario | Resolution |
|----------|-----------|
| **Scope combo doesn't fit a single tier** (e.g., mostly Tier 1 + one Tier 3 scope) | Highest-risk scope determines the tier. One `assistant:write` in a sea of read scopes = Tier 3. |
| **Bot owner disagrees with tier assignment** | Approver's assessment stands. Bot owner can appeal to Tod for Tier 4 decisions. |
| **Emergency — bot actively leaking PII or impersonating** | Level 3 escalation. Approver suspends immediately, notifies Tod after. Don't wait for permission. |
| **Requester resubmits identical scopes after restriction** | Restrict again. DM requester pointing to the policy. If repeated, escalate to Tod. |
| **Etienne approves his own bot via workspace admin** | Conflict of interest. Raise with Tod at next review. Tier 4 approval authority is Tod, not workspace admins. |
| **Bot changes behavior after approval** (approved as Tier 2 but starts replying to conversations) | Behavioral standards apply regardless of approved tier. Enter escalation process. |
| **Tod is unavailable for Tier 4 approval** | Hold the request. Tier 4 cannot be approved without Tod. No delegation unless Tod explicitly sets it up. |

---

## 10. Acceptance Criteria

| Deliverable | How We Know It's Working |
|-------------|------------------------|
| Scope-to-tier mapping | Approvers can determine tier for any request in under 2 minutes |
| Approval checklist | Used for every new request. No requests approved without tier assessment. |
| Bot identity standard | All new Tier 3-4 bots have clearly identifiable display names |
| 30-day review | Completed by April 15. All TBD owners resolved. Charter Section 8 updated. |
| Policy overall | Zero "PLEASE ALFRED" resubmits — requesters understand the rules before submitting |

---

## 11. Appendix: Quick Reference Card

**For approvers to reference when a Slackbot notification arrives.**

```
┌─────────────────────────────────────────────────┐
│         SLACK BOT APPROVAL CHECKLIST            │
├─────────────────────────────────────────────────┤
│                                                 │
│  STEP 1: INSTANT REJECT?                        │
│  Scan for these scopes → auto-RESTRICT:         │
│  ✗ chat:write.customize (impersonation)         │
│  ✗ chat:write.public (post anywhere)            │
│  ✗ channels:join (join uninvited)               │
│  ✗ users:read.email (harvest emails)            │
│  ✗ users.profile:write (edit profiles)          │
│  ✗ team.billing:read (billing access)           │
│  ✗ User-scope chat:write (posts as human)       │
│                                                 │
│  STEP 2: WHAT TIER?                             │
│  Read-only scopes only ──────── Tier 1 → APPROVE│
│  + chat:write / commands ────── Tier 2 → APPROVE│
│  + assistant:write / mentions ─ Tier 3 → ASK WHY│
│  + im:write / channels:manage ─ Tier 4 → JUSTINE → TOD│
│                                                 │
│  STEP 3: RED FLAGS?                             │
│  □ "Not approved by Slack"?                     │
│  □ files:write? (includes DELETE)               │
│  □ im:history / groups:history / mpim:history?   │
│    (reads private conversations)                │
│  □ Weak reason? ("PLEASE ALFRED" = no)          │
│  □ "On behalf of the user" scopes?              │
│    (user chat:write = posts as human = reject)  │
│                                                 │
│  STEP 4: ACT                                    │
│  Tier 1-2: Approve                              │
│  Tier 3: DM requester for justification         │
│  Tier 4: DM Justine → she gets Tod → she acts   │
│  Restrict: DM requester what to remove           │
│                                                 │
└─────────────────────────────────────────────────┘
```
