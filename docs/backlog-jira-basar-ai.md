# Backlog Jira-like: Basar AI Product Upgrade

## 1. Executive Summary

هذا الـ backlog يترجم متطلبات SRS إلى tickets قابلة للتنفيذ داخل الفريق. يتم تنظيمها على أساس Epics و User Stories و Acceptance Criteria و Technical Tasks.

### Project goals
- تحويل المنتج إلى SaaS polished
- إمكانية استخدام API الخاص أو الاشتراك
- تحسين صفحة Generator لتصبح template-first
- إضافة Groq للنصوص
- تحسين UX marketing-grade

---

## 2. Epic Map

### EPIC-01: Product & Brand Experience
- Landing page
- auth redesign
- onboarding flow
- user choice between API key vs subscription

### EPIC-02: Brand & Kit Management
- enhanced brand setup
- logo handling
- brand kit completion flow

### EPIC-03: Smart Template System
- template library
- template metadata
- prompt mapping by skill

### EPIC-04: Hybrid Generation Engine
- generation orchestration
- provider selection
- post-processing
- AI copy generation

### EPIC-05: Billing & Usage Controls
- plan model
- usage limits
- billing integration

### EPIC-06: Analytics & Admin
- admin dashboard refinement
- stats and brand overview

### EPIC-07: Quality & Launch Readiness
- testing
- QA
- security validation
- deployment validation

---

## 3. User Stories & Acceptance Criteria

## EPIC-01: Product & Brand Experience

### US-01: Landing Page for SaaS Product
**As a** new user  
**I want** to see a polished landing page  
**so that** I understand the product value and choose a path quickly.

**Acceptance Criteria**
1. Landing page loads on root route.
2. Hero section includes value proposition.
3. CTA buttons exist for signup and API key flow.
4. Pricing section is visible.
5. User can identify the primary use cases.

### US-02: Auth Experience with Usage Mode Selection
**As a** new user  
**I want** to choose between using my own API key or subscribing  
**so that** I can start according to my preference.

**Acceptance Criteria**
1. Login/signup screen shows two options.
2. Each option loads a different onboarding flow.
3. API-key path stores configuration in secure backend flow.
4. Subscription path routes to plan selection.

### US-03: Redirect & Session Flow
**As a** authenticated user  
**I want** to be redirected correctly based on session state  
**so that** I land in the correct dashboard.

**Acceptance Criteria**
1. Authenticated users cannot access login/signup pages.
2. Unauthenticated users are redirected to login.
3. Root route forwards user to /brands or /login.

---

## EPIC-02: Brand & Kit Management

### US-04: Brand Creation & Setup
**As a** user  
**I want** to create and manage brands  
**so that** I can organize content per brand.

**Acceptance Criteria**
1. User can create brand with valid name.
2. Brand appears in dashboard immediately.
3. User can rename brand.
4. User can delete brand with confirm flow.

### US-05: Brand Logo Upload
**As a** user  
**I want** to upload a logo to my brand  
**so that** it can be used in generated content.

**Acceptance Criteria**
1. User can upload PNG/JPG/WebP file.
2. Image is validated and resized.
3. Logo URL is saved in database.
4. Old logo is removed if replaced.

### US-06: Brand Kit Completion
**As a** user  
**I want** to complete my brand identity form  
**so that** the AI understands my brand style.

**Acceptance Criteria**
1. User can save tagline, tone, audience, colors, avoid words.
2. Status updates to complete when required fields are present.
3. Summary is generated and stored.
4. Generator uses the result in prompt composition.

---

## EPIC-03: Smart Template System

### US-07: Template Selection
**As a** user  
**I want** to choose a content template  
**so that** the generated result fits the platform and use case.

**Acceptance Criteria**
1. User can select from a template list.
2. Templates include product ad, promo, quote, cover, etc.
3. Template metadata is shown to the user.
4. Selected template impacts prompt generation.

### US-08: Skill-aware Prompting
**As a** user  
**I want** the system to build a structured prompt from my selected skill and template  
**so that** content has better quality and consistency.

**Acceptance Criteria**
1. The system maps template to prompt guidance.
2. The prompt includes platform rules and tone.
3. The prompt uses brand context when available.
4. System rejects empty or weak prompts.

---

## EPIC-04: Hybrid Generation Engine

### US-09: Image Generation by Provider
**As a** user  
**I want** to generate an image for my brand  
**so that** I can publish content visually.

**Acceptance Criteria**
1. User can choose provider: OpenAI or Gemini.
2. Provider key validation occurs before generation.
3. Result is stored with status and metadata.
4. Generation result is returned with image URL.

### US-10: Hybrid Copy Generation
**As a** user  
**I want** the image to come with supporting text  
**so that** the output is ready for posting.

**Acceptance Criteria**
1. Result includes caption text.
2. Result includes CTA or hook if enabled.
3. Copy aligns with selected tone and platform.
4. If Groq is unavailable, fallback behavior is clear and safe.

### US-11: Logo and Watermark Handling
**As a** user  
**I want** logo placement to be controlled  
**so that** the design matches my brand and platform requirements.

**Acceptance Criteria**
1. User can choose logo mode: none / prompt / watermark / both.
2. Watermark is applied correctly to final image.
3. Brand logo metadata is read from storage object.
4. Invalid logo state is handled with clear error.

---

## EPIC-05: Billing & Usage Controls

### US-12: Plan Selection
**As a** user  
**I want** to select a plan  
**so that** I can use the product without managing my own provider key.

**Acceptance Criteria**
1. User sees plan cards with limits and price.
2. User can select one plan.
3. System records selected plan and active state.
4. On inactive plan, usage restrictions apply.

### US-13: Usage Limitation
**As a** user  
**I want** the system to enforce my plan limits  
**so that** I do not exceed my quota.

**Acceptance Criteria**
1. Sum of usage is tracked per user/brand.
2. System blocks generation when the limit is reached.
3. Clear error message is shown to the user.
4. Admin can review usage metrics.

---

## EPIC-06: Admin & Analytics

### US-14: Admin Dashboard
**As an** admin  
**I want** to review stats and brand activity  
**so that** I can monitor platform health.

**Acceptance Criteria**
1. Admin route is restricted to approved emails.
2. Admin stats are served from backend.
3. Admin dashboard shows total accounts, brands, generations, provider breakdown.
4. Admin can view per-brand overview.

---

## EPIC-07: Quality & Launch Readiness

### US-15: Regression Safety
**As a** team  
**I want** validations and regressions to be automated  
**so that** release confidence is high.

**Acceptance Criteria**
1. Backend tests cover authentication and generation contracts.
2. Frontend route smoke checks cover critical flows.
3. Regression check runs before release.

---

## 4. Jira-like Ticket Breakdown

## Team: Product / UX / Frontend / Backend / QA / AI

### Epic 1: Product & Brand Experience

#### TICKET-01: Landing Page UI Build
- Type: Story
- Priority: P0
- Assignee: Frontend + UX
- Description: Build polished landing page with hero, value props, pricing, CTA.
- Acceptance: root route renders marketing page and CTA links work.

#### TICKET-02: Auth page redesign
- Type: Story
- Priority: P0
- Assignee: Frontend + UX
- Description: redesign login/signup with modern SaaS look and two mode buttons.
- Acceptance: page includes BYOK and subscription options.

#### TICKET-03: Onboarding route logic
- Type: Story
- Priority: P0
- Assignee: Frontend + Backend
- Description: route user after auth based on plan or key selection.
- Acceptance: correct dashboards and session states.

### Epic 2: Brand & Kit Management

#### TICKET-04: Brand CRUD UX improvements
- Type: Story
- Priority: P1
- Assignee: Frontend
- Description: improve brand cards and dashboard experience.
- Acceptance: cards show logo, summary, status, CTA.

#### TICKET-05: Brand kit validation enhancement
- Type: Story
- Priority: P1
- Assignee: Backend
- Description: improve validation logic for brand kit inputs.
- Acceptance: complete/in-progress states align with UI.

### Epic 3: Smart Template System

#### TICKET-06: Define template schema and catalog
- Type: Story
- Priority: P0
- Assignee: Product + Backend
- Description: create template schema with metadata and prompt guidance.
- Acceptance: catalog contains at least 6 templates.

#### TICKET-07: Template selector UI
- Type: Story
- Priority: P0
- Assignee: Frontend
- Description: UI cards for template selection.
- Acceptance: user can select and preview template metadata.

#### TICKET-08: Template-to-prompt mapping engine
- Type: Story
- Priority: P0
- Assignee: Backend
- Description: convert template selection into structured prompt data.
- Acceptance: generated prompt contains template variables and brand context.

### Epic 4: Hybrid Generation Engine

#### TICKET-09: Generation request schema evolution
- Type: Story
- Priority: P0
- Assignee: Backend
- Description: extend generation payload to include template, skill, tone, caption generation flag.
- Acceptance: request body supports new fields without breaking current model.

#### TICKET-10: Groq caption generation service
- Type: Story
- Priority: P1
- Assignee: Backend + AI
- Description: integrate Groq text generation for caption/CTA/hook.
- Acceptance: caption generation works and handles failures gracefully.

#### TICKET-11: Output metadata enhancement
- Type: Story
- Priority: P1
- Assignee: Backend
- Description: return image and text output in a single generation response.
- Acceptance: UI can render caption and image together.

#### TICKET-12: Post-process rendering and watermark control
- Type: Story
- Priority: P1
- Assignee: Backend
- Description: ensure watermark, logo placement, and output dimensions are correct.
- Acceptance: generated image respects preset and logo rules.

### Epic 5: Billing & Usage Controls

#### TICKET-13: Plan schema and data model
- Type: Story
- Priority: P1
- Assignee: Backend + Product
- Description: create plans tables and usage records.
- Acceptance: plan records exist and selected plan is stored.

#### TICKET-14: Usage enforcement service
- Type: Story
- Priority: P1
- Assignee: Backend
- Description: enforce generation quotas by user/plan.
- Acceptance: blocked generation returns clear message.

#### TICKET-15: Payment integration ready
- Type: Story
- Priority: P2
- Assignee: Backend + Product
- Description: prepare checkout route(s) for payment provider.
- Acceptance: routes and payloads are valid for integration.

### Epic 6: Admin & Analytics

#### TICKET-16: Admin dashboard UI polish
- Type: Story
- Priority: P2
- Assignee: Frontend
- Description: improve admin pages and stats cards.
- Acceptance: admin dashboard clearly shows KPIs.

#### TICKET-17: Additional admin views
- Type: Story
- Priority: P2
- Assignee: Backend
- Description: extend admin views for subscription and generation usage stats.
- Acceptance: summary and per-brand info are accessible.

### Epic 7: Quality & Launch Readiness

#### TICKET-18: Regression test suite update
- Type: Story
- Priority: P0
- Assignee: QA + Backend
- Description: update backend tests after schema and feature changes.
- Acceptance: critical flows pass.

#### TICKET-19: Security review
- Type: Story
- Priority: P0
- Assignee: Backend + DevOps
- Description: review vault use, auth, and storage policies.
- Acceptance: no plain-text secrets in repo or frontend.

#### TICKET-20: Release checklist
- Type: Story
- Priority: P0
- Assignee: PM + Tech Lead
- Description: final checklist before go-live.
- Acceptance: release gate passes before production deployment.

---

## 5. Priortization

### P0 (must have for launch)
- Landing page
- Modern auth flow
- API key flow
- Template system foundation
- Generator redesign
- Groq caption generation
- Security review
- Regression coverage

### P1 (important next)
- Billing and plan model
- Usage limits
- Advanced output metadata
- Brand kit enhancements

### P2 (post-launch)
- Payment provider integration deep work
- Multi-team roles
- More templates and more providers

---

## 6. Definition of Ready

A ticket is ready when:
1. Business value is clear
2. Acceptance criteria are written
3. Dependencies identified
4. Technical assumptions documented
5. owner assigned

---

## 7. Definition of Done

A ticket is done when:
1. The code is implemented
2. Tests are updated or added
3. UI/UX behavior matches acceptance
4. Security review is complete if necessary
5. Merge to main is approved
6. Documentation is updated

---

## 8. Suggested Agile sprint layout

### Sprint 1
- Landing page
- Auth redesign
- BYOK flow
- brand management improvements

### Sprint 2
- template library and selector
- prompt mapping
- generator redesign

### Sprint 3
- Hybrid generation engine
- Groq copy flow
- output metadata and download

### Sprint 4
- billing and plan gating
- usage enforcement
- admin analytics
- release prep

---

## 9. Suggested team assignment

### Frontend
- Landing page
- Auth redesign
- Template UI
- Generator UX
- Dashboard polish

### Backend
- Brand APIs
- Template orchestration
- Groq integration
- Billing endpoints
- usage enforcement

### Data / Supabase
- schema changes
- RLS validation
- storage bucket logic
- vault hardening

### Product / UX
- copywriting
- roadmap alignment
- user stories
- acceptance validation

### QA / DevOps
- regression checks
- deployment validation
- smoke tests

---

## 10. Final summary

هذا الـ backlog يحول المتطلبات من فكرة منتج إلى خطة تنفيذ قابلة للتنفيذ من قبل فريق. الهدف الأساسي هو أن يكون المنتج في نهاية المراحل الأولى جاهزاً لمشهد SaaS محترف، وفي نفس الوقت يحافظ على القاعدة التقنية الحالية الموجودة في المشروع.
