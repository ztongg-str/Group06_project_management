# Scrum Plan — UniPulse

## Product Vision

UniPulse is a Campus Event Management Platform that enables students to discover, register, and attend events while organizers and admins manage the ecosystem. This Scrum plan covers **8 weeks (4 Sprints)** to harden security, add testing, set up CI/CD, and deliver a production-ready release.

---

## Scrum Team

| Role | Responsibility |
|------|---------------|
| **Product Owner** | Prioritizes backlog, accepts/rejects sprint work, defines release criteria |
| **Scrum Master** | Facilitates ceremonies, removes blockers, ensures Scrum process is followed |
| **Developers (2-3)** | Backend, frontend, and full-stack implementation |
| **QA / Tester** | Writes test cases, performs manual and automated testing |

---

## Scrum Ceremonies

| Ceremony | Frequency | Duration | Purpose |
|----------|-----------|----------|---------|
| **Sprint Planning** | Day 1 of each sprint | 1-2 hours | Select backlog items, break into tasks, estimate |
| **Daily Standup** | Every weekday | 15 min | Sync: what I did, what I'll do, blockers |
| **Sprint Review** | Last day of sprint | 1 hour | Demo completed work to stakeholders |
| **Sprint Retrospective** | Last day of sprint | 45 min | What went well, what to improve, action items |
| **Backlog Refinement** | Mid-sprint | 30-60 min | Clarify and reprioritize upcoming stories |

---

## Story Point Reference

| Points | Effort |
|--------|--------|
| 1 | Trivial (config, small fix) |
| 2 | Small (single function, simple UI) |
| 3 | Medium (multi-file change, new endpoint) |
| 5 | Large (new feature, cross-cutting concern) |
| 8 | Very large (must be split if possible) |

---

## Product Backlog

### Priority: P0 — Critical (Must be done before any release)

| ID | Story | Points |
|----|-------|--------|
| US-01 | Remove hardcoded JWT secret fallback — require `JWT_SECRET` at startup | 2 |
| US-02 | Remove hardcoded crypto key fallback — require `CRYPTO_KEY` at startup | 1 |
| US-03 | Add `helmet` middleware for security headers (CSP, HSTS, X-Frame-Options) | 2 |
| US-04 | Restrict CORS to specific frontend origin(s) | 1 |
| US-05 | Add rate limiting to auth endpoints (login, register, OTP) | 3 |
| US-06 | Sanitize error messages in production — no internal details leaked | 2 |
| US-07 | Fix `GET /api/users/` authorization to use `requirePermission` consistently | 2 |
| US-08 | Rate-limit OTP verify/reset attempts — max 5 per window | 2 |
| US-09 | Require password re-entry before role switching | 3 |
| US-10 | Restrict query console: SELECT-only for developer role; separate dangerous ops | 3 |
| US-11 | Remove SVG from allowed upload types OR serve with `Content-Disposition: attachment` | 1 |
| US-12 | Add request body size limits explicitly | 1 |
| US-13 | Generate a proper `.env.example` for the team (no real secrets) | 1 |

### Priority: P1 — High (Should be done for production)

| ID | Story | Points |
|----|-------|--------|
| US-14 | Add account lockout after 5 failed login attempts (or progressive delay) | 5 |
| US-15 | Add input sanitization middleware — strip HTML/Script from user fields | 3 |
| US-16 | Implement account lockout after failed login attempts | 3 |
| US-17 | Add CSRF protection if switching to cookie-based tokens | 3 |
| US-18 | Write unit tests for Auth module (register, login, OTP, JWT) | 5 |
| US-19 | Write unit tests for Users module (CRUD, role changes, RBAC) | 5 |
| US-20 | Write unit tests for Events module (CRUD, approval workflow) | 5 |
| US-21 | Write integration tests for full registration → event → attendance flow | 5 |
| US-22 | Set up GitHub Actions CI pipeline (lint, test on push) | 3 |
| US-23 | Add ESLint security rules to frontend (`eslint-plugin-security`) | 1 |
| US-24 | Fix username enumeration in registration error messages | 1 |

### Priority: P2 — Medium (Improves quality, not blocking release)

| ID | Story | Points |
|----|-------|--------|
| US-25 | Implement token refresh mechanism (short-lived JWT + refresh token) | 5 |
| US-26 | Add session management — view and revoke active sessions | 5 |
| US-27 | Enforce minimum password strength (8+ chars, complexity rules) | 2 |
| US-28 | Set up staging environment (separate DB, env vars) | 3 |
| US-29 | Add API documentation (Swagger/OpenAPI) | 5 |
| US-30 | Performance: add database query logging and optimize slow queries | 3 |
| US-31 | Add frontend component testing (React Testing Library) | 5 |
| US-32 | Encrypt database backups at rest | 3 |
| US-33 | Add HTTPS redirect middleware for production | 1 |

---

## Sprint Plan

### Sprint 1: Security Hardening Foundation (Weeks 1-2)

**Goal:** Eliminate all critical security vulnerabilities. No hardcoded secrets, basic protections in place.

**Stories:** US-01 through US-13

| Task | Assigned To | Est. Hours |
|------|-------------|------------|
| Remove JWT secret fallback; throw error if unset | Backend Dev | 1 |
| Remove crypto key fallback; throw error if unset | Backend Dev | 0.5 |
| Add `helmet` middleware with CSP config | Backend Dev | 2 |
| Configure CORS whitelist via env var | Backend Dev | 1 |
| Install `express-rate-limit`; apply to `/api/auth/*` routes | Backend Dev | 3 |
| Add rate limiter to OTP verify and reset routes | Backend Dev | 2 |
| Create production error sanitizer middleware | Backend Dev | 2 |
| Fix `GET /api/users/` route to use `requirePermission('manageUsers')` | Backend Dev | 1 |
| Add password re-entry to `switchRole` endpoint | Backend Dev | 3 |
| Restrict query console to SELECT-only for developer role | Backend Dev | 2 |
| Restrict query console — block all for non-superadmin | Backend Dev | 1 |
| Remove SVG from allowed upload types | Backend Dev | 0.5 |
| Add explicit JSON body size limits | Backend Dev | 0.5 |
| Create `.env.example` with placeholder values | Backend Dev | 1 |
| Update README with production deployment notes | Backend Dev | 1 |
| **Sprint Review:** Demo all security fixes, show rate limiter working | All | — |
| **Sprint Retro:** Discuss what went well, what to improve | All | — |

**Sprint Velocity Target:** 13 story points

---

### Sprint 2: Auth Hardening & Testing Foundation (Weeks 3-4)

**Goal:** Strengthen authentication flows and establish testing infrastructure.

**Stories:** US-14 through US-17, US-24

| Task | Assigned To | Est. Hours |
|------|-------------|------------|
| Implement failed login tracking in DB (or in-memory store) | Backend Dev | 3 |
| Add account lockout after 5 failed attempts (30 min cooldown) | Backend Dev | 3 |
| Add CAPTCHA trigger after 3 failed attempts (optional for now) | Backend Dev | 2 |
| Add input sanitization middleware (strip tags from string fields) | Backend Dev | 3 |
| Add XSS protection — sanitize event description, user bio | Backend Dev | 2 |
| Unify registration error messages (fix username enumeration) | Backend Dev | 1 |
| Install testing framework: `vitest` + `supertest` | Backend Dev | 1 |
| Write Auth unit tests: register validation, duplicate email, login success/fail | QA + Dev | 4 |
| Write Auth unit tests: JWT generation, verification, expiration | QA + Dev | 3 |
| Write Auth unit tests: OTP generation, verification, expiry | QA + Dev | 3 |
| Write Auth unit tests: role switch with password | QA + Dev | 2 |
| Add test coverage reporting (`c8` or `vitest --coverage`) | Backend Dev | 1 |
| **Sprint Review:** Demo account lockout, show test suite running | All | — |
| **Sprint Retro:** Review test coverage, discuss CI readiness | All | — |

**Sprint Velocity Target:** 17 story points

---

### Sprint 3: Test Coverage & CI/CD Pipeline (Weeks 5-6)

**Goal:** Full test coverage for core modules, automated CI pipeline running on every push.

**Stories:** US-18 through US-22 (overlaps with Sprint 2 testing)

| Task | Assigned To | Est. Hours |
|------|-------------|------------|
| Write Users module unit tests: getProfile, updateProfile | QA + Dev | 3 |
| Write Users module unit tests: admin CRUD, role change, status change | QA + Dev | 4 |
| Write Users module unit tests: RBAC permission checks | QA + Dev | 3 |
| Write Events module unit tests: create, edit, delete | QA + Dev | 3 |
| Write Events module unit tests: approval workflow, draft save | QA + Dev | 3 |
| Write Events module unit tests: authorization checks | QA + Dev | 2 |
| Write integration test: full student journey (register → login → browse → register event → check-in) | QA + Dev | 4 |
| Write integration test: organizer journey (create event → approve → check-in) | QA + Dev | 3 |
| Set up GitHub Actions workflow: lint + test on push/PR | Backend Dev | 3 |
| Add CI badge to README | Backend Dev | 0.5 |
| Configure ESLint security plugin for frontend | Frontend Dev | 1 |
| **Sprint Review:** Demo CI pipeline passing, show test coverage report | All | — |
| **Sprint Retro:** Discuss coverage gaps, CI reliability | All | — |

**Sprint Velocity Target:** 21 story points

---

### Sprint 4: Production Readiness & Polish (Weeks 7-8)

**Goal:** Production deployment ready. Staging environment, performance tuning, documentation.

**Stories:** US-25, US-27, US-28 through US-33

| Task | Assigned To | Est. Hours |
|------|-------------|------------|
| Set up staging environment (separate DB, env config) | Backend Dev | 3 |
| Implement HTTPS redirect middleware | Backend Dev | 1 |
| Add password strength validation (min 8 chars, complexity) | Backend Dev + Frontend Dev | 2 |
| Add API rate limiting for production (global limiter) | Backend Dev | 2 |
| Write Swagger/OpenAPI documentation for all endpoints | Backend Dev | 5 |
| Performance audit: log slow queries, add missing indexes | Backend Dev | 3 |
| Optimize N+1 queries in event listing, friend discovery | Backend Dev | 3 |
| Add frontend component tests (React Testing Library) | Frontend Dev | 5 |
| Encrypt database backups at rest | Backend Dev | 2 |
| Final security review — verify all P0/P1 items resolved | All | 2 |
| Production deployment checklist | All | 1 |
| **Sprint Review:** Full demo of production-ready app on staging | All | — |
| **Sprint Retro:** Final retro — lessons learned, future roadmap | All | — |

**Sprint Velocity Target:** 20 story points

---

## 8-Week Timeline

```
Week 1   Week 2   Week 3   Week 4   Week 5   Week 6   Week 7   Week 8
├────────┼────────┼────────┼────────┼────────┼────────┼────────┼────────┤
│  SPRINT 1         │  SPRINT 2         │  SPRINT 3         │  SPRINT 4         │
│  Security         │  Auth +           │  Testing +        │  Production       │
│  Foundation       │  Testing          │  CI/CD            │  Readiness        │
├────────┼────────┼────────┼────────┼────────┼────────┼────────┼────────┤
│Mon Plan│Mon Plan│Mon Plan│Mon Plan│Mon Plan│Mon Plan│Mon Plan│Mon Plan│
│Tue Dev │Tue Dev │Tue Dev │Tue Dev │Tue Dev │Tue Dev │Tue Dev │Tue Dev │
│Wed Dev │Wed Dev │Wed Dev │Wed Dev │Wed Dev │Wed Dev │Wed Dev │Wed Dev │
│Thu Dev │Thu Dev │Thu Dev │Thu Dev │Thu Dev │Thu Dev │Thu Dev │Thu Dev │
│Fri Dev │Fri Dev │Fri Dev │Fri Dev │Fri Dev │Fri Dev │Fri Dev │Fri Dev │
│        │        │        │        │        │        │        │        │
│        │Review  │        │Review  │        │Review  │        │Review  │
│        │Retro   │        │Retro   │        │Retro   │        │Retro   │
│        │Refine  │        │Refine  │        │Refine  │        │        │
└────────┴────────┴────────┴────────┴────────┴────────┴────────┴────────┘
```

### Weekly Breakdown

| Week | Sprint | Focus | Deliverable |
|------|--------|-------|-------------|
| **1** | Sprint 1 | Remove secrets, add helmet, rate limiting, CORS fix | Rate limiter on auth routes, security headers active |
| **2** | Sprint 1 | Query console restrictions, role switch auth, error sanitization | All P0 stories complete, Sprint 1 Review |
| **3** | Sprint 2 | Account lockout, input sanitization, test framework setup | Auth tests passing, lockout working |
| **4** | Sprint 2 | Auth test suite complete, XSS fixes, registration fixes | Sprint 2 Review, auth module fully tested |
| **5** | Sprint 3 | Users & Events unit tests, integration tests | Core modules tested, coverage >70% |
| **6** | Sprint 3 | CI/CD pipeline, frontend linting, final integration tests | Sprint 3 Review, CI green on main branch |
| **7** | Sprint 4 | Staging env, password strength, performance optimization | Staging deployed, API docs drafted |
| **8** | Sprint 4 | Backup encryption, final security review, deployment | Sprint 4 Review, production-ready release |

---

## Definition of Done (DoD)

A story is considered **Done** when:

- [ ] Code is written and self-reviewed
- [ ] Code passes ESLint with no errors
- [ ] Unit/integration tests are written and passing
- [ ] No new security vulnerabilities introduced (check against security doc)
- [ ] Tested locally on dev environment
- [ ] Pull request created and reviewed by at least 1 other developer
- [ ] Merged to `main` branch
- [ ] CI pipeline passes (lint + test)
- [ ] Story moved to "Done" on the board
- [ ] Acceptance criteria verified by Product Owner

---

## Risk Management

| Risk | Impact | Probability | Mitigation |
|------|--------|-------------|------------|
| Developer unfamiliar with testing framework | Medium | Medium | Sprint 2 starts with test setup spike |
| Breaking changes from security fixes | High | Low | Comprehensive test suite before major changes |
| Scope creep in Sprint 4 | Medium | Medium | Strict DoD, PO prioritizes ruthlessly |
| Staging environment delays | Low | Medium | Can use Docker locally as fallback |
| Unfixed critical vulnerability found late | High | Low | Security review is first sprint priority |

---

## Sprint Board Columns

```
┌──────────┬──────────┬──────────┬──────────┬──────────┐
│ Backlog   │ To Do    │ In       │ In       │ Done     │
│           │ (Sprint) │ Progress │ Review   │          │
├──────────┼──────────┼──────────┼──────────┼──────────┤
│ All       │ Stories  │ Active   │ PR open, │ Merged   │
│ stories   │ selected │ work in  │ awaiting │ & tested │
│ not yet   │ for this │ progress │ review   │ & accepted│
│ sprinted  │ sprint   │          │          │          │
└──────────┴──────────┴──────────┴──────────┴──────────┘
```

---

## Cumulative Flow

| Sprint | Points Planned | Cumulative |
|--------|---------------|------------|
| Sprint 1 | 13 | 13 |
| Sprint 2 | 17 | 30 |
| Sprint 3 | 21 | 51 |
| Sprint 4 | 20 | 71 |

**Total backlog for 8 weeks:** 71 story points across 33 user stories.
