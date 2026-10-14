# Security Documentation — UniPulse

## Overview

This document covers the security architecture, known vulnerabilities, and recommended fixes for the UniPulse Campus Event Management Platform. Issues are marked with severity levels and status indicators.

---

## Authentication & Authorization

### JWT Authentication

- **Implementation**: JSON Web Tokens via `jsonwebtoken` library.
- **Token expiration**: Configurable via `JWT_EXPIRES_IN` env var (default: `7d`).
- **Token storage**: Frontend stores JWT in `localStorage`.

### Password Hashing

- **Algorithm**: bcrypt with configurable rounds (default: `10`).
- **Implementation**: `back/src/utils/password.js`

### Role-Based Access Control (RBAC)

- Built-in roles: `student`, `organizer`, `superadmin`, `developer`, `coordinator`.
- Custom admin roles with granular permissions via `admin_roles` and `user_admin_roles` tables.
- Middleware: `authorize()` for role-based, `requirePermission()` for feature-based.

---

## Known Security Issues

### Critical

| # | Issue | Location | Recommendation |
|---|-------|----------|----------------|
| S1 | **Hardcoded JWT secret fallback** | `back/src/config/env.js:21` | The fallback `'unipulse_jwt_secret_key_2024'` is used when `JWT_SECRET` is not set. This means anyone running the server without setting this variable can forge tokens. | Remove the fallback entirely — throw an error at startup if `JWT_SECRET` is not set. Use a cryptographically random secret of at least 256 bits. |
| S2 | **Hardcoded crypto key fallback** | `back/src/config/env.js:27` | The fallback `'unipulse_default_crypto_key_32bytes!'` is used when `CRYPTO_KEY` is not set. | Remove the fallback — require `CRYPTO_KEY` to be set at startup. Generate via `crypto.randomBytes(32).toString('hex')`. |
| S3 | **No rate limiting** | `back/src/app.js` | No rate limiting middleware is applied. The `/api/auth/login` and `/api/auth/register` endpoints are vulnerable to brute-force and credential stuffing attacks. | Add `express-rate-limit` middleware. Apply stricter limits to auth endpoints (e.g., 5 attempts per 15 min for login). |
| S4 | **No account lockout** | `back/src/modules/auth/auth.service.js` | After repeated failed login attempts, there is no account lockout or CAPTCHA trigger. | Implement account lockout after 5 failed attempts, or add progressive delays / CAPTCHA. |
| S5 | **JWT stored in localStorage** | `front/src/context/AuthContext.jsx` | localStorage is accessible to any JavaScript running on the page, making tokens vulnerable to XSS attacks. | Consider using `httpOnly` cookies for token storage. If localStorage is kept, ensure strict CSP headers and sanitize all user inputs to prevent XSS. |
| S6 | **No CORS origin whitelist** | `back/src/app.js:31` | `app.use(cors())` allows all origins. Any website can make authenticated API calls. | Restrict CORS to known frontend origin(s): `cors({ origin: ['https://yourdomain.com'], credentials: true })`. |
| S7 | **SQL injection via query console** | `back/src/modules/queryConsole/queryConsole.service.js:35` | The query console executes raw SQL with only pattern-based guards. A determined admin with `queryConsole` permission can run `SELECT * FROM users` and extract all password hashes, or `UPDATE users SET role='superadmin'` on any account. The blocklist only catches `DROP DATABASE`/`DROP SCHEMA`/protected table drops. | Restrict query console to `SELECT`/`SHOW` statements only for developer role. Separate destructive operations into a separate higher-privilege tool. Never expose password hash columns. |

### High

| # | Issue | Location | Recommendation |
|---|-------|----------|----------------|
| S8 | **Error stack traces exposed** | `back/src/middlewares/errorMiddleware.js:11` | `err.stack` is logged but `err.message` is returned to the client. Some errors may leak internal paths, SQL snippets, or environment details. | Sanitize error messages in production. Only return generic messages for 500 errors. Never expose SQL errors to the client. |
| S9 | **No input sanitization beyond validation** | `back/src/middlewares/validationMiddleware.js` | Validation checks for type and length but does not sanitize HTML/script content in fields like `description`, `bio`, or `fullName`. Stored XSS is possible if these values are rendered without escaping. | Use a library like `DOMPurify` on the frontend and `sanitize-html` on the backend. Ensure React's default escaping is not bypassed with `dangerouslySetInnerHTML`. |
| S10 | **OTP brute-force vulnerability** | `back/src/modules/auth/otp.controller.js` | OTP codes are 6-digit numbers (1,000,000 combinations). If no rate limiting exists on the `/verify` or `/reset-password` endpoints, OTPs can be brute-forced. | Add rate limiting to OTP verification endpoints. Limit to 5 attempts per OTP. Expire OTPs after 5 minutes. |
| S11 | **No CSRF protection** | `back/src/app.js` | The API does not implement CSRF tokens. While JWT in localStorage is somewhat resilient to CSRF, any future cookie-based auth would be vulnerable. | If switching to cookie-based tokens, add `csurf` middleware or SameSite cookie attribute. |
| S12 | **File upload accepts SVG** | `back/src/middlewares/uploadMiddleware.js:21` | SVG files can contain JavaScript (XSS via image). Even with the extension/mime check, a malicious SVG can execute scripts when rendered in-browser. | Either strip SVG from allowed types, or serve uploaded SVGs with `Content-Disposition: attachment` and a strict `Content-Security-Policy` that disallows script execution in SVG context. |
| S13 | **Role switching without re-authentication** | `back/src/modules/auth/auth.service.js:84-111` | `switchRole` allows toggling between `student` and `organizer` without password confirmation. If an attacker steals a token, they can switch to organizer and create events. | Require password re-entry for role switches. Restrict role switching to admin-granted roles only. |
| S14 | **Authorization middleware order vulnerability** | `back/src/modules/users/users.routes.js:24-33` | The `GET /api/users/` route uses an inline role check that does not load `adminRoles`, while other routes use `requirePermission()`. A user with a custom role that has `manageUsers` could be blocked from listing users because the inline check only looks at `req.user.role`. | Unify authorization — use `requirePermission('manageUsers')` consistently across all user management routes. |

### Medium

| # | Issue | Location | Recommendation |
|---|-------|----------|----------------|
| S15 | **No security headers** | `back/src/app.js` | No `helmet` or custom security headers are set. Missing: `X-Content-Type-Options`, `X-Frame-Options`, `Strict-Transport-Security`, `Content-Security-Policy`, `X-XSS-Protection`. | Add `helmet` middleware: `app.use(helmet())`. |
| S16 | **No request size limits** | `back/src/app.js` | `express.json()` uses the default 100kb limit. While generally sufficient, the upload route bypasses this with multer. Ensure the JSON body limit is adequate but not excessive. | Explicitly set `express.json({ limit: '100kb' })` and `express.urlencoded({ limit: '100kb', extended: true })`. |
| S17 | **No HTTPS enforcement** | `back/src/app.js` | The server does not redirect HTTP to HTTPS or set HSTS headers. | In production, enforce HTTPS at the reverse proxy (nginx) or add redirect middleware. Set `Strict-Transport-Security` header. |
| S18 | **Verbose API error messages** | `back/src/middlewares/errorMiddleware.js:14` | The error handler returns `err.message` which may include database error details (e.g., SQL syntax errors revealing table structure). | In production (`NODE_ENV=production`), replace detailed messages with generic ones. |
| S19 | **Backup files not access-controlled on disk** | `back/src/modules/backup/backup.controller.js` | Backup SQL files are stored in `back/backups/` and served via a download endpoint. Ensure this directory is not directly accessible via the static file server. | Verify `/backups/` is not under the `/uploads` static route. Add authentication middleware to all backup routes (already done). Consider encrypting backup files at rest. |
| S20 | **Activity logs not tamper-proof** | `back/src/modules/activityLogs/activityLogs.service.js` | Activity logs are stored in the database and can be modified by anyone with database access. They are not cryptographically signed. | This is acceptable for audit purposes within a trusted database, but consider append-only logging or log signing for compliance requirements. |

### Low

| # | Issue | Location | Recommendation |
|---|-------|----------|----------------|
| S21 | **No token refresh mechanism** | `back/src/utils/jwt.js` | Tokens are valid for 7 days with no refresh token mechanism. A stolen token remains valid for the full duration. | Implement short-lived access tokens (15-30 min) with refresh tokens, or provide a token revocation list. |
| S22 | **No concurrent session management** | Auth system | Multiple devices can use the same account simultaneously. There is no session tracking or ability to invalidate specific sessions. | Store active sessions in the database. Allow users to view and revoke active sessions. |
| S23 | **Username enumeration** | `back/src/modules/auth/auth.service.js:9-19` | Registration returns distinct error messages for "Email already registered" vs "Username already taken", allowing enumeration of existing accounts. | Use a generic "Registration failed" message, or always return the same message for both cases. |
| S24 | **No password strength requirements** | `back/src/middlewares/validationMiddleware.js:54-58` | Minimum length of 6 is the only requirement. No complexity rules (uppercase, numbers, symbols). | Increase minimum to 8-12 characters. Consider using a password strength library like `zxcvbn`. |

---

## Environment Variables Security Checklist

Ensure the following are set in production (never rely on fallbacks):

| Variable | Purpose | Current Fallback (INSECURE) |
|----------|---------|-----------------------------|
| `JWT_SECRET` | JWT signing key | `unipulse_jwt_secret_key_2024` |
| `CRYPTO_KEY` | AES encryption key | `unipulse_default_crypto_key_32bytes!` |
| `DB_PASSWORD` | Database password | `''` (empty) |
| `EMAIL_PASS` | SMTP password | `''` (empty) |
| `BCRYPT_ROUNDS` | bcrypt cost factor | `10` |
| `NODE_ENV` | Environment flag | `development` |

**Recommendation**: Remove all fallback secrets from `env.js`. Throw an error if required variables are missing.

---

## Security Hardening Roadmap

### Phase 1 — Immediate (Before Production)

1. Remove all hardcoded secret fallbacks from `env.js`.
2. Add `helmet` for security headers.
3. Add `express-rate-limit` to auth endpoints.
4. Restrict CORS to frontend origin.
5. Sanitize error messages in production mode.
6. Rate-limit OTP verification attempts.

### Phase 2 — Short Term

7. Add account lockout after failed login attempts.
8. Require password re-entry for role switching.
9. Remove SVG from allowed upload types or serve with `Content-Disposition: attachment`.
10. Unify authorization middleware (fix inline role check in `GET /api/users/`).
11. Add `Content-Security-Policy` header.
12. Implement HTTPS enforcement in production.

### Phase 3 — Long Term

13. Migrate token storage from localStorage to httpOnly cookies.
14. Implement short-lived access tokens + refresh tokens.
15. Add session management and concurrent session controls.
16. Restrict query console to SELECT-only for developer role.
17. Add `csurf` or equivalent CSRF protection if using cookies.
18. Consider encrypting database backups at rest.
19. Add security audit logging (separate from activity logs).
20. Conduct a penetration test.
