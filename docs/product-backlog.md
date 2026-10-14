# Product Backlog — UniPulse

## Product Vision

UniPulse is a Campus Event Management Platform that enables students to discover, register for, and attend campus events while organizers create and manage events, and administrators oversee the entire system with advanced tools.

---

## Backlog Conventions

**Priority levels:**
- **P0** — Critical (blocks release, security risk)
- **P1** — High (significant user impact, production readiness)
- **P2** — Medium (improves quality of life)
- **P3** — Low (nice to have, future enhancement)

**Story Points:**
| Points | Effort |
|--------|--------|
| 1 | Trivial (config, copy change) |
| 2 | Small (single function, simple component) |
| 3 | Medium (multi-file change, new endpoint or page) |
| 5 | Large (cross-cutting feature, new module) |
| 8 | Very large (must be split) |

**Story format:**
> As a **[role]**, I want **[action]** so that **[benefit]**

**Acceptance Criteria (AC):** Verifiable conditions that must be true for the story to be "Done."

---

## Epic 1: Security Hardening

*Priority: P0 — Must be resolved before any production deployment*

| ID | User Story | Priority | Points | Status |
|----|-----------|----------|--------|--------|
| SEC-01 | As a system owner, I want hardcoded secret fallbacks removed so that attackers cannot forge tokens using known default values. | P0 | 2 | Open |
| | **AC:** Server throws a fatal error at startup if `JWT_SECRET` or `CRYPTO_KEY` is not set. No fallback values exist in source code. | | | |
| SEC-02 | As a system owner, I want rate limiting on all authentication endpoints so that brute-force attacks are prevented. | P0 | 3 | Open |
| | **AC:** Login/register endpoints allow max 5 attempts per 15 min per IP. OTP endpoints allow max 3 requests per 5 min. HTTP 429 returned when exceeded. | | | |
| SEC-03 | As a system owner, I want all error messages sanitized in production so that internal details (SQL, paths, stack traces) are never exposed to clients. | P0 | 2 | Open |
| | **AC:** In `NODE_ENV=production`, 500 errors return generic message. SQL/database errors never reach the client. Detailed errors only appear in server logs. | | | |
| SEC-04 | As a system owner, I want CORS restricted to known frontend origins so that malicious websites cannot make authenticated API calls. | P0 | 1 | Open |
| | **AC:** `Access-Control-Allow-Origin` is set to specific origin(s), not `*`. Requests from untrusted origins are rejected by the browser. | | | |
| SEC-05 | As a system owner, I want security headers (CSP, HSTS, X-Frame-Options, etc.) configured so that common web attacks (clickjacking, XSS, MIME sniffing) are mitigated. | P0 | 2 | Open |
| | **AC:** `helmet` middleware is active. Lighthouse security audit shows no missing headers. CSP allows only trusted sources. | | | |
| SEC-06 | As a system owner, I want the query console restricted to read-only for developer role so that destructive SQL cannot be executed. | P0 | 3 | Open |
| | **AC:** Only `SELECT`, `SHOW`, `DESCRIBE`, `EXPLAIN` queries execute for developer role. Sensitive columns (password, otp) are blocked for all roles. | | | |
| SEC-07 | As a system owner, I want admin passwords never returned in API responses (decrypted or otherwise) so that credential leaks are impossible. | P0 | 2 | Open |
| | **AC:** `GET /api/admin-roles/accounts` never contains plaintext or decrypted passwords. The password field is omitted entirely. | | | |
| SEC-08 | As a system owner, I want reversible encryption removed from admin credential storage so that passwords cannot be decrypted even with the crypto key. | P1 | 3 | Open |
| | **AC:** Admin credentials stored with bcrypt only. `passwordEncrypted` column removed or unused. Existing encrypted passwords are migrated to bcrypt. | | | |
| SEC-09 | As a user, I want my password re-entry required before switching roles so that a stolen token cannot escalate privileges. | P1 | 3 | Open |
| | **AC:** `POST /api/auth/switch-role` requires `{ password }` in body. Wrong password returns 401. | | | |
| SEC-10 | As a system owner, I want OTP brute-force protection (max attempts, shorter expiry) so that 6-digit codes cannot be guessed. | P1 | 2 | Open |
| | **AC:** After 5 failed OTP attempts, user must request new OTP. OTP expires in 5 minutes (not 10). | | | |
| SEC-11 | As a system owner, I want input sanitization on all user-provided strings so that stored XSS is prevented. | P1 | 3 | Open |
| | **AC:** All string inputs are stripped of HTML/script tags before storage. Sanitized input test: `<script>alert(1)</script>` stored as `alert(1)`. | | | |
| SEC-12 | As a system owner, I want SVG files removed from allowed upload types so that XSS via malicious SVG is impossible. | P1 | 1 | Open |
| | **AC:** Uploading an SVG file returns 400 error. Only jpg, png, gif, webp are accepted. | | | |
| SEC-13 | As a system owner, I want pagination limits capped on all list endpoints so that a single request cannot fetch the entire database. | P1 | 2 | Open |
| | **AC:** `?limit=` parameter is capped at 100. Requests exceeding 100 are clamped to 100. `?page` validated as positive integer. | | | |
| SEC-14 | As a system owner, I want the `GET /api/users/` route to use consistent authorization middleware so that custom admin roles are properly checked. | P1 | 2 | Open |
| | **AC:** Route uses `requirePermission('manageUsers')` instead of inline role check. Users with custom roles containing `users.manage` can list users. | | | |
| SEC-15 | As a system owner, I want event registration viewing to properly authorize admin roles so that admins with permissions can access any event's registrations. | P1 | 2 | Open |
| | **AC:** Superadmin/developer/coordinator with `manageEvents` permission can view any event's registrations. Organizers can only view their own events. | | | |
| SEC-16 | As a student, I want registration errors to use generic messages so that my email/username cannot be enumerated by others. | P2 | 1 | Open |
| | **AC:** Registration of existing email or username returns the same message: "Registration failed. Email or username may already be in use." | | | |
| SEC-17 | As a system owner, I want Content-Disposition headers on uploaded files so that browsers cannot execute malicious content. | P2 | 1 | Open |
| | **AC:** Files served from `/uploads` include `Content-Disposition: attachment` header. Browser prompts download instead of rendering. | | | |

---

## Epic 2: Testing & Quality Assurance

*Priority: P1 — Needed for production confidence*

| ID | User Story | Priority | Points | Status |
|----|-----------|----------|--------|--------|
| TEST-01 | As a developer, I want a testing framework (Vitest) configured so that I can write and run automated tests. | P1 | 3 | Open |
| | **AC:** `npm run test` runs Vitest. Coverage reporting is available. Framework supports both unit and integration tests with `supertest`. | | | |
| TEST-02 | As a developer, I want unit tests for the Auth module so that login, registration, OTP, and JWT logic is verified. | P1 | 5 | Open |
| | **AC:** Tests cover: successful registration, duplicate email, successful login, wrong password, JWT generation, JWT verification, JWT expiry, OTP generation, OTP verification, OTP expiry, OTP max attempts. Coverage >80%. | | | |
| TEST-03 | As a developer, I want unit tests for the Users module so that profile, role changes, and RBAC logic is verified. | P1 | 5 | Open |
| | **AC:** Tests cover: getProfile, updateProfile, admin createUser, updateUserRole (incl. last-admin protection), updateUserStatus (incl. self-protection), deleteUser, RBAC permission checks. Coverage >80%. | | | |
| TEST-04 | As a developer, I want unit tests for the Events module so that CRUD, approval workflow, and authorization are verified. | P1 | 5 | Open |
| | **AC:** Tests cover: create event, edit event (own vs others), delete event, approve event, reject event (requires reason), draft save, authorization checks for organizer/admin. Coverage >80%. | | | |
| TEST-05 | As a QA engineer, I want integration tests for the full student journey so that end-to-end flows are validated. | P1 | 5 | Open |
| | **AC:** Integration test covers: register → verify OTP → login → browse events → register for event → check-in → view history. Test runs against test database. | | | |
| TEST-06 | As a QA engineer, I want integration tests for the organizer journey so that event management flows are validated. | P1 | 5 | Open |
| | **AC:** Integration test covers: login as organizer → create event → admin approves → view registrations → generate QR code → check-in student → mark attendance. | | | |
| TEST-07 | As a developer, I want a GitHub Actions CI pipeline so that every push and PR is automatically linted and tested. | P1 | 3 | Open |
| | **AC:** CI runs on push to `main` and all PRs. Pipeline steps: install → lint → test → coverage report. Failed pipeline blocks merge. CI badge in README. | | | |
| TEST-08 | As a developer, I want frontend component tests so that critical UI components render correctly with various states. | P2 | 5 | Open |
| | **AC:** React Testing Library installed. Tests cover: Login form validation, EventCard rendering, ProtectedRoute redirect, AdminRoute redirect. Coverage >60%. | | | |
| TEST-09 | As a system owner, I want a test database configuration so that tests never touch production data. | P1 | 2 | Open |
| | **AC:** `NODE_ENV=test` uses a separate database (`unipulse_test`). Test data is created and destroyed per test run. Production DB is never affected by tests. | | | |

---

## Epic 3: CI/CD & DevOps

*Priority: P1 — Needed for reliable deployments*

| ID | User Story | Priority | Points | Status |
|----|-----------|----------|--------|--------|
| CICD-01 | As a developer, I want a GitHub Actions CI pipeline so that code quality is verified on every commit. | P1 | 3 | Open |
| | **AC:** Workflow file at `.github/workflows/ci.yml`. Runs on push/PR to main. Steps: checkout → setup Node → npm install → lint → test → build. Status badge in README. | | | |
| CICD-02 | As a system owner, I want a staging environment configured so that changes are validated before production. | P1 | 3 | Open |
| | **AC:** Staging environment accessible at a non-production URL. Uses separate database and env vars. Deployment to staging is automated via CI. | | | |
| CICD-03 | As a system owner, I want HTTPS enforced in production so that all traffic is encrypted. | P2 | 1 | Open |
| | **AC:** HTTP requests redirect to HTTPS (301). HSTS header set with max-age of 1 year. SSL certificate valid and auto-renewing. | | | |
| CICD-04 | As a developer, I want environment variable validation at startup so that missing configs fail fast with clear messages. | P1 | 2 | Open |
| | **AC:** Server checks for all required env vars on startup. Missing var produces: `FATAL: Environment variable JWT_SECRET is required but not set.` Server does not start. | | | |
| CICD-05 | As a system owner, I want database backups encrypted at rest so that backup files cannot be read if storage is compromised. | P2 | 3 | Open |
| | **AC:** Backup files are encrypted with AES-256 before being written to disk. Decryption requires a key not stored alongside backups. | | | |
| CICD-06 | As a developer, I want a `.env.example` file committed to the repo so that new team members know what variables are required. | P1 | 1 | Open |
| | **AC:** `back/.env.example` lists all required variables with placeholder values. README references it. No real secrets in the file. | | | |

---

## Epic 4: Performance & Reliability

*Priority: P2 — Improves user experience under load*

| ID | User Story | Priority | Points | Status |
|----|-----------|----------|--------|--------|
| PERF-01 | As a system owner, I want slow query logging and monitoring so that performance bottlenecks are identified quickly. | P2 | 3 | Open |
| | **AC:** Queries exceeding 1 second are logged with duration and query text. Log includes endpoint and user ID. Dashboard shows top 10 slowest queries. | | | |
| PERF-02 | As a student, I want event listing pages to load quickly (under 2 seconds) even with many events. | P2 | 3 | Open |
| | **AC:** Event list endpoint responds in <2s with 100+ events. Composite indexes exist on frequently queried columns. N+1 queries in list endpoints are eliminated. | | | |
| PERF-03 | As a system owner, I want database connection pooling tuned so that the server handles concurrent users without exhausting connections. | P2 | 2 | Open |
| | **AC:** Connection pool size is configurable via env var (default 10). Pool waits for connections when exhausted (no crash). Connection timeout is 10 seconds. | | | |
| PERF-04 | As a student, I want images to be lazy-loaded and optimized so that pages with many event images load quickly. | P2 | 3 | Open |
| | **AC:** Images use `loading="lazy"` attribute. Uploaded images are resized to max 1200px wide. Thumbnail variants are generated for list views. | | | |
| PERF-05 | As a system owner, I want cron jobs (attendance cleanup, backups) to have error alerting so that failures are noticed immediately. | P2 | 3 | Open |
| | **AC:** Failed cron jobs send an alert (email or log warning). Backup failures are recorded in the backups table with error message. Attendance cleanup logs failures. | | | |

---

## Epic 5: API Documentation & Developer Experience

*Priority: P2 — Improves maintainability*

| ID | User Story | Priority | Points | Status |
|----|-----------|----------|--------|--------|
| DOC-01 | As a developer, I want Swagger/OpenAPI documentation for all endpoints so that API consumers can discover and test endpoints easily. | P2 | 5 | Open |
| | **AC:** Swagger UI available at `/api-docs`. All endpoints documented with request/response schemas. Authentication requirements noted. Try-it-out feature works. | | | |
| DOC-02 | As a developer, I want all route files to have JSDoc comments so that the purpose of each endpoint is clear from the code. | P2 | 3 | Open |
| | **AC:** Every route has a JSDoc comment describing: HTTP method, path, auth required, permissions required, request body shape, response shape. | | | |
| DOC-03 | As a system owner, I want a production deployment checklist so that releases are consistent and nothing is missed. | P2 | 2 | Open |
| | **AC:** Checklist includes: env vars set, HTTPS configured, CI passing, security review done, backups verified, staging tested, rollback plan documented. | | | |

---

## Epic 6: User Experience Improvements

*Priority: P2/P3 — Enhances the product but not blocking release*

| ID | User Story | Priority | Points | Status |
|----|-----------|----------|--------|--------|
| UX-01 | As a student, I want email notifications when an event I registered for is about to start so that I don't miss it. | P2 | 5 | Open |
| | **AC:** Email sent 1 hour before event start to all registered students. Email contains event title, time, location, and link. Uses nodemailer with queue/retry. | | | |
| UX-02 | As a student, I want to filter events by category, date range, and location so that I can find relevant events quickly. | P2 | 3 | Open |
| | **AC:** Events page has filter dropdowns for category and date range. Results update without page reload. Filters persist in URL query params. | | | |
| UX-03 | As a student, I want a calendar view of upcoming events so that I can see what's happening this week/month. | P2 | 5 | Open |
| | **AC:** Calendar component shows events on their dates. Clicking a date shows events for that day. Month/week toggle works. | | | |
| UX-04 | As a student, I want to share an event on social media or copy a link so that I can invite friends. | P2 | 2 | Open |
| | **AC:** Event detail page has a "Share" button. Clicking copies event URL to clipboard. Twitter/WhatsApp share links available. | | | |
| UX-05 | As an organizer, I want to export my event's registration list to CSV so that I can use it for attendance tracking outside the platform. | P2 | 3 | Open |
| | **AC:** Export button on event registrations page. Downloads a CSV with columns: name, email, registration date, attendance status. CSV is properly escaped. | | | |
| UX-06 | As a student, I want a dark mode toggle so that I can use the app comfortably in low-light conditions. | P3 | 3 | Open |
| | **AC:** Toggle in navbar switches between light and dark themes. Theme preference persists in localStorage. All components render correctly in both modes. | | | |
| UX-07 | As a student, I want to see a map with event locations so that I can plan my route across campus. | P2 | 3 | Open |
| | **AC:** Events page shows a map (Leaflet) with markers for each event location. Clicking a marker shows event summary. Map and list are synchronized. | | | |
| UX-08 | As an admin, I want to see analytics dashboards with charts (registrations over time, popular categories, attendance rates) so that I can make data-driven decisions. | P2 | 5 | Open |
| | **AC:** Admin dashboard includes: bar chart of registrations per week, pie chart of events by category, line chart of attendance rate. Data is real-time. | | | |
| UX-09 | As a student, I want to search events by keyword so that I can find specific events quickly. | P2 | 3 | Open |
| | **AC:** Search bar on events page. Searches title and description. Results update as you type (debounced). No results state shown when empty. | | | |
| UX-10 | As a user, I want loading states and skeleton screens so that the app feels responsive while data loads. | P2 | 3 | Open |
| | **AC:** All data-fetching pages show a skeleton/spinner while loading. No content flash or layout shift. Loading states match the shape of the content. | | | |

---

## Epic 7: Token & Session Management

*Priority: P2 — Long-term security improvement*

| ID | User Story | Priority | Points | Status |
|----|-----------|----------|--------|--------|
| TOKEN-01 | As a system owner, I want short-lived access tokens with refresh tokens so that stolen tokens have a limited window of use. | P2 | 5 | Open |
| | **AC:** Access tokens expire in 1 hour. Refresh tokens stored in httpOnly cookie. Refresh endpoint issues new access token. Expired refresh token requires re-login. | | | |
| TOKEN-02 | As a user, I want to see and manage my active sessions so that I can log out suspicious devices. | P2 | 5 | Open |
| | **AC:** Profile page shows active sessions with device, IP, last active. "Log out all other sessions" button works. Sessions are invalidated server-side on logout. | | | |
| TOKEN-03 | As a system owner, I want token revocation on logout so that logged-out tokens cannot be reused. | P2 | 3 | Open |
| | **AC:** Logout adds token to a revocation list (DB or Redis). Revoked tokens are rejected with 401. Revocation list is cleaned up after token expiry. | | | |

---

## Epic 8: Admin & Moderation

*Priority: P2 — Improves admin capabilities*

| ID | User Story | Priority | Points | Status |
|----|-----------|----------|--------|--------|
| ADMIN-01 | As an admin, I want to suspend a user with a reason so that I can moderate the platform effectively. | P2 | 3 | Open |
| | **AC:** Admin can set user status to "suspended" with a reason field. Suspended users see a "Account suspended: [reason]" message and cannot perform actions. | | | |
| ADMIN-02 | As an admin, I want to bulk-approve or bulk-reject events so that I can process the verification queue efficiently. | P2 | 3 | Open |
| | **AC:** Admin can select multiple pending events and approve/reject all at once. Confirmation dialog shown. Action is logged per event. | | | |
| ADMIN-03 | As an admin, I want to view detailed activity logs with filtering so that I can investigate incidents. | P2 | 3 | Open |
| | **AC:** Logs page supports filtering by: user, action type, date range. Exportable to CSV. Shows IP address and user agent if available. | | | |
| ADMIN-04 | As a system owner, I want the system health endpoint to include disk space monitoring so that I can prevent storage-related outages. | P2 | 2 | Open |
| | **AC:** `/api/admin/system-health` includes disk usage percentage. Alert triggered when disk usage exceeds 80%. | | | |

---

## Backlog Summary

| Epic | Stories | Total Points |
|------|---------|-------------|
| Epic 1: Security Hardening | 17 | 33 |
| Epic 2: Testing & Quality Assurance | 9 | 38 |
| Epic 3: CI/CD & DevOps | 6 | 13 |
| Epic 4: Performance & Reliability | 5 | 13 |
| Epic 5: API Documentation & DX | 3 | 10 |
| Epic 6: User Experience Improvements | 10 | 35 |
| Epic 7: Token & Session Management | 3 | 13 |
| Epic 8: Admin & Moderation | 4 | 11 |
| **Total** | **57** | **166** |

---

## Sprint Mapping (8-Week Plan)

| Sprint | Weeks | Epics Covered | Target Points |
|--------|-------|---------------|--------------|
| Sprint 1 | 1-2 | Epic 1 (SEC-01 to SEC-15) | ~25 pts |
| Sprint 2 | 3-4 | Epic 2 (TEST-01 to TEST-06, TEST-09) + SEC-16, SEC-17 | ~35 pts |
| Sprint 3 | 5-6 | Epic 2 (TEST-07, TEST-08) + Epic 3 (CICD-01 to CICD-06) | ~20 pts |
| Sprint 4 | 7-8 | Epic 4 (PERF-01 to PERF-05) + Epic 5 + remaining P2 items | ~30 pts |

**Post-release (future sprints):**
- Epic 6: User Experience Improvements
- Epic 7: Token & Session Management
- Epic 8: Admin & Moderation

---

## Done Criteria (per story)

- [ ] Code written and self-reviewed
- [ ] ESLint passes with no errors
- [ ] Tests written and passing (where applicable)
- [ ] No new security vulnerabilities (checked against security-audit-fixes.md)
- [ ] Tested locally on dev environment
- [ ] PR reviewed by at least 1 other developer
- [ ] Merged to `main`
- [ ] CI pipeline passes
- | **AC verified by Product Owner**
