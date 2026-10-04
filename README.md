# 🔐 SecureSphere Enterprise IAM

**SecureSphere Enterprise IAM** is a secure, scalable **Enterprise Identity and Access Management (IAM)** system designed to manage users, roles, permissions, authentication, multi-factor authentication, and security audit logs.

The system provides **JWT-based authentication, Role-Based Access Control (RBAC), TOTP-based MFA, backup codes, audit logging, account security, and PostgreSQL database integration**.

---

## 🚀 Live Demo

### 🌐 Frontend

**SecureSphere IAM**

https://securesphereiam.netlify.app

### ⚙️ Backend API

**Enterprise IAM API**

https://enterprise-identity-iam.onrender.com

### 💻 GitHub Repository

https://github.com/vamsi-bear/Enterprise-Identity-IAM

---

# 📌 Project Overview

Modern enterprise applications require secure identity management to control:

* Who can access the system
* What resources users can access
* What actions users are allowed to perform
* How authentication is verified
* How security events are monitored

SecureSphere Enterprise IAM provides a centralized identity and access management platform for:

* 👤 User Management
* 🔑 Secure Authentication
* 🛡️ Role-Based Access Control
* 🔐 Multi-Factor Authentication
* 🎫 JWT-Based Authorization
* 🔑 MFA Backup Codes
* 📋 Audit Logging
* 🔒 Account Security Controls
* 🗄️ PostgreSQL Database Management
* 🌐 RESTful API Architecture

The project follows a modular backend architecture using **Node.js and Express.js**, with **PostgreSQL** as the relational database.

---

# ✨ Key Features

## 👤 User Management

Administrators can manage users through the IAM dashboard.

Supported operations include:

* View users
* Create users
* Update users
* Delete users
* Assign roles
* View account status
* Manage authentication status

---

## 🔑 Authentication

Secure authentication is implemented using:

* Email/password authentication
* Password hashing
* JSON Web Tokens (JWT)
* Authentication middleware
* Account status validation
* Failed login tracking
* Account locking support

### Authentication Flow

```text
User
 │
 ▼
Email + Password
 │
 ▼
Backend Authentication
 │
 ├── Invalid → Access Denied
 │
 └── Valid
       │
       ▼
   MFA Required?
       │
   ┌───┴────┐
   │        │
  Yes       No
   │        │
   ▼        ▼
  MFA      JWT
   │
   ▼
Final JWT
   │
   ▼
Dashboard
```

---

# 🔐 Multi-Factor Authentication

SecureSphere supports **TOTP-based Multi-Factor Authentication (MFA)**.

Compatible authenticator applications include:

* Google Authenticator
* Microsoft Authenticator
* Other TOTP-compatible authenticator applications

## MFA Setup

```text
Login
  ↓
Enable MFA
  ↓
Generate Secret
  ↓
Generate QR Code
  ↓
Scan using Authenticator App
  ↓
Enter 6-digit TOTP
  ↓
Verify
  ↓
MFA Enabled
```

MFA adds an additional authentication layer beyond the user's password.

---

# 🔑 MFA Backup Codes

Users can generate backup codes in case they cannot access their authenticator application.

Features include:

* Generate backup codes
* Each code can be used only once
* Previous codes are invalidated when new codes are generated
* Backup codes are stored securely as hashes
* Backup-code authentication generates a final authenticated JWT
* Remaining backup-code count can be tracked

### Backup Code Flow

```text
Backup Code
     ↓
Hash Code
     ↓
Compare with Database
     ↓
Valid?
 ┌───┴────┐
Yes       No
 │         │
 ▼         ▼
Mark      Reject
Used      Login
 │
 ▼
Generate JWT
 │
 ▼
Dashboard
```

---

# 🛡️ Role-Based Access Control

SecureSphere uses **Role-Based Access Control (RBAC)** to control access to protected resources.

## Available Roles

| Role          | Description                 |
| ------------- | --------------------------- |
| `EMPLOYEE`    | Standard employee access    |
| `DEVELOPER`   | Developer-level access      |
| `ADMIN`       | Administrative access       |
| `SUPER_ADMIN` | Highest administrative role |

> **Note:** The current implementation provides the configured permissions shown below. `ADMIN` and `SUPER_ADMIN` currently have the same six permissions in the seeded permission model. They can be differentiated further in future versions using additional critical-level permissions.

---

# 🔑 Permissions

The current system supports the following permissions:

| Permission    | Description     |
| ------------- | --------------- |
| `USER_READ`   | View users      |
| `USER_CREATE` | Create users    |
| `USER_UPDATE` | Update users    |
| `USER_DELETE` | Delete users    |
| `ROLE_ASSIGN` | Assign roles    |
| `AUDIT_READ`  | View audit logs |

### Current RBAC Mapping

```text
ADMIN
 │
 ├── USER_READ
 ├── USER_CREATE
 ├── USER_UPDATE
 ├── USER_DELETE
 ├── ROLE_ASSIGN
 └── AUDIT_READ
```

```text
DEVELOPER
 │
 └── USER_READ
```

```text
EMPLOYEE
 │
 └── USER_READ
```

---

# 🔗 RBAC Model

```text
User
 │
 ▼
User Role
 │
 ▼
Role
 │
 ▼
Role Permissions
 │
 ▼
Permission
 │
 ▼
Protected API Resource
```

Authorization is enforced at the backend rather than relying only on frontend UI restrictions.

For example, a user without `USER_CREATE` permission cannot create a user even if they attempt to call the protected API directly.

---

# 📋 Audit Logging

SecureSphere records important security and administrative events.

Examples include:

* Login attempts
* MFA verification
* MFA failures
* MFA setup
* Backup-code generation
* Backup-code authentication
* Role assignments
* User management actions
* Access-denied events

Audit logs can contain information such as:

```text
User
Action
Resource
Resource ID
Result
Risk Level
IP Address
User Agent
Metadata
Timestamp
```

### Possible Results

```text
SUCCESS
FAILED
DENIED
```

### Risk Levels

```text
LOW
MEDIUM
HIGH
CRITICAL
```

Audit logging provides traceability for important authentication, authorization, and administrative operations.

---

# 🔒 Security Features

SecureSphere includes multiple security mechanisms:

* 🔐 JWT authentication
* 🔑 Password hashing
* 🛡️ Role-Based Access Control
* 🔒 Multi-Factor Authentication
* 🎫 MFA backup codes
* 🪪 Authorization middleware
* 🛡️ Helmet security headers
* 🌐 CORS protection
* 🚦 API rate limiting
* 🔐 Account locking
* 📋 Audit logging
* 🗄️ Parameterized PostgreSQL queries
* 🔑 Environment-based secrets
* 🔒 Least-privilege authorization

---

# 🏗️ System Architecture

```text
                         ┌──────────────────────┐
                         │     User Browser     │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │   Netlify Frontend   │
                         │    HTML / CSS / JS   │
                         └──────────┬───────────┘
                                    │
                               HTTPS / REST
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │    Render Backend    │
                         │ Node.js + Express.js │
                         └──────────┬───────────┘
                                    │
             ┌──────────────────────┼──────────────────────┐
             │                      │                      │
             ▼                      ▼                      ▼
      Authentication            RBAC / MFA          Audit Logging
             │                      │                      │
             └──────────────────────┼──────────────────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │   Neon PostgreSQL    │
                         │      Database        │
                         └──────────────────────┘
```

---

# 🧰 Technologies Used

## Frontend

* HTML5
* CSS3
* JavaScript
* Responsive Web Design
* Fetch API
* Local Storage

## Backend

* Node.js
* Express.js
* JSON Web Token (JWT)
* bcrypt
* Speakeasy
* QRCode
* Helmet
* CORS
* express-rate-limit

## Database

* PostgreSQL
* UUID
* JSONB
* Foreign Keys
* Constraints
* Transactions

## Development Tools

* Visual Studio Code
* Git
* GitHub
* npm
* Nodemon
* PostgreSQL
* Postman
* cURL

## Deployment

* Netlify — Frontend
* Render — Backend API
* Neon — PostgreSQL Database

---

# 📂 Project Structure

```text
Enterprise-Identity-IAM/
│
├── backend/
│   │
│   ├── src/
│   │   │
│   │   ├── config/
│   │   │   └── database.js
│   │   │
│   │   ├── controllers/
│   │   │   ├── authController.js
│   │   │   ├── mfaController.js
│   │   │   ├── roleController.js
│   │   │   ├── userController.js
│   │   │   └── auditController.js
│   │   │
│   │   ├── middleware/
│   │   │   └── authMiddleware.js
│   │   │
│   │   ├── routes/
│   │   │   ├── authRoutes.js
│   │   │   ├── mfaRoutes.js
│   │   │   ├── userRoutes.js
│   │   │   ├── roleRoutes.js
│   │   │   └── auditRoutes.js
│   │   │
│   │   └── server.js
│   │
│   ├── package.json
│   └── .env
│
├── frontend/
│   ├── login.html
│   ├── mfa.html
│   ├── dashboard.html
│   └── ...
│
├── database/
│   └── schema.sql
│
├── .gitignore
└── README.md
```

> `.env` is used locally but must never be committed to GitHub.

---

# 🗄️ Database Design

The PostgreSQL database contains tables for identity, authorization, authentication, MFA, auditing, applications, policies, and security events.

## Main Tables

```text
users
roles
permissions
user_roles
role_permissions
groups
group_members
applications
application_permissions
policies
policy_statements
mfa_credentials
backup_codes
sessions
login_attempts
audit_logs
security_events
```

## Core Relationship

```text
users
 │
 ▼
user_roles
 │
 ▼
roles
 │
 ▼
role_permissions
 │
 ▼
permissions
```

## MFA Relationship

```text
users
 │
 ├── mfa_credentials
 │
 └── backup_codes
```

---

# 🌐 API Endpoints

## Authentication

### Register

```http
POST /api/auth/register
```

### Login

```http
POST /api/auth/login
```

---

# 👤 Users

### Get Current User

```http
GET /api/users/me
```

### Get All Users

```http
GET /api/users
```

### Create User

```http
POST /api/users
```

### Update User

```http
PUT /api/users/:userId
```

### Delete User

```http
DELETE /api/users/:userId
```

### Assign Role

```http
PUT /api/users/:userId/role
```

---

# 🛡️ Roles

### Get Roles

```http
GET /api/roles
```

---

# 🔐 MFA

### Setup MFA

```http
POST /api/mfa/setup
```

### Verify MFA

```http
POST /api/mfa/verify
```

### Verify Login MFA

```http
POST /api/mfa/verify-login
```

### Generate Backup Codes

```http
POST /api/mfa/backup-codes
```

### Verify Backup Code

```http
POST /api/mfa/verify-backup
```

### Disable MFA

```http
DELETE /api/mfa/disable
```

---

# 📋 Audit Logs

### Get Audit Logs

```http
GET /api/audit-logs
```

Example:

```http
GET /api/audit-logs?limit=5
```

---

# ❤️ Health Check

```http
GET /api/health
```

Example response:

```json
{
  "status": "success",
  "message": "Enterprise IAM API is running",
  "database": "connected"
}
```

---

# ⚙️ Local Installation

## 1. Clone the Repository

```bash
git clone https://github.com/vamsi-bear/Enterprise-Identity-IAM.git
```

Navigate into the project:

```bash
cd Enterprise-Identity-IAM
```

---

# 2. Install Backend Dependencies

```bash
cd backend
npm install
```

---

# 3. Configure Environment Variables

Create:

```text
backend/.env
```

Example:

```env
PORT=5000

DATABASE_URL=postgresql://postgres:YOUR_PASSWORD@localhost:5433/enterprise_iam

JWT_SECRET=YOUR_SECRET_KEY

JWT_EXPIRES_IN=1h

CLIENT_ORIGIN=http://localhost:5500
```

### ⚠️ Important

Never commit `.env` to GitHub.

Your `.gitignore` should contain:

```gitignore
node_modules/
.env
*.log
.DS_Store
Thumbs.db
```

---

# 🗄️ Database Setup

Make sure PostgreSQL is running.

Create the database:

```sql
CREATE DATABASE enterprise_iam;
```

Import the schema:

```bash
psql -U postgres -h localhost -p 5433 -d enterprise_iam -f database/schema.sql
```

If `psql` is not available in PATH on Windows:

```cmd
"C:\Program Files\PostgreSQL\18\bin\psql.exe" -U postgres -h localhost -p 5433 -d enterprise_iam -f database\schema.sql
```

---

# ▶️ Run the Backend

From the `backend` directory:

```bash
npm run dev
```

The API will run on:

```text
http://localhost:5000
```

---

# 🌐 Run the Frontend

The frontend consists of static HTML, CSS, and JavaScript files.

You can use:

* VS Code Live Server
* Netlify
* Any static web server

For VS Code Live Server, open:

```text
frontend/login.html
```

and launch it using **Live Server**.

---

# 🧪 Testing

The system can be tested using:

* Browser
* Postman
* cURL
* PostgreSQL CLI

## Authentication Test

```text
Register
   ↓
Login
   ↓
Password Validation
   ↓
MFA
   ↓
JWT
   ↓
Dashboard
```

## RBAC Test

Example employee/developer-level access:

```text
User
 ↓
USER_READ
 ↓
Can view users
```

Administrative access:

```text
ADMIN
 │
 ├── USER_READ
 ├── USER_CREATE
 ├── USER_UPDATE
 ├── USER_DELETE
 ├── ROLE_ASSIGN
 └── AUDIT_READ
```

A user without the required permission should receive an authorization failure when directly calling a protected API.

---

# 🔑 MFA Testing

```text
Login
  ↓
MFA Required
  ↓
Enter TOTP
  ↓
MFA Verification
  ↓
Authenticated JWT
  ↓
Dashboard
```

---

# 🔐 Backup Code Testing

```text
Generate Backup Codes
        ↓
Use Code
        ↓
Authentication Successful
        ↓
Code Marked Used
        ↓
Reuse Same Code
        ↓
Authentication Rejected
```

---

# 🚀 Production Deployment

## Frontend

The frontend is deployed using **Netlify**.

Production URL:

```text
https://securesphereiam.netlify.app
```

## Backend

The Express.js backend is deployed using **Render**.

Production API:

```text
https://enterprise-identity-iam.onrender.com
```

## Database

The production PostgreSQL database is hosted using **Neon PostgreSQL**.

The Render backend connects to the Neon database through the production `DATABASE_URL` environment variable.

### Production Architecture

```text
GitHub
  │
  ├──────────────► Netlify
  │                  │
  │                  ▼
  │              Frontend
  │                  │
  │               HTTPS
  │                  │
  │                  ▼
  └──────────────► Render
                     │
                     ▼
                Node.js API
                     │
                     ▼
                Neon PostgreSQL
```

---

# ✅ Production Verification

The deployed application has been designed and tested around the following security flow:

```text
User Registration
       ↓
User Login
       ↓
JWT Authentication
       ↓
MFA Setup
       ↓
TOTP Verification
       ↓
Authenticated Dashboard
       ↓
Role Assignment
       ↓
Permission Enforcement
       ↓
Audit Logging
       ↓
Logout
```

The RBAC implementation also prevents users without the required permission from performing protected administrative operations directly through the API.

---

# 🔐 Production Security

For production deployments:

* Never expose `.env` files
* Use strong JWT secrets
* Use HTTPS
* Use secure database credentials
* Configure appropriate CORS origins
* Use API rate limiting
* Keep dependencies updated
* Rotate compromised secrets
* Never store plaintext passwords
* Never store plaintext backup codes
* Monitor audit logs
* Use least-privilege access
* Protect database credentials
* Avoid exposing sensitive authentication secrets in source code

---

# 📊 Security Model

SecureSphere follows the principle of **least privilege**.

```text
Authentication
      ↓
Identity Verification
      ↓
MFA Verification
      ↓
Authorization
      ↓
Permission Check
      ↓
Resource Access
      ↓
Audit Event
```

A user must successfully authenticate before accessing protected resources.

Authorization middleware then verifies whether the authenticated user has the required permission for the requested operation.

---

# 🎯 Project Objectives

The main objectives of SecureSphere Enterprise IAM are:

1. Provide secure centralized authentication.
2. Implement Role-Based Access Control.
3. Provide Multi-Factor Authentication.
4. Provide secure MFA backup mechanisms.
5. Manage enterprise users and roles.
6. Record security and administrative events.
7. Protect APIs using authentication and authorization middleware.
8. Provide a scalable PostgreSQL-based identity architecture.
9. Demonstrate secure full-stack application deployment.
10. Demonstrate practical enterprise security concepts.

---

# 💡 Why This Project Matters

Identity and access management is a core security component of modern software systems.

SecureSphere demonstrates practical implementation of:

* Authentication
* Authorization
* Identity management
* RBAC
* MFA
* JWT security
* API security
* Database security
* Auditability
* Least-privilege access
* Full-stack deployment

The project is therefore suitable as an **academic project, portfolio project, security-focused application, and software engineering demonstration**.

---

# 🔮 Future Enhancements

Possible future improvements include:

* OAuth 2.0 / OpenID Connect
* Google/GitHub login
* WebAuthn / Passkeys
* Email-based account verification
* Password reset functionality
* Advanced policy engine
* Group-based authorization
* Application-level access management
* Session management dashboard
* Security analytics
* Login anomaly detection
* SIEM integration
* Redis-based rate limiting
* Automated security alerts
* Admin notification system
* More granular SUPER_ADMIN permissions
* Device/session risk analysis
* Security dashboard and analytics

---

# 📚 Learning Outcomes

This project provided practical experience with:

* Full-stack web development
* Node.js and Express.js
* PostgreSQL database design
* REST API development
* JWT authentication
* RBAC implementation
* TOTP-based MFA
* Backup-code authentication
* Middleware-based authorization
* API security
* Audit logging
* Environment configuration
* Git and GitHub
* Netlify deployment
* Render deployment
* Neon PostgreSQL
* Production debugging and testing

---

# 👨‍💻 Author

**Anga Vamsi**

B.Tech — Computer Science

### GitHub

https://github.com/vamsi-bear

---

# 📄 License

This project is intended for **educational, academic, portfolio, and demonstration purposes**.

---

# ⭐ Support

If you find this project useful, consider giving the repository a ⭐ on GitHub.

---

<div align="center">

### 🔐 SecureSphere Enterprise IAM

**Secure Identity. Controlled Access. Trusted Systems.**

</div>
