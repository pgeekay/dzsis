# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**openSIS Classic** is an open-source Student Information System (SIS) for K-12, trade schools, and higher education. It manages student demographics, staff data, scheduling, grades, attendance, report cards, transcripts, and communication.

**Requirements:**
- Apache 2.4+
- MySQL 5.7, 8.0, or MariaDB 10.4.x
- PHP 8.x

**Key Components:**
- Student management
- Staff management
- Course scheduling
- Grade tracking & teacher gradebook
- Attendance management
- Eligibility tracking
- Report cards & transcripts
- Built-in messaging
- Data import/export

## Architecture

### High-Level Design

OpenSIS uses a **module-based, procedural PHP architecture** with auto-loaded utility functions and a centralized database layer:

1. **Single Entry Point**: `index.php` handles routing and session management
2. **Modular System**: Feature-specific code isolated in `/modules/{moduleName}/`
3. **Utility Functions**: All functions in `/functions/` are auto-loaded by `Warehouse.php`
4. **Configuration-Driven**: Enabled modules and settings defined in `ConfigInc.php` and `Data.php`
5. **Direct Database Access**: No ORM; queries executed via `DBQuery()` and `DBGet()` helper functions
6. **Session-Based Access Control**: User roles stored in `$_SESSION`, checked via `AuthorizationFnc.php`

### Request Flow

```
index.php
  ├─ Session initialization & CSRF validation
  ├─ Load Warehouse.php (sets up DB & function auto-loading)
  ├─ Route to modules/{moduleName}/Menu.php or specific module page
  └─ Execute module logic & render HTML/PDF
```

### Module Structure

Each module in `/modules/{moduleName}/` contains:
- **Menu.php**: Navigation/dispatcher for that module
- **Search.php** / **SearchInc.php**: Query UI & filtering logic
- **{Entity}.php**: Main CRUD interface (e.g., `Student.php`, `Staff.php`)
- **{Report}.php**: Report generation & printing
- **ConfigInc.php**: Module-specific settings (some modules)

**Enabled modules** are defined in `ConfigInc.php` array (`$openSISModules`):
- schoolsetup, students, users, scheduling, grades, attendance, eligibility, messaging, tools, etc.

### Database Layer

- **Connection**: `DatabaseInc.php` initializes `$connection` (mysqli) via `db_start()`
- **Configuration**: Database credentials read from `Data.php` (not in repo; created during install)
- **Query Helper**: `DBQuery($sql)` executes queries; returns result identifier
- **Fetch Helper**: `DBGet($result)` returns all rows as keyed array; `DBGet($result, MYSQLI_ASSOC)` for assoc array
- **Auto-increment**: Use `mysqli_insert_id($connection)` after INSERT

### Function Auto-Loading

`Warehouse.php` scans `/functions/` and includes all PHP files (except those in `$IgnoreFiles`). This means:
- No explicit imports needed
- All utility functions available globally
- Functions grouped by purpose via filename (e.g., `AuthorizationFnc.php`, `ButtonsFnc.php`, `DbDateFnc.php`)
- Avoid duplicating function names across files

## Key Files & Responsibilities

### Core Initialization
- **index.php**: Main entry point; handles login, routing, session lifecycle
- **Warehouse.php**: Bootstraps configuration, database, and auto-loads all functions
- **ConfigInc.php**: Sets up configuration from `Data.php`, enables/disables modules
- **DatabaseInc.php**: Database connection & query helpers

### Configuration & Credentials
- **Data.php**: Database server, username, password, name, and port (created during install; not in repo)
- **RedirectRootInc.php**: Sets root path for relative includes
- **ConnectionClass.php**: Database connection class wrapper

### Security & Validation
- **CSRFSecurityFnc.php**: CSRF token generation & validation
- **AuthorizationFnc.php**: Role/permission checking
- **PasswordHashFnc.php**: Password hashing & validation
- **ParamLibFnc.php**: Input sanitization & parameter extraction (`optional_param()`)

### Common Utility Functions
- **DbGetFnc.php**: `DBGet()`, `DBQuery()`, database helpers
- **GetNameFnc.php**: Student/staff name lookups
- **GetStuListFnc.php**: Student list queries with filtering
- **GetStaffListFnc.php**: Staff list queries
- **AuthorizationFnc.php**: Permission/role checking
- **CurrentFnc.php**: Current school year, marking period, date helpers
- **InputsFnc.php**: Form input rendering (text, dropdown, checkbox, etc.)
- **ButtonsFnc.php**: Standard button/link generation

### UI & Rendering
- **Top.php**: Page header template
- **Bottom.php**: Page footer template
- **Side.php**: Left sidebar menu
- **Menu.php**: Module navigation
- **DrawHeaderFnc.php**: Page section headers
- **DrawTabFnc.php**: Tabbed UI components
- **DrawBlockFnc.php**: Collapsible sections
- **PrepareDataTable.php**: HTML table formatting for data display
- **ExportDataTable.php**: CSV/Excel export utilities

### API Endpoints (if applicable)
- **api/SchoolInfo.php**: School information queries
- **api/StaffInfo.php**: Staff data endpoints
- **api/StudentEnrollmentInfo.php**: Enrollment data queries

Note: API endpoints are standalone PHP files that handle their own DB connection and JSON/XML output.

## Development Guidelines

### Working with Modules

To add or modify a module feature:
1. Locate the module in `/modules/{moduleName}/`
2. Edit the relevant file (Menu.php for routing, {Entity}.php for CRUD, {Report}.php for reports)
3. Use existing query functions (`GetStuListFnc.php`, `GetStaffListFnc.php`, etc.) for consistency
4. Render UI via `InputsFnc.php` (form fields), `ButtonsFnc.php` (buttons), `DrawTabFnc.php` (tabs)
5. Ensure all user inputs are validated with `optional_param()` and sanitized via `ParamLibFnc.php`

### Adding Database Queries

- Use `DBQuery($sql)` to execute; check for FALSE on error
- Use `DBGet($result)` to fetch all rows (returns 1-indexed array)
- For single rows, access via `$rows[1]` (arrays are 1-indexed, not 0-indexed)
- Escape user input with `mysqli_real_escape_string($connection, $value)` before SQL concatenation
- Alternative: Use prepared statements if available in utility functions

### Session & Authorization

- User info stored in `$_SESSION` during login (`LoginInc.php`)
- Check permissions with `AllowEdit()`, `AllowView()` from `AuthorizationFnc.php`
- User role/type determines module access via `AuthorizationFnc.php` and module Menu.php logic
- Session ID tracked in `log_maintain` table for audit trails

### CSRF Protection

- All POST forms must include `CSRFSecure::Token()` in a hidden field
- Validate with `CSRFSecure::ValidateToken($token)` before processing

### Localization

- Language strings defined in `/lang/language.php`
- Use constants like `_studentName`, `_staffName` (prefixed with underscore) in code
- Translations stored in language-specific files (e.g., `/lang/en.php`)

### API Conventions

If adding or modifying API endpoints (`/api/`):
- Include Data.php and manually set up DB connection (see `api/StudentEnrollmentInfo.php`)
- Return JSON or XML based on request parameter
- Set appropriate Content-Type header (`application/json` or `application/xml`)
- Handle large datasets with pagination or streaming

## Common Development Tasks

### Debugging

- Check `error_reporting(0)` in `index.php` (disabled for production; enable locally)
- Use `ini_set('display_errors', 1)` and `ini_set('log_errors', 1)` in development
- Database errors logged via `db_show_error()` in `DatabaseInc.php`
- Query inspection: Append `print_r($result)` or `var_dump()` before table display

### Data Export

- Use `ExportDataTable.php` functions to export student/staff lists to CSV/Excel
- `ForExport.php` handles file download headers and formatting

### Report Generation

- PDF reports use external htmldoc library (referenced in `ConfigInc.php` via `$htmldocPath`)
- HTML reports rendered to browser, then user can print to PDF via browser
- Reports typically located in module folders as `{Report}.php`

### Backup & Rollover

- `BackupForRollover.php`: Create database backup before year rollover
- `RolloverShadow.php`: Copy current year data to new school year
- `RemoveBackup.php`: Clean up old backups

## Testing & Deployment

### Before Committing Changes

1. Test in a local/staging environment with a copy of the database
2. Verify form submission with CSRF token validation enabled
3. Check authorization for different user roles (admin, teacher, parent, student)
4. Validate SQL queries for injection vulnerabilities
5. Test with various data sizes (large student/staff lists) for performance

### Installation & Upgrade

- Run web-based installer (`install/` directory)
- Installer creates `Data.php` with database credentials
- Installer initializes database schema and default data
- After install, remove `install/` directory (checked in `index.php`)

## Notes for Code Conversion

If converting this codebase to a modern framework or language:

1. **Preserve Module Structure**: Each module can be migrated independently; prioritize high-traffic modules first
2. **Database Schema**: Understand current table schema before refactoring queries (request data dictionary from install docs)
3. **Session State**: User roles & permissions are stored in `$_SESSION`; any new auth system must maintain role/permission model
4. **Function Dependencies**: Many utility functions depend on global `$connection` and session data; plan extraction carefully
5. **UI Conventions**: Extensive use of custom drawing functions (`DrawTabFnc.php`, `DrawHeaderFnc.php`); consider replacing with template system
6. **API Expansion**: Current API endpoints are minimal; new REST/GraphQL layer could coexist with legacy pages during transition
7. **Testing**: No automated test suite exists; prioritize unit tests for core functions (database, authorization, validation)

## File Paths

- **Root**: `/`
- **Modules**: `/modules/{moduleName}/`
- **Functions**: `/functions/` (auto-loaded)
- **Assets**: `/assets/` (CSS, JS, images, locales)
- **Libraries**: `/libraries/` (PhpSpreadsheet, htmlpurifier)
- **Documentation**: `/docs/`
- **Language Files**: `/lang/`
- **Installation Files**: `/install/`
- **API Endpoints**: `/api/`

