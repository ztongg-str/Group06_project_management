# Admin Documentation — UniPulse

## Overview

This document covers all features available to **Administrators** on the UniPulse Campus Event Management Platform. The admin panel provides system-wide control over users, events, security, and infrastructure.

---

## Admin Roles

UniPulse has three built-in admin sub-roles:

| Role | Description |
|------|-------------|
| **Superadmin** | Full system access — can create other admin accounts, manage roles, and use all features. |
| **Developer** | System health monitoring, query console, backups. Cannot manage users or events. |
| **Coordinator** | User management, event verification, and management. No developer tools. |

Additionally, **custom admin roles** can be created with granular permissions via the Admin Roles module.

---

## Built-in Permission Map

| Permission | Superadmin | Developer | Coordinator |
|------------|:----------:|:---------:|:-----------:|
| `dashboard` | ✅ | ✅ | ✅ |
| `logs` | ✅ | ✅ | ✅ |
| `systemHealth` | ✅ | ✅ | ❌ |
| `manageUsers` | ✅ | ❌ | ✅ |
| `manageEvents` | ✅ | ❌ | ✅ |
| `verifyEvents` | ✅ | ❌ | ✅ |
| `backup` | ✅ | ✅ | ❌ |
| `queryConsole` | ✅ | ✅ | ❌ |

---

## Accessing the Admin Panel

1. Login with an admin account.
2. The **Admin Dashboard** is accessible via the `/admin` route.
3. All admin routes require authentication and appropriate permissions.
4. The admin layout provides a sidebar with links to all admin pages.

---

## Dashboard

### Statistics

The admin dashboard displays:
- Total users (with breakdown by role)
- Total events (with breakdown by status)
- Total registrations
- Total attendance records
- Recent activity logs

### System Health

- **Accessible to**: Superadmin, Developer
- View database connection status.
- View server uptime.
- View memory and CPU usage.
- Endpoint: `GET /api/admin/system-health`

### Activity Logs

- **Accessible to**: Superadmin, Developer, Coordinator
- View audit trail of all admin actions.
- Filter by date range, action type, and admin user.
- Endpoint: `GET /api/admin/logs`

---

## User Management

### View All Users

- Navigate to **Admin → Users**.
- See all registered users with: username, email, full name, role, status, XP, level.
- Search and filter by role or status.

### Create a User

- **Requires**: `manageUsers` permission
- Fill in: `username`, `email`, `password`, `fullName`, `role`.
- Endpoint: `POST /api/users/`

### Change User Role

- **Requires**: `manageUsers` permission
- Select a new role for any user.
- Roles: `student`, `organizer`, `superadmin`, `developer`, `coordinator`.
- Endpoint: `PATCH /api/users/:id/role`

### Suspend / Activate User

- **Requires**: `manageUsers` permission
- Toggle user status between `active` and `suspended`.
- Suspended users cannot log in.
- Endpoint: `PATCH /api/users/:id/status`

### Delete a User

- **Requires**: `manageUsers` permission
- Permanently remove a user and all associated data.
- Endpoint: `DELETE /api/users/:id`

---

## Event Management

### View All Events

- Navigate to **Admin → Events**.
- See all events regardless of status.
- Filter by status (draft, pending, approved, rejected).

### Approve an Event

- **Requires**: `verifyEvents` permission
- Click **Approve** on a pending event.
- The event becomes visible to all students.
- Endpoint: `PATCH /api/events/:id/approve`

### Reject an Event

- **Requires**: `verifyEvents` permission
- Click **Reject** on a pending event.
- Provide a rejection reason.
- The organizer can edit and resubmit.
- Endpoint: `PATCH /api/events/:id/reject`

### Edit / Delete Any Event

- **Requires**: `manageEvents` permission
- Edit or delete any event in the system (not just your own).
- Endpoints: `PUT /api/events/:id`, `DELETE /api/events/:id`

---

## Verification Queue

- Navigate to **Admin → Verification**.
- See all events with `pending` status.
- Quick approve/reject actions with reason input.
- This page centralizes the event approval workflow.

---

## Custom Admin Roles (RBAC)

### Overview

UniPulse supports creating custom admin roles with fine-grained permissions. Each custom role has permissions organized by module and action.

### Permission Structure

Permissions are defined per module with specific actions:

| Module | Actions |
|--------|---------|
| `users` | `manage` |
| `events` | `manage`, `approve` |

### Create a Custom Role

- Navigate to **Admin → Roles**.
- Enter a role name and select permissions.
- Endpoint: `POST /api/admin-roles/`

### Edit a Custom Role

- Update the name or permissions of an existing role.
- Endpoint: `PUT /api/admin-roles/:id`

### Delete a Custom Role

- Remove a custom role. Users assigned this role lose those permissions.
- Endpoint: `DELETE /api/admin-roles/:id`

### Assign a Role to a User

- Select a user and assign one or more custom admin roles.
- The user gains the permissions from those roles in addition to their built-in role.
- Endpoint: `POST /api/admin-roles/assign`

### Unassign a Role

- Remove a custom role assignment from a user.
- Endpoint: `DELETE /api/admin-roles/unassign/:userId/:roleId`

### View Role Schema

- See the full permission schema defining available modules and actions.
- Endpoint: `GET /api/admin-roles/schema`

### View Users with Custom Roles

- List all users who have custom admin roles assigned.
- Endpoint: `GET /api/admin-roles/users`

---

## Admin Account Management

### Create an Admin Account

- **Accessible to**: Superadmin only
- Create a new admin account with a specific role.
- Endpoint: `POST /api/admin/create-admin`

### Manage Admin Accounts

- Navigate to **Admin → Accounts**.
- View all users with admin-level roles.
- Delete admin accounts (removes admin role).
- Endpoints:
  - `GET /api/admin-roles/accounts`
  - `POST /api/admin-roles/accounts`
  - `DELETE /api/admin-roles/accounts/:userId`

---

## Backup & Restore

### Overview

The backup module provides full database backup, per-table backup, row-level backup, and restore functionality.

### Create Full Backup

- **Requires**: `backup` permission
- Creates a complete SQL dump of the database.
- Endpoint: `POST /api/backup/full`

### Create Table Backup

- **Requires**: `backup` permission
- Select specific tables to back up.
- Endpoint: `POST /api/backup/tables`

### Create Row-Level Backup

- **Requires**: `backup` permission
- Backup specific rows from a table.
- Endpoint: `POST /api/backup/rows`

### Restore Backup

- **Requires**: `backup` permission
- Upload a SQL backup file to restore the database.
- Endpoint: `POST /api/backup/restore`

### List Backups

- **Requires**: `backup` permission
- View all stored backup files with metadata.
- Endpoint: `GET /api/backup/`

### Download Backup

- **Requires**: `backup` permission
- Download a specific backup file.
- Endpoint: `GET /api/backup/:filename/download`

### Delete Backup

- **Requires**: `backup` permission
- Remove a stored backup file.
- Endpoint: `DELETE /api/backup/:filename`

### Preview Table Rows

- **Requires**: `backup` permission
- Preview the data in a table before backing up.
- Endpoint: `GET /api/backup/tables/:table/preview`

### List Database Tables

- **Requires**: `backup` permission
- View all tables in the database.
- Endpoint: `GET /api/backup/tables`

### Automatic Scheduled Backups

- A **node-cron** job runs daily at **midnight**.
- Creates a full backup automatically.
- Backups are stored in `back/backups/`.

---

## Query Console

### Overview

The query console allows executing raw SQL queries directly against the database. **Use with extreme caution.**

### Execute a Query

- **Requires**: `queryConsole` permission
- Enter a SQL query and execute.
- Results are returned as JSON.
- Endpoint: `POST /api/query-console/execute`

### Safety Guards

The query console includes built-in protections:

- **Blocked patterns**: Certain destructive SQL patterns are blocked (e.g., `DROP DATABASE`, `TRUNCATE` on protected tables).
- **Protected tables**: Specific tables cannot be modified through the console.
- **Query logging**: All executed queries are logged for audit purposes.

### Query Logs

- **Requires**: `queryConsole` permission
- View a history of all queries executed through the console.
- Shows: query text, executor, timestamp, success/failure.
- Endpoint: `GET /api/query-console/logs`

---

## Admin Settings

- Navigate to **Admin → Settings**.
- Configure system-wide settings:
  - Email/SMTP configuration (for OTP password reset).
  - Default role assignments.
  - System preferences.

---

## Background Jobs

### Attendance Cleanup (Every 15 Minutes)

- Automatically marks registrants as `missed` 30 minutes after event start.
- Creates history entries for missed events.
- Removes past events from upcoming lists.

### Backup Scheduler (Daily at Midnight)

- Creates a full database backup.
- Stores it in `back/backups/`.

---

## API Endpoints (Admin)

### Dashboard & System

| Method | Endpoint | Permission | Description |
|--------|----------|------------|-------------|
| GET | `/api/admin/dashboard` | `dashboard` | Dashboard statistics |
| GET | `/api/admin/logs` | `logs` | Admin activity logs |
| GET | `/api/admin/system-health` | `systemHealth` | System health metrics |
| POST | `/api/admin/create-admin` | superadmin | Create admin account |

### User Management

| Method | Endpoint | Permission | Description |
|--------|----------|------------|-------------|
| GET | `/api/users/` | superadmin, coordinator, organizer | List all users |
| POST | `/api/users/` | `manageUsers` | Create a user |
| PATCH | `/api/users/:id/role` | `manageUsers` | Change user role |
| PATCH | `/api/users/:id/status` | `manageUsers` | Suspend/activate user |
| DELETE | `/api/users/:id` | `manageUsers` | Delete a user |
| GET | `/api/users/:id` | authenticated | Get user by ID |

### Event Management

| Method | Endpoint | Permission | Description |
|--------|----------|------------|-------------|
| PATCH | `/api/events/:id/approve` | `verifyEvents` | Approve event |
| PATCH | `/api/events/:id/reject` | `verifyEvents` | Reject event |
| PUT | `/api/events/:id` | `manageEvents` | Edit any event |
| DELETE | `/api/events/:id` | `manageEvents` | Delete any event |

### Custom Roles

| Method | Endpoint | Permission | Description |
|--------|----------|------------|-------------|
| GET | `/api/admin-roles/schema` | authenticated | Permission schema |
| GET | `/api/admin-roles/` | superadmin | List custom roles |
| POST | `/api/admin-roles/` | superadmin | Create custom role |
| PUT | `/api/admin-roles/:id` | superadmin | Update custom role |
| DELETE | `/api/admin-roles/:id` | superadmin | Delete custom role |
| POST | `/api/admin-roles/assign` | superadmin | Assign role to user |
| DELETE | `/api/admin-roles/unassign/:userId/:roleId` | superadmin | Unassign role |
| GET | `/api/admin-roles/users` | superadmin | Users with roles |
| GET | `/api/admin-roles/user/:userId` | superadmin | User's roles |
| GET | `/api/admin-roles/:id` | superadmin | Get role by ID |
| GET | `/api/admin-roles/accounts` | superadmin | Admin accounts |
| POST | `/api/admin-roles/accounts` | superadmin | Create admin account |
| DELETE | `/api/admin-roles/accounts/:userId` | superadmin | Delete admin account |

### Backup

| Method | Endpoint | Permission | Description |
|--------|----------|------------|-------------|
| GET | `/api/backup/tables` | `backup` | List tables |
| GET | `/api/backup/tables/:table/preview` | `backup` | Preview table data |
| POST | `/api/backup/full` | `backup` | Full backup |
| POST | `/api/backup/tables` | `backup` | Table backup |
| POST | `/api/backup/rows` | `backup` | Row backup |
| POST | `/api/backup/restore` | `backup` | Restore backup |
| GET | `/api/backup/` | `backup` | List backups |
| GET | `/api/backup/:filename/download` | `backup` | Download backup |
| DELETE | `/api/backup/:filename` | `backup` | Delete backup |

### Query Console

| Method | Endpoint | Permission | Description |
|--------|----------|------------|-------------|
| POST | `/api/query-console/execute` | `queryConsole` | Execute SQL query |
| GET | `/api/query-console/logs` | `queryConsole` | Query history logs |
