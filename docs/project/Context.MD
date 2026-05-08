CONTEXT FILE
Project: Map OpenSIS Classic to California TK–8 SIS Requirements
1. Objective

Evaluate OpenSIS Classic Community Edition as the base open-source SIS and define the extensions required for California TK–8 compliance.

OpenSIS Classic is a PHP/MySQL web-based SIS. Public documentation/repository descriptions indicate it includes student demographics, scheduling, gradebook, attendance, report cards, eligibility, transcripts, parent/student portals, and related school operations.

The goal is to determine whether OpenSIS can be extended to independently support:

California TK–8 student information requirements
CALPADS reporting
California code sets and validations
Exportable data for reporting, analytics, and external systems
2. Platform Being Evaluated
Base System
Product: OpenSIS Classic Community Edition
Repository: OS4ED/openSIS-Classic
Stack: PHP + MySQL + Apache/Linux compatible
Current public release noted as Community Edition 9.3, release date 06/02/2026.
Installation requirements reference Apache 2.4+, MySQL 5.6/5.7, and PHP 7.x.
3. Scope
In Scope: TK–8

Evaluate and extend OpenSIS for:

Student demographics
Enrollment
Attendance
Program participation
English learner status
Discipline
Basic course/section setup
CALPADS extracts
Data validation
Data export
Out of Scope for Initial Phase

Defer 9–12-specific features:

A–G eligibility logic
Credit accumulation
Graduation requirements
Transcripts as compliance artifact
Graduation cohorts
College/career indicators

However, the design should not block later 9–12 expansion.

4. OpenSIS Module-to-California Requirement Mapping
4.1 Student Demographics
OpenSIS Starting Point

OpenSIS includes student demographic information capabilities.

California TK–8 Requirement

Extend student profile to support CALPADS-aligned demographic and identity elements.

Required Additions / Review Areas

Evaluate whether OpenSIS supports or must be extended for:

SSID
Local student ID
Legal first/middle/last name
Preferred name
Date of birth
Gender code
Race/ethnicity codes
Hispanic/Latino indicator
Primary language
Birth country/state
Parent/guardian education level
Residential address
Mailing address
Student language acquisition status
Effective-dated demographic history
Likely Gap

OpenSIS may support basic demographics, but California requires state-specific code values, effective dating, and CALPADS field-level alignment.

Extension Action

Create or extend:

ca_student_profile
ca_student_demographic_history
ca_student_language
ca_parent_guardian_education
ca_code_set
ca_code_value
4.2 Enrollment
OpenSIS Starting Point

OpenSIS supports student information and school operations; enrollment-related tables/workflows must be inspected in the schema.

California TK–8 Requirement

Support CALPADS SENR-style enrollment history.

Required Elements
School of attendance
Academic year
Grade level: TK, K, 1–8
Enrollment start date
Enrollment exit date
Enrollment status
Entry reason
Exit reason
School transfer status
Primary enrollment indicator
No-show handling
Concurrent enrollment handling if applicable
Likely Gap

OpenSIS may support enrollment but may not fully support CALPADS enrollment lifecycle rules.

Extension Action

Create/extend:

ca_student_enrollment
ca_enrollment_status_history
ca_enrollment_exit_reason
ca_school_year_calendar
4.3 Attendance
OpenSIS Starting Point

OpenSIS includes attendance functionality.

California TK–8 Requirement

Support daily attendance for TK–8 with state-aligned absence/tardy categories.

Required Elements
Attendance date
School
Student
Grade level
Attendance status
Excused absence
Unexcused absence
Tardy
Suspension-related absence where applicable
Independent study attendance if needed
Daily attendance aggregation
Likely Gap

OpenSIS attendance may exist, but code mappings may not align to California attendance categories.

Extension Action

Create/extend:

ca_attendance_code_map
ca_daily_attendance
ca_attendance_summary
ca_attendance_validation_result
4.4 Programs
OpenSIS Starting Point

OpenSIS may support some student attributes or custom fields, but California program tracking should be modeled explicitly.

California TK–8 Requirement

Support CALPADS SPRG-style program participation.

Required Programs
English Learner
Special Education
Foster Youth
Homeless
Migrant
Socioeconomically disadvantaged
Title I
504 plan
Gifted/Talented if locally needed
Required Elements
Program code
Start date
End date
Eligibility status
Participation status
School year
Source of determination
Effective-dated history
Likely Gap

This is likely one of the largest gaps if OpenSIS does not already have state-program history tables.

Extension Action

Create:

ca_program
ca_student_program
ca_student_program_history
ca_program_code_map
4.5 English Learner / Language Status
OpenSIS Starting Point

Must inspect whether OpenSIS stores home language and language proficiency fields.

California TK–8 Requirement

Support CALPADS SELA-style English Language Acquisition Status.

Required Elements
Primary language
English Language Acquisition Status:
EL
IFEP
RFEP
EO
TBD, if applicable
EL status start date
RFEP date
Initial ELPAC date/status if available
Reclassification information
Likely Gap

OpenSIS may support language fields but likely does not fully support California SELA history and validation rules.

Extension Action

Create:

ca_student_ela_status
ca_student_ela_history
ca_language_code_map
4.6 Discipline
OpenSIS Starting Point

OpenSIS feature descriptions emphasize core SIS functions; discipline must be verified in the installed schema/module set.

California TK–8 Requirement

Support CALPADS STAS-style student discipline reporting.

Required Elements
Incident ID
Incident date
School
Student
Incident type/state offense code
Action type
In-school suspension
Out-of-school suspension
Expulsion
Action start/end dates
Duration
Weapon indicator if applicable
Law enforcement referral if applicable
Likely Gap

If OpenSIS discipline exists, it may be local-operational rather than CALPADS-aligned.

Extension Action

Create:

ca_discipline_incident
ca_discipline_student_incident
ca_discipline_action
ca_discipline_code_map
4.7 Courses and Sections
OpenSIS Starting Point

OpenSIS includes scheduling and gradebook functionality.

California TK–8 Requirement

For TK–8, course logic is simpler than 9–12.

Required Elements
Course catalog
Section
Teacher assignment
Term
Grade level
Subject area
State course code mapping where needed
Local course code mapping
Deferred for Later
A–G completion logic
Credit-based graduation tracking
Transcript-driven compliance
Extension Action

Create:

ca_course_code_map
ca_course_subject_area
ca_section_state_reporting_map
5. CALPADS File Support Mapping
Required TK–8 CALPADS Extracts
CALPADS File	OpenSIS Source Area	Gap	Extension Needed
SENR	Enrollment	Likely partial	Enrollment history + CA entry/exit codes
SINF	Student demographics	Partial	CALPADS demographic fields + history
SPRG	Programs	Likely major gap	Program participation tables
SELA	Language/EL	Likely major gap	EL status history
STAS	Discipline	Likely partial/unknown	Discipline incident/action model
6. California Code Set Requirements

OpenSIS must support configurable California code sets, not hardcoded values.

Required code domains:

Grade levels: TK, K, 1–8
Gender codes
Race/ethnicity codes
Primary language codes
Enrollment status
Entry reason
Exit reason
Program codes
EL status codes
Attendance codes
Discipline incident codes
Discipline action codes
Course state code mappings

Create centralized tables:

ca_code_set
ca_code_value
ca_code_crosswalk
7. Recommended OpenSIS Extension Architecture
Principle

Avoid heavy modification of OpenSIS core tables wherever possible.

Recommended Pattern

Use an extension layer:

OpenSIS Core Tables
        ↓
California Extension Tables
        ↓
Validation Layer
        ↓
CALPADS Extract Generator
        ↓
CSV/API Export
Why

This reduces upgrade risk while allowing California-specific compliance.

8. Proposed California Extension Tables
Student
ca_student_profile
ca_student_demographic_history
ca_student_language
ca_student_ela_status
ca_student_guardian_education
Enrollment
ca_student_enrollment
ca_enrollment_status_history
ca_enrollment_code_map
Programs
ca_program
ca_student_program
ca_student_program_history
Attendance
ca_attendance_code_map
ca_daily_attendance_summary
Discipline
ca_discipline_incident
ca_discipline_student_incident
ca_discipline_action
ca_discipline_code_map
Courses
ca_course_code_map
ca_section_state_reporting_map
CALPADS
ca_calpads_extract_batch
ca_calpads_extract_file
ca_calpads_validation_rule
ca_calpads_validation_result
ca_calpads_submission_error
9. OpenSIS Evaluation Checklist

Use this checklist against the actual OpenSIS database and PHP modules.

Database Review
Identify student master table
Identify enrollment table
Identify school/year/calendar tables
Identify attendance tables
Identify course/section tables
Identify staff/teacher tables
Identify discipline tables, if present
Identify custom field framework, if present
Identify audit/history support
Application Review
Can student profile UI be extended?
Can enrollment workflow be customized?
Can new code sets be configured by district?
Can validation rules be added before save?
Can reports/export files be generated?
Is there a clean module pattern?
Are upgrades likely to overwrite customizations?
Reporting Review
Does OpenSIS already support export templates?
Can CSV files be generated from custom SQL?
Can scheduled exports be run?
Can district-specific extract batches be retained?
10. Feasibility Assessment
Likely Feasible
Adding California demographic fields
Adding code-set tables
Adding program participation tracking
Adding CALPADS extract generation
Adding CSV/API exports
Supporting TK–8 workflows
Moderate Complexity
Effective-dated historical tracking
Enrollment lifecycle validation
Discipline incident/action modeling
SELA history
CALPADS validation rules
High Complexity / Defer
9–12 transcripts
A–G eligibility
Graduation requirements
Cohort tracking
Credit recovery
CCI logic
11. Sequential Action Plan
Phase 1 — Install and Baseline OpenSIS
Install OpenSIS Classic Community Edition.
Load sample TK–8 school data.
Document database schema.
Identify core tables for:
Students
Enrollment
Attendance
Courses
Sections
Staff
Discipline
Custom fields
Phase 2 — California Requirement Mapping
Map OpenSIS student fields to CALPADS SINF.
Map OpenSIS enrollment fields to SENR.
Map attendance fields to California attendance categories.
Map program-related fields to SPRG.
Map EL/language fields to SELA.
Map discipline fields to STAS.
Phase 3 — Gap Analysis
Identify missing columns.
Identify missing history tables.
Identify missing code sets.
Identify missing validation rules.
Identify missing UI workflows.
Rank gaps as:
Critical
High
Medium
Low
Phase 4 — California Data Model Extension
Add California extension tables.
Add centralized code-set tables.
Add crosswalk tables from OpenSIS/local values to California values.
Add effective dating to key California records.
Phase 5 — UI and Workflow Extensions
Extend student demographic screens.
Extend enrollment screens.
Add program participation screens.
Add EL status screens.
Add discipline incident/action screens.
Add code-set administration screens.
Phase 6 — Validation Layer
Create validation rule catalog.
Implement required-field validation.
Implement cross-field validation.
Implement enrollment status validation.
Implement program-date validation.
Implement discipline validation.
Store validation results by student, file, and batch.
Phase 7 — CALPADS Extracts
Build SENR extract.
Build SINF extract.
Build SPRG extract.
Build SELA extract.
Build STAS extract.
Add extract batch tracking.
Add file regeneration and audit history.
Phase 8 — Data Export
Add CSV export for all major domains.
Add API or database views for external systems.
Add scheduled export capability.
Add district-level export configuration.
Phase 9 — Testing
Unit test new tables.
Test UI workflows.
Test validation rules.
Test CALPADS file generation.
Test historical changes.
Test rollback and upgrade behavior.
Phase 10 — Pilot
Pilot with one TK–8 district/school.
Load real student/enrollment/program data.
Run CALPADS-style validation.
Generate test extracts.
Collect feedback.
Finalize production readiness.
12. Success Criteria

OpenSIS is considered California TK–8 ready when:

Student demographics support CALPADS-aligned fields
Enrollment history supports SENR reporting
Program participation supports SPRG reporting
EL status supports SELA reporting
Discipline supports STAS reporting
Required California code sets are configurable
Validation errors are caught before extract generation
CALPADS files can be generated consistently
Data can be exported via CSV/API
The design remains extensible for 9–12 later
13. Key Recommendation

For TK–8, OpenSIS appears feasible as a base platform, but California compliance should be implemented as a California extension layer, not by deeply rewriting core OpenSIS logic. The largest work areas are likely:

CALPADS-aligned data model
Effective-dated history
Program participation
SELA language status
Validation engine
CALPADS extract generation
