---
starter_id: next
package_manager: npm
project_name: closeflow-agent
hints:
  language_family: js
  team_size: solo
  deployment_target: vercel
  ci_provider: github-actions
  ci_default_flow: auto-deploy-on-merge
  bootstrapper_confidence: verified
  path_taken: standard
  quality_override: false
  self_check_answers: null
  has_auth: true
  has_payments: false
  has_realtime: false
  has_ai: true
  has_background_jobs: false
---

## Why this stack

CloseFlow Agent is a greenfield web-app with a 3-week MVP timeline and solo builder profile. Next.js provides full-stack JavaScript with server components, API routes, and TypeScript built-in—eliminating context switching and reducing setup friction. The Vercel + GitHub Actions + auto-deploy flow matches the after-hours-only constraint: push to main and the app is live in seconds. Chat UI components integrate seamlessly with React, the Claude API SDK works natively in Node.js, and the admin panel with password auth requires no additional infra. For the flooring estimate logic and conversation persistence, Next.js serverless functions are sufficient; no background jobs or real-time features in scope. TypeScript enforces safety on PRD-critical contracts (visitor email, estimate data, auth). Bootstrapper confidence is **verified**: Next.js is battle-tested in the 10x curriculum and supported end-to-end by the scaffolding tool.
