<div align="center">

<br/>

```
██╗  ██╗██████╗ ███╗   ███╗███████╗
██║  ██║██╔══██╗████╗ ████║██╔════╝
███████║██████╔╝██╔████╔██║███████╗
██╔══██║██╔══██╗██║╚██╔╝██║╚════██║
██║  ██║██║  ██║██║ ╚═╝ ██║███████║
╚═╝  ╚═╝╚═╝  ╚═╝╚═╝     ╚═╝╚══════╝
```

### **Kawādir — Human Resource Management System**
*Enterprise-grade HR platform. Clean Architecture. AI-powered.*

<br/>

[![.NET](https://img.shields.io/badge/.NET_8-512BD4?style=for-the-badge&logo=dotnet&logoColor=white)](https://dotnet.microsoft.com)
[![Angular](https://img.shields.io/badge/Angular_19-DD0031?style=for-the-badge&logo=angular&logoColor=white)](https://angular.io)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)](https://postgresql.org)
[![SignalR](https://img.shields.io/badge/SignalR-512BD4?style=for-the-badge&logo=dotnet&logoColor=white)](https://dotnet.microsoft.com/apps/aspnet/signalr)
[![Cloudinary](https://img.shields.io/badge/Cloudinary-3448C5?style=for-the-badge&logo=cloudinary&logoColor=white)](https://cloudinary.com)

[![JWT](https://img.shields.io/badge/JWT-000000?style=for-the-badge&logo=jsonwebtokens&logoColor=white)](https://jwt.io)
[![AutoMapper](https://img.shields.io/badge/AutoMapper-BE4B48?style=for-the-badge&logoColor=white)](https://automapper.org)
[![EF Core](https://img.shields.io/badge/EF_Core_9-512BD4?style=for-the-badge&logo=dotnet&logoColor=white)](https://docs.microsoft.com/ef)
[![Swagger](https://img.shields.io/badge/Swagger-85EA2D?style=for-the-badge&logo=swagger&logoColor=black)](https://swagger.io)
[![PWA](https://img.shields.io/badge/PWA-5A0FC8?style=for-the-badge&logo=pwa&logoColor=white)](https://web.dev/progressive-web-apps)

<br/>

> 🎓 **Graduation Project** — Computer Science · Al-Balqa Applied University · 2026
>
> 👨‍💻 **Abedalqader Alfaqeeh**

<br/>

[📖 API Reference](#-api-endpoints) · [🏗 Architecture](#-architecture) · [🤖 AI Assistant](#-ai-assistant) · [🚀 Quick Start : https://kawadir-hrms.pages.dev/](https://kawadir-hrms.pages.dev/)

<br/>

---

</div>

## 🌟 What is Kawādir?

**Kawādir** is a production-ready Human Resource Management System that covers the complete employee lifecycle — from onboarding to payroll — all secured with role-based access control and powered by an intelligent AI assistant.

It was built from the ground up using **Clean Architecture** principles on **.NET 8**, with a reactive **Angular 19** PWA frontend, real-time communication via **SignalR**, and an AI layer running on **Groq Cloud** (`llama-3.3-70b-versatile`).

<br/>

### 🗂 What's Inside

| Module | Description |
|---|---|
| 🔐 **Auth & Security** | JWT authentication · BCrypt hashing · Role-based access · Session management |
| 👥 **Employee Management** | Full CRUD · Custom ID generation · Profile pictures with approval workflow |
| 🏢 **Departments & Positions** | Hierarchical org structure · Salary ranges per position |
| 📅 **Leave Management** | Multi-type requests · Overlap detection · Annual balance tracking · Auto-reset |
| ⏰ **Attendance Tracking** | Clock-in/out · Stale session auto-close · QR-code support |
| 💰 **Salary & Payroll** | Monthly records · Auto-applied bonuses & penalties · Unique constraint per period |
| 🎯 **Payroll Adjustments** | Bonuses and penalties that auto-apply on next salary creation |
| 📅 **Meetings** | Schedule · Google Meet link auto-generation · Conflict detection |
| 📢 **Announcements** | Targeted or company-wide · Priority levels · Expiry dates |
| 🔔 **Notifications** | Real-time via SignalR · Per-event types · Mark as read / delete |
| 📧 **Email Service** | SMTP via MailKit · Beautiful HTML templates for every HR event |
| 🤖 **AI Assistant** | 3 modes: Normal, DeepThink (tool-calling), Executive (can act on data) |
| 🖼 **Profile Pictures** | Upload → HR approval → Applied · Cloudinary storage |
| 📊 **Dashboard** | Live statistics · Charts · PDF system report export |

<br/>

---

## 🏗 Architecture

Kawādir follows **Clean Architecture** strictly — dependencies only point inward, and the domain layer has zero external dependencies.

```
╔══════════════════════════════════════════════════════════════╗
║                       HRMS_API                               ║
║                  Presentation Layer                          ║
║   Controllers · Middleware · Filters · SignalR Hubs          ║
╚═══════════════════════════╦══════════════════════════════════╝
                            ║  depends on ↓
╔═══════════════════════════╩══════════════════════════════════╗
║                      Application                             ║
║                   Business Logic Layer                       ║
║     Services · DTOs · Interfaces · AutoMapper · Settings     ║
╚═══════════════════════════╦══════════════════════════════════╝
                            ║  depends on ↓
╔═══════════════════════════╩══════════════════════════════════╗
║                    Infrastructure                            ║
║                   Data Access Layer                          ║
║  AppDbContext · Repositories · UnitOfWork · Cloudinary       ║
║  EmailService · HrAiService · MeetingService · SignalR Hub   ║
╚═══════════════════════════╦══════════════════════════════════╝
                            ║  depends on ↓
╔═══════════════════════════╩══════════════════════════════════╗
║                       Domain                                 ║
║                     Core Layer                               ║
║          Entities · Enums · Interfaces                       ║
║           ⬆ zero external dependencies                       ║
╚══════════════════════════════════════════════════════════════╝
```

### Patterns & Principles

| Pattern | Where | Why |
|---|---|---|
| **Generic Repository** | `Infrastructure` | Single `Repository<T>()` — no duplication across 10+ entities |
| **Unit of Work** | `Infrastructure` | Wraps all repositories + single `SaveChangesAsync()` |
| **CQRS-lite** | `Application` | Separate DTOs for read vs. write (Create/Update/Response) |
| **Strategy (AI Modes)** | `HrAiService` | Swappable behavior: Normal, DeepThink, Executive |
| **Observer** | `SignalR + TokenTracker` | Broadcast token usage updates to all connected clients |
| **Background Service** | `LeaveResetBackgroundService` | Annual leave balance reset every January 1st |
| **Unified Response** | `ApiResponse<T>` | Every endpoint returns the same shape — predictable for the frontend |
| **Global Exception Handler** | `ExceptionHandlingMiddleware` | One place catches all unhandled exceptions, maps to HTTP codes |

<br/>

---

## 🛠 Tech Stack

### Backend
| Technology | Version | Role |
|---|---|---|
| **ASP.NET Core** | .NET 8 | Web API framework |
| **Entity Framework Core** | 9.x | ORM + Code-first migrations |
| **PostgreSQL** | 15+ | Primary relational database |
| **SignalR** | 8.x | Real-time bidirectional communication |
| **JWT Bearer** | 8.x | Stateless authentication |
| **BCrypt.Net** | 4.x | Secure password hashing |
| **AutoMapper** | 16.x | Entity ↔ DTO mapping profiles |
| **MailKit / MimeKit** | 4.x | SMTP email delivery with HTML templates |
| **Cloudinary SDK** | 1.x | Cloud image storage & face-crop transformation |
| **Groq Cloud API** | — | LLM backend (`llama-3.3-70b-versatile`) |
| **Swagger / OpenAPI** | 6.x | Interactive API documentation |

### Frontend
| Technology | Role |
|---|---|
| **Angular 19** | SPA framework (standalone components) |
| **TypeScript 5.7** | Type-safe code across all layers |
| **Bootstrap 5 + Bootstrap Icons** | Responsive UI |
| **Chart.js** | Dashboard charts (doughnut, line) |
| **jsPDF + autoTable** | Client-side PDF report generation |
| **SweetAlert2** | Rich confirmation dialogs |
| **@microsoft/signalr** | Real-time notification & AI token updates |
| **Angular Service Worker** | PWA — installable, offline-capable |
| **i18n (custom)** | English / Arabic with RTL support |

<br/>

---

## 🤖 AI Assistant

The built-in **HRMS-AI** is not just a chatbot — it's a contextual HR companion powered by **Groq Cloud** with three operating modes:

```
┌─────────────────────────────────────────────────────────────┐
│                     HRMS-AI Modes                           │
├─────────────────┬───────────────────────────────────────────┤
│  🟢 Normal      │ Conversational HR Q&A with live employee  │
│                 │ context injected per request               │
├─────────────────┼───────────────────────────────────────────┤
│  🔵 DeepThink   │ Function-calling loop — searches employees,│
│                 │ fetches leaves, attendance, salaries, depts│
├─────────────────┼───────────────────────────────────────────┤
│  🔴 Executive   │ DeepThink + write access — can approve /  │
│  (HR/Admin only)│ reject leave requests directly via AI     │
└─────────────────┴───────────────────────────────────────────┘
```

### Available AI Tools (DeepThink / Executive)

| Tool | Description |
|---|---|
| `SearchEmployeeByName` | Find employees by first or last name |
| `GetEmployeeLeaves` | Full leave history + annual days used |
| `GetDepartmentsOverview` | All departments with employee counts |
| `GetAttendanceRecords` | Attendance filtered by employee / date range |
| `GetSalaries` | Salary records filtered by employee / period |
| `GetSystemOverview` | Total employees, active count, department count |
| `ApproveOrRejectLeave` | *(Executive only)* Approve or reject a pending leave |

### Context Injection
Every Normal-mode response automatically receives a structured employee snapshot:
```
Employee · Department · Position · Hire Date · Status
Leave balance used this year · Pending requests
Attendance this month (days + hours + currently clocked in)
Latest salary breakdown (Base · Allowances · Deductions · Net)
```

### Token Tracking
A `TokenTrackerService` singleton tracks per-minute token consumption and broadcasts live updates to all connected clients via SignalR. The UI shows a real-time usage bar.

<br/>

---

## 📁 Project Structure

```
HRMS-GradProject/
│
├── 📦 Domain/                         ← Core (zero dependencies)
│   ├── Entities/
│   │   ├── User.cs                    (ProfilePictureUrl · PendingProfilePictureUrl)
│   │   ├── Employee.cs                (custom ID · AnnualLeaveBalance)
│   │   ├── Department.cs
│   │   ├── Position.cs
│   │   ├── Leave.cs                   (RejectionReason · ReviewedBy)
│   │   ├── Attendance.cs
│   │   ├── Salary.cs
│   │   ├── Notification.cs
│   │   ├── Meeting.cs                 (MeetLink · MeetingStatus)
│   │   ├── Announcement.cs            (TargetEmployeeIds · ExpiryDate)
│   │   └── PayrollAdjustment.cs       (Bonus | Penalty · IsApplied)
│   ├── Enums/
│   │   ├── UserRole.cs                (Admin · HR · Employee)
│   │   ├── LeaveType.cs               (Annual · Sick · Emergency · Unpaid)
│   │   ├── LeaveStatus.cs             (Pending · Approved · Rejected)
│   │   ├── NotificationType.cs        (14 distinct types)
│   │   ├── MeetingStatus.cs           (Scheduled · Completed · Cancelled)
│   │   ├── AdjustmentType.cs          (Bonus · Penalty)
│   │   └── AiMode.cs                  (Normal · DeepThink · Executive)
│   └── Interfaces/
│       ├── IGenericRepository.cs
│       └── IUnitOfWork.cs
│
├── 📦 Application/                    ← Business Logic
│   ├── Common/
│   │   ├── ApiResponse.cs             ← unified {success, message, data, errors}
│   │   └── PagedResult.cs             ← {items, totalCount, totalPages, ...}
│   ├── DTOs/                          (Auth · Employee · Leave · Attendance · ...)
│   ├── Services/
│   │   ├── Interfaces/                (13 service interfaces)
│   │   └── Implementations/           (AuthService · EmployeeService · LeaveService
│   │                                   SalaryService · MeetingService · AnnouncementService
│   │                                   PayrollAdjustmentService · NotificationService · ...)
│   ├── Mapping/
│   │   └── MappingProfile.cs          ← all AutoMapper profiles
│   └── Settings/
│       ├── EmailSettings.cs
│       ├── GroqSettings.cs
│       └── AttendanceSettings.cs
│
├── 📦 Infrastructure/                 ← Data Access & External Services
│   ├── Data/
│   │   ├── AppDbContext.cs            ← EF Core context + Fluent API config
│   │   └── Repositories/
│   │       ├── GenericRepository.cs
│   │       └── UnitOfWork.cs
│   ├── Services/
│   │   ├── HrAiService.cs             ← Groq API + function calling loop
│   │   ├── TokenTrackerService.cs     ← Singleton + SignalR broadcast
│   │   ├── AiHub.cs                   ← SignalR hub
│   │   ├── MeetingService.cs
│   │   ├── EmailService.cs            ← MailKit + HTML templates
│   │   ├── CloudinaryService.cs       ← image upload + face-crop
│   │   └── LeaveResetBackgroundService.cs  ← annual balance reset
│   ├── Migrations/                    (full migration history)
│   └── DependencyInjection.cs
│
├── 📦 HRMS_API/                       ← Presentation
│   ├── Controllers/                   (14 controllers)
│   ├── Middleware/
│   │   └── ExceptionHandlingMiddleware.cs
│   ├── Filters/
│   │   └── ValidateModelAttribute.cs
│   ├── appsettings.json
│   └── Program.cs
│
└── 📦 Frontend/                       ← Angular 19 PWA
    └── src/app/
        ├── core/
        │   ├── guards/                (auth · admin · hr)
        │   ├── interceptors/          (jwt injection · 401 session expiry)
        │   ├── services/              (15+ services)
        │   ├── models/                (TypeScript interfaces)
        │   ├── pipes/                 (translate pipe — EN/AR)
        │   └── utils/                 (error-handler.util.ts)
        ├── shared/
        │   ├── header/                (notifications · dark mode · install PWA)
        │   └── sidebar/               (resizable · role-aware · profile pic)
        └── features/
            ├── auth/                  (login · register)
            ├── dashboard/             (charts · announcements · PDF report)
            ├── employees/             (table · details modal · per-employee PDF)
            ├── employee-form/
            ├── departments/
            ├── positions/
            ├── leave/ + leave-form/
            ├── attendance/ + all-attendance/
            ├── salary/
            ├── meetings/
            ├── payroll-adjustments/
            ├── ai-assistant/          (3 modes · token bar · chat history)
            ├── my-profile/            (edit · change password · upload picture)
            └── pending-approvals/     (profile picture review queue)
```

<br/>

---

## 📡 API Endpoints

### 🔐 Auth — `/api/auth`
| Method | Endpoint | Access | Description |
|---|---|---|---|
| `POST` | `/login` | Public | Login → JWT token + profile picture URL |
| `POST` | `/register` | Admin | Register new user |
| `POST` | `/change-password` | Auth | Change own password |
| `GET` | `/me` | Auth | Current user with pending picture status |
| `POST` | `/upload-profile-picture` | Auth | Submit picture for HR approval |
| `GET` | `/pending-profile-pictures` | HR · Admin | Approval queue |
| `POST` | `/approve-profile-picture/{userId}` | HR · Admin | Approve picture |
| `POST` | `/reject-profile-picture/{userId}` | HR · Admin | Reject picture |
| `POST` | `/admin-update-profile-picture/{userId}` | HR · Admin | Direct override |
| `GET` | `/users` | HR · Admin | All users (paginated) |
| `GET` | `/unassigned-employees` | HR · Admin | Users without employee profile |
| `GET` | `/get-user-id-by-email/{email}` | HR · Admin | Lookup user ID |

### 👥 Employees — `/api/employees`
| Method | Endpoint | Access | Description |
|---|---|---|---|
| `GET` | `/` | HR · Admin | All employees (paginated) |
| `GET` | `/{id}` | HR · Admin | Get by ID |
| `GET` | `/{id}/profile` | HR · Admin | Full profile with dept + position |
| `GET` | `/me` | Auth | My own profile |
| `POST` | `/` | HR · Admin | Create employee (custom ID generated) |
| `PUT` | `/{id}` | HR · Admin | Update employee |
| `DELETE` | `/{id}` | HR · Admin | Delete employee |

### 📅 Leave — `/api/leaves`
| Method | Endpoint | Access | Description |
|---|---|---|---|
| `GET` | `/` | HR · Admin | All requests (paginated) |
| `GET` | `/my` | Employee | My leave requests |
| `GET` | `/{id}` | HR · Admin | Get by ID |
| `GET` | `/my/{id}` | Employee | My specific request |
| `POST` | `/` | Employee | Submit request (overlap-checked) |
| `PUT` | `/{id}/status` | HR · Admin | Approve or reject with reason |
| `DELETE` | `/{id}` | Employee | Cancel pending request |

### ⏰ Attendance — `/api/attendance`
| Method | Endpoint | Access | Description |
|---|---|---|---|
| `GET` | `/` | HR · Admin | All records (paginated) |
| `GET` | `/my` | Auth | My attendance history |
| `GET` | `/{id}` | HR · Admin | Get by ID |
| `POST` | `/clockin` | Auth | Clock in (auto-closes stale sessions) |
| `PUT` | `/clockout` | Auth | Clock out |

### 💰 Salary — `/api/salaries`
| Method | Endpoint | Access | Description |
|---|---|---|---|
| `GET` | `/` | HR · Admin | All records (paginated) |
| `GET` | `/my` | Auth | My salary history |
| `GET` | `/{id}` | HR · Admin | Get by ID |
| `POST` | `/` | Admin | Create (auto-applies pending adjustments) |
| `PUT` | `/{id}` | Admin | Update |
| `DELETE` | `/{id}` | Admin | Delete |

### 🎯 Payroll Adjustments — `/api/payroll-adjustments`
| Method | Endpoint | Access | Description |
|---|---|---|---|
| `GET` | `/` | HR · Admin | All adjustments (paginated) |
| `GET` | `/employee/{employeeId}` | HR · Admin | By employee |
| `GET` | `/my` | Auth | My adjustments |
| `POST` | `/` | HR · Admin | Create bonus or penalty |
| `DELETE` | `/{id}` | HR · Admin | Delete (only if not yet applied) |

### 📅 Meetings — `/api/meetings`
| Method | Endpoint | Access | Description |
|---|---|---|---|
| `GET` | `/` | HR · Admin | All meetings (paginated) |
| `GET` | `/my` | Auth | My scheduled meetings |
| `GET` | `/{id}` | HR · Admin | Get by ID |
| `POST` | `/` | HR · Admin | Schedule (conflict-checked · Meet link generated) |
| `PUT` | `/{id}` | HR · Admin | Update meeting details |
| `PUT` | `/{id}/cancel` | HR · Admin | Cancel |
| `PUT` | `/{id}/complete` | HR · Admin | Mark completed |

### 📢 Announcements — `/api/announcement`
| Method | Endpoint | Access | Description |
|---|---|---|---|
| `GET` | `/` | Auth | All active announcements (role-filtered) |
| `POST` | `/` | HR · Admin | Create (general or targeted) |
| `DELETE` | `/{id}` | HR · Admin | Delete |

### 🔔 Notifications — `/api/notifications`
| Method | Endpoint | Access | Description |
|---|---|---|---|
| `GET` | `/` | Auth | My notifications |
| `GET` | `/unread-count` | Auth | Unread badge count |
| `PUT` | `/{id}/read` | Auth | Mark one as read |
| `PUT` | `/read-all` | Auth | Mark all as read |
| `DELETE` | `/{id}` | Auth | Delete one |
| `DELETE` | `/delete-all` | Auth | Clear all |

### 🏢 Departments — `/api/departments`
| Method | Endpoint | Access | Description |
|---|---|---|---|
| `GET` | `/` | Auth | All departments |
| `GET` | `/{id}` | Auth | Get by ID |
| `POST` | `/` | Admin | Create |
| `PUT` | `/{id}` | Admin | Update |
| `DELETE` | `/{id}` | Admin | Delete (blocked if has employees) |

### 🪑 Positions — `/api/positions`
| Method | Endpoint | Access | Description |
|---|---|---|---|
| `GET` | `/` | Auth | All positions |
| `GET` | `/{id}` | Auth | Get by ID |
| `GET` | `/department/{id}` | Auth | By department |
| `POST` | `/` | Admin | Create |
| `PUT` | `/{id}` | Admin | Update |
| `DELETE` | `/{id}` | Admin | Delete (blocked if has employees) |

### 🤖 AI — `/api/ai`
| Method | Endpoint | Access | Description |
|---|---|---|---|
| `POST` | `/chat` | Auth | Chat with HRMS-AI (Normal/DeepThink/Executive) |
| `GET` | `/analyze-leave` | Auth | AI analysis of my leave patterns |
| `GET` | `/salary-insight` | Auth | AI salary trend analysis |

**SignalR Hub** — `ws://.../hubs/ai` — pushes `ReceiveTokenUpdate` events live.

<br/>

---

## 🔒 Roles & Permissions Matrix

| Feature | Employee | HR | Admin |
|---|---|---|---|
| Login · Change Password | ✅ | ✅ | ✅ |
| View own profile · Upload picture | ✅ | ✅ | ✅ |
| Submit / Cancel leave request | ✅ | ✅ | ✅ |
| Clock in / Clock out | ✅ | ✅ | ✅ |
| View own salary & adjustments | ✅ | ✅ | ✅ |
| Use AI Assistant | ✅ | ✅ | ✅ |
| View all employees | ❌ | ✅ | ✅ |
| Approve / Reject leaves | ❌ | ✅ | ✅ |
| View all attendance | ❌ | ✅ | ✅ |
| Schedule meetings | ❌ | ✅ | ✅ |
| Post announcements | ❌ | ✅ | ✅ |
| Create payroll adjustments | ❌ | ✅ | ✅ |
| Approve profile pictures | ❌ | ✅ | ✅ |
| AI Executive mode (act on data) | ❌ | ✅ | ✅ |
| Register new users | ❌ | ❌ | ✅ |
| Add / Delete employees | ❌ | ❌ | ✅ |
| Manage departments & positions | ❌ | ❌ | ✅ |
| Create / Delete salaries | ❌ | ❌ | ✅ |

<br/>

---

## 🗄 Database Design

The system has **11 entities** with carefully designed relationships:

```
Users ──────────────── Employees (1:1 · cascade)
         │                  │
         │              Departments (M:1)
         │              Positions (M:1)
         │              Attendances (1:M · cascade)
         │              Salaries (1:M · cascade · unique per month)
         │              Leaves (1:M · cascade)
         │              PayrollAdjustments (1:M · cascade)
         │
         └── Notifications (1:M · no-action)
         └── Meetings (organizer · no-action)

Meetings ────────────── Employees (invitee · cascade)
Announcements ──────── Employees (author · restrict/nullable)
PayrollAdjustments ─── Salaries (applied-to · nullable FK)
```

### Custom Employee ID
Employee IDs are auto-generated from hire context:
```
Format: YEAR + DEPT_ID (2 digits) + SEQUENCE (4 digits)
Example: 2026020001  → Year 2026 · Dept IT (02) · First employee
```

<br/>

---

## 📧 Email Templates

Every HR event triggers a beautifully styled HTML email with the Kawādir brand:

| Event | Recipient | Trigger |
|---|---|---|
| Welcome | New user | On registration |
| Leave Requested | HR / Admin | Employee submits request |
| Leave Approved / Rejected | Employee | Status change (with rejection reason) |
| Leave Cancelled | HR / Admin | Employee cancels |
| Clock In | Employee | Successful check-in |
| Clock Out | Employee | Successful check-out (with total hours) |
| Salary Created | Employee | New monthly salary added |
| Salary Updated | Employee | Salary record modified |
| Meeting Scheduled | Employee | Join link included |
| Meeting Updated | Employee | New time + updated link |
| Meeting Cancelled | Employee | Cancellation notice |
| Announcement | All / Targeted | New company announcement |
| Payroll Adjustment | Employee | Bonus or penalty applied |

> All email calls are fire-and-forget (`Task.Run`) and wrapped in `try/catch` — a failed email never breaks the main request flow.

<br/>

---

## 🚀 Getting Started

### Prerequisites

```bash
✅ .NET 8 SDK          https://dotnet.microsoft.com/download
✅ PostgreSQL 15+       https://www.postgresql.org/download
✅ Node.js 18+          https://nodejs.org
✅ Angular CLI 19       npm install -g @angular/cli
```

### 1 · Clone & Restore

```bash
git clone https://github.com/your-username/HRMS-GradProject.git
cd HRMS-GradProject
dotnet restore
```

### 2 · Configure Secrets

```bash
cd HRMS-GradProject

dotnet user-secrets set "ConnectionStrings:DefaultConnection" \
  "Host=localhost;Database=HRMSDb;Username=postgres;Password=yourpassword"

dotnet user-secrets set "Jwt:Key" "YourSuperSecretKeyHere_AtLeast32Chars!!"

dotnet user-secrets set "GroqSettings:ApiKey" "gsk_your_groq_api_key"

dotnet user-secrets set "Cloudinary:CloudName" "your_cloud"
dotnet user-secrets set "Cloudinary:ApiKey"    "your_key"
dotnet user-secrets set "Cloudinary:ApiSecret" "your_secret"
```

Or edit `appsettings.Development.json` directly (never commit real secrets).

### 3 · Migrate & Run

```bash
# Apply all migrations
dotnet ef database update --project Infrastructure --startup-project HRMS-GradProject

# Run the API
dotnet run --project HRMS-GradProject

# Swagger UI → https://localhost:7204/swagger
```

### 4 · Frontend

```bash
cd Frontend
npm install
ng serve

# App → http://localhost:4200
```

<br/>

---

## ⚙️ Configuration Reference

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Host=...;Database=HRMSDb;Username=postgres;Password=..."
  },
  "Jwt": {
    "Key": "min-32-character-secret",
    "Issuer": "HRMS_API",
    "Audience": "HRMS_Client",
    "ExpiryHours": "24"
  },
  "GroqSettings": {
    "ApiKey": "gsk_...",
    "Model": "llama-3.3-70b-versatile",
    "BaseUrl": "https://api.groq.com/openai/v1",
    "MaxTokens": 1024
  },
  "Cloudinary": {
    "CloudName": "...",
    "ApiKey": "...",
    "ApiSecret": "..."
  },
  "EmailSettings": {
    "Host": "smtp.gmail.com",
    "Port": 587,
    "Username": "your@gmail.com",
    "Password": "gmail-app-password",
    "FromName": "Kawādir HRMS",
    "FromEmail": "your@gmail.com"
  },
  "AttendanceSettings": {
    "WorkDayEndTime": "23:59:00"
  },
  "AllowedOrigins": [
    "http://localhost:4200",
    "https://your-production-domain.com"
  ]
}
```

<br/>

---

## 🧱 API Conventions

### Unified Response Shape
```json
{
  "success": true,
  "message": "Leave request submitted successfully",
  "data": { ... },
  "errors": null
}
```

### Pagination Shape
```json
{
  "items": [ ... ],
  "totalCount": 95,
  "pageNumber": 2,
  "pageSize": 10,
  "totalPages": 10,
  "hasPreviousPage": true,
  "hasNextPage": true
}
```

### Exception → HTTP Mapping

| Exception | Status |
|---|---|
| `KeyNotFoundException` | `404 Not Found` |
| `UnauthorizedAccessException` | `401 Unauthorized` |
| `ArgumentException` | `400 Bad Request` |
| `InvalidOperationException` | `400 Bad Request` |
| Any other | `500 Internal Server Error` |

<br/>

---

## 🏆 Key Engineering Decisions

**1 · Why a custom Employee ID?**
Employee IDs encode hire year and department (`2026020001`). This makes IDs human-readable and provides instant context without a database lookup.

**2 · Why fire-and-forget emails?**
Email failures (SMTP timeouts, rejected messages) should never roll back a business operation. All email calls use `Task.Run` + `try/catch` to fully decouple delivery from the request lifecycle.

**3 · Why Groq instead of OpenAI?**
Groq's free tier with `llama-3.3-70b-versatile` provides sufficient throughput for academic use. The service abstraction (`IHrAiService`) makes it trivial to swap providers.

**4 · Why SignalR for token tracking?**
The AI rate limit (tokens per minute) is a shared resource. All connected clients need to see the same counter in real time so users don't hit the limit unexpectedly. A singleton `TokenTrackerService` + SignalR broadcast solves this elegantly.

**5 · Why `DateTime.SpecifyKind(..., Utc)` everywhere?**
PostgreSQL `timestamp with time zone` rejects `DateTimeKind.Unspecified`. Every DateTime written to the DB is explicitly tagged UTC to prevent the Npgsql exception.

<br/>

---

## ✅ Build Status

### Backend
| Module | Status |
|---|---|
| Auth (JWT · BCrypt · Roles) | ✅ Production ready |
| Employee Management (custom ID · soft relations) | ✅ Production ready |
| Department & Position CRUD | ✅ Production ready |
| Leave Management (overlap detection · balance tracking) | ✅ Production ready |
| Attendance (stale session auto-close · configurable end time) | ✅ Production ready |
| Salary (duplicate guard · payroll adjustment auto-apply) | ✅ Production ready |
| Payroll Adjustments (bonus · penalty · applied tracking) | ✅ Production ready |
| Meetings (conflict detection · Google Meet link generation) | ✅ Production ready |
| Announcements (targeted · general · expiry) | ✅ Production ready |
| Notifications (14 types · real-time SignalR) | ✅ Production ready |
| Email Service (9 template types · fire-and-forget) | ✅ Production ready |
| Profile Picture Approval Workflow | ✅ Production ready |
| AI Assistant (Normal · DeepThink · Executive + 6 tools) | ✅ Production ready |
| Annual Leave Reset Background Service | ✅ Production ready |
| Global Exception Middleware | ✅ Production ready |

### Frontend (Angular 19)
| Feature | Status |
|---|---|
| Auth (login · logout · session guard) | ✅ Complete |
| Dashboard (charts · announcements · PDF report) | ✅ Complete |
| Employee Management (modal details · per-employee PDF) | ✅ Complete |
| Departments & Positions | ✅ Complete |
| Leave Management (request · review · rejection reason modal) | ✅ Complete |
| Attendance (clock in/out · stale session handling) | ✅ Complete |
| Salary (payslip PDF download) | ✅ Complete |
| Meetings (schedule · cancel · complete) | ✅ Complete |
| Payroll Adjustments | ✅ Complete |
| Notifications (real-time · smart navigation routing) | ✅ Complete |
| AI Assistant (3 modes · token bar · chat persistence) | ✅ Complete |
| My Profile (edit · change password · picture upload) | ✅ Complete |
| Pending Approvals (picture review queue) | ✅ Complete |
| Dark Mode | ✅ Complete |
| Arabic / English i18n | ✅ Complete |
| PWA (installable · service worker) | ✅ Complete |

<br/>

---

## 🤝 Contributing

```bash
# 1 · Branch from main
git checkout -b feature/your-feature

# 2 · Commit with a prefix
git commit -m "feat: add leave overlap validation"

# 3 · Push and open a PR
git push origin feature/your-feature
```

| Prefix | Use when |
|---|---|
| `feat:` | New feature |
| `fix:` | Bug fix |
| `refactor:` | Code improvement, no behavior change |
| `docs:` | Documentation update |
| `chore:` | Build, deps, config |

<br/>

---

## 👨‍💻 Team

| Name | Role |
|---|---|
| **Abedalqader Alfaqeeh** | Backend Lead · Clean Architecture · API Design · AI Integration |
| **Mohammad Frehat** | Frontend Developer |
| **Saad Rabadi** | UI/UX Designer |
| **Mohammad Alghazo** | Frontend Developer |

<br/>

---

## 📄 License

Developed as a graduation project for academic purposes.
© 2026 Kawādir HRMS Team — Computer Science · Al-Balqa Applied University.

---

<div align="center">

<br/>

**Built with precision and passion — Kawādir HRMS 2026**

[![Made with .NET 8](https://img.shields.io/badge/Made_with-.NET_8-512BD4?style=flat-square&logo=dotnet)](https://dotnet.microsoft.com)
[![Angular 19](https://img.shields.io/badge/Frontend-Angular_19-DD0031?style=flat-square&logo=angular)](https://angular.io)
[![PostgreSQL](https://img.shields.io/badge/Database-PostgreSQL-316192?style=flat-square&logo=postgresql)](https://postgresql.org)
[![Groq AI](https://img.shields.io/badge/AI-Groq_LLaMA_3-FF6B35?style=flat-square)](https://groq.com)
[![PWA Ready](https://img.shields.io/badge/PWA-Ready-5A0FC8?style=flat-square&logo=pwa)](https://web.dev/pwa)

</div>
