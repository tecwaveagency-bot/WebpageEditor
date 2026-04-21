# Website Change-Request Portal — Blueprint

## Purpose

Build a secure client portal where authenticated users can:
- sign in / sign up
- view a list of house websites
- open a house-specific workspace
- see the current live webpage inside the portal
- select parts of the page they want changed
- chat with an AI assistant in simple language
- upload files such as replacement images or documents
- confirm the requested change
- submit a structured change request
- trigger a webhook so the internal team can execute the edits manually

This is a change-request portal, not a self-editing website builder.

---

## House list

- www.jewelhomecare.com
- www.annissagemcare.com
- www.carverseniorhomes.com
- www.sunnycarehomes.com
- www.saintclairseniorhome.com
- www.carevanna.com
- www.carejacob.com
- www.saintagneshome.com
- www.carejuliet.com
- www.avemariacares.com
- www.fountainvalleyseniorhomes.com
- www.ravpremeracare.com
- www.maryknollcare.com
- www.carejordanseniorhomes.com
- www.santamarianacare.com
- www.rosellecarehome.com
- www.carecharlize.com
- www.saintbenedictcare.com
- www.candleberrycare.com
- www.carephoebe.com
- www.careayesha.com
- www.amothertheresacare.com

---

## Main routes

- `/login`
- `/signup`
- `/forgot-password`
- `/dashboard`
- `/houses/:houseId`
- `/houses/:houseId/history`
- `/requests/:requestId`

---

## House workspace layout

### Left side
- live page preview
- page URL
- refresh button
- selection mode

### Right side
- AI chat panel
- selected element summary
- uploads section
- draft request summary
- action buttons

---

## Main tables

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

## Required AI behavior

The AI assistant must:
- understand what the user wants changed
- reference current page context
- ask short follow-up questions
- maintain a structured request draft
- support uploaded files
- never claim the website was changed

---

## Live preview rule

Because the house sites are controlled by the same operator, the portal should use a real live embed for v1.

Required review:
- `Content-Security-Policy` with `frame-ancestors`
- `X-Frame-Options`
- related app headers

Only allow framing from the portal domain.

---

## Submission rule

A submitted request must:
- create a request record
- create request item records
- link uploaded files
- send one webhook payload
- log webhook attempt/result

---

## Core implementation phases

### Phase 1
- auth
- protected routes
- dashboard data integration

### Phase 2
- house workspace shell
- history shell
- live preview shell

### Phase 3
- selection capture
- selected element persistence
- page context model

### Phase 4
- AI request drafting
- structured outputs
- confirmation flow

### Phase 5
- uploads
- final submission
- webhook
- logging

### Phase 6
- request detail
- status history
- polish
