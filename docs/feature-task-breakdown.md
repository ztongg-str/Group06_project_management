# Feature Task Breakdown — UniPulse

> Granular implementation tasks for every **To Do** feature in `feature-backlog.md`.
> Each task is sized for **one day or less** (≤ 8 hrs). Task type: **DB** (migration) · **BE** (backend) · **FE** (frontend) · **TEST** · **DOC**.

**Rules for splitting:**
- A task must be independently completable and reviewable.
- Backend and frontend tasks are separate so they can be parallelized.
- Every feature ends with **TEST** and (if needed) **DOC** tasks.

---

## How to read a feature

Example:
```
F-DISC-04 — Category Filter (P1, 3 pts)
  T1 [FE] Add category dropdown to Events page    (3h)
  T2 [FE] Wire dropdown to existing query param    (2h)
  T3 [TEST] Component + filter behavior test       (3h)
```
Total = 8h = 1 day.

---

# Epic 1: Accounts & Authentication

## F-AUTH-06 — Social Login (Google) (P3, 5 pts)

| Task | Type | Description | Est | Depends |
|------|------|-------------|-----|---------|
| AUTH06-T1 | DB | Add `provider`, `providerId` columns to `users` (nullable); migration 007 | 2h | — |
| AUTH06-T2 | BE | Install `passport` + `passport-google-oauth20`; configure strategy from env | 3h | T1 |
| AUTH06-T3 | BE | Create `/api/auth/google` and `/api/auth/google/callback` routes | 3h | T2 |
| AUTH06-T4 | BE | Service: find-or-create user by providerId, issue JWT | 3h | T3 |
| AUTH06-T5 | FE | Add "Sign in with Google" button to Login/Register | 2h | T3 |
| AUTH06-T6 | FE | Handle OAuth callback redirect + token storage | 3h | T4 |
| AUTH06-T7 | TEST | Mock Google strategy; test find-or-create + token | 4h | T4 |
| AUTH06-T8 | DOC | Document Google OAuth env vars in `.env.example` | 1h | T2 |

**Total: 21h (~3 days)**

---

## F-AUTH-07 — Two-Factor Authentication (P2, 5 pts)

| Task | Type | Description | Est | Depends |
|------|------|-------------|-----|---------|
| AUTH07-T1 | DB | Add `twoFactorEnabled`, `twoFactorSecret` to `users`; migration 008 | 2h | — |
| AUTH07-T2 | BE | Install `speakeasy` + `qrcode`; generate TOTP secret endpoint | 3h | T1 |
| AUTH07-T3 | BE | Endpoint to verify + enable 2FA with 6-digit code | 3h | T2 |
| AUTH07-T4 | BE | Modify login: require TOTP code when 2FA enabled | 3h | T3 |
| AUTH07-T5 | BE | Endpoint to disable 2FA (requires password + code) | 2h | T3 |
| AUTH07-T6 | FE | 2FA setup page with QR code display | 4h | T2 |
| AUTH07-T7 | FE | 2FA code input on login flow | 3h | T4 |
| AUTH07-T8 | TEST | Test setup, verify, login-with-2FA, disable | 4h | T5 |

**Total: 24h (~3 days)**

---

# Epic 2: User Profiles

## F-PROF-06 — Privacy Settings (P2, 5 pts)

| Task | Type | Description | Est | Depends |
|------|------|-------------|-----|---------|
| PROF06-T1 | DB | Add `profileVisibility`, `showEvents`, `showFriends` to `users`; migration | 2h | — |
| PROF06-T2 | BE | Add fields to allowedFields in `updateProfile` | 1h | T1 |
| PROF06-T3 | BE | Enforce visibility in `viewFriendProfile` + `getUserById` | 3h | T1 |
| PROF06-T4 | FE | Privacy settings section in Profile page | 3h | T2 |
| PROF06-T5 | FE | Hide restricted sections on other profiles | 3h | T3 |
| PROF06-T6 | TEST | Test each visibility combination | 4h | T3 |

**Total: 16h (~2 days)**

---

## F-PROF-07 — Delete Account (P2, 2 pts)

| Task | Type | Description | Est | Depends |
|------|------|-------------|-----|---------|
| PROF07-T1 | BE | `DELETE /api/users/me` endpoint (password confirm required) | 3h | — |
| PROF07-T2 | BE | Anonymize or cascade-delete related records (logs, events) | 3h | T1 |
| PROF07-T3 | FE | Danger zone in Profile with confirm modal | 2h | T1 |
| PROF07-T4 | TEST | Test deletion + related data handling | 2h | T2 |

**Total: 10h (~1.5 days)**

---

# Epic 3: Event Discovery

## F-DISC-04 — Category Filter (P1, 3 pts) · 🔨 In Progress

| Task | Type | Description | Est | Depends |
|------|------|-------------|-----|---------|
| DISC04-T1 | FE | Add category dropdown to Events page | 3h | — |
| DISC04-T2 | FE | Wire selection to `eventsAPI.getAll({ category })` | 2h | T1 |
| DISC04-T3 | FE | Persist filter in URL query param | 2h | T1 |
| DISC04-T4 | TEST | Filter returns correct subset | 2h | T2 |

**Total: 9h (~1 day)**

---

## F-DISC-05 — Search Events (P1, 3 pts) · 🔨 In Progress

| Task | Type | Description | Est | Depends |
|------|------|-------------|-----|---------|
| DISC05-T1 | FE | Debounced search input on Events page | 3h | — |
| DISC05-T2 | FE | Call existing `search` query param | 2h | T1 |
| DISC05-T3 | FE | Empty-state UI for no results | 1h | T2 |
| DISC05-T4 | BE | Add index on `events(title, description)` for LIKE search | 2h | — |
| DISC05-T5 | TEST | Search matches title + description | 2h | T2 |

**Total: 10h (~1.5 days)**

---

## F-DISC-06 — Date Range Filter (P2, 3 pts)

| Task | Type | Description | Est | Depends |
|------|------|-------------|-----|---------|
| DISC06-T1 | BE | Add `dateFrom`/`dateTo` params to `events.repository.findAll` | 3h | — |
| DISC06-T2 | FE | Date range picker on Events page | 3h | T1 |
| DISC06-T3 | FE | Combine with category + search filters | 2h | T2 |
| DISC06-T4 | TEST | Boundary dates included/excluded correctly | 2h | T1 |

**Total: 10h (~1.5 days)**

---

## F-DISC-07 — Calendar View (P2, 5 pts)

| Task | Type | Description | Est | Depends |
|------|------|-------------|-----|---------|
| DISC07-T1 | FE | Add calendar library (e.g., `react-big-calendar`) | 2h | — |
| DISC07-T2 | FE | Fetch events and map to calendar events | 3h | T1 |
| DISC07-T3 | FE | Month/week toggle | 3h | T2 |
| DISC07-T4 | FE | Click date → show event list for that day | 3h | T2 |
| DISC07-T5 | FE | Link calendar entry → event detail | 2h | T3 |
| DISC07-T6 | TEST | Calendar renders correct events per date | 3h | T2 |

**Total: 16h (~2 days)**

---

## F-DISC-08 — Map View (P2, 3 pts)

| Task | Type | Description | Est | Depends |
|------|------|-------------|-----|---------|
| DISC08-T1 | BE | Return lat/lng for events (add columns or geocode `location`) | 3h | — |
| DISC08-T2 | FE | Events map component with Leaflet markers | 4h | T1 |
| DISC08-T3 | FE | Sync map marker ↔ list item | 3h | T2 |
| DISC08-T4 | TEST | Markers match event count/positions | 2h | T2 |

**Total: 12h (~2 days)**

---

## F-DISC-10 — Recommended Events (P3, 5 pts)

| Task | Type | Description | Est | Depends |
|------|------|-------------|-----|---------|
| DISC10-T1 | DB | Add index on `events(category)` for recommendation query | 1h | — |
| DISC10-T2 | BE | Recommendation service: match `interests` → event categories | 4h | T1 |
| DISC10-T3 | BE | `GET /api/events/recommended` endpoint | 2h | T2 |
| DISC10-T4 | FE | "Recommended for you" section on StudentHome | 3h | T3 |
| DISC10-T5 | TEST | Recommendations respect interests | 3h | T2 |

**Total: 13h (~2 days)**

---

# Epic 4: Event Registration

## F-REG-05 — Cancel Registration (P1, 3 pts)

| Task | Type | Description | Est | Depends |
|------|------|-------------|-----|---------|
| REG05-T1 | BE | `DELETE /api/registrations/:eventId` endpoint | 3h | — |
| REG05-T2 | BE | Service: only allow if event not started; free the spot | 2h | T1 |
| REG05-T3 | BE | Remove from upcoming events + past history on cancel | 2h | T2 |
| REG05-T4 | FE | Cancel button + confirm modal on My Registrations | 3h | T1 |
| REG05-T5 | TEST | Cancel then re-register works | 3h | T2 |

**Total: 13h (~2 days)**

---

## F-REG-06 — Waitlist (P2, 5 pts)

| Task | Type | Description | Est | Depends |
|------|------|-------------|-----|---------|
| REG06-T1 | DB | Add `waitlist` table (eventId, userId, position, createdAt) | 2h | — |
| REG06-T2 | BE | Join waitlist when event full | 3h | T1 |
| REG06-T3 | BE | On cancel, promote first waitlisted user + notify | 4h | T2 |
| REG06-T4 | BE | `GET /api/registrations/waitlist/:eventId` position | 2h | T1 |
| REG06-T5 | FE | "Join waitlist" button when full + position badge | 3h | T2 |
| REG06-T6 | TEST | Promotion order is FIFO | 4h | T3 |

**Total: 18h (~2.5 days)**

---

## F-REG-07 — Registration Confirmation Email (P2, 3 pts)

| Task | Type | Description | Est | Depends |
|------|------|-------------|-----|---------|
| REG07-T1 | BE | Email template for registration confirmation | 2h | — |
| REG07-T2 | BE | Send email in `registerForEvent` (non-blocking) | 2h | T1 |
| REG07-T3 | BE | Retry/queue on send failure | 2h | T2 |
| REG07-T4 | TEST | Email send is triggered (mock transporter) | 2h | T2 |

**Total: 8h (~1 day)**

---

## F-REG-08 — Recurring Events (P3, 5 pts)

| Task | Type | Description | Est | Depends |
|------|------|-------------|-----|---------|
| REG08-T1 | DB | Add `seriesId`, `recurrenceRule` to `events` | 2h | — |
| REG08-T2 | BE | Create-event accepts recurrence rule; generate occurrences | 5h | T1 |
| REG08-T3 | BE | Register-for-series option (all occurrences) | 3h | T2 |
| REG08-T4 | FE | Recurrence picker in Create Event form | 4h | T2 |
| REG08-T5 | FE | Show series grouping on Events list | 3h | T3 |
| REG08-T6 | TEST | Occurrence generation + series registration | 4h | T3 |

**Total: 21h (~3 days)**

---

# Epic 5: Event Management

## F-EVT-11 — Duplicate Event (P2, 3 pts)

| Task | Type | Description | Est | Depends |
|------|------|-------------|-----|---------|
| EVT11-T1 | BE | `POST /api/events/:id/duplicate` endpoint | 3h | — |
| EVT11-T2 | BE | Copy fields, new draft status, new checkin code | 2h | T1 |
| EVT11-T3 | FE | Duplicate button on My Events | 2h | T1 |
| EVT11-T4 | TEST | Duplicate creates independent draft | 2h | T2 |

**Total: 9h (~1 day)**

---

## F-EVT-12 — Draft Autosave (P2, 3 pts)

| Task | Type | Description | Est | Depends |
|------|------|-------------|-----|---------|
| EVT12-T1 | FE | Debounced autosave hook (every 5s on change) | 3h | — |
| EVT12-T2 | FE | "Saved" indicator + last-saved time | 2h | T1 |
| EVT12-T3 | FE | Restore draft on page reload | 3h | T1 |
| EVT12-T4 | TEST | Autosave fires + restore works | 3h | T2 |

**Total: 11h (~1.5 days)**

---

# Epic 6: Attendance

## F-ATT-07 — GPS Self Check-In (P2, 5 pts)

| Task | Type | Description | Est | Depends |
|------|------|-------------|-----|---------|
| ATT07-T1 | DB | Add `latitude`, `longitude`, `radiusMeters` to `events` | 2h | — |
| ATT07-T2 | BE | Accept coords in check-in; compute distance | 3h | T1 |
| ATT07-T3 | BE | Reject check-in outside radius (configurable) | 3h | T2 |
| ATT07-T4 | FE | Request geolocation permission; send coords on check-in | 3h | T2 |
| ATT07-T5 | FE | Show "too far" error with distance | 2h | T3 |
| ATT07-T6 | TEST | In-radius passes, out-of-radius fails | 3h | T3 |

**Total: 16h (~2 days)**

---

## F-ATT-08 — Attendance Analytics (P2, 3 pts)

| Task | Type | Description | Est | Depends |
|------|------|-------------|-----|---------|
| ATT08-T1 | BE | Aggregation query: attendance rate per event | 3h | — |
| ATT08-T2 | BE | `GET /api/events/:id/attendance-stats` | 2h | T1 |
| ATT08-T3 | FE | Chart on event registrations page | 3h | T2 |
| ATT08-T4 | TEST | Rate math correct (attended/registered) | 2h | T1 |

**Total: 10h (~1.5 days)**

---

# Epic 7: Gamification

## F-GAME-05 — Leaderboard (P2, 5 pts)

| Task | Type | Description | Est | Depends |
|------|------|-------------|-----|---------|
| GAME05-T1 | DB | Index on `users(xp, level)` for ranking | 1h | — |
| GAME05-T2 | BE | `GET /api/leaderboard?scope=global|friends` | 4h | T1 |
| GAME05-T3 | BE | Compute user's rank | 2h | T2 |
| GAME05-T4 | FE | Leaderboard page with top N + my rank | 4h | T3 |
| GAME05-T5 | FE | Global/friends tab toggle | 3h | T4 |
| GAME05-T6 | TEST | Ranking order + friends scope correct | 3h | T3 |

**Total: 17h (~2.5 days)**

---

## F-GAME-06 — Attendance Streaks (P3, 3 pts)

| Task | Type | Description | Est | Depends |
|------|------|-------------|-----|---------|
| GAME06-T1 | DB | Add `currentStreak`, `longestStreak`, `lastAttendedDate` | 2h | — |
| GAME06-T2 | BE | Update streak logic on check-in | 3h | T1 |
| GAME06-T3 | FE | Streak display on Profile | 2h | T2 |
| GAME06-T4 | TEST | Streak increments/resets correctly | 3h | T2 |

**Total: 10h (~1.5 days)**

---

## F-GAME-07 — Points Redemption (P3, 8 pts)

| Task | Type | Description | Est | Depends |
|------|------|-------------|-----|---------|
| GAME07-T1 | DB | `rewards` table (name, cost, stock, image) | 2h | — |
| GAME07-T2 | DB | `redemptions` table (userId, rewardId, status, code) | 2h | — |
| GAME07-T3 | BE | Admin CRUD for rewards | 3h | T1 |
| GAME07-T4 | BE | Redeem endpoint: deduct XP, decrement stock | 4h | T2 |
| GAME07-T5 | BE | Generate redemption code/voucher | 3h | T4 |
| GAME07-T6 | FE | Rewards catalog page | 4h | T3 |
| GAME07-T7 | FE | Redeem flow + "My Rewards" list | 4h | T5 |
| GAME07-T8 | FE | Admin rewards management page | 4h | T3 |
| GAME07-T9 | TEST | Redeem deducts XP, prevents over-redeem | 4h | T5 |

**Total: 30h (~4 days)** → *split across 2 stories if needed*

---

# Epic 8: Social / Friends

## F-SOC-07 — Friend Activity Feed (P2, 5 pts)

| Task | Type | Description | Est | Depends |
|------|------|-------------|-----|---------|
| SOC07-T1 | BE | Query: recent registrations/check-ins of friends | 4h | — |
| SOC07-T2 | BE | `GET /api/friends/activity` with pagination | 2h | T1 |
| SOC07-T3 | FE | Activity feed component on StudentHome | 4h | T2 |
| SOC07-T4 | FE | Event/user links in feed items | 2h | T3 |
| SOC07-T5 | TEST | Feed only shows friends' activity | 3h | T1 |

**Total: 15h (~2 days)**

---

## F-SOC-08 — Invite Friends to Event (P2, 3 pts)

| Task | Type | Description | Est | Depends |
|------|------|-------------|-----|---------|
| SOC08-T1 | DB | `event_invites` table (eventId, inviterId, inviteeId, status) | 2h | — |
| SOC08-T2 | BE | Invite endpoint + notification | 3h | T1 |
| SOC08-T3 | FE | "Invite friends" modal on Event Detail (friend picker) | 4h | T2 |
| SOC08-T4 | TEST | Invite created + duplicate prevented | 3h | T2 |

**Total: 12h (~2 days)**

---

## F-SOC-09 — Event Chat / Comments (P3, 8 pts)

| Task | Type | Description | Est | Depends |
|------|------|-------------|-----|---------|
| SOC09-T1 | DB | `comments` table (eventId, userId, body, createdAt, parentId) | 2h | — |
| SOC09-T2 | BE | CRUD + pagination for comments | 4h | T1 |
| SOC09-T3 | BE | Input sanitization + length limits | 2h | T2 |
| SOC09-T4 | FE | Comment list + composer on Event Detail | 5h | T2 |
| SOC09-T5 | FE | Nested replies (if enabled) | 4h | T4 |
| SOC09-T6 | FE | Delete own comment | 2h | T4 |
| SOC09-T7 | TEST | Post, list, delete, sanitization | 4h | T3 |

**Total: 23h (~3 days)**

---

# Epic 9: Saved & History

## F-SAVE-05 — Attendance Certificate (P2, 5 pts)

| Task | Type | Description | Est | Depends |
|------|------|-------------|-----|---------|
| SAVE05-T1 | BE | Generate PDF certificate (pdfkit) for attended event | 4h | — |
| SAVE05-T2 | BE | `GET /api/history/:eventId/certificate` download | 2h | T1 |
| SAVE05-T3 | BE | Verify user actually attended | 1h | T2 |
| SAVE05-T4 | FE | "Download certificate" button on History | 2h | T2 |
| SAVE05-T5 | TEST | Certificate blocked for non-attendees | 2h | T3 |

**Total: 11h (~1.5 days)**

---

## F-SAVE-06 — Event Photos Album (P3, 5 pts)

| Task | Type | Description | Est | Depends |
|------|------|-------------|-----|---------|
| SAVE06-T1 | DB | `event_photos` table (eventId, uploaderId, url, createdAt) | 2h | — |
| SAVE06-T2 | BE | Upload endpoint (organizer/attendees) | 4h | T1 |
| SAVE06-T3 | BE | List + delete own photos | 3h | T2 |
| SAVE06-T4 | FE | Gallery grid on past events | 4h | T3 |
| SAVE06-T5 | TEST | Upload/list/delete + access control | 3h | T3 |

**Total: 16h (~2 days)**

---

# Epic 10: Reporting

## F-REP-04 — Report Notifications (P2, 3 pts)

| Task | Type | Description | Est | Depends |
|------|------|-------------|-----|---------|
| REP04-T1 | BE | Notify organizer/admins on new report (in-app + email) | 3h | — |
| REP04-T2 | BE | Notification badge count endpoint | 2h | T1 |
| REP04-T3 | FE | Bell badge in navbar for admins/organizers | 3h | T2 |
| REP04-T4 | TEST | Notification created on report | 2h | T1 |

**Total: 10h (~1.5 days)**

---

# Epic 11: Admin Portal

## F-ADM-11 — Analytics Charts (P2, 5 pts)

| Task | Type | Description | Est | Depends |
|------|------|-------------|-----|---------|
| ADM11-T1 | BE | Aggregation queries: registrations/week, events by category, attendance rate | 4h | — |
| ADM11-T2 | BE | `GET /api/admin/analytics` | 2h | T1 |
| ADM11-T3 | FE | Add chart library (e.g., Recharts) | 2h | — |
| ADM11-T4 | FE | Bar chart: registrations per week | 3h | T2 |
| ADM11-T5 | FE | Pie chart: events by category | 3h | T2 |
| ADM11-T6 | FE | Line chart: attendance rate | 3h | T2 |
| ADM11-T7 | TEST | Aggregations match raw data | 3h | T1 |

**Total: 20h (~3 days)**

---

## F-ADM-12 — Bulk Approve/Reject (P2, 3 pts)

| Task | Type | Description | Est | Depends |
|------|------|-------------|-----|---------|
| ADM12-T1 | BE | `PATCH /api/events/bulk-approve` accepting id array | 3h | — |
| ADM12-T2 | BE | `PATCH /api/events/bulk-reject` with reason | 2h | — |
| ADM12-T3 | BE | Log each event action individually | 2h | T1 |
| ADM12-T4 | FE | Checkbox selection in verification queue | 3h | T1 |
| ADM12-T5 | FE | Bulk action bar + confirm modal | 3h | T4 |
| ADM12-T6 | TEST | Bulk ops apply to all selected only | 3h | T2 |

**Total: 16h (~2 days)**

---

## F-ADM-13 — Suspend With Reason (P2, 3 pts)

| Task | Type | Description | Est | Depends |
|------|------|-------------|-----|---------|
| ADM13-T1 | DB | Add `suspensionReason`, `suspendedAt` to `users` | 2h | — |
| ADM13-T2 | BE | Extend status endpoint to store reason | 2h | T1 |
| ADM13-T3 | BE | Block suspended users at login with reason | 2h | T1 |
| ADM13-T4 | FE | Reason input in suspend dialog | 2h | T2 |
| ADM13-T5 | TEST | Suspended login blocked + reason shown | 2h | T3 |

**Total: 10h (~1.5 days)**

---

## F-ADM-14 — Scheduled Announcements (P3, 5 pts)

| Task | Type | Description | Est | Depends |
|------|------|-------------|-----|---------|
| ADM14-T1 | DB | `announcements` table (title, body, audience, publishAt) | 2h | — |
| ADM14-T2 | BE | CRUD for announcements | 3h | T1 |
| ADM14-T3 | BE | Cron job to publish scheduled announcements | 3h | T2 |
| ADM14-T4 | FE | Admin announcements management page | 4h | T2 |
| ADM14-T5 | FE | User-facing announcement banner | 3h | T3 |
| ADM14-T6 | TEST | Scheduled publish fires at correct time | 3h | T3 |

**Total: 18h (~2.5 days)**

---

# Epic 12: Notifications

## F-NOTIF-01 — In-App Notifications (P2, 5 pts)

| Task | Type | Description | Est | Depends |
|------|------|-------------|-----|---------|
| NOTIF01-T1 | DB | `notifications` table (userId, type, payload, readAt) | 2h | — |
| NOTIF01-T2 | BE | Create/list/mark-read endpoints | 4h | T1 |
| NOTIF01-T3 | BE | Unread count endpoint | 2h | T2 |
| NOTIF01-T4 | FE | Notification bell + dropdown panel | 4h | T2 |
| NOTIF01-T5 | FE | Poll unread count (or SSE) | 3h | T3 |
| NOTIF01-T6 | TEST | Create → list → mark read | 3h | T2 |

**Total: 18h (~2.5 days)**

---

## F-NOTIF-02 — Event Reminder Email (P2, 3 pts)

| Task | Type | Description | Est | Depends |
|------|------|-------------|-----|---------|
| NOTIF02-T1 | BE | Reminder email template | 2h | — |
| NOTIF02-T2 | BE | Cron job: 1h before event → email registrants | 4h | T1 |
| NOTIF02-T3 | BE | Prevent duplicate reminders (flag column) | 2h | T2 |
| NOTIF02-T4 | TEST | Reminder sent once per user/event | 2h | T3 |

**Total: 10h (~1.5 days)**

---

## F-NOTIF-03 — Friend Request Notification (P2, 2 pts)

| Task | Type | Description | Est | Depends |
|------|------|-------------|-----|---------|
| NOTIF03-T1 | BE | Emit notification on friend request (reuse NOTIF01) | 2h | NOTIF01-T2 |
| NOTIF03-T2 | FE | Show in notification panel with accept/decline | 3h | NOTIF01-T4 |
| NOTIF03-T3 | TEST | Notification created on request | 2h | T1 |

**Total: 7h (~1 day)**

---

## F-NOTIF-04 — Event Update Notifications (P2, 3 pts)

| Task | Type | Description | Est | Depends |
|------|------|-------------|-----|---------|
| NOTIF04-T1 | BE | On event edit/cancel → notify registrants | 3h | NOTIF01-T2 |
| NOTIF04-T2 | BE | Distinguish edit vs cancellation message | 2h | T1 |
| NOTIF04-T3 | FE | Render update notification with link | 2h | NOTIF01-T4 |
| NOTIF04-T5 | TEST | Notify on edit + cancel | 2h | T1 |

**Total: 9h (~1.5 days)**

---

# Master Task Summary

| Feature | Tasks | Est. Hours | Est. Days |
|---------|:-----:|:----------:|:---------:|
| F-AUTH-06 Social Login | 8 | 21 | 3 |
| F-AUTH-07 2FA | 8 | 24 | 3 |
| F-PROF-06 Privacy Settings | 6 | 16 | 2 |
| F-PROF-07 Delete Account | 4 | 10 | 1.5 |
| F-DISC-04 Category Filter | 4 | 9 | 1 |
| F-DISC-05 Search | 5 | 10 | 1.5 |
| F-DISC-06 Date Filter | 4 | 10 | 1.5 |
| F-DISC-07 Calendar | 6 | 16 | 2 |
| F-DISC-08 Map View | 4 | 12 | 2 |
| F-DISC-10 Recommendations | 5 | 13 | 2 |
| F-REG-05 Cancel Registration | 5 | 13 | 2 |
| F-REG-06 Waitlist | 6 | 18 | 2.5 |
| F-REG-07 Confirmation Email | 4 | 8 | 1 |
| F-REG-08 Recurring Events | 6 | 21 | 3 |
| F-EVT-11 Duplicate Event | 4 | 9 | 1 |
| F-EVT-12 Draft Autosave | 4 | 11 | 1.5 |
| F-ATT-07 GPS Check-In | 6 | 16 | 2 |
| F-ATT-08 Attendance Analytics | 4 | 10 | 1.5 |
| F-GAME-05 Leaderboard | 6 | 17 | 2.5 |
| F-GAME-06 Streaks | 4 | 10 | 1.5 |
| F-GAME-07 Redemption | 9 | 30 | 4 |
| F-SOC-07 Activity Feed | 5 | 15 | 2 |
| F-SOC-08 Invite Friends | 4 | 12 | 2 |
| F-SOC-09 Comments | 7 | 23 | 3 |
| F-SAVE-05 Certificate | 5 | 11 | 1.5 |
| F-SAVE-06 Photos | 5 | 16 | 2 |
| F-REP-04 Report Notifications | 4 | 10 | 1.5 |
| F-ADM-11 Analytics Charts | 7 | 20 | 3 |
| F-ADM-12 Bulk Approve/Reject | 6 | 16 | 2 |
| F-ADM-13 Suspend With Reason | 5 | 10 | 1.5 |
| F-ADM-14 Announcements | 6 | 18 | 2.5 |
| F-NOTIF-01 In-App Notifications | 6 | 18 | 2.5 |
| F-NOTIF-02 Reminder Email | 4 | 10 | 1.5 |
| F-NOTIF-03 Friend Request Notif | 3 | 7 | 1 |
| F-NOTIF-04 Event Update Notif | 4 | 9 | 1.5 |
| **TOTAL** | **183** | **500h** | **~63 days** |

---

## Suggested Order (by value & dependencies)

**Phase 1 — Quick wins (low effort, high value)**
1. F-DISC-04, F-DISC-05, F-DISC-06 (filters/search)
2. F-REG-05 (cancel registration)
3. F-REG-07 (confirmation email)
4. F-EVT-11 (duplicate event)
5. F-ADM-13 (suspend with reason)

**Phase 2 — Notifications foundation (unblocks 3 features)**
6. F-NOTIF-01 (in-app notifications) → then F-NOTIF-02/03/04, F-REP-04

**Phase 3 — Engagement**
7. F-GAME-05 (leaderboard), F-SOC-07 (activity feed), F-SOC-08 (invites)
8. F-DISC-07 (calendar), F-DISC-08 (map)

**Phase 4 — Big features**
9. F-REG-06 (waitlist), F-ATT-07 (GPS), F-ADM-11 (analytics), F-ADM-12 (bulk ops)

**Phase 5 — Advanced (defer)**
10. F-AUTH-06/07, F-GAME-07, F-SOC-09, F-SAVE-05/06, F-REG-08, F-ADM-14
