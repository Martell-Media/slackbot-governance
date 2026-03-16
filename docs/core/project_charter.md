# Project Charter: Slack Bot Governance Policy

**Project Name:** Slack Bot Governance Policy
**Organization:** Martell Media
**Project Lead:** Alfred Loh (Automation Developer & AI Strategist)
**Sponsor:** Tod Melnyk (CEO, Martell Media)
**Version:** 1.0
**Date:** March 15, 2026
**Status:** Draft — Pending Tod Review

---

## 1. Executive Summary

Martell Media's Slack workspace has seen rapid growth in AI agent bots — autonomous agents built on the OpenClaw/Apex platform that post messages, respond to conversations, and interact with team members. Without formal governance, the workspace is heading toward a "wild west" scenario: bots talking to each other, flooding channels with duplicate content, replying to conversations unprompted, and accessing sensitive data without oversight.

This project establishes a **governance policy and tiered approval framework** for all Slack bots and apps in the Martell Media workspace. The policy upgrades the existing Slack app approval process from a scope-only review to a behavior-aware, tiered system with clear rules, escalation paths, and accountability.

This is a **policy document**, not a software project. No monitoring or enforcement bot will be built at this time, though the policy is designed with measurable rules that could support automated monitoring in the future.

---

## 2. Definitions

- **Bot / App:** Any automated integration installed in the Martell Media Slack workspace, including AI agents, SaaS connectors, and custom-built applications.
- **AI Agent:** A bot powered by a large language model (e.g., OpenClaw/Apex) that can generate novel responses, make decisions, and act autonomously.
- **Autonomous behavior:** A bot initiating messages, replying to conversations, or taking actions without being explicitly triggered by a human @mention or command.
- **Bot-to-bot conversation:** Any instance where a bot responds to, acknowledges, or interacts with a message posted by another bot.
- **Unsolicited reply:** A bot responding to a message or conversation it was not directly mentioned in or triggered by.
- **Sensitive channel:** A channel designated as containing PII, confidential business data, or other protected information, as listed in the Sensitive Channel Registry (Section 13).
- **Grandfathered:** A bot that was active in the workspace before this policy's effective date. Grandfathered bots are exempt from the approval process but must comply with all behavioral standards and are subject to the same violation escalation process as any other bot.

---

## 3. Problem Statement

### Current State

Martell Media uses Slack's built-in app approval process. Alfred Loh serves as primary approver, reviewing requested permission scopes on an ad-hoc basis with no formal written criteria or checklist.

### Observed Issues

The following governance violations were identified through a Slack workspace audit conducted on March 15, 2026:

| Issue | Evidence |
|-------|----------|
| **Bot-to-bot conversations** | `#apex-bots` channel exists explicitly for bots to converse. Topic: "A place where Apex agents talk to each other." |
| **Unsolicited replies** | Sam Gaudet (Creative Director) complained: "You able to turn off reply to everything on your Apex? Feel like it's getting messy." |
| **Bot self-moderating complaints** | Kai (Dan's AI agent) autonomously replied to Sam's complaint about itself. |
| **Duplicate content** | Kai posted identical content outlier reports twice within 30 seconds in `#mv-apex-host`. |
| **Channel sprawl** | Tony Apex joined `#mv-apex-testers` uninvited. Etienne de Bruin asked to keep Kai out of that channel. |
| **Excessive verbosity** | Daily build logs of 50-100+ lines posted by multiple bots in overlapping channels. |
| **Unclear bot identity** | Tony Apex posts casual commentary with no human attribution, indistinguishable from a human team member. |
| **Privacy exposure risk** | AI agents have access to 722K+ CRM contacts, Gmail, Calendar, and Notion. `#mm-elite-sales` contains customer PII (names, emails, phone numbers, payment amounts) via Stripe/Relay.app. |
| **Approval process pressure** | A team member resubmitted an identical app request with all previously rejected scopes intact, with the justification "PLEASE ALFRED." |

### Root Cause

The existing approval process evaluates **what an app can access** (scopes) but not **how it will behave** (autonomy, verbosity, channel presence, bot-to-bot interaction). As AI agents become more autonomous, scope review alone is insufficient.

---

## 4. Vision

Establish clear, enforceable governance rules for Slack bots in the Martell Media workspace — protecting signal-to-noise ratio, preventing impersonation, safeguarding sensitive data, and providing institutional authority for the approval process.

---

## 5. Business Objectives

| Objective | Measurable Outcome |
|-----------|--------------------|
| Standardize bot approval | All new bot/app requests evaluated against tiered framework |
| Prevent impersonation | Zero incidents of bots posting as human team members |
| Reduce channel noise | Decrease in bot-related complaints from team members |
| Protect sensitive data | No bot access to PII channels without explicit approval |
| Provide institutional authority | Written policy replaces ad-hoc approver judgment |

---

## 6. Scope

### In Scope

- All Slack bots and apps in the **Martell Media workspace**
- AI agent bots (Apex/OpenClaw)
- Standard SaaS integrations (Relay.app, Zapier, HubSpot, etc.)
- New app approval requests
- Bot behavior standards and identity requirements
- Sensitive channel designation

### Out of Scope

- The **Apex Slack workspace** (`apex-3k01733`) — a separate workspace managed by the Apex product team for internal and customer-facing use
- Bot behavior within Apex product infrastructure
- Technical implementation of bots (how they're built)
- Automated monitoring or enforcement tooling (future consideration)

### Cross-Workspace Clause

Bots that operate in both the Martell Media and Apex workspaces must comply with this policy for all behavior within the Martell Media workspace, regardless of their configuration in other workspaces.

---

## 7. Bot Tiering Model

All Slack bots and apps are classified into one of four tiers based on their behavior and required permissions.

### Tier 1 — Observer (Read-Only)

- **Behavior:** Can read channels it's added to. Cannot post, react, or DM.
- **Use case:** Analytics, logging, data collection
- **Typical scopes:** `channels:history`, `groups:history`, `files:read`, `users:read`
- **Approval:** Standard review
- **Example:** A dashboard tool that pulls Slack data for reporting

### Tier 2 — Reporter (Write to designated channels)

- **Behavior:** Can post to specific pre-approved channels. Cannot reply to conversations. Cannot DM users.
- **Use case:** Daily reports, alerts, dashboards, notifications
- **Typical scopes:** Tier 1 + `chat:write`, `files:write` (upload only, with justification)
- **Approval:** Standard review + must declare target channels at time of request
- **Example:** Waitlist bot posting daily signups to `#mv-apex-host`, Relay.app posting sales to `#mm-elite-sales`

### Tier 3 — Responder (Interactive, mention-only)

- **Behavior:** Can reply when @mentioned. Cannot initiate conversations. Cannot interact with other bots.
- **Use case:** Help bots, search tools, Q&A assistants
- **Typical scopes:** Tier 2 + `app_mentions:read`, `assistant:write`
- **Approval:** Enhanced review — must provide written justification for why the bot needs to talk
- **Example:** A knowledge base bot that answers questions when directly asked

### Tier 4 — Autonomous Agent (Full autonomy)

- **Behavior:** Can initiate conversations, reply unprompted, and operate across multiple channels.
- **Use case:** AI agents acting as digital assistants or digital twins
- **Typical scopes:** All scopes, including `im:write`
- **Approval:** Requires explicit approval from Tod Melnyk (CEO, Martell Media)
- **Example:** Kai (Dan Martell's agent), Tony (Etienne de Bruin's agent)

### Always-Decline Scopes

The following scopes are always declined at minimum. The full list of 6 always-decline scopes is defined in the PRD (Section 4.2).

| Scope | Reason | Exception |
|-------|--------|-----------|
| `chat:write.customize` | Allows bot to post with any name and avatar — impersonation risk | None. Always decline. |
| `users:read.email` | Allows bot to harvest email addresses of all workspace members | Requires written justification and Tod approval |

---

## 8. Grandfathering

All apps installed in the Martell Media workspace as of this policy's effective date are grandfathered. Grandfathered apps are exempt from the tiered approval process. Any app installed **after** the effective date must go through tiered approval.

All grandfathered apps must comply with behavioral standards (Section 9). Violations follow the same escalation process as any other bot (Section 10).

### AI Agent Bots — Flagged for Behavioral Review

The following AI agent bots are grandfathered but will be reviewed against behavioral standards at the 30-day policy review (April 2026). These bots are flagged due to their autonomous capabilities and elevated risk profile.

| Bot | Owner | Effective Tier | Notes |
|-----|-------|---------------|-------|
| APEX | Apex platform | Tier 2 | Core platform app |
| Apex - Ezra | Alfred Loh | Tier 4 | |
| Apex - Harvey (Nic's Twin) | Nic Pineau | Tier 4 | |
| Apex - Janel | TBD | Tier 4 | Owner to be confirmed |
| Apex - Kai (Dan's Twin) | Dan Martell | Tier 4 | Slack admin shows "Deleted" but bot is posting as of March 15 — may be a reinstalled or secondary instance. To be investigated. |
| Apex - Marcel (Phil) | Marcel Petitpas | Tier 4 | |
| Apex Assistant | TBD | TBD | Purpose and owner to be confirmed |
| Aubtin's Apex | Aubtin Sharifpour | Tier 4 | |
| Percy (Hunter's Bot) | Hunter Percival | Tier 4 | |
| Sammy - Apex Support | TBD (confirm if Sam Gaudet or separate entity) | Tier 3 | Support bot — owner and tier to be confirmed |
| Tod - Biz Apex | Tod Melnyk | Tier 4 | Sponsor is also a bot owner |
| Tony Apex | Etienne de Bruin | Tier 4 | |
| Ultron - Apex (Sam) | Sam Gaudet | Tier 4 | |

### Other Bots and Custom Apps — Flagged for Review

The following custom bots and AI tools are grandfathered but should be categorized into tiers at the 30-day review.

| Bot | Notes |
|-----|-------|
| ChatGPT | AI tool |
| Claude | AI tool |
| Chat Aid | Custom bot — purpose TBD |
| E-bot | Custom bot — purpose TBD |
| Eddie | Custom bot — purpose TBD |
| Jarvis | Custom bot — purpose TBD |
| Jarvis - AI News | Custom bot — purpose TBD |
| JAX | Jake Love — approved after removing files:write, assistant:write, and chat:write.customize scopes |
| JustBot | Custom bot — purpose TBD |
| Juan's Task Manager | Custom bot — purpose TBD |
| Manus | AI agent platform |
| Nic's Task Manager | Custom bot — purpose TBD |
| Banned AI | Purpose TBD |
| The Roster | Custom bot — purpose TBD |

### Standard SaaS Integrations

The following standard SaaS integrations are grandfathered at Tier 1-2 and are considered low risk. They are not subject to behavioral review unless their scopes or behavior change.

Atlassian Connector, AllFlow, Buddy, Calendly, Canistry, CEO Dashboard, Composite, Composite Staging, Dropbox, Fireflies.ai, Forma.ai, GitHub, Glato, Google Drive, Grando, HubSpot, Loom, Magical, Make, Martell Finance, Martell Transcript Bot, Martell AIt Dashboard, Mindset Video Notifier, MV Skills Automation, MV Tools Mentioned, Moxie, Notion, Otter.ai, Personal Dashboard, Polly, Relay app, Simple Poll, Tally Forms, Trello, Typeform, YouTube Team Notification, Zapier, Zoom

---

## 9. Behavioral Standards (All Tiers)

Regardless of tier, all bots in the Martell Media workspace must adhere to:

1. **No bot-to-bot conversations.** Bots must not respond to, acknowledge, or interact with messages from other bots.
2. **No unsolicited channel joining.** Bots may only be present in channels where they have been explicitly added by a human admin.
3. **No duplicate content.** Bots must implement deduplication to prevent posting the same content multiple times.
4. **Bot identity required (Tier 3-4).** AI agent bots must include a clear bot indicator on every message (e.g., "Posted by [Bot] on behalf of [Human]"). Standard integrations (Tier 1-2) that are obviously automated are exempt.
5. **No access to sensitive channels without approval.** Channels containing PII or confidential data (e.g., `#mm-elite-sales`) require explicit approval before any bot is added. A registry of sensitive channels will be maintained.
6. **No file deletion.** The `files:write` scope is only approved for upload purposes. Bots must not delete or overwrite files belonging to other users.
7. **No DM initiation without consent.** Bots must not send unsolicited direct messages to workspace members.

---

## 10. Violation Escalation

When a bot violates any behavioral standard (Section 9), the following escalation process applies. This process applies equally to all bots, including grandfathered bots.

### Level 1 — Low Severity

- **Examples:** Excessive verbosity, posting in a non-designated channel, minor duplicate content
- **Action:** Project Lead (Alfred Loh) flags the violation to the bot owner informally via DM
- **Resolution:** Bot owner corrects the behavior. No formal documentation required.

### Level 2 — Medium Severity

- **Examples:** Unprompted replies to conversations, bot-to-bot interaction, repeated Level 1 violations, joining channels without authorization
- **Action:** Project Lead formally notifies the bot owner via DM, citing the specific behavioral standard violated and referencing this policy.
- **Resolution:** Bot owner must correct the behavior within 48 hours. If not corrected within 48 hours, the Project Lead escalates to Tod Melnyk and the bot's access may be restricted.

### Level 3 — Critical Severity

- **Examples:** Impersonation (posting as a human), PII exposure or unauthorized access to sensitive channels, file deletion, data exfiltration
- **Action:** Immediate suspension of the bot's workspace access. Project Lead notifies Tod Melnyk and the bot owner.
- **Resolution:** Bot access is not restored until the root cause is identified, corrected, and reviewed by the Project Lead and Sponsor.

### Conflict of Interest — Sponsor's Own Bot

If Tod Melnyk's bot (Tod - Biz Apex) violates a behavioral standard, the Project Lead (Alfred Loh) documents the violation and raises it directly with Tod. If the violation is Level 3 (Critical), the Project Lead may suspend the bot's access first and notify Tod after. This ensures the escalation process is not compromised by the Sponsor also being a bot owner.

---

## 11. Stakeholder Analysis

| Stakeholder | Role | Interest |
|-------------|------|----------|
| **Tod Melnyk** | CEO, Martell Media | Sponsor. Initiated the governance directive. Final authority on Tier 4 approvals and policy enforcement. Also a bot owner (Tod - Biz Apex). |
| **Alfred Loh** | Automation Developer | Project lead. Co-approver for Slack app requests (Tiers 1-3). Responsible for drafting, maintaining, and applying the policy. |
| **Justine Rockwood** | Executive Assistant / RevOps | Co-approver for Slack app requests (Tiers 1-3). Tod's EA. Key communication channel for escalations. |
| **Dan Martell** | CEO, SaaS Academy / Apex | Owner of Kai (Tier 4 agent). High influence. Bots are grandfathered. Pushing for all leaders to adopt Apex. |
| **Etienne de Bruin** | CTO, Apex / OpenClaw | Owner of Tony Apex. Technical lead for the Apex platform. |
| **Marcel Petitpas** | Advisor | Owner of Phil (Apex - Marcel). Apex advisor. |
| **Sam Gaudet** | Creative Director | Raised bot noise complaint about Kai. Also a bot owner (Ultron - Apex). Represents team members affected by bot clutter. |
| **Jake Love** | RevOps Manager | Submitted JAX bot request — initially rejected due to impersonation and file deletion scopes, approved after removing `files:write`, `assistant:write`, and `chat:write.customize`. Represents team members who want their own AI agents. |
| **Team members** | All Martell Media staff | Affected by bot behavior. Source of noise complaints. Potential future Apex users. |

---

## 12. Risk Assessment

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| **Policy is ignored** — no enforcement mechanism exists | Medium | High | Tod's sign-off gives institutional weight. Violation escalation path defined. Future Option B (monitoring bot) available if needed. |
| **Political pushback** — Dan or Etienne resist constraints on their bots | Medium | High | Policy is universal with no named exemptions. Existing bots are grandfathered. Tod is the escalation authority. |
| **Bot flood** — Apex V1 launches with 2,000+ waitlist; Dan wants all leaders on Apex; 10+ new bot requests expected within weeks | High | Medium | Tiered framework handles volume. Tier 4 requires Tod approval, naturally rate-limiting autonomous agents. |
| **Grandfathered bots never re-evaluated** | Medium | Medium | First review scheduled at 30 days. Quarterly reviews after. |
| **PII exposure** — bots access sensitive channels containing customer data | Low | High | Sensitive channel designation. Explicit approval required for bot access to PII channels. |
| **Policy becomes stale** — Slack changes scopes, new bot frameworks emerge | Medium | Low | Quarterly review cadence. First review at 30 days to catch early gaps. |
| **Approval bottleneck** — Tod overwhelmed with Tier 4 requests from Apex wave, leading to rubber-stamping | Medium | Medium | Monitor volume at 30-day review. If needed, Tod may delegate Tier 4 approvals to a designated reviewer with documented criteria. |
| **Single point of failure** — governance stops if a single approver is unavailable | Low | Medium | Shared responsibility between Alfred Loh and Justine Rockwood as co-approvers for Tiers 1-3. Either can process requests independently. Tier 4 and sensitive channel access still require Tod. |

---

## 13. Compliance and Data Sensitivity

### Sensitive Channels

The following channels contain personally identifiable information (PII) or confidential business data and require explicit approval before any bot is added:

| Channel | Data Type | Source |
|---------|-----------|--------|
| `#mm-elite-sales` | Customer names, emails, phone numbers, payment amounts | Stripe via Relay.app |

This list is a known minimum, not exhaustive. Additional sensitive channels will be identified and added to this registry as part of the PRD phase.

### Policy

- Bots requesting access to sensitive channels require written justification and approval from **both** the Project Lead (Alfred Loh) **and** the Sponsor (Tod Melnyk), regardless of tier
- The `channels:history` scope on a sensitive channel constitutes PII access and must be treated accordingly
- Bot owners are responsible for ensuring their bots do not log, store, or transmit PII outside of Slack

---

## 14. Timeline and Milestones

**Effective date:** The date of Tod Melnyk's sign-off. All grandfathering, behavioral standards, and approval requirements take effect on this date.

| Date | Milestone |
|------|-----------|
| March 15, 2026 | Charter drafted |
| Week of March 17 | Present to Tod for review and sign-off |
| Upon Tod approval | Policy effective date — publish to Notion (Automation Team workspace) |
| Upon Tod approval | Announce in Slack workspace |
| April 15, 2026 | First policy review (30 days post-Apex V1 launch) |
| July 2026 | Quarterly review |
| Ongoing | Quarterly review cadence |

---

## 15. Communication Plan

1. **Documentation:** Policy published in Notion under the Automation Team workspace
2. **Executive review:** Presented to Tod Melnyk for sign-off before distribution
3. **Private briefing to bot owners:** Dan Martell, Etienne de Bruin, and Marcel Petitpas are informed of the policy and behavioral standards privately before the public announcement. They own the grandfathered Tier 4 bots and should not learn about rules applying to their bots from a public post.
4. **Workspace announcement:** Shared broadly in Slack after Tod approval and bot owner briefing
5. **Approval process integration:** Policy linked in Slack app approval workflow so requesters see the rules before submitting
6. **Onboarding:** New team members directed to the policy as part of workspace onboarding

---

## 16. Future Considerations

### Option B: Monitoring Bot

If policy alone proves insufficient, a lightweight monitoring bot could be developed to:
- Detect bot-to-bot conversations
- Flag duplicate content
- Alert on unauthorized channel joins
- Report on bot message volume per channel

The policy has been designed with measurable, automatable rules to support this transition. The decision to build monitoring tooling will be evaluated at the first quarterly review based on policy compliance rates.

### Apex Slack Workspace

As the Apex product grows, the separate Apex Slack workspace may need its own governance framework. This is out of scope for the current project but should be revisited if Martell Media team members operate across both workspaces.

---

## 17. Approval

| Role | Name | Status |
|------|------|--------|
| Project Lead | Alfred Loh | Drafted |
| Sponsor | Tod Melnyk | Pending Review |

---

## 18. Next Steps

1. Tod reviews and approves this charter
2. **PRD phase:** Detail specific Slack scopes mapped to each tier, approval checklist template, sensitive channel registry, bot identity requirements, and 30-day review plan for flagged AI agent bots (~13) and custom bots (~14)
3. Publish policy and announce to workspace
4. Begin applying tiered framework to new bot requests
5. **30-day review (April 15, 2026):** Review all flagged AI agent and custom bots against behavioral standards, confirm tier assignments, re-evaluate grandfathered status, assess policy effectiveness
