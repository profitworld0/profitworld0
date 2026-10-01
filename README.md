# Profit World — Investment Management & Demonstration Platform

**Document Version:** 1.0  
**Compliance Architecture:** Immutable double-entry financial ledger, idempotent returns worker, protected receipt evidence verification, and anti-double-spending balance holds.

---

## 🌟 Key Features Implemented

### 1. 🛡️ Authentication & Role-Based Access Control (RBAC)
- **Member Portal & Admin Console separation**:
  - Secure bcrypt password hashing.
  - JWT token session management via secure HTTP-only cookies.
  - Distinct `/dashboard/*` (Member) and `/admin/*` (Compliance/Admin) route guards.
  - Quick demo account switchers on the login page.

### 2. 💎 Dynamic 9-Product Catalogue (SRS Section 16)
- Fully dynamic catalogue with 9 pre-configured demonstration tiers (Plan 1 through Plan 9: Rs. 300 to Rs. 160,000 with 60-day lifecycles).
- Admin Product Management: Add, edit rates/limits, update risk disclosures, and safely archive products without breaking historical participations.

### 3. 🧾 Investment Application & Payment Evidence Workflow (SRS Section 18)
- 4 supported payment channels: **Bank Wire**, **EasyPaisa**, **JazzCash**, and **USDT (TRC20)** with configurable destination details.
- Receipt screenshot/PDF upload with file-type/size security validation.
- Uniqueness constraints to prevent duplicate payment references.
- Status lifecycle: `PENDING` $\rightarrow$ `APPROVED` or `REJECTED` (with mandatory reason).

### 4. 📒 Immutable Financial Ledger & Daily Accrual Engine (SRS Section 19)
- Double-entry append-only ledger (`INVESTMENT_DEPOSIT`, `DAILY_RETURN`, `WITHDRAWAL_HOLD`, `WITHDRAWAL_PAID`, `WITHDRAWAL_REFUND`, `REFERRAL_BONUS`, `MANUAL_ADJUSTMENT`).
- Balance breakdown: **Available Balance**, **Held Balance**, and **Total Account Capital**.
- **Idempotent Daily Returns Worker**: Can be triggered manually by Admin or executed automatically without duplicate credits.

### 5. 💳 Anti-Double-Spend Withdrawal Engine (SRS Section 20)
- Configurable minimum withdrawal threshold (default: Rs. 50).
- Immediate balance hold reservation upon withdrawal submission.
- Admin Review:
  - **Disburse / Pay**: Deducts from held balance, marks `PAID`, logs payment transaction ID.
  - **Reject**: Restores held funds back to the member's available balance with mandatory rejection notes.

### 6. 👥 Referral & Community Rewards (SRS Section 21)
- Auto-generated unique referral codes and shareable links (`/register?ref=CODE`).
- 5% prototype commission calculation upon verified application approval.
- Anti-self-referral prevention and referral statistics tracker.

### 7. 🔒 Compliance & Audit Logging (SRS Section 9 & Section 22)
- Tamper-evident administrative audit trail tracking every financial approval, rejection, manual adjustment, and configuration change with admin timestamps and metadata.
- KYC profile verification module.

---

## 🚀 Quick Start Guide

### 1. Start Application Server
```bash
npm run dev
# Or for production:
npm run start
```
App will run at: `http://localhost:3000`

### 2. Demo Login Credentials

| Role | Email | Password | Access Level |
| :--- | :--- | :--- | :--- |
| **Administrator** | `admin@profitworld.com` | `Admin@ProfitWorld2026!` | Full Admin Panel (`/admin`) |
| **Demo Member** | `member@profitworld.com` | `Member123!@#` | Member Dashboard (`/dashboard`) |

*(One-click demo login buttons are also available directly on the `/login` page)*

---

## 🗄️ Database Management
- **Database File:** `dev.db` (SQLite with Prisma ORM)
- **Sync Schema:** `npm run db:push`
- **Re-seed Data:** `npm run db:seed`
- **Prisma Visual Studio:** `npm run db:studio`
