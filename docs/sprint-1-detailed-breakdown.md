# Sprint 1 - Detailed Breakdown: Product & Brand Experience

## 📋 Overview

**Duration:** 1 week (5 working days)  
**Goal:** Launch minimal polished SaaS entry experience with landing page and modern auth  
**Key Deliverables:** Landing page MVP, modern auth flow, BYOK option, session management

---

## 1. Sprint Goals

- [ ] Launch MVP landing page at `/` (Hero + CTA)
- [ ] Redesign login/signup with two-mode choice UI
- [ ] Implement session management and redirects
- [ ] API-key (BYOK) basic onboarding ready
- [ ] Backend auth endpoints validated
- [ ] Ready for UAT testing

---

## 2. Sprint 1 Tickets & User Stories

### Story US-01: Landing Page for SaaS Product

**Issue ID:** TICKET-01  
**Priority:** P0  
**Estimation:** 8 story points (2-3 days)  
**Status:** Not started

#### Acceptance Criteria
1. Root route `/` renders landing page (MVP)
2. Hero section with: title, value prop, primary CTA
3. Single features section (2-3 key features)
4. Pricing section (simplified - link to pricing page)
5. Footer with basic links
6. Responsive on mobile and desktop
7. CTAs route to auth pages

#### Deliverables

**Figma Design** (UX/Designer - 0.5 day)
- [ ] Quick design mockup (Hero + CTA + Footer)
- [ ] Component alignment with existing UI
- [ ] Responsive layout (mobile + desktop)
- [ ] Share design mockup

**Frontend Implementation** (Frontend Engineer - 2-2.5 days)
1. Create `frontend/app/page.tsx` (root landing page)
   - Layout: Hero + 2 Features + CTA + Footer
   - Use existing shadcn/ui components
   - Tailwind styling
   - Mobile responsive

2. Create minimal components in `frontend/components/marketing/`:
   - `HeroSection.tsx` - Hero with title and CTA
   - `FooterSection.tsx` - Footer

3. Navigation & Redirect Logic
   - If user logged in, redirect to `/brands`
   - CTA buttons route to `/auth/signup`
   - Pricing link to static pricing page (can be separate sprint)

#### Testing Acceptance
- [ ] Navigate to `/` and see landing page
- [ ] CTAs route to correct auth pages
- [ ] Logged-in users redirected to dashboard
- [ ] Mobile view is responsive
- [ ] Lighthouse score >= 85

#### Completion Criteria
- Code merged to `main`
- Figma design finalized
- Screenshot in Confluence/Notion
- Ready for UAT

---

### Story US-02: Auth Experience with Usage Mode Selection

**Issue ID:** TICKET-02  
**Priority:** P0  
**Estimation:** 8 story points (2-3 days)  
**Status:** Not started

#### Acceptance Criteria
1. Login/signup screen shows two options: BYOK vs Subscription
2. Basic styling for both paths
3. BYOK path accepts email/password
4. Subscription path accepts email/password
5. Form validation for email and password
6. Error messages display
7. Mobile responsive

#### Deliverables

**Design** (UX/Designer - 0.5 day)
- [ ] Quick auth page mockup with two buttons
- [ ] Mode selection layout
- [ ] Error state mockup

**Frontend Auth Pages** (Frontend Engineer - 2.5 days)

1. Update `frontend/app/(auth)/layout.tsx`
   - Clean centered card layout
   - Basic styling

2. Update `frontend/app/(auth)/signup/page.tsx`
   - Mode selection buttons (BYOK | Subscription)
   - Email + password fields
   - Basic form validation
   - Error message display
   - Submit button with loading state

3. Existing login endpoint
   - Reuse existing login logic
   - Add session persistence

4. Hook integration
   - Use existing `useAuth()` hook or extend it
   - Call `POST /auth/signup` with mode
   - Store JWT in session
   - Handle basic errors

#### Backend Auth Updates (Backend Engineer - 3 days)
1. Ensure existing auth endpoints are functional:
   - `POST /auth/signup` - creates user + profile
   - `POST /auth/login` - returns JWT
   - `GET /auth/me` - returns current user

2. Add new fields to signup payload:
   ```python
   class SignupRequest(BaseModel):
       email: str
       password: str
       mode: Literal["byok", "subscription"]
       plan_id: Optional[str] = None  # Only for subscription mode
   ```

3. Update profile creation logic:
   ```python
   # After signup, create profile with mode preference
   profile = {
       "user_id": user_id,
       "mode": "byok" | "subscription",
       "selected_plan": plan_id or None,
       "created_at": now()
   }
   ```

4. Add validation:
   - Email uniqueness check
   - Password strength validation
   - Plan exists check if subscription mode

#### Testing Acceptance
- [ ] Signup form with BYOK mode works
- [ ] Signup form with Subscription mode works
- [ ] Form validation catches invalid inputs
- [ ] Login works with valid credentials
- [ ] Error messages are clear
- [ ] Session persists after login
- [ ] Logged-in users cannot access auth pages
- [ ] Mobile form is usable

#### Completion Criteria
- Frontend auth pages completed and styled
- Backend auth endpoints validated
- Session management working
- All form validations pass
- Merged to `main`

---

### Story US-03: Redirect & Session Flow

**Issue ID:** TICKET-03  
**Priority:** P0  
**Estimation:** 5 story points (1.5-2 days)  
**Status:** Not started

#### Acceptance Criteria
1. Authenticated users accessing `/` redirect to `/brands`
2. Unauthenticated users accessing `/brands` redirect to `/auth/login`
3. Session persists on page reload
4. Logout clears session and redirects to `/`

#### Deliverables

**Middleware & Route Protection** (Frontend Engineer - 1.5 days)

1. Update `frontend/middleware.ts`:
   ```typescript
   // Basic redirects:
   // - No session + accessing /brands → /auth/signup
   // - Session + accessing / → /brands
   ```

2. Update `frontend/app/(auth)/layout.tsx`:
   - Redirect authenticated users to /brands

3. Add logout in `frontend/app/(dashboard)/layout.tsx`:
   - Logout button in header

4. Use existing `use-profile()` hook:
   - Verify it gets user from session
   - Add logout method

5. Backend JWT validation:
   - Ensure existing JWT validation works
   - Return 401 for invalid tokens

#### Testing Acceptance
- [ ] New user starts at `/` and sees landing page
- [ ] User clicks signup and goes to auth
- [ ] After signup, redirected to `/brands`
- [ ] Refreshing page maintains session
- [ ] Logout clears session and goes to `/`
- [ ] Accessing protected route without auth redirects to login
- [ ] Admin email gets `/admin` access
- [ ] Non-admin accessing `/admin` is blocked

#### Completion Criteria
- Middleware fully implemented
- All redirects work correctly
- Session persists and validates
- Logout flow working
- Merged to `main`

---

### Story US-04: Brand CRUD UX Improvements

**Issue ID:** TICKET-04  
**Priority:** P1 (Post-launch iteration)  
**Estimation:** Moved to Sprint 2  
**Status:** Not started

#### Note
Brand dashboard UX improvements moved to Sprint 2 to fit 1-week timeline. Focus Sprint 1 on auth and landing page.

#### Deliverables

**Design** (UX/Designer - 2 days)
- [ ] Brand dashboard layout mockup
- [ ] Brand card design with placeholder logo
- [ ] Action menu design
- [ ] Empty state design
- [ ] Mobile responsive mockup

**Frontend Brand Dashboard** (Frontend Engineer - 4 days)

1. Update `frontend/app/(dashboard)/brands/page.tsx`:
   ```typescript
   // Main brands listing page
   // Components:
   // - Page header with "My Brands" title and "New Brand" button
   // - BrandGrid component
   // - Empty state (if no brands)
   // - Loading skeleton
   ```

2. Create `frontend/components/brand/BrandCard.tsx`:
   ```typescript
   interface BrandCardProps {
     brand: Brand;
     onEdit: (brandId: string) => void;
     onDelete: (brandId: string) => void;
     onView: (brandId: string) => void;
   }
   
   // Displays:
   // - Brand logo (or placeholder)
   // - Brand name
   // - Creation date
   // - Brand status (complete/incomplete)
   // - Action menu (Edit, Delete, View)
   // - Hover effects
   ```

3. Create `frontend/components/brand/BrandGrid.tsx`:
   ```typescript
   // Grid layout of brand cards
   // Responsive: 1 col on mobile, 2 on tablet, 3+ on desktop
   // Add/Remove animation
   ```

4. Create `frontend/components/brand/CreateBrandModal.tsx`:
   ```typescript
   // Modal for creating new brand
   // Fields:
   // - Brand name (required)
   // - Submit button
   // - Cancel button
   // - Loading state
   // - Error display
   ```

5. Update `frontend/hooks/use-brands.ts`:
   ```typescript
   // Already exists, ensure it has:
   // - useBrands() - fetch all brands
   // - useCreateBrand() - create new brand
   // - useDeleteBrand() - delete brand
   // - useUpdateBrand() - update brand
   // - Loading, error, data states
   ```

6. Styling using Tailwind CSS
   - Consistent spacing and colors
   - Smooth transitions
   - Proper focus states

#### Backend Brand Endpoints Validation (Backend Engineer - 2 days)

1. Validate existing endpoints work:
   - `GET /brands` - list user's brands
   - `POST /brands` - create new brand
   - `GET /brands/{brand_id}` - get single brand
   - `PATCH /brands/{brand_id}` - update brand
   - `DELETE /brands/{brand_id}` - delete brand

2. Ensure response format is consistent:
   ```json
   {
     "id": "uuid",
     "user_id": "uuid",
     "name": "Brand Name",
     "logo_url": "https://...",
     "status": "complete|incomplete",
     "created_at": "2026-08-18T...",
     "updated_at": "2026-08-18T..."
   }
   ```

3. Add necessary fields if missing:
   - `status` field (complete/incomplete based on kit)
   - `created_at` timestamp

#### Testing Acceptance
- [ ] Brand listing page loads and displays brands
- [ ] Create brand modal works
- [ ] Brand card shows all required info
- [ ] Delete confirmation dialog appears
- [ ] Brand delete works and card is removed
- [ ] Empty state shows when no brands
- [ ] Mobile layout is responsive
- [ ] Loading states display correctly

#### Completion Criteria
- Brand dashboard fully redesigned and functional
- All CRUD operations working
- UX matches design mockup
- Mobile responsive
- Merged to `main`

---

## 3. Team Breakdown & Assignments (1 Week)

### Total Team Size: 4-5 people (focused & lean)

| Role | Person | Allocation | Main Tasks |
|------|--------|-----------|-----------|
| **UX/Design** | Designer 1 | 100% | Landing page MVP + Auth UI design (Day 1-2) |
| **Frontend Engineer** | FE 1 | 100% | Landing page + Auth pages + Session logic (Day 1-5) |
| **Backend Engineer** | BE 1 | 100% | Auth endpoints + Session validation (Day 1-5) |
| **QA/Testing** | QA 1 | 80% | Functional testing + UAT prep (Day 3-5) |
| **Product Manager** | PM 1 | 40% | Daily standups + acceptance testing (async) |

---

## 4. Detailed Task Breakdown by Role (1 Week - 5 Days)

### 4.1 UX/Designer Tasks (Designer 1) - Days 1-2 (1 day per ticket)

**Day 1: Landing Page Design (0.5 day)**
- [ ] Review requirements with PM
- [ ] Quick sketch: Hero + CTA + Footer
- [ ] High-fi mockup in Figma
- [ ] Component specs documented
- [ ] Share with FE team

**Day 2: Auth Page Design (0.5 day)**
- [ ] Design signup page with mode buttons
- [ ] Design error states
- [ ] Component alignment
- [ ] Share with FE team

**Days 3-5: Design Support & QA**
- [ ] Review FE implementation daily
- [ ] Quick feedback turnarounds
- [ ] Responsive validation
- [ ] Polish tweaks

**Deliverables:**
- Figma file with 2 main screens
- Design specs document (1 page)

**Success Criteria:**
- Designs ready by EOD Day 2
- Mobile + desktop responsive
- FE can implement without waiting

---

### 4.2 Frontend Engineer Tasks (FE 1) - Days 1-5

**Day 1: Setup & Landing Page Start**
- [ ] Review design mockups
- [ ] Create folder structure (`components/marketing/`)
- [ ] Start `frontend/app/page.tsx`
- [ ] Create `HeroSection.tsx` and `FooterSection.tsx`
- [ ] Add Tailwind styling
- [ ] Add redirect logic for logged-in users

**Day 2: Landing Page Completion**
- [ ] Finish landing page styling
- [ ] Test responsive design (mobile + desktop)
- [ ] Fix layout issues
- [ ] Merge landing page code

**Day 3: Auth Pages - Setup**
- [ ] Create auth layout (`frontend/app/(auth)/layout.tsx`)
- [ ] Create basic signup form
- [ ] Add mode selection buttons (BYOK | Subscription)
- [ ] Add form fields (email, password)

**Day 4: Auth Pages - Completion**
- [ ] Add form validation (email, password)
- [ ] Add error display
- [ ] Add loading states
- [ ] Test form submission
- [ ] Mobile responsive

**Day 5: Session & Integration**
- [ ] Implement middleware redirects
- [ ] Add logout button
- [ ] Test end-to-end flow
- [ ] Fix any bugs
- [ ] Final testing & merge

**Deliverables:**
- Landing page fully implemented
- Auth pages with two modes
- Session redirect logic
- All responsive

**Success Criteria:**
- Landing page renders correctly
- Auth forms validate and submit
- Redirects work (/ → /brands for auth users)
- Mobile responsive
- No console errors

---

### 4.3 Backend Engineer Tasks (BE 1) - Days 1-5

**Day 1: Audit & Planning**
- [ ] Review existing auth implementation
- [ ] Check `POST /auth/signup` endpoint
- [ ] Check `POST /auth/login` endpoint
- [ ] Document current behavior
- [ ] Plan changes needed

**Day 2-3: Auth Endpoint Updates**
- [ ] Add `mode` field to signup (byok/subscription)
- [ ] Validate `mode` in request
- [ ] Update profile creation with mode
- [ ] Test with Postman
- [ ] Document new payload schema

**Day 4: Session & Token Validation**
- [ ] Verify JWT validation on protected routes
- [ ] Ensure 401 errors work correctly
- [ ] Test token expiration
- [ ] Validate session persistence
- [ ] Write simple unit tests

**Day 5: Testing & Documentation**
- [ ] Test all auth flows end-to-end
- [ ] Document API changes
- [ ] Create Postman collection
- [ ] Prepare test data for QA
- [ ] Final code review

**Deliverables:**
- Auth endpoints working with mode field
- API documentation
- Test scenarios for QA
- Postman collection

**Success Criteria:**
- Signup/login work correctly
- Mode field properly stored
- JWT validation working
- 401 errors returned correctly
- Ready for FE integration

**Testing with Postman/cURL:**
```bash
# Signup
POST http://localhost:8000/auth/signup
{
  "email": "test@example.com",
  "password": "SecurePass123",
  "mode": "byok",
  "plan_id": null
}

# Get brands
GET http://localhost:8000/brands
Headers: Authorization: Bearer <token>

# Create brand
POST http://localhost:8000/brands
{
  "name": "My Brand"
}
```

---

### 4.4 QA/Testing Tasks (QA 1) - Days 3-5 (starts mid-sprint)

**Day 1-2: Preparation (async)**
- [ ] Review requirements and AC
- [ ] Prepare test cases document
- [ ] Setup test environment
- [ ] Create test data

**Day 3: API & Landing Page Testing**
- [ ] Test landing page on mobile/desktop
- [ ] Test CTA buttons
- [ ] Test auth signup with both modes
- [ ] Test form validation
- [ ] Test error messages

**Day 4: Integration Testing**
- [ ] Test end-to-end signup flow
- [ ] Test session persistence
- [ ] Test redirect logic (/ → /brands for auth users)
- [ ] Test logout
- [ ] Test on different browsers

**Day 5: Final Testing & UAT Prep**
- [ ] Regression testing on existing features
- [ ] Document all issues found
- [ ] UAT test case preparation
- [ ] Create UAT scenarios
- [ ] Sign-off on readiness

**Test Cases (Critical Only):**

| Test ID | Description | Expected | Status |
|---------|-------------|----------|--------|
| TC-01 | Landing page loads | Page displays with hero and CTA | [ ] |
| TC-02 | CTA routes to auth | Click CTA → /auth/signup | [ ] |
| TC-03 | Signup BYOK mode | Create account, redirected to /brands | [ ] |
| TC-04 | Signup Subscription mode | Create account, redirected to /brands | [ ] |
| TC-05 | Form validation | Invalid email shows error | [ ] |
| TC-06 | Session persists | Refresh page, user still logged in | [ ] |
| TC-07 | Logout works | Click logout, redirected to / | [ ] |
| TC-08 | Mobile responsive | Page works on 375px width | [ ] |

**Deliverables:**
- Test cases document (critical paths only)
- Bug report (if any)
- UAT scenarios
- Go/No-go recommendation

**Success Criteria:**
- All critical tests pass
- < 2 critical bugs
- Mobile responsive verified
- Ready for UAT

---

### 4.5 Product Manager Tasks (PM 1) - Days 1-5 (async/parallel)

**Day 1: Kickoff**
- [ ] Brief team on goals and AC
- [ ] Answer questions
- [ ] Confirm priorities

**Days 2-4: Daily Standups & Support**
- [ ] 15-min daily standup with team
- [ ] Remove blockers (decisions, clarifications)
- [ ] Support FE/BE with questions
- [ ] Track progress

**Day 5: Acceptance & Launch Prep**
- [ ] Verify all AC are met
- [ ] Test critical user flows
- [ ] Approve for release
- [ ] Create release notes (simple 1-pager)
- [ ] Prepare Sprint 2 backlog

**Deliverables:**
- Daily standup notes
- Release notes (1 page)
- Sprint 2 backlog document
- Go/No-go decision

**Success Criteria:**
- All stories accepted
- No critical blockers
- Team aligned on Sprint 2
- Ready to launch

---

### 4.6 DevOps/Infra Tasks (DevOps - minimal for 1-week sprint)

**Day 1: Environment Check**
- [ ] Verify dev environment is ready
- [ ] Verify Supabase connection
- [ ] Verify Docker setup

**Day 5: Deployment Readiness**
- [ ] Deploy to staging (if available)
- [ ] Verify all endpoints work
- [ ] Prepare production deployment checklist
- [ ] Document any environment changes

**Deliverables:**
- Deployment checklist
- Environment validation report

---

## 5. Daily Standup Template

**Duration:** 15 minutes  
**Format:**
- What did I complete yesterday?
- What will I work on today?
- Any blockers or help needed?

---

## 6. Definition of Done for Sprint 1

A ticket is **Done** when:
- ✅ Code is implemented and merged
- ✅ Tests pass (critical paths only for 1-week sprint)
- ✅ AC are verified
- ✅ No console errors or warnings
- ✅ Mobile responsive tested
- ✅ Code review approved

**Sprint 1 is complete when:**
- ✅ Landing page MVP is live
- ✅ Auth flow works with both modes
- ✅ Session management works
- ✅ QA has approved with < 2 critical bugs
- ✅ All code merged to main
- ✅ Ready for production deploy

---

## 7. Risk Mitigation

| Risk | Mitigation |
|------|-----------|
| Design takes too long | Designer starts Day 1 at 6 AM, FE reviews WIP mockup |
| Auth complexity | BE starts Day 1, pair with FE on integration |
| Scope creep | PM gates all requests until after Friday |
| Late bugs found | QA tests starting Day 3, early feedback |

---

## 8. Success Metrics (1-Week Sprint)

| Metric | Target | Owner |
|--------|--------|-------|
| Story completion | 100% | PM |
| Code merged | All by EOD Friday | Tech Lead |
| QA approval | < 2 critical bugs | QA |
| Mobile responsive | All pages tested | FE |
| Lighthouse score | >= 85 | FE |

---

## 9. Communication Plan (1-Week)

### Daily
- **Standup:** 9:00 AM (15 min) - what done, what next, blockers

### Channels
- **Slack:** #basar-sprint-1 (quick questions)
- **GitHub:** PR comments (code review)
- **Confluence:** Backlog updates

### Key Dates
- **Monday 9 AM:** Sprint kickoff
- **Friday 4 PM:** Sprint review + demo

---

## 10. Key Dependencies

1. **Design → FE:** Mockups ready by EOD Day 2
2. **FE ↔ BE:** Sync on API contracts by Day 2
3. **BE → FE:** API endpoints ready by Day 3
4. **FE + BE → QA:** Ready for testing by Day 3

---

## 11. Sprint Success Definition

✅ Sprint 1 is successful when:
- Landing page MVP is live (/dashboard accessible to logged-in users)
- Auth works with BYOK and Subscription mode selection
- Session persists and redirects work correctly
- QA approves with < 2 critical bugs
- All code merged to main
- Team ready to start Sprint 2 Monday

---

## 12. 5-Week Development Roadmap

The full product transformation is planned across 5 sequential 1-week sprints:

### **Sprint 1: Foundation & Auth (Week 1)**
- ✅ Landing page MVP
- ✅ Modern auth flow (BYOK vs Subscription)
- ✅ Session management
- **Deliverable:** Public landing + auth system ready

### **Sprint 2: Template System (Week 2)**
- Template library API & UI
- Skill/category mapping
- Smart prompt builder
- Template selection page
- **Deliverable:** Template-driven generation ready

### **Sprint 3: Hybrid Generation Engine (Week 3)**
- Generator V2 redesign (template-first UI)
- Groq caption generation service
- Image + caption unified output
- Result history and download
- **Deliverable:** Full generation workflow

### **Sprint 4: Billing & Usage Control (Week 4)**
- Plan model & storage
- Usage tracking service
- Quota enforcement
- Billing endpoints (ready for payment integration)
- **Deliverable:** Plan gating and usage limits work

### **Sprint 5: Polish & Launch (Week 5)**
- Admin analytics refinement
- Full regression testing
- Security audit
- Performance optimization
- Production deployment
- **Deliverable:** Production-ready SaaS platform

---

## 13. File Structure After Sprint 1

### Frontend Changes
```
frontend/app/
├── page.tsx                    # NEW: Landing page
├── (auth)/
│   ├── layout.tsx              # UPDATED: Auth layout
│   ├── login/page.tsx
│   └── signup/page.tsx         # UPDATED: Mode selection
├── (dashboard)/
│   └── brands/page.tsx         # Uses existing brands list
└── middleware.ts               # NEW: Redirect logic

frontend/components/
├── marketing/                  # NEW folder
│   ├── HeroSection.tsx
│   └── FooterSection.tsx
└── ...existing components
```

### Backend Changes
```
backend/app/
├── models/
│   └── auth.py                 # UPDATED: Add mode field
├── routers/
│   └── auth.py                 # UPDATED: Add mode to signup
└── core/
    └── auth.py                 # VALIDATED: JWT logic
```
