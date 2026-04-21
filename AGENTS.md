# AGENTS.md

## Role
You are implementing a website change-request portal inside this repository.

## Primary rule
Follow the project docs strictly. Do not redesign the product beyond the documented scope.

## Read first
Before making changes, read:
1. `/docs/change-request-portal-blueprint.md`
2. `/docs/codex-engineering-plan.md`

## Product intent
This product is a secure portal where authenticated users can:
- view a live house website inside the app
- point to what they want changed
- chat with an AI clarification assistant
- upload files
- submit a structured change request
- trigger a webhook for manual execution by the internal team

This is **not**:
- a CMS
- a drag-and-drop site editor
- an auto-publishing system
- an autonomous code-editing tool

## Implementation rules
- Use TypeScript only
- Preserve existing working Lovable structure where reasonable
- Prefer small, reviewable changes
- Keep components modular and production-clean
- Use explicit types where possible
- Do not commit secrets
- Do not invent business logic outside the docs
- Add TODO markers only when a later phase is intentionally deferred
- Do not replace real integrations with fake mocks if wiring can be done safely

## Backend rules
- Use Supabase for auth, database, storage, and server-side functions
- Use structured outputs for AI request drafting
- Keep request records in draft state until the user explicitly submits
- Webhooks should fire only on request submission
- Log webhook attempts and results

## AI rules
The AI assistant is a request clarification assistant only.
It must:
- speak simply
- confirm what the user wants changed
- ask for missing information
- maintain a structured draft request
- never claim a website was edited
- never imply deployment happened

## UX rules
- Keep the UI simple and clean
- Prefer clarity over novelty
- Do not overdesign
- Preserve straightforward navigation
- Surface blockers and failure states clearly

## Output expectations after each task
At the end of each task:
1. list files changed
2. summarize what was implemented
3. list assumptions
4. list blockers or follow-up items
