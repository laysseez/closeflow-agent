---
project: CloseFlow Agent (Service Business Lead Handler)
context_type: greenfield
updated: 2026-09-24
product_type: web-app
target_scale: small
timeline_budget:
  mvp_weeks: 3
  hard_deadline: "2026-12-06"
  after_hours_only: true
checkpoint:
  current_phase: 8
  phases_completed: [1, 2, 3, 4, 5, 6, 7]
  frs_drafted: 9
  quality_check_status: accepted
---

# Shape Notes: CloseFlow Agent

## Vision & Problem Statement

Service business owners (flooring contractors, electricians, plumbers) respond manually to incoming leads, but slowly and poorly. While they're on job sites, clients message via chat or forms asking for estimates, availability, and service details. The owner can't respond promptly — leads go cold and clients book competitors instead. Existing lead-handling agents are built for restaurants and e-commerce (inventory-aware, menu-driven); they don't fit service businesses where the conversation is about project scope, location, budget, and timeline.

**Niche insight:** No agent exists tailored to service-business lead qualification. This is an unmet gap.

## User & Persona

**Primary persona:** Owner of a small service business (flooring, electrical, plumbing, HVAC, etc.). Runs the business solo or with a small team. Spends most time on job sites, not answering phones or emails. Loses leads because he can't be reached.

**First scope:** Flooring contractors (narrow MVP, real domain rules for flooring estimates).

## Success Criteria

### Primary
First working flow (visitor → chat → estimate → lead saved → dashboard):
1. Visitor enters email, starts chat.
2. Agent asks: "Jakie miasto?" (What city?)
3. Visitor answers → Agent asks: "Ile metrów i jaki materiał?" (Sq ft + surface type?)
4. Visitor answers → Agent generates estimate (using business rules, not inventing) or says "potrzebna rozmowa" (needs call).
5. Conversation + structured data (email, city, sqft, material, estimate, status) saved to database.
6. Owner logs in to dashboard → sees table: Status (HOT/WARM/NEW), Customer, Job, Estimate, Next Action.

### Secondary
- Agent can handle incomplete answers (skip a question, ask clarifying follow-up).

### Guardrails
- Agent never invents a price — uses rules or declines estimate.
- Lead always saved, even if conversation breaks.
- Dashboard loads and displays all leads correctly.

## Timeline & Budget

**MVP weeks:** 3 weeks (confirmed by user).  
**Hard deadline:** 2026-12-06 (December 6).  
**After-hours only:** Yes, 10–15 hours/week, evenings/weekends.  

## Timeline Acknowledgment

Acknowledged on 2026-09-23: 3-week MVP requires sustained dedication through setup (week 1–2, potentially invisible progress) and sprint (week 3). User accepted the cost and is committed.

---

## Access Control

**Customer (visitor) chat interface:**
- Entry: email-based login. Customer provides email to start/continue conversation.
- Purpose: Track conversation by customer identity; allow owner to follow up.

**Owner dashboard:**
- Entry: email + password login.
- Single owner, no team roles in MVP.
- Purpose: Access to lead list, conversation history, lead record (estimate, next action).

---

## Functional Requirements

- FR-001: Visitor can access public chat page without login. Priority: must-have
  > Socrates: Kontrargument: spam, boty. Rozwiązanie: rate limit/captcha w v1.5; MVP public bez ochrony to ok.

- FR-002: Visitor can type question/message in chat. Priority: must-have

- FR-003: Agent can receive visitor's message and process it. Priority: must-have
  > Socrates: Kontrargument: API padł, timeout. Rozwiązanie: fallback message "Agent niedostępny, spróbuj później".

- FR-004: Agent can ask qualifying questions (location, size, surface type). Priority: must-have

- FR-005: Agent can generate estimate based on business rules. Priority: must-have
  > Socrates: Kontrargument: agent wymyśli cenę poza regułami. Rozwiązanie: agent robi best-effort; właściciel zawsze sprawdza w dashboardzie.

- FR-006: System saves conversation in memory (or simple JSON log file). Priority: must-have
  > Socrates: Kontrargument: co jeśli server się restartuje? Rozwiązanie: MVP bez persistent DB; logi wystarczają do testowania. V2 = prawdziwa baza.

- FR-007: Visitor can provide email or phone number for contact, saved with conversation. Priority: must-have

- FR-008: Owner can access admin panel at /admin with password. Priority: must-have

- FR-009: Owner can see list of saved conversations in admin panel (basic UI, just view). Priority: must-have
  > Socrates: Kontrargument: admin panel to dodatkowa praca. Rozwiązanie: bardzo podstawowy - tylko lista + view, bez edytowania/usuwania.

---

## Business Logic

System generates preliminary estimate based on price per square meter and project location. Agent collects project scope (location, size, surface type), applies flooring pricing rules, and delivers estimate to customer. Owner reviews estimate and determines next action (approve, adjust, call customer).

## Non-Functional Requirements

- Availability: System accessible 24/7 (public chat always available, no downtime windows).
- Response time: Agent responds quickly, perceived as near-instantaneous to user (target < 3 seconds).
- Data privacy: Customer data protected locally. GDPR compliance will be added in v2.

---

## Product Type & Scale

**Product type:** Web application (public chat page + admin panel in browser).  
**Target scale:** Small — solo flooring business owner initially; single-user admin account.  
**Timeline:** 3 weeks, after-hours (10–15 hrs/week). Hard deadline: December 6, 2026.

## Non-Goals

- No team collaboration — solo owner only; no roles or multi-user access for MVP.
- No persistent database — conversations stored in memory/logs only; do not survive server restart (v2 feature).
- No mobile app or external integrations — web-only, standalone; no Slack/email/SMS hooks in MVP.
- No GDPR compliance, advanced analytics, or reporting — MVP minimalist; data privacy added in v2.

---

## Quality Cross-Check

All six criteria verified on 2026-09-24:
- ✓ Access Control: Owner login at /admin with password
- ✓ Business Logic: One-sentence rule captured
- ✓ Project artifacts: shape-notes.md with full frontmatter
- ✓ Timeline-cost acknowledged: User accepted 3-week sustained effort
- ✓ Non-Goals: Four clearly scoped non-goals defined

**Status:** ACCEPTED. Ready for `/10x-prd`.
