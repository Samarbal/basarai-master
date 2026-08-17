# Technical Implementation Plan: Basar AI SaaS Upgrade

## 1. Overview

هذا الملف يحدد الخطة التقنية التنفيذية لتحويل المشروع الحالي إلى منتج SaaS جاهز للسوق. يتم بناء الخطة على الكود الحالي الموجود في المشروع، مع تركيز على:
- Frontend Next.js 14
- Backend FastAPI
- Supabase Auth + DB + Storage + Vault
- Template-first Generator redesign
- Groq text generation
- Billing and usage model

---

## 2. Current architecture baseline

### Existing repo modules
- Frontend routes: [frontend/app](frontend/app)
- Frontend hooks: [frontend/hooks](frontend/hooks)
- Frontend components: [frontend/components](frontend/components)
- Backend routes: [backend/app/routers](backend/app/routers)
- Backend services: [backend/app/services](backend/app/services)
- Data models: [backend/app/models](backend/app/models)
- Supabase migrations: [supabase/migrations](supabase/migrations)

### Strong foundational elements already present
- brand creation and management
- auth via Supabase
- keys management via Vault
- generation pipeline with AI services
- storage for images and logos
- admin stats view

### Main upgrade goals
- redesign product experience
- add template-driven generation
- add Groq caption generation
- add billing / plan model
- add landing page and SaaS polish

---

## 3. Target architecture

### 3.1 Frontend target structure
```text
frontend/
  app/
    page.tsx                    # landing page
    (auth)/
      login/page.tsx
      signup/page.tsx
    (dashboard)/
      brands/page.tsx
      [brandId]/
        page.tsx               # smart generator
        kit/page.tsx
        keys/page.tsx
        history/page.tsx
        settings/page.tsx
      admin/page.tsx
  components/
    marketing/
    auth/
    generation/
      template-selector.tsx
      skill-selector.tsx
      caption-panel.tsx
      generator-form.tsx
    billing/
    brand/
    ui/
  hooks/
    use-template-catalog.ts
    use-plan.ts
    use-usage.ts
    use-generate.ts
    use-keys.ts
    use-profile.ts
  lib/
    api.ts
    supabase/
    pricing.ts
    templates.ts
```

### 3.2 Backend target structure
```text
backend/
  app/
    routers/
      landing.py            # optional marketing data endpoint
      plans.py              # billing plans
      usage.py              # quota tracking
      templates.py          # template catalog
      generation_v2.py      # hybrid generation flow
      groq.py               # caption generation integration
    services/
      groq_service.py
      template_engine.py
      prompt_builder_v2.py
      usage_service.py
      billing_service.py
      generation_orchestrator.py
    models/
      plan.py
      usage.py
      template.py
      generation_v2.py
```

---

## 4. Technical changes by layer

## 4.1 Frontend changes

### Feature 1: Landing page
**Files to edit/create**
- [frontend/app/page.tsx](frontend/app/page.tsx)
- create `frontend/components/marketing/` UI blocks
- optional `frontend/lib/pricing.ts`

**Requirements**
- modern SaaS landing page
- pricing cards
- CTA buttons
- social proof section
- feature blocks

**Acceptance**
- root route renders polished landing page
- CTA routes work

### Feature 2: Login/Signup redesign
**Files to edit**
- [frontend/app/(auth)/login/page.tsx](frontend/app/(auth)/login/page.tsx)
- [frontend/app/(auth)/signup/page.tsx](frontend/app/(auth)/signup/page.tsx)

**Requirements**
- two-mode choice UI
- modern card layout
- distinction between BYOK and subscription onboarding

### Feature 3: Generator redesign
**Files to edit**
- [frontend/components/generation/generator-form.tsx](frontend/components/generation/generator-form.tsx)
- add new components under `frontend/components/generation/`

**New UX structure**
1. Template selector
2. Skill selector
3. Platform preset selector
4. Brand tone and audience selector
5. Copy generation toggle
6. Preview panel
7. Submit button

### Feature 4: Smart template UI
**Files to create**
- `frontend/components/generation/template-selector.tsx`
- `frontend/components/generation/skill-selector.tsx`
- `frontend/lib/templates.ts`

**Goal**
- template metadata and smart prompt mapping displayed to user

---

## 4.2 Backend changes

### Feature 1: Template catalog API
**New module**
- `backend/app/routers/templates.py`
- `backend/app/models/template.py`

**Purpose**
- expose available templates to frontend
- include fields: id, name, category, platform, skill, prompt, description

**Example response**
```json
{
  "templates": [
    {
      "id": "product-ad",
      "name": "إعلان منتج",
      "category": "marketing",
      "platform": "instagram_post",
      "skill": "conversion",
      "description": "إعلان تسويقي لمنتج جديد",
      "prompt_template": "Create a premium product ad ..."
    }
  ]
}
```

### Feature 2: Generation V2 / Hybrid engine
**Files to modify**
- [backend/app/routers/generations.py](backend/app/routers/generations.py)
- [backend/app/models/generation.py](backend/app/models/generation.py)
- [backend/app/services/prompt_composer.py](backend/app/services/prompt_composer.py)

**New fields in request**
- `template_id`
- `skill`
- `tone`
- `platform_preset`
- `brand_kit_context`
- `caption_mode`
- `include_brand_text`
- `use_groq_copy`

**New backend orchestration flow**
1. Validate user authorization
2. Validate template exists and is allowed
3. Build structured prompt
4. Generate image
5. Generate caption via Groq if enabled
6. Compose final output model
7. Save generation metadata

### Feature 3: Groq service integration
**Files to create**
- `backend/app/services/groq_service.py`
- `backend/app/routers/groq.py`

**Purpose**
- generate short caption, headline, CTA
- provide `copy` support alongside final image

**API contract**
```json
{
  "brand_name": "Basar",
  "tone": "professional",
  "platform": "instagram_post",
  "prompt": "Product launch announcement ...",
  "template": "product-ad"
}
```

**Response**
```json
{
  "headline": "اكتشف تجربة جديدة",
  "caption": "انضم الآن ...",
  "cta": "اطلب الآن",
  "hashtags": ["#brand", "#launch"]
}
```

### Feature 4: Billing and plan model
**Files to create**
- `backend/app/models/plan.py`
- `backend/app/models/usage.py`
- `backend/app/routers/plans.py`
- `backend/app/routers/usage.py`
- `backend/app/services/usage_service.py`

**Database additions**
- `plans`
- `subscriptions`
- `usage_events`
- `usage_limits`

**Plan model**
- free
- starter
- pro
- business

**Quota rules**
- images per month
- caption generations per month
- active brands
- API provider quota handling

### Feature 5: Security and key mode
**Files to modify**
- [backend/app/core/auth.py](backend/app/core/auth.py)
- [backend/app/routers/keys.py](backend/app/routers/keys.py)

**Requirements**
- maintain BYOK flow
- add subscription path with backend gating
- ensure user cannot exceed plan quotas

---

## 4.3 Database/schema changes

### New tables needed

#### `plans`
```sql
CREATE TABLE plans (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  code TEXT NOT NULL UNIQUE,
  name TEXT NOT NULL,
  price_cents INTEGER NOT NULL,
  currency TEXT NOT NULL DEFAULT 'SAR',
  image_limit INTEGER,
  caption_limit INTEGER,
  enabled BOOLEAN NOT NULL DEFAULT TRUE,
  created_at TIMESTAMPTZ DEFAULT NOW()
);
```

#### `subscriptions`
```sql
CREATE TABLE subscriptions (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES auth.users(id),
  plan_id UUID REFERENCES plans(id),
  status TEXT NOT NULL CHECK (status IN ('trial','active','past_due','cancelled')),
  started_at TIMESTAMPTZ DEFAULT NOW(),
  ends_at TIMESTAMPTZ,
  created_at TIMESTAMPTZ DEFAULT NOW()
);
```

#### `usage_events`
```sql
CREATE TABLE usage_events (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES auth.users(id),
  brand_id UUID REFERENCES brands(id),
  event_type TEXT NOT NULL,
  quantity INTEGER NOT NULL DEFAULT 1,
  created_at TIMESTAMPTZ DEFAULT NOW()
);
```

### RLS updates
- `plans` can be read publicly for display
- `subscriptions` restricted to owner
- `usage_events` restricted to owner

---

## 5. API endpoint design

### Existing endpoints to evolve
- `POST /brands/{brand_id}/generate`
- `GET /brands/{brand_id}/generations`
- `GET /brands/{brand_id}/keys`
- `PATCH /me`

### New endpoints to add

#### `GET /templates`
- returns all available templates

#### `GET /plans`
- returns active plans

#### `POST /plans/select`
- selects user plan

#### `GET /usage`
- returns usage quota state

#### `POST /generate/hybrid`
- accepts template + skill + prompt + provider + copy mode

#### `POST /generate/caption`
- generates only copy/caption based on user prompt and brand kit

---

## 6. Implementation phases

## Phase 1: Product UX & onboarding
**Deliverables**
- Landing page
- auth redesign
- plan selection flow
- route protection refinement

**Duration**
2-3 weeks

**Team**
- Frontend: 1 engineer
- UX: 1 designer
- Backend: 0.5 engineer for auth/session logic

---

## Phase 2: Template & Generator redesign
**Deliverables**
- template catalog
- skill mapping
- template-first generator UI
- backend structured prompt builder

**Duration**
2-3 weeks

**Team**
- Frontend: 1 engineer
- Backend: 1 engineer
- AI engineer: 0.5

---

## Phase 3: Hybrid AI + Groq output
**Deliverables**
- Groq copy generation
- caption + CTA creation
- hybrid generation orchestrator
- output metadata response

**Duration**
2 weeks

**Team**
- Backend: 1 engineer
- AI: 1 engineer
- Frontend: 0.5 engineer

---

## Phase 4: Subscription & Billing
**Deliverables**
- plans and subscriptions schema
- usage tracking
- quota enforcement
- payment integration readiness

**Duration**
2-3 weeks

**Team**
- Backend: 1 engineer
- Product: 1 owner
- QA: 0.5

---

## Phase 5: QA, Security, Launch Prep
**Deliverables**
- testing suite updates
- security review
- regression validation
- deployment checklist

**Duration**
1-2 weeks

**Team**
- QA: 1
- Backend: 1
- DevOps: 0.5

---

## 7. Development tasks per role

## Frontend tasks
- build landing page marketing UI
- redesign login/signup auth views
- build template selector UI
- update generator form to skills + templates
- display generated caption + CTA panel
- add pricing cards and plan selection UI
- ensure routing and protected route behavior works

## Backend tasks
- add template API endpoints
- add plans API endpoints
- implement usage tracking service
- expand generation pipeline to support hybrid output
- add Groq service integration
- validate provider keys and usage quotas
- add new Pydantic response models

## Supabase / Data tasks
- add plans and subscriptions tables
- add usage_events
- update RLS policies
- ensure Vault secret access is restricted
- validate storage policy behavior

## AI / Prompt tasks
- define template catalog
- create mapping between templates and prompt structure
- design caption generation logic
- quality evaluation of outputs

## QA tasks
- backend regression suite
- frontend smoke tests
- plan and quota tests
- auth flow test
- generation failure handling validation

---

## 8. Risks and mitigations

### Risk 1: Generation quality inconsistency
**Mitigation**: use template + brand kit + prompt engineering + platform-specific guidance.

### Risk 2: Cost of AI usage
**Mitigation**: enforce quotas and separate BYOK vs subscription plan.

### Risk 3: Unclear pricing model
**Mitigation**: define a small set of clear plans before implementation.

### Risk 4: Security of provider keys
**Mitigation**: maintain Vault-based storage and strict RLS protections.

### Risk 5: UI complexity
**Mitigation**: start with template + vibe selection, then expand later.

---

## 9. Definition of Done for this release

The release is ready when:
- landing page is implemented
- auth is redesigned and functional
- generator is template-first and not free-form only
- Groq caption generation works or has fallback
- user can choose BYOK or platform subscription
- base billing and usage tracking exists
- all critical flows pass QA
- deployment checklist is signed off

---

## 10. Recommended initial execution order

1. Landing page and auth redesign
2. Template library + generator redesign
3. Groq caption generation
4. usage plan model
5. billing readiness
6. security QA and release

---

## 11. Final recommendation

The repo already contains a strong technical core, so the safest path is not a full rewrite. We should keep the current architecture and gradually layer in:
- SaaS landing experience
- template-first generation
- smart content engine
- usage and billing model
- stronger visual UI and product experience

This approach reduces risk and keeps the engineering effort aligned with the existing codebase.
