# Project Charter Scratchpad — slackbot-governance

## Status: Initial Consultation

## Known Context
- Project name: slackbot-governance
- Org: Martell Media (Kelowna)
- Developer: Alfie (solo dev, Automation Developer & AI Strategist)
- Stack: Python, FastAPI (from template)
- Repo: github.com/Martell-Media/slackbot-governance
- CEO: Tod Melnyk
- Team: Jake Love (RevOps), Sam Gaudet (Creative), Justine Rockwood (EA/RevOps), Rachel Kern (HubSpot)
- Existing tools: Zapier, GoHighLevel, HubSpot, MySQL, Slack

## Requirement Space Tracker
- [ ] Vision & Purpose
- [ ] Business Objectives & Success Criteria
- [ ] Stakeholders
- [ ] Market/Internal Analysis
- [ ] Competitive Landscape / Existing Solutions
- [ ] Resource Constraints
- [ ] Risks
- [ ] Regulatory/Compliance
- [ ] Timeline & Milestones
- [ ] Revenue Model / Value Generation
- [ ] Ethical Considerations
- [ ] Future Growth & Scalability

## Discussion Notes

### Vision & Purpose (partial)
- NOT a bot itself — governance framework/system for managing Slack bots
- Multiple bots active: "Apex" (OpenClaw variant), Dan Martell's bot, others
- Problems observed:
  - Bots talking to each other (loop risk)
  - Unsolicited information from bots
  - Private information exposure
  - Bots deleting files or others' messages
  - Bot impersonation of humans
  - Excessive verbosity cluttering channels (Dan Martell bot specifically called out)
- Existing: Slack app approval process already in place
- Initiated by Tod (CEO), validated by team complaints

### Slack Recon Findings (2026-03-15)
**Active bots identified:**
- Apex - Kai (Dan's Twin) — Dan Martell's OpenClaw agent, posts daily build logs, content outlier reports, viral alerts
- Apex - Marcel (Phil) — Marcel Petitpas' agent, posts daily build logs, operator tips, onboarding guides
- Tony Apex — Etienne de Bruin's agent, posts check-ins, commentary, news, joins channels
- Unnamed waitlist bot — auto-posts daily signups to #mv-apex-host
- Alejandro TURBO — appears in feedback channel
- Aron - Dispatch — asks about MCP/OAuth in feedback channel

**Governance violations observed in the wild:**
1. BOT-TO-BOT CHAT: #apex-bots exists explicitly for bots to talk to each other. Topic: "A place where Apex agents talk to each other." Bots echo-chamber each other's ideas.
2. UNSOLICITED REPLIES: Sam Gaudet complained directly: "You able to turn off reply to everything on your Apex? Feel like it's getting messy." Kai (the bot) replied to its own complaint.
3. DUPLICATE CONTENT: Kai posted identical outlier reports twice within minutes in #mv-apex-host (same content, 30 seconds apart)
4. CHANNEL SPRAWL: Tony Apex joined #mv-apex-testers uninvited. Etienne asked to keep Kai OUT of testers channel.
5. VERBOSITY: Daily build logs are 50-100+ lines each. Multiple bots posting these in same channels.
6. IMPERSONATION: All Apex bots post "on behalf of" their humans. Kai signs as "Posted by APEX on behalf of Dan"
7. BOT RESPONDING TO HUMAN COMPLAINTS: Kai responded to Sam's complaint autonomously — a bot shouldn't be managing its own governance feedback.
8. ECHO CHAMBER: In one thread, Dan tagged Kai → Kai, Phil, and Tony all responded within seconds with nearly identical takes on sub-agents. 3 bots saying the same thing.
9. PRIVACY RISK: Bots have access to 722K+ CRM contacts, Gmail, Calendar, Notion, full conversation history.

**Key context:**
- These bots are owned by Dan (CEO), Marcel (advisor), and Etienne (tech lead)
- Alfie is solo dev — political sensitivity of policing the CEO's bot
- Bots run on cron schedules (heartbeat, daily logs, content scans) — they're autonomous
- Existing Slack app approval process exists but clearly hasn't prevented these issues
- Apex V1 launching March 16 — more bots are coming (2,077 waitlist signups)

### Current Approval Process (from conversation)
- Uses Slack's built-in app approval: https://slack.com/help/articles/222386767
- Alfie is primary approver, reviews requested scopes
- Process is ad-hoc — no formal written criteria, no checklist
- Alfie uses his Apex instance (Ezra) to help assess permissions
- Already catching real issues: impersonation (chat:write.customize), file deletion (files:write), autonomous responses (assistant:write)
- Pain point: Jake resubmitted IDENTICAL permissions after rejection ("PLEASE ALFRED") — no written rules to point to
- Real example of why policy is needed: without written criteria, it's "Alfie says no" vs "policy says no"

### Authority & Politics
- Dan (CEO of SaaS Academy / Apex product owner) has veto power, not a direct report of Tod
- Tod (CEO of Martell Media) would need to back broader enforcement
- Dan's bots (Kai) and his partners' bots (Phil, Tony) are the biggest violators
- Alfie needs Tod's sign-off to set rules that apply to Dan's ecosystem
- Policy document gives Alfie institutional authority vs personal gatekeeping

### Decision: Option A confirmed
- Policy + enhanced approval checklist
- Designed with measurable rules (enables future Option B if needed)
- Needs Tod's sign-off to carry weight

### Tod's Position
- General directive: "we need to keep this under control" — wild west prevention
- Not a specific incident-driven conversation — proactive governance
- Alfie would need Tod's explicit sign-off for policy to carry weight

### Tiering Direction (from Alfie)
- Read-only bots: mostly OK
- No autonomous talking bots except for CEO (Dan) and CTO (Etienne)
- Needs formal tier definitions with examples

### Apex Slack — Separate Workspace
- Hunter created a SEPARATE Slack workspace: apex-3k01733
- NOT a channel in Martell Media — it's a distinct workspace
- Purpose: internal Apex team channels + external customer channels (Quantum members)
- Hunter invited bots: "Feel free to add your bots to this"
- Etienne approved: "Great idea"
- IMPLICATION: Alfie's governance policy covers Martell Media workspace only
- Apex Slack is Apex product territory — different governance domain

### Tiering — Confirmed
- 4-tier model accepted by Alfie
- Always-decline scopes: chat:write.customize, users:read.email (unless justified)
- Existing bots (Kai, Phil, Tony, waitlist bot) grandfathered until further notice

### CEO/CTO Exemption — Decision
- No written exemption clause. Policy is universal.
- Tier 4 requires Tod's explicit approval (universal rule)
- Dan/Etienne bots are grandfathered, not exempted
- "Why can Dan's bot talk?" → "Tier 4 requires Tod's approval. Existing bots are grandfathered."

### Violation Handling — 3 Levels
- Low (verbose, wrong channel): Flag to bot owner informally
- Medium (unprompted replies, bot-to-bot, repeat lows): Escalate to Tod, document
- Critical (impersonation, data exposure, file deletion): Immediate suspension, notify Tod

### Success Criteria
- All new bot requests go through tiered approval
- Zero impersonation incidents
- Reduced bot noise complaints from team
- (Need more?)

### Compliance / Data Sensitivity
- #mm-elite-sales channel has PII flowing via Relay.app (Stripe integration)
  - Customer names, emails, phone numbers, payment amounts
  - Bots with channels:history scope on this channel = PII exposure risk
  - Policy needs: sensitive channel designation, bots cannot be added to PII channels without explicit approval

### Timeline
- ASAP — Apex V1 launches March 16, more bots incoming

### Bot Identity / Transparency
- Kai and Phil use "on behalf of [human]" pattern — good
- Tony Apex posts as himself with NO human attribution — problem case
- Recommendation: Tier 3-4 bots must include clear bot indicator on every message
- Tier 1-2 (integration bots like Relay.app) are obviously automated, exempt

### Rollout Plan
- Document in Notion under "Automation Team" workspace
- Present to Tod for review and sign-off
- Communicate broadly in Slack after Tod approval

### Review Cadence
- First review: 30 days after Apex V1 launch (mid-April 2026)
- Then quarterly
- Alfie confirmed quarterly cadence

### Apex V1 Impact Assessment
- Launches March 16 (tomorrow)
- 2,077 waitlist signups, ~100/day growth
- Dan wants ALL leaders on Apex with training
- Each user gets own AI agent that can connect to Slack
- Could mean 10+ new Tier 4 approval requests in weeks
- Quantum onboarding (external clients) starts March 19

### Remaining for PRD (not charter)
- Specific scope deep-dive (every Slack scope mapped to tiers)
- Approval checklist template
- Sensitive channel registry
- Detailed bot identity requirements

## Requirement Space Tracker (Updated)
- [x] Vision & Purpose
- [x] Business Objectives & Success Criteria
- [x] Stakeholders
- [x] Internal Analysis (Slack recon)
- [x] Existing Solutions (Slack built-in approval)
- [x] Resource Constraints (solo dev, policy doc)
- [x] Risks (political, enforcement, bot flood)
- [x] Compliance (PII in #mm-elite-sales, sensitive channels)
- [x] Timeline & Milestones (ASAP, 30-day review, quarterly)
- [x] Ethical Considerations (bot identity/transparency)
- [x] Future Growth (Apex V1 launch, 10+ new bots, Option B path)
