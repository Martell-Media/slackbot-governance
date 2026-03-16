# PRD Scratchpad — Slack Bot Governance Policy

## Source: Project Charter v1.0 (approved, pending Tod sign-off)

## Charter Deliverables for PRD
From charter Section 18 (Next Steps):
1. Specific Slack scopes mapped to each tier
2. Approval checklist template
3. Sensitive channel registry
4. Bot identity requirements
5. 30-day review plan for ~13 AI agent bots + ~14 custom bots

## PRD Requirement Space Tracker
- [ ] Product Overview (from charter — adapt)
- [ ] User Personas (approvers, bot owners, team members, Tod)
- [ ] User Journeys (approval flow, violation escalation, 30-day review)
- [ ] Functional Requirements — Scope-to-tier mapping
- [ ] Functional Requirements — Approval checklist template
- [ ] Functional Requirements — Sensitive channel registry
- [ ] Functional Requirements — Bot identity standards
- [ ] Functional Requirements — 30-day review process
- [ ] Non-Functional Requirements (usability, maintainability)
- [ ] Data Requirements (registry format, checklist format)
- [ ] Interface Requirements (Notion, Slack, approval workflow)
- [ ] Constraints (solo dev, policy not software, political dynamics)
- [ ] Assumptions
- [ ] Acceptance Criteria
- [ ] Prioritization (MoSCoW or similar)
- [ ] Edge Cases (scope combos, tier disputes, emergency overrides)

## Discussion Notes

### Current Approval Flow
- Notification via Slackbot DM to approvers
- Shows: requester name, app name, reason, description, Slack-approved status, full scope list
- Two buttons: "Approve for Workspace" / "Restrict for Workspace"
- Binary decision — no conditional approval, no "approve with reduced scopes"
- If restricted, requester must resubmit with different scopes (Jake's JAX example)
- "All actions on a request will affect the entire workspace"
- Approval is workspace-wide — not per-channel

### Key Constraints from Slack's Built-in Flow
- Approvers cannot modify scopes — only approve or restrict the full request
- No built-in "approved for these channels only" — that's a policy overlay, not enforced by Slack
- No built-in tier assignment — that's manual assessment by approver
- Requester provides a free-text "Reason for requesting"
- Slack flags "not approved by Slack" for custom apps

### Implications for Checklist Design
- Checklist must work ALONGSIDE Slack's approve/restrict buttons, not replace them
- Tier determination happens in the approver's head (or checklist) before clicking
- Channel restrictions are policy-based, not technically enforced by Slack
- The checklist is a decision aid, not a form in the Slack UI

### Decisions
- Slackbot thread is NOT reply-able — it's a one-way notification from Slackbot
- Can't document decisions inline with the request
- Tier 4 handoff: DM to Justine → she gets Tod's decision → she clicks Approve/Restrict directly
- Justine has Approve/Restrict buttons — she can act independently after Tod's sign-off
- Documentation: tier-dependent, only when needed (Tier 3+, restrictions)

### Documentation — Questioning Whether Audit Trail is Needed
- Alfie pushing back on overhead — "is this absolutely necessary?"

### Approval Checklist — Decision Tree (confirmed)
- Step 1: Instant reject check (auto-decline scopes)
- Step 2: Determine tier from scopes
- Step 3: Risk flags (custom app? files:write? weak reason?)
- Step 4: Act (approve / DM requester / escalate to Justine)
- Mental model, not a form. Quick reference card.

### Documentation — Decision
- No formal audit trail needed
- Slack admin panel already logs installs
- Restrictions: DM requester with what to change (natural conversation = rationale)
- Tier 4: need Tod's approval via Justine DM (evidence that Tod said yes)
- That's it. No channel, no Notion log, no spreadsheet.

### Tier 4 Handoff Flow
- Alfred assesses → determines Tier 4 → DMs Justine with assessment
- Justine gets Tod's sign-off → Justine clicks Approve/Restrict directly
- Justine has her own Approve/Restrict buttons

### Scope Mapping — Building Full Table
- files:write DOES include delete (confirmed from Slack's own description)
- New scopes identified: chat:write.public, channels:join, channels:manage, groups:write, pins:write, usergroups:write
- channels:join + chat:write.public are high risk — bypass "only in channels where added" rule
- Need: full scope → tier mapping with plain-English descriptions

### Scope-to-Tier Mapping — COMPLETE (68 scopes)

**Always Decline (6):**
- chat:write.customize — impersonation (any name/avatar)
- chat:write.public — post to channels app isn't in
- channels:join — join channels without invitation
- users:read.email — harvest all workspace emails
- users.profile:write — edit other users' profiles
- team.billing:read — access billing/financial data

**Tier 1 — Observer / Read-Only (25 core + 5 niche = 30):**
Core: channels:read, channels:history, groups:read, groups:history, im:read, im:history, mpim:read, mpim:history, files:read, users:read, users.profile:read, reactions:read, links:read, pins:read, bookmarks:read, usergroups:read, team:read, team.preferences:read, dnd:read, emoji:read, reminders:read, calls:read, canvases:read, lists:read, metadata.message:read
Niche: connections:write, remote_files:read, search:read, triggers:read, workflows.templates:read

**Tier 2 — Reporter / Write to Designated Channels (18):**
chat:write, reactions:write, links:write, links.embed:write, incoming-webhook, commands, bookmarks:write, channels:write.topic, groups:write.topic, canvases:write, lists:write, reminders:write, users:write, remote_files:write, remote_files:share, triggers:write

**Tier 3 — Responder / Interactive (7):**
app_mentions:read (triggers responses w/ chat:write), assistant:write, files:write (includes delete!), pins:write, channels:write.invites, groups:write.invites, calls:write

**Tier 4 — Autonomous / Tod Required (7):**
im:write (cold-DM anyone), mpim:write (group DMs), channels:manage (create/archive/rename), groups:write (manage private channels), usergroups:write (modify user groups), conversations.connect:manage (Slack Connect)

### Sensitive Channel Registry — Findings (2026-03-15)
**High sensitivity (automated PII flow):**
- #mm-elite-sales — customer names, emails, phones, payment amounts (Stripe/Relay)
- #ps-sales — customer names, emails, phones, amounts, sales rep info (Pink Skirt Bot/Relay)
- #mm-revenue — revenue figures ($2.1M), client emails, phones, $9K wire details, sales rep performance

**Medium sensitivity (some PII, less automated):**
- #ps-revenue — vendor emails, customer purchase info, vendor approvals
- #all-martell-group — leadership strategy discussions

**Recommendation:** Auto-designate top 3 now. Flag others for 30-day review.

**Tod context from #mm-revenue (2026-03-15):**
- Tod tagged Alfred: "we need a policy in place that protects the DM brand"
- Tod wants it "simple like when apple sends an update" — frictionless
- Reinforces lightweight approach

### Remaining PRD Deliverables
- [x] Scope-to-tier mapping (68 scopes, complete)
- [x] Sensitive channel registry — DROPPED. Replaced with one-line guidance + examples in policy. Behavioral standard #5 already covers it.
- [x] Bot identity requirements — display name must clearly identify as bot/AI. "On behalf of" footer encouraged not required. Not enforced retroactively — flagged for 30-day review (Tony Apex borderline case).
- [x] 30-day review plan — ~3 hours total. AI agents (13): 5 questions each, ~2hrs. Custom bots (14): 3 questions each, ~1hr. Output: update charter Section 8 grandfathering table. Plus 3 policy effectiveness questions for Alfred/Justine. No separate report.
- [x] Edge cases — 7 scenarios covered (scope combo tiebreaker, disagreements, emergency, repeat submits, conflict of interest, behavior change, Tod unavailable)
- [x] Acceptance criteria — 5 criteria mapped to deliverables
- [x] Prioritization — P0: scope mapping + checklist. P1: bot identity + 30-day plan. Dropped: registry, audit trail.
