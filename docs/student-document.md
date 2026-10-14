# Student Documentation — UniPulse

## Overview

This document covers all features available to **Students** on the UniPulse Campus Event Management Platform.

---

## Getting Started

### 1. Registration

- Navigate to **Register** page.
- Provide: `username` (min 3 chars), `email` (valid), `password` (min 6 chars), `fullName`.
- Submit to create your account. You will be auto-assigned the `student` role.
- After registration, you will be redirected to the login page.

### 2. Login

- Navigate to **Login** page.
- Enter your `email` and `password`.
- On success, a JWT token is stored in localStorage and you are redirected to the Student Dashboard.

### 3. Password Recovery

- Navigate to **Forgot Password** page.
- Enter your `email` to receive a 6-digit OTP code.
- Enter the OTP and a new `password` (min 6 chars) to reset.

---

## Student Dashboard

After login, the **Student Home** page displays:
- Your XP, level, and progress bar.
- Upcoming events you are registered for.
- Recent activity log entries.
- Quick navigation to all student features.

---

## Browsing Events

### View All Events

- Navigate to **Events** page from the navbar.
- Browse the list of approved campus events.
- Each card shows: title, date, location, category, and available spots.

### View Event Details

- Click on any event card to see the **Event Detail** page.
- Full information includes: description, date, time, location (map), category, max participants, and organizer info.
- Buttons available: **Register**, **Save to Bookmarks**, **Report**.

---

## Registration

### Register for an Event

- On the Event Detail page, click **Register**.
- A confirmation appears — you are now registered.
- You can view your registrations under **My Registrations**.

### My Registrations

- Navigate to **My Registrations** from the dashboard or navbar.
- Shows all events you have registered for with status: `registered`, `attended`, `missed`.
- You can filter by status.

### Check Registration Status

- On any event detail page, the system checks if you are already registered and shows "Already Registered" instead of the register button.

---

## Attendance & Check-In

### QR Code Check-In

- When an event is active, the organizer generates a QR code.
- Navigate to **Scan QR** page.
- Allow camera access and scan the organizer's QR code.
- On success, your attendance is recorded and XP is awarded.

### Manual Code Check-In

- The organizer may also provide a 6-digit check-in code.
- Enter the code on the check-in screen as an alternative to QR scanning.

### Attendance History

- Navigate to **History** to see all past events you attended.
- Each entry shows the event name, date, and XP earned.

---

## Gamification

### XP & Levels

- Earn **XP** by attending events (each check-in grants XP).
- Level up by accumulating XP. Higher levels unlock achievement badges.

### Achievements

- Navigate to **Achievements** to view all available badges.
- Badges show: name, description, icon, and whether you have earned them.
- Some achievements are auto-granted (e.g., attend 3 events, reach level 5).

---

## Social Features

### Friends

- Navigate to **Friends** to view your friend list.
- Use **Discover** to browse other students.
- **Search** for users by username or full name.
- Send a friend request — the other user must accept it.

### Friend Requests

- View **Pending Requests** (incoming) and **Sent Requests**.
- Accept, decline, or cancel friend requests.

### Friend Profile

- Click on a friend to view their **Friend Profile**.
- See their XP, level, achievements, and attended events.

### Activity Log

- Navigate to **Activity Log** to see your personal activity history.
- Entries include: event registrations, check-ins, achievements earned, friend actions.

---

## Saved Events

- On any event detail page, click **Save** to bookmark it.
- Navigate to **Saved Events** to view all bookmarked events.
- Remove bookmarks at any time.

---

## Profile Management

- Navigate to **Profile** from the navbar.
- Update your: `username`, `fullName`, `bio`, `interests`, `avatar`, `cover image`.
- Upload images via the built-in file upload.
- Change your password through the profile settings.

---

## Upcoming Events

- Navigate to **Upcoming** to see all approved future events.
- Sorted by date — nearest events first.
- Quick-register directly from the list.

---

## Reporting

- On any event or user profile, click **Report**.
- Select a reason and provide details.
- Reports are reviewed by administrators and organizers.

---

## API Endpoints (Student)

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/auth/register` | Create student account |
| POST | `/api/auth/login` | Login and receive JWT |
| GET | `/api/auth/me` | Get current user profile |
| GET | `/api/events` | Browse all approved events |
| GET | `/api/events/:id` | View event details |
| POST | `/api/events/` | Create a new event (student role allowed) |
| GET | `/api/events/upcoming` | Get upcoming events |
| GET | `/api/events/my` | Get events created by user |
| POST | `/api/events/draft` | Save event as draft |
| POST | `/api/registrations/` | Register for an event |
| GET | `/api/registrations/my` | View my registrations |
| GET | `/api/registrations/check/:eventId` | Check if registered |
| POST | `/api/attendance/checkin` | QR code check-in |
| POST | `/api/attendance/checkin-by-code` | Manual code check-in |
| GET | `/api/attendance/my-history` | Attendance history |
| GET | `/api/friends/` | My friends list |
| GET | `/api/friends/discover` | Discover other users |
| GET | `/api/friends/search` | Search users |
| POST | `/api/friends/add/:userId` | Send friend request |
| DELETE | `/api/friends/:id` | Remove friend |
| GET | `/api/friends/profile/:userId` | View friend profile |
| GET | `/api/friends/requests` | Pending friend requests |
| GET | `/api/friends/requests/sent` | Sent friend requests |
| POST | `/api/friends/requests/:requestId/accept` | Accept friend request |
| POST | `/api/friends/requests/:requestId/decline` | Decline friend request |
| POST | `/api/friends/requests/:requestId/cancel` | Cancel sent request |
| GET | `/api/achievements/` | My achievements |
| GET | `/api/achievements/status` | All achievements with status |
| GET | `/api/achievements/user/:userId` | Another user's achievements |
| GET | `/api/activity-logs/` | My activity log |
| GET | `/api/saved-events/` | My saved events |
| POST | `/api/saved-events/` | Save an event |
| DELETE | `/api/saved-events/:eventId` | Remove saved event |
| GET | `/api/saved-events/check/:eventId` | Check if saved |
| POST | `/api/reports/` | Submit a report |
| GET | `/api/history/upcoming` | Upcoming events list |
| GET | `/api/history/past` | Past event history |
| GET | `/api/users/profile` | Get my profile |
| PUT | `/api/users/profile` | Update my profile |
| POST | `/api/upload/` | Upload a file (avatar, image) |

---

## Error Handling

All API responses follow this format:

```json
// Success
{ "success": true, "data": { ... } }

// Error
{ "success": false, "message": "Description of the error" }
```

Common HTTP status codes:
- `200` — Success
- `400` — Bad request (validation error)
- `401` — Unauthorized (not logged in)
- `403` — Forbidden (wrong role)
- `404` — Resource not found
- `500` — Server error
