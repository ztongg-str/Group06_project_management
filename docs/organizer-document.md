# Event Organizer Documentation — UniPulse

## Overview

This document covers all features available to **Event Organizers** on the UniPulse Campus Event Management Platform. Organizers can create and manage events, track registrations, perform check-ins, and use the social/gamification features available to students.

---

## Getting Started

### Role Assignment

- The `organizer` role can be granted by a **Superadmin** or **Coordinator** via the Admin Panel.
- Users registered through the public registration form receive the `student` role by default.
- Role switching is available via `POST /api/auth/switch-role` for users with multiple roles.

---

## Organizer Dashboard

After login, the **Organizer Dashboard** displays:
- Summary statistics: total events created, total registrations, total attendance.
- List of your events with status indicators (draft, pending, approved, rejected).
- Quick actions: Create Event, View All Events.

---

## Event Management

### Create a New Event

- Navigate to **Create Event** from the dashboard or navbar.
- Fill in the required fields:

| Field | Type | Description |
|-------|------|-------------|
| `title` | string (min 3) | Event name |
| `description` | string (min 10) | Full event description |
| `date` | date | Event date and time |
| `location` | string | Venue name or address |
| `category` | string | Event category |
| `maxParticipants` | integer | Maximum number of attendees |

- Click **Submit** to publish or **Save Draft** to finish later.

### Edit an Event

- Navigate to **My Events** and click **Edit** on any of your events.
- Modify any field and submit.
- Only the event organizer who created it (or admins with `manageEvents` permission) can edit.

### Delete an Event

- From **My Events**, click **Delete** on an event you own.
- Confirmation is required — this action is permanent.
- All associated registrations and attendance records are also removed.

### Save as Draft

- While creating an event, click **Save Draft** to save your progress.
- Drafts are stored and can be edited and submitted later via **My Events**.

### My Events

- Navigate to **My Events** to see all events you have created.
- Events are listed with their approval status:

| Status | Meaning |
|--------|---------|
| `draft` | Saved but not submitted |
| `pending` | Submitted, awaiting admin approval |
| `approved` | Approved — visible to students |
| `rejected` | Rejected by admin — see reason and resubmit |

### Event Approval Workflow

```
Create/Save Draft → Submit (Pending) → Admin Approves/Rejects → Approved (visible) or Rejected (hidden)
```

- Only **Superadmin** and **Coordinator** roles can approve or reject events.
- Rejected events include a reason — the organizer can edit and resubmit.

---

## Registration Management

### View Event Registrations

- From **My Events**, click **View Registrations** on an approved event.
- See all students registered for that event with:
  - Username, full name, email
  - Registration date
  - Attendance status (`registered`, `attended`, `missed`)

### Export Registrations

- The registrations page provides a table view suitable for export.
- All registration data is available via the API.

---

## Attendance & Check-In

### Generate QR Check-In Code

- Navigate to **QR Check-In** from the organizer dashboard.
- Select an event you organize.
- A unique QR code is generated for that event.
- Display this QR code on a projector or device at the venue.

### How Students Check In

1. Organizer displays the QR code at the event venue.
2. Student opens the **Scan QR** page on their device.
3. Student scans the QR code with their camera.
4. Attendance is recorded instantly and XP is awarded to the student.

### Manual 6-Digit Code Check-In

- An alternative to QR scanning.
- Students can enter a 6-digit code manually on the check-in page.
- The code is generated alongside the QR code.

### Mark Attendance Manually

- From the event registrations page, organizers can manually mark attendees.
- Useful when QR/code methods fail or for walk-in attendees.
- Endpoint: `PUT /api/history/attendance`

### Automatic No-Show Cleanup

- A background job runs **every 15 minutes**.
- Registrants who did not check in within 30 minutes of the event start time are automatically marked as `missed`.
- This creates a history entry and removes the event from upcoming lists.

---

## Gamification Management

### Achievements

- Navigate to **Achievements** from the organizer panel.
- Organizers can view all achievement definitions.
- View which achievements exist, their criteria, and who has earned them.

---

## Social Features

Organizers have access to all student social features:

### Friends

- Browse, search, and add other users as friends.
- Manage friend requests and view friend profiles.

### Activity Log

- Track your own activity: event creations, approvals, attendance markings.

---

## Reporting

### View Reports

- Navigate to **Reports** from the organizer panel.
- View all reports submitted by users against events or other users.
- Each report shows: reporter, reported item, reason, status.

### Resolve a Report

- Click **Resolve** on a report to mark it as handled.
- Resolved reports are archived.

### Delete a Report

- Remove a report entirely if it is invalid or spam.

---

## API Endpoints (Organizer)

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/events` | Browse all approved events |
| GET | `/api/events/my` | List events created by organizer |
| GET | `/api/events/:id` | View event details |
| POST | `/api/events/` | Create a new event |
| PUT | `/api/events/:id` | Edit own event |
| DELETE | `/api/events/:id` | Delete own event |
| POST | `/api/events/draft` | Save event as draft |
| GET | `/api/registrations/event/:eventId` | View registrations for own event |
| GET | `/api/registrations/my` | My registrations (as attendee) |
| POST | `/api/registrations/` | Register for an event |
| GET | `/api/registrations/check/:eventId` | Check registration status |
| POST | `/api/attendance/checkin` | QR code check-in |
| POST | `/api/attendance/checkin-by-code` | Manual code check-in |
| GET | `/api/attendance/my-history` | Attendance history |
| GET | `/api/history/upcoming` | Upcoming events |
| GET | `/api/history/past` | Past events |
| PUT | `/api/history/attendance` | Mark attendance manually |
| GET | `/api/achievements/all` | List all achievement definitions |
| GET | `/api/achievements/` | My achievements |
| GET | `/api/achievements/status` | All achievements with earned status |
| GET | `/api/friends/` | My friends |
| GET | `/api/friends/discover` | Discover users |
| GET | `/api/friends/search` | Search users |
| POST | `/api/friends/add/:userId` | Add friend |
| DELETE | `/api/friends/:id` | Remove friend |
| GET | `/api/friends/requests` | Pending requests |
| POST | `/api/friends/requests/:requestId/accept` | Accept request |
| POST | `/api/friends/requests/:requestId/decline` | Decline request |
| GET | `/api/reports/` | View all reports |
| PATCH | `/api/reports/:id/resolve` | Resolve a report |
| DELETE | `/api/reports/:id` | Delete a report |
| POST | `/api/reports/` | Submit a report |
| GET | `/api/saved-events/` | Saved events |
| POST | `/api/saved-events/` | Save an event |
| DELETE | `/api/saved-events/:eventId` | Unsave an event |
| GET | `/api/activity-logs/` | My activity log |
| GET | `/api/users/profile` | My profile |
| PUT | `/api/users/profile` | Update profile |
| GET | `/api/users/` | List all users |
| GET | `/api/users/:id` | View user by ID |

---

## Permissions

Organizers have the following implicit permissions:
- Create, edit, and delete **their own** events.
- View registrations for **their own** events.
- View all users (for friend discovery).
- Manage reports.
- Mark attendance for their events.

Organizers **cannot**:
- Approve or reject events (admin only).
- Manage other users' events.
- Access the admin dashboard.
- Access backup, query console, or system health features.
