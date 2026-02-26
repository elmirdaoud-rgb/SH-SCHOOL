# SH SCHOOL - نظام إدارة المدارس

## Overview
SH SCHOOL is a comprehensive multi-tenant school management system designed to streamline educational institution operations. Built with React, Express, and PostgreSQL, it ensures complete data isolation for each school using a `school_id`. The system supports custom domains/subdomains, extensive Role-based Access Control (RBAC), and comprehensive management features for staff, students, families, and academics (grading, homework, exams). It includes dynamic branding, advanced notification systems, dedicated portals for all stakeholders, RTL support for Arabic, and full internationalization. The project aims to be a scalable, production-ready solution that enhances communication and provides a tailored user experience.

## User Preferences
I prefer detailed explanations and iterative development. Ask before making major changes. Do not make changes to the folder `shared` and the file `server/seed.ts`.

## System Architecture

### UI/UX Decisions
- **Team & Branding**: Dynamic branding allows school administrators to customize logos and theme colors, applied across all users within their school.
- **RTL Support**: Full Right-to-Left (RTL) support for Arabic layouts, including automatic positioning of UI elements and text alignment.
- **Cascading Dropdowns**: Used for student forms (Cycle, Level, Group) to ensure data consistency.

### Technical Implementations
- **Multi-tenant Architecture**: All database tables (except `roles` and `system_settings`) are linked to a `school_id` for data isolation, with all operations filtered by this ID.
- **Custom Domain/Subdomain Routing**: Schools can use a platform subdomain or a custom domain. A middleware detects the school from the request host, enforcing domain-specific logins.
- **Role-based Access Control**: Implemented with 15 predefined roles and 27 granular permissions, controlling access via `requireRoleOrPermission()` middleware.
- **Database**: PostgreSQL with Drizzle ORM for type-safe interactions.
- **Safe Seeding**: Utilizes `system_settings` and `SEED_VERSION` to ensure one-time initial setup.
- **Teacher Data Filtering**: Teachers automatically access data relevant to their assignments (levels, subjects, groups, students).
- **Internationalization (i18n)**: Supports Arabic (RTL), French (LTR), and English (LTR) using `react-i18next`, with central direction management.
- **Autoscale Compute Optimization**: Minimizes background requests and uses TanStack Query's `focusManager` to pause refetch intervals, optimizing server load. Night mode (00:00–08:00) disables background requests entirely.

### Feature Specifications
- **Staff, Family & Sibling Management**: Comprehensive management of staff, student families, and sibling relationships, including automated family discounts and a dedicated family portal with child switching.
- **Pricing System**: Supports monthly fees, transport fees, and sibling discounts applied once per family invoice.
- **Family Payment Mode**: Accountants can record a single combined invoice for all siblings in a family, applying the sibling discount to the total.
- **Notifications & Announcements**: Automated notifications for academic events and a scheduled pop-up announcement system.
- **Continuous Assessment**: A complete grading system for two semesters, including tests, integrated activities, and Massar (Moroccan Ministry of Education) file management for grades export.
- **School Clubs Management**: Allows administrators to create and manage extracurricular clubs with membership tracking and automated fee calculation in payments.
- **Timetable Management**: Structured system for creating and viewing timetables by group, with student views supporting both structured entries and image uploads.
- **System Admin Platform Settings**: Super Admin exclusive feature for managing platform branding, and OTP-secured credential management.
- **Unified Profile Page**: A single `/profile` route providing role-specific information for all user types.
- **Expenses Management**: Tracks expenses with categories, filtering, and search functionality.
- **Accounting Dashboard**: Provides a financial overview with income breakdown, expenses, and net balance, filterable by various criteria.

## External Dependencies
- **PostgreSQL**: Primary database.
- **Drizzle ORM**: For database interactions.
- **Object Storage**: Used for file uploads (e.g., school logos, Massar templates, media) via an authenticated endpoint.
- **Twilio (Optional)**: For SMS OTP verification during System Admin credential changes. Requires `TWILIO_ACCOUNT_SID`, `TWILIO_AUTH_TOKEN`, and `TWILIO_PHONE_NUMBER` environment variables. If not configured, OTP codes are logged to console in development.