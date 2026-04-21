# Engineering Plan — Website Change-Request Portal

## Goal

Build a secure portal where a client can:

1. sign in
2. choose one of the house websites
3. see the **current live webpage** inside the portal
4. click an element or describe a change
5. chat with an AI assistant that understands the current page context
6. upload a replacement file if needed
7. confirm the request
8. submit the request
9. send the structured request to a webhook so the internal team can execute the change manually

This is a **change-request portal**.  
It is **not** a CMS, not a page builder, and not an auto-publishing editor.

---

## The split: what goes to Lovable, what goes to Codex, what goes into GitHub

## 1) What to give to Lovable

Use Lovable for the **frontend shell and UX layout**, not for the full product logic.

Lovable should create:

- auth pages
- dashboard page
- house workspace page
- clean split layout
- upload area UI
- chat panel UI
- request summary UI
- protected route shell
- placeholder components ready for Supabase / Codex wiring

### Do **not** ask Lovable to do these first
- full AI request orchestration
- webhook logic
- complex database design decisions
- iframe selection bridge logic
- security rules
- production RLS policy design

Those parts should be handled in Codex.

### Exact prompt to give Lovable

```txt
Build a clean React + TypeScript frontend for a website change-request portal.

Main purpose:
Authenticated users log in, pick one house website, view the live page inside the app, describe requested website changes, upload files, review a draft request, and submit that request for manual execution by an internal team.

Important:
This is not a CMS and not a visual site builder.
Do not build automatic publishing.
Do not make up backend logic that conflicts with Supabase.
Keep the UI production-clean and simple.

Pages needed:
1. /login
2. /signup
3. /forgot-password
4. /dashboard
5. /houses/:houseId
6. /houses/:houseId/history
7. /requests/:requestId

UI requirements:
- Modern but simple layout
- Protected app shell
- Dashboard with cards for each house
- House workspace with:
  - top bar
  - left live preview panel
  - right AI chat panel
  - uploads section
  - request draft summary
  - action buttons: Keep editing, Save draft, Submit request
- Request history page
- Request detail page

House list for seed UI:
www.jewelhomecare.com
www.annissagemcare.com
www.carverseniorhomes.com
www.sunnycarehomes.com
www.saintclairseniorhome.com
www.carevanna.com
www.carejacob.com
www.saintagneshome.com
www.carejuliet.com
www.avemariacares.com
www.fountainvalleyseniorhomes.com
www.ravpremeracare.com
www.maryknollcare.com
www.carejordanseniorhomes.com
www.santamarianacare.com
www.rosellecarehome.com
www.carecharlize.com
www.saintbenedictcare.com
www.candleberrycare.com
www.carephoebe.com
www.careayesha.com
www.amothertheresacare.com

Constraints:
- TypeScript only
- React frontend only
- clean reusable components
- add clear placeholder hooks for Supabase data loading
- add clear placeholder hooks for live iframe preview
- add clear placeholder hooks for AI chat integration
- do not hardcode final business logic
- do not invent auto-editing features
- do not overdesign

Output:
Generate the UI shell and route structure only, in a way that Codex can continue implementation safely.
```

---

## 2) What to give to Codex

Use Codex for the **real implementation work**.

Codex should handle:

- Supabase integration
- auth wiring
- protected routes
- database schema integration
- houses data load
- live preview component
- selection capture architecture
- AI structured output flow
- upload wiring
- webhook submission
- request history
- production cleanup

### The rule for Codex
Do **not** give Codex the whole project in one giant prompt.

Use phases.

### Codex implementation order

#### Phase 1
- Supabase auth
- protected routing
- profile handling
- environment setup
- houses table integration
- dashboard data load

#### Phase 2
- house workspace page
- live page iframe panel
- right-side request/chat panel shell
- request history page shell

#### Phase 3
- element selection capture model
- page context capture
- selected element metadata persistence

#### Phase 4
- AI backend function
- structured JSON responses
- draft request state
- confirmation flow

#### Phase 5
- upload to storage
- attach files to request
- final request summary
- submit request
- webhook dispatch
- webhook logging

#### Phase 6
- request detail page
- status history
- admin-ready cleanup
- error handling and polish

### Exact first prompt to give Codex

```txt
Read /docs/change-request-portal-blueprint.md and /AGENTS.md first and follow them strictly.

Implement only Phase 1 and Phase 2.

Scope:
1. Supabase auth integration
2. protected routes
3. profile and session handling
4. houses data integration from database
5. dashboard page loading houses from database
6. house workspace shell
7. request history shell
8. placeholder live preview component
9. placeholder AI chat/request panel component

Constraints:
- TypeScript only
- preserve existing Lovable project structure where reasonable
- do not redesign the product
- do not implement webhook submission yet
- do not implement automatic website editing
- keep components clean and production-ready
- use clear types
- add TODO markers only where later phases are intentionally deferred

At the end:
- list changed files
- explain any schema assumptions
- note blockers clearly
```

### Exact second prompt to give Codex

```txt
Continue from the existing implementation.

Implement Phase 3 only.

Scope:
1. live iframe preview component
2. page URL state
3. refresh behavior
4. selection mode UI
5. selected element metadata capture model
6. persistence for selected element records
7. safe fallback behavior when selection cannot be captured

Do not implement AI logic yet.
Do not redesign existing pages.
Keep all code TypeScript and production-clean.

At the end:
- list changed files
- explain technical limits around iframe selection capture
- note any required follow-up work for the AI phase
```

### Exact third prompt to give Codex

```txt
Continue from the existing implementation.

Implement Phase 4 and Phase 5.

Scope:
1. AI request drafting flow
2. backend function for model call
3. structured response parsing
4. upload support using Supabase Storage
5. request draft summary persistence
6. final submission flow
7. webhook dispatch and logging

Requirements:
- the AI is a request clarification assistant, not an editor
- use structured outputs, not ad hoc free-text parsing
- never claim a website was changed
- requests must stay in draft state until user submits
- submission must create a change_request record and webhook log

At the end:
- list changed files
- explain environment variables required
- explain webhook payload shape
- note any blockers
```

---

## 3) What to paste into GitHub

GitHub is the source of truth.  
Use it for repo files, version control, reviews, and Vercel deployment linkage.

### Create these files in the repo

#### `/docs/change-request-portal-blueprint.md`
Use the full project blueprint file.

#### `/AGENTS.md`
Use the Codex rules file.

#### `/.env.example`
Add placeholder variable names only.

Suggested content:

```env
VITE_SUPABASE_URL=
VITE_SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=
OPENAI_API_KEY=
WEBHOOK_CHANGE_REQUEST_URL=
VITE_APP_BASE_URL=
```

#### `/README.md`
Add a short project summary and setup instructions.

### What GitHub should contain
- application code
- docs
- AGENTS.md
- env example
- migration files if you create them
- no secrets
- no real API keys
- no production upload URLs with sensitive tokens

### What should **not** be committed
- `.env`
- real API keys
- Supabase service secrets
- private webhook secrets
- downloaded production data dumps

---

# Architecture

## Frontend
- Lovable React app
- TypeScript
- React Router
- Tailwind

## Backend
- Supabase Auth
- Supabase Postgres
- Supabase Storage
- Supabase Edge Functions

## AI
- OpenAI API
- structured JSON output
- clarification agent only

## Hosting
- Vercel

## Automation handoff
- webhook to n8n or internal endpoint

---

# Product pages

## `/login`
- sign in form
- link to sign up
- link to password reset

## `/signup`
- sign up form

## `/forgot-password`
- password reset flow

## `/dashboard`
- all houses in cards or rows
- each row shows:
  - display name
  - domain
  - status
  - open workspace button

## `/houses/:houseId`
Main workspace:
- top bar
- current page URL
- refresh button
- live preview panel
- AI chat panel
- uploads section
- draft request summary
- action buttons

## `/houses/:houseId/history`
- request list for that house

## `/requests/:requestId`
- full submitted request detail

---

# Data model

Use these main tables:

- `profiles`
- `houses`
- `house_pages`
- `page_snapshots`
- `chat_sessions`
- `chat_messages`
- `selected_elements`
- `change_requests`
- `change_request_items`
- `uploaded_files`
- `webhook_logs`
- `request_status_history`

---

# What the AI must do

The AI assistant must:

- understand what the user wants changed
- reference the selected element accurately
- speak in simple plain words
- ask only necessary follow-up questions
- keep a structured draft request
- support multiple changes in one session
- ask for upload if image replacement is requested
- never say the change was already made

The AI assistant must **not**:
- edit the live website
- claim deployment happened
- invent missing page context
- convert the portal into a builder

---

# Live preview strategy

Because you control the sites, code, domain, and DNS:

Use a real live embed for v1.

That means the target sites must allow being framed by the portal domain.

Review and adjust:
- `Content-Security-Policy` `frame-ancestors`
- `X-Frame-Options`
- any conflicting Vercel or app headers

Do not allow open framing from everywhere.  
Allow framing only from the portal domain.

---

# Upload support

Allow uploads for:
- replacement images
- supporting files
- optional documents

Use Supabase Storage buckets:
- `request-uploads`
- `page-snapshots`
- `element-crops`

---

# Submission lifecycle

Statuses:
- `draft`
- `submitted`
- `queued`
- `in_review`
- `completed`
- `cancelled`

Webhook fires only on:
- `submitted`

---

# Exact execution order

## First
1. Put blueprint doc in repo
2. Put AGENTS.md in repo
3. Connect GitHub repo to Vercel
4. Set env vars in Vercel
5. Confirm Supabase project exists

## Then Lovable
Use Lovable to generate or refine:
- route shell
- dashboard UI
- house workspace UI
- history page UI
- request detail UI

Do not ask Lovable to solve backend architecture.

## Then Codex
Phase by phase:
1. auth + protected routes
2. houses integration + dashboard
3. workspace + live preview shell
4. selection capture
5. AI + draft request
6. uploads + submit + webhook
7. history + cleanup

## Then Vercel
Deploy preview branch first.  
Do not point production users to it until:
- auth works
- iframe works
- submission works
- webhook logs are verified

---

# What you should physically do next

## Give to Lovable
Use the Lovable prompt from this document and get the clean UI shell.

## Put in GitHub
Create:
- `/docs/change-request-portal-blueprint.md`
- `/docs/codex-engineering-plan.md`
- `/AGENTS.md`
- `/.env.example`

## Give to Codex
After the UI shell exists in the repo, give Codex:
1. the first prompt
2. then the second prompt
3. then the third prompt

Do not skip phases.

---

# Final recommendation

Use this operating model:

- **Lovable** = UI shell and visual structure
- **GitHub** = source of truth
- **Codex** = implementation engineer
- **Vercel** = deployment
- **Supabase** = backend
- **n8n/webhook** = handoff to manual ops

That is the correct split for this project.
