# Feature Backlog — UniPulse

> Product feature backlog. Shows what the platform **does today** (Done) and what features to **build next** (To Do).
> For security/testing/DevOps work, see `product-backlog.md`.

**Legend:** ✅ Done · 🔨 In Progress · 📋 To Do · Priority: P0 (critical) → P3 (nice to have)

---

## Epic 1: Accounts & Authentication

| ID | Feature | User Story | Priority | Pts | Status |
|----|---------|-----------|----------|-----|--------|
| F-AUTH-01 | Registration | As a visitor, I want to create an account so that I can join the platform. | P0 | 3 | ✅ |
| F-AUTH-02 | Email OTP verification | As a new user, I want to verify my email with a 6-digit code so that my account is secure. | P0 | 3 | ✅ |
| F-AUTH-03 | Login | As a user, I want to log in with email and password so that I can access my account. | P0 | 2 | ✅ |
| F-AUTH-04 | Forgot password | As a user, I want to reset my password via OTP so that I can recover my account. | P0 | 3 | ✅ |
| F-AUTH-05 | Role switching | As a student, I want to switch to organizer mode so that I can create events. | P1 | 3 | ✅ |
| F-AUTH-06 | Social login (Google) | As a user, I want to sign in with Google so that I don't need another password. | P3 | 5 | 📋 |
| F-AUTH-07 | Two-factor authentication | As a user, I want 2FA on login so that my account is protected even if my password leaks. | P2 | 5 | 📋 |

---

## Epic 2: User Profiles

| ID | Feature | User Story | Priority | Pts | Status |
|----|---------|-----------|----------|-----|--------|
| F-PROF-01 | View profile | As a user, I want to view my profile so that I can see my info and stats. | P0 | 2 | ✅ |
| F-PROF-02 | Edit profile | As a user, I want to edit my name, bio, and interests so that others know about me. | P0 | 3 | ✅ |
| F-PROF-03 | Avatar & cover image | As a user, I want to upload a profile picture and cover so that my profile looks personal. | P1 | 3 | ✅ |
| F-PROF-04 | XP & level display | As a user, I want to see my XP and level so that I can track my progress. | P1 | 2 | ✅ |
| F-PROF-05 | View other profiles | As a user, I want to view a friend's profile so that I can see their activity. | P1 | 3 | ✅ |
| F-PROF-06 | Privacy settings | As a user, I want to control who sees my profile so that my data stays private. | P2 | 5 | 📋 |
| F-PROF-07 | Delete account | As a user, I want to delete my account so that I can leave the platform. | P2 | 2 | 📋 |

---

## Epic 3: Event Discovery

| ID | Feature | User Story | Priority | Pts | Status |
|----|---------|-----------|----------|-----|--------|
| F-DISC-01 | Browse events | As a student, I want to browse all approved events so that I can find things to attend. | P0 | 3 | ✅ |
| F-DISC-02 | Event detail page | As a student, I want to view full event details so that I can decide to attend. | P0 | 3 | ✅ |
| F-DISC-03 | Upcoming events | As a student, I want to see upcoming events sorted by date so that I know what's next. | P1 | 2 | ✅ |
| F-DISC-04 | Category filter | As a student, I want to filter events by category so that I find relevant events. | P1 | 3 | 🔨 |
| F-DISC-05 | Search events | As a student, I want to search events by keyword so that I find specific events fast. | P1 | 3 | 🔨 |
| F-DISC-06 | Date range filter | As a student, I want to filter events by date so that I can plan ahead. | P2 | 3 | 📋 |
| F-DISC-07 | Calendar view | As a student, I want a calendar view of events so that I can see my schedule. | P2 | 5 | 📋 |
| F-DISC-08 | Map view | As a student, I want to see events on a map so that I can plan my route on campus. | P2 | 3 | 📋 |
| F-DISC-09 | Featured events | As a student, I want to see featured/popular events on the homepage so that I discover highlights. | P2 | 3 | ✅ |
| F-DISC-10 | Recommended events | As a student, I want event recommendations based on my interests so that I find things I'll like. | P3 | 5 | 📋 |

---

## Epic 4: Event Registration

| ID | Feature | User Story | Priority | Pts | Status |
|----|---------|-----------|----------|-----|--------|
| F-REG-01 | Register for event | As a student, I want to register for an event so that I can attend. | P0 | 3 | ✅ |
| F-REG-02 | My registrations | As a student, I want to see my registrations so that I can track what I signed up for. | P0 | 3 | ✅ |
| F-REG-03 | Registration status check | As a student, I want to see if I'm already registered so that I don't double-register. | P1 | 2 | ✅ |
| F-REG-04 | Event capacity limit | As a student, I want events to show available spots so that I know if I can join. | P1 | 2 | ✅ |
| F-REG-05 | Cancel registration | As a student, I want to cancel my registration so that I can free up my spot. | P1 | 3 | 📋 |
| F-REG-06 | Waitlist | As a student, I want to join a waitlist when an event is full so that I get in if someone cancels. | P2 | 5 | 📋 |
| F-REG-07 | Registration confirmation email | As a student, I want a confirmation email when I register so that I have a record. | P2 | 3 | 📋 |
| F-REG-08 | Recurring events | As a student, I want to see recurring events so that I can register for all sessions. | P3 | 5 | 📋 |

---

## Epic 5: Event Management (Organizer)

| ID | Feature | User Story | Priority | Pts | Status |
|----|---------|-----------|----------|-----|--------|
| F-EVT-01 | Create event | As an organizer, I want to create an event so that students can attend. | P0 | 5 | ✅ |
| F-EVT-02 | Edit event | As an organizer, I want to edit my event so that I can fix mistakes. | P0 | 3 | ✅ |
| F-EVT-03 | Delete event | As an organizer, I want to delete an event so that I can cancel it. | P0 | 2 | ✅ |
| F-EVT-04 | Save draft | As an organizer, I want to save a draft so that I can finish later. | P1 | 3 | ✅ |
| F-EVT-05 | My events list | As an organizer, I want to see all my events so that I can manage them. | P0 | 3 | ✅ |
| F-EVT-06 | Event approval workflow | As an organizer, I want my event reviewed before it goes live so that quality is maintained. | P0 | 5 | ✅ |
| F-EVT-07 | Event image upload | As an organizer, I want to add a cover image so that my event looks attractive. | P1 | 3 | ✅ |
| F-EVT-08 | Location picker (map) | As an organizer, I want to pick a location on a map so that students know where to go. | P1 | 3 | ✅ |
| F-EVT-09 | Organizer dashboard | As an organizer, I want a dashboard with stats so that I can track my events' performance. | P1 | 5 | ✅ |
| F-EVT-10 | Rejection feedback | As an organizer, I want to see why my event was rejected so that I can fix it. | P1 | 2 | ✅ |
| F-EVT-11 | Duplicate event | As an organizer, I want to duplicate an event so that I can create recurring ones quickly. | P2 | 3 | 📋 |
| F-EVT-12 | Draft autosave | As an organizer, I want my draft to autosave so that I don't lose work. | P2 | 3 | 📋 |

---

## Epic 6: Attendance & Check-In

| ID | Feature | User Story | Priority | Pts | Status |
|----|---------|-----------|----------|-----|--------|
| F-ATT-01 | QR code check-in | As a student, I want to check in by scanning a QR code so that attendance is fast. | P0 | 5 | ✅ |
| F-ATT-02 | Manual code check-in | As a student, I want to check in with a 6-digit code so that I have a fallback. | P0 | 3 | ✅ |
| F-ATT-03 | QR code generation | As an organizer, I want to generate a QR code for my event so that students can check in. | P0 | 3 | ✅ |
| F-ATT-04 | Attendance history | As a student, I want to see my attendance history so that I can track my participation. | P1 | 3 | ✅ |
| F-ATT-05 | Auto no-show marking | As an organizer, I want no-shows marked automatically so that I don't have to do it manually. | P1 | 3 | ✅ |
| F-ATT-06 | Manual attendance marking | As an organizer, I want to mark attendance manually so that I can handle walk-ins. | P1 | 2 | ✅ |
| F-ATT-07 | Self-check-in with GPS | As a student, I want to check in via location so that I can verify I'm at the venue. | P2 | 5 | 📋 |
| F-ATT-08 | Attendance analytics | As an organizer, I want attendance rate charts so that I can measure engagement. | P2 | 3 | 📋 |

---

## Epic 7: Gamification

| ID | Feature | User Story | Priority | Pts | Status |
|----|---------|-----------|----------|-----|--------|
| F-GAME-01 | XP on check-in | As a student, I want to earn XP when I attend so that I'm motivated to participate. | P1 | 3 | ✅ |
| F-GAME-02 | Levels | As a student, I want to level up as I earn XP so that I can progress. | P1 | 3 | ✅ |
| F-GAME-03 | Achievements/badges | As a student, I want to unlock badges so that I can show off my accomplishments. | P1 | 5 | ✅ |
| F-GAME-04 | Auto-granting achievements | As a student, I want achievements granted automatically so that I don't have to claim them. | P1 | 3 | ✅ |
| F-GAME-05 | Leaderboard | As a student, I want a leaderboard so that I can compare with friends. | P2 | 5 | 📋 |
| F-GAME-06 | Streaks | As a student, I want attendance streaks so that I'm rewarded for consistency. | P3 | 3 | 📋 |
| F-GAME-07 | Points redemption | As a student, I want to redeem XP for rewards so that gamification has value. | P3 | 8 | 📋 |

---

## Epic 8: Social / Friends

| ID | Feature | User Story | Priority | Pts | Status |
|----|---------|-----------|----------|-----|--------|
| F-SOC-01 | Friend requests | As a student, I want to send friend requests so that I can connect with others. | P1 | 3 | ✅ |
| F-SOC-02 | Accept/decline requests | As a student, I want to manage friend requests so that I control my connections. | P1 | 3 | ✅ |
| F-SOC-03 | Friends list | As a student, I want to see my friends so that I can find them easily. | P1 | 2 | ✅ |
| F-SOC-04 | Discover users | As a student, I want to discover other users so that I can make new friends. | P1 | 3 | ✅ |
| F-SOC-05 | Search users | As a student, I want to search for users so that I can find specific people. | P1 | 3 | ✅ |
| F-SOC-06 | View friend profile | As a student, I want to view a friend's profile so that I can see their events. | P1 | 3 | ✅ |
| F-SOC-07 | Friend activity feed | As a student, I want to see what events my friends are attending so that we can go together. | P2 | 5 | 📋 |
| F-SOC-08 | Invite friends to event | As a student, I want to invite friends to an event so that we can attend together. | P2 | 3 | 📋 |
| F-SOC-09 | Event chat/comments | As a student, I want to comment on events so that I can ask questions. | P3 | 8 | 📋 |

---

## Epic 9: Saved & History

| ID | Feature | User Story | Priority | Pts | Status |
|----|---------|-----------|----------|-----|--------|
| F-SAVE-01 | Save/bookmark events | As a student, I want to save events so that I can find them later. | P1 | 3 | ✅ |
| F-SAVE-02 | View saved events | As a student, I want to view my saved events so that I can track interest. | P1 | 2 | ✅ |
| F-SAVE-03 | Past event history | As a student, I want to see my past events so that I have a record. | P1 | 3 | ✅ |
| F-SAVE-04 | Activity log | As a student, I want to see my activity log so that I can track my actions. | P2 | 2 | ✅ |
| F-SAVE-05 | Attendance certificate | As a student, I want to download a certificate for events I attended so that I have proof. | P2 | 5 | 📋 |
| F-SAVE-06 | Event photos album | As a student, I want to see photos from past events so that I can relive them. | P3 | 5 | 📋 |

---

## Epic 10: Reporting & Moderation

| ID | Feature | User Story | Priority | Pts | Status |
|----|---------|-----------|----------|-----|--------|
| F-REP-01 | Report event/user | As a user, I want to report inappropriate content so that the platform stays safe. | P1 | 3 | ✅ |
| F-REP-02 | View reports (organizer) | As an organizer, I want to view all reports so that I can moderate. | P1 | 3 | ✅ |
| F-REP-03 | Resolve report | As an organizer, I want to resolve a report so that issues are tracked. | P1 | 2 | ✅ |
| F-REP-04 | Report notifications | As an organizer, I want to be notified of new reports so that I respond quickly. | P2 | 3 | 📋 |

---

## Epic 11: Admin Portal

| ID | Feature | User Story | Priority | Pts | Status |
|----|---------|-----------|----------|-----|--------|
| F-ADM-01 | Admin dashboard | As an admin, I want a dashboard with key metrics so that I can monitor the platform. | P0 | 5 | ✅ |
| F-ADM-02 | User management | As an admin, I want to view/edit/suspend/delete users so that I can manage accounts. | P0 | 5 | ✅ |
| F-ADM-03 | Event management | As an admin, I want to manage all events so that I can oversee content. | P0 | 3 | ✅ |
| F-ADM-04 | Event verification queue | As an admin, I want a queue of pending events so that I can approve/reject them. | P0 | 3 | ✅ |
| F-ADM-05 | Custom admin roles (RBAC) | As a superadmin, I want to create roles with granular permissions so that I can delegate. | P1 | 5 | ✅ |
| F-ADM-06 | Admin accounts | As a superadmin, I want to create admin accounts so that I can build a team. | P1 | 3 | ✅ |
| F-ADM-07 | System health | As a developer, I want to see system health so that I can monitor uptime. | P1 | 3 | ✅ |
| F-ADM-08 | Activity logs | As an admin, I want to view activity logs so that I can audit actions. | P1 | 3 | ✅ |
| F-ADM-09 | Query console | As a developer, I want to run SQL queries so that I can debug data issues. | P1 | 3 | ✅ |
| F-ADM-10 | Backup & restore | As an admin, I want to back up and restore the database so that data is safe. | P1 | 5 | ✅ |
| F-ADM-11 | Analytics charts | As an admin, I want charts of registrations/attendance so that I can make decisions. | P2 | 5 | 📋 |
| F-ADM-12 | Bulk approve/reject | As an admin, I want to bulk-process events so that I save time. | P2 | 3 | 📋 |
| F-ADM-13 | Suspend with reason | As an admin, I want to suspend users with a reason so that moderation is clear. | P2 | 3 | 📋 |
| F-ADM-14 | Scheduled announcements | As an admin, I want to broadcast announcements so that I can reach all users. | P3 | 5 | 📋 |

---

## Epic 12: Notifications

| ID | Feature | User Story | Priority | Pts | Status |
|----|---------|-----------|----------|-----|--------|
| F-NOTIF-01 | In-app notifications | As a user, I want in-app notifications so that I don't miss updates. | P2 | 5 | 📋 |
| F-NOTIF-02 | Event reminder email | As a student, I want an email reminder before an event so that I don't forget. | P2 | 3 | 📋 |
| F-NOTIF-03 | Friend request notification | As a student, I want to be notified of friend requests so that I respond. | P2 | 2 | 📋 |
| F-NOTIF-04 | Event update notifications | As a student, I want to be notified if an event changes or is cancelled so that I stay informed. | P2 | 3 | 📋 |

---

## Feature Backlog Summary

| Epic | Features Done | Features To Do | Total Pts (To Do) |
|------|:-------------:|:--------------:|:-----------------:|
| 1. Accounts & Auth | 5 | 2 | 10 |
| 2. User Profiles | 5 | 2 | 7 |
| 3. Event Discovery | 4 | 6 | 22 |
| 4. Event Registration | 4 | 4 | 16 |
| 5. Event Management | 10 | 2 | 6 |
| 6. Attendance | 6 | 2 | 8 |
| 7. Gamification | 4 | 3 | 16 |
| 8. Social/Friends | 6 | 3 | 16 |
| 9. Saved & History | 4 | 2 | 10 |
| 10. Reporting | 3 | 1 | 3 |
| 11. Admin Portal | 10 | 4 | 18 |
| 12. Notifications | 0 | 4 | 13 |
| **Total** | **61** | **35** | **145** |

---

## Suggested Feature Roadmap

| Release | Focus | Epics |
|---------|-------|-------|
| **v1.0 (Current)** | Core platform | Auth, Profiles, Events, Registration, Attendance, Gamification, Social, Admin |
| **v1.1** | Discovery & polish | Filters, Search, Reminders, Notifications |
| **v1.2** | Engagement | Leaderboard, Activity feed, Invites, Calendar |
| **v2.0** | Growth | Redemption, Chat, Certificates, 2FA, Social login |

---

## Backlog Grooming Notes

**Next 3 features to build (highest value, already partially started):**
1. **F-DISC-04 / F-DISC-05** — Category filter + Search (UI hooks exist, endpoints ready)
2. **F-NOTIF-02** — Event reminder emails (nodemailer already configured)
3. **F-REG-05** — Cancel registration (small, high user demand)

**Defer:** Chat, redemption, event photos (high effort, low immediate value)
