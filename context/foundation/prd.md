---
project: CloseFlow Agent (Service Business Lead Handler)
version: 1
status: draft
created: 2026-09-24
context_type: greenfield
product_type: web-app
target_scale: small
timeline_budget:
  mvp_weeks: 3
  hard_deadline: "2026-12-06"
  after_hours_only: true
---

# CloseFlow Agent: Product Requirements Document

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
5. Conversation + structured data (email, city, sqft, material, estimate, status) saved.
6. Owner logs in to admin panel → sees table: Status (HOT/WARM/NEW), Customer, Job, Estimate, Next Action.

### Secondary

- Agent can handle incomplete answers (skip a question, ask clarifying follow-up).

### Guardrails

- Agent never invents a price — uses rules or declines estimate.
- Lead always saved, even if conversation breaks.
- Admin panel loads and displays all leads correctly.

## User Stories

### US-001: Visitor Gets Instant Estimate

**Given** a visitor lands on the public chat page  
**When** they enter their email and ask about a flooring project  
**Then** the agent responds with clarifying questions (location, size, surface type) and, once enough data is collected, generates a preliminary estimate based on flooring pricing rules.

### US-002: Owner Reviews Leads in Admin Panel

**Given** the owner logs into `/admin` with their password  
**When** they view the leads dashboard  
**Then** they see a table of all saved conversations, each row showing Status (HOT/WARM/NEW), Customer (email), Job scope, Estimated cost, and Next Action (call, approve, follow-up).

## Functional Requirements

- FR-001: Visitor can access public chat page without login. Priority: must-have
- FR-002: Visitor can type question/message in chat. Priority: must-have
- FR-003: Agent can receive visitor's message and process it. Priority: must-have
- FR-004: Agent can ask qualifying questions (location, size, surface type). Priority: must-have
- FR-005: Agent can generate estimate based on business rules. Priority: must-have
- FR-006: System saves conversation in memory (or simple JSON log file). Priority: must-have
- FR-007: Visitor can provide email or phone number for contact, saved with conversation. Priority: must-have
- FR-008: Owner can access admin panel at /admin with password. Priority: must-have
- FR-009: Owner can see list of saved conversations in admin panel (basic UI, just view). Priority: must-have

## Non-Functional Requirements

- **Availability:** System accessible 24/7 (public chat always available, no downtime windows).
- **Response time:** Agent responds quickly, perceived as near-instantaneous to user (target < 3 seconds).
- **Data privacy:** Customer data protected locally. GDPR compliance will be added in v2.

## Business Logic

System generates preliminary estimate based on price per square meter and project location. Agent collects project scope (location, size, surface type), applies flooring pricing rules, and delivers estimate to customer. Owner reviews estimate and determines next action (approve, adjust, call customer).

## Access Control

**Visitor (chat interface):**
- Entry: Email-based login. Visitor provides email to start/continue conversation.
- Purpose: Track conversation by customer identity; allow owner to follow up.

**Owner (admin panel):**
- Entry: Email + password login to `/admin`.
- Single owner account, no team roles in MVP.
- Purpose: Access to lead list, conversation history, lead record (estimate, next action).

## Non-Goals

- No team collaboration — solo owner only; no roles or multi-user access for MVP.
- No persistent database — conversations stored in memory/logs only; do not survive server restart (v2 feature).
- No mobile app or external integrations — web-only, standalone; no Slack/email/SMS hooks in MVP.
- No GDPR compliance, advanced analytics, or reporting — MVP minimalist; data privacy added in v2.

## Open Questions

1. **Flooring pricing rules:** What is the exact pricing formula for flooring estimates? Is it per sq ft? Does it vary by location, material type, and surface condition? Who defines and maintains these rules?  
   *Block: yes (business logic is the core of the MVP)*

2. **Estimate status badges (HOT/WARM/NEW):** What rules determine if a lead is HOT vs. WARM vs. NEW? Is it time-based (how old?), customer interaction, or owner-assigned?  
   *Block: no (can be inferred from context or owner-managed in v1)*

3. **Message persistence across restarts:** In-memory storage means messages are lost if the server restarts. Is this acceptable for MVP, or should we use a simple file-based log that persists?  
   *Block: no (non-goal confirms MVP scope)*

4. **Owner notification:** When a new lead arrives, does the owner get notified? Via email, SMS, in-app? Or do they just check the admin panel periodically?  
   *Block: no (nice-to-have for v2)*

5. **Fallback if agent fails:** If the Claude API times out or returns an error, what message does the visitor see? "Agent unavailable, try again later"? Or a fallback option like "Call for quote"?  
   *Block: yes (error handling is essential for MVP)*
