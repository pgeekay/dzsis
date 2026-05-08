# PROJECT_PLAN.md

## California TK–8 SIS Extension for OpenSIS Classic
### Phased Development & Testing Strategy

**Status**: Initial Planning Phase  
**Start Date**: 2026-05-07  
**Target**: TK–8 California compliance via extension layer  
**Reference**: Context.MD (master requirements document)

---

## Overview

This document operationalizes the Context.MD requirements into discrete, testable phases. Each phase includes:
- **Development Tasks**: Code changes and deliverables
- **Test Cases**: Unit, integration, and data validation tests
- **Test Data**: Sample datasets for each phase
- **Documentation**: Technical specs, API docs, configuration guides
- **Regression Testing**: Checks to ensure prior phases still work
- **Success Criteria**: Pass/fail metrics for phase completion

---

## Phase Progression & Dependencies

```
Phase 1: Install & Baseline
    ↓
Phase 2: Data Model Design (California Extension Tables)
    ↓
Phase 3: California Code Sets & Crosswalks
    ↓
Phase 4: Student Demographics Extension
    ↓
Phase 5: Enrollment History Extension
    ↓
Phase 6: Program Participation
    ↓
Phase 7: English Learner Status
    ↓
Phase 8: Attendance Extension
    ↓
Phase 9: Discipline Module
    ↓
Phase 10: Validation Layer
    ↓
Phase 11: CALPADS Extraction
    ↓
Phase 12: Data Export & Reporting
    ↓
Phase 13: Testing & Pilot
```

---

## Phase 1: Install & Baseline

**Duration**: 1-2 weeks  
**Goal**: Establish working OpenSIS environment and document baseline schema

### Development Tasks

1. **Install OpenSIS Classic Community Edition**
   - Set up on development server (Apache 2.4+, MySQL 8.0, PHP 8.x)
   - Complete web-based installer
   - Verify login, main modules, basic functionality
   - **Deliverable**: Working OpenSIS installation

2. **Load Sample TK–8 Data**
   - Create test school with TK–8 grades
   - Create sample staff (admin, teachers)
   - Create sample students (20–50 per grade)
   - Create sample enrollment records
   - **Deliverable**: Sample data SQL scripts

3. **Document Database Schema**
   - Extract schema for all core tables
   - Create visual ER diagram
   - Document table purposes, key columns, relationships
   - Identify sequence of foreign keys
   - **Deliverable**: `SCHEMA_BASELINE.md` with DDL and ER diagram

4. **Identify Key Tables**
   - Document student master table and key columns
   - Document enrollment table structure (if exists)
   - Document attendance table structure
   - Document course/section tables
   - Document staff/teacher tables
   - Document discipline tables (if any)
   - Document custom fields framework
   - **Deliverable**: `SCHEMA_MAPPING.md`

5. **Test Core OpenSIS Workflows**
   - Student enrollment
   - Attendance entry
   - Grade entry
   - Report generation
   - User login/logout
   - **Deliverable**: `WORKFLOW_BASELINE.md`

### Test Cases

| Test Case | Input | Expected Output | Pass/Fail |
|-----------|-------|-----------------|-----------|
| Install completes | Run installer | Installation wizard succeeds, app loads | |
| Login works | Valid credentials | Dashboard loads, user role visible | |
| Add student | Student form + data | Student appears in list, can be searched | |
| Enroll student | Student + school + grade | Enrollment appears in records | |
| Record attendance | Student + date + status | Attendance entry saved, viewable | |
| Enter grade | Student + course + grade | Grade saved, appears in gradebook | |
| Export student list | Students module + export | CSV/Excel file downloads | |

### Test Data

```
Sample Schools: 
  - Lincoln Elementary (TK-3)
  - Washington Middle (4-8)

Sample Students (100 total):
  - 15 per grade TK-8
  - Varied demographics
  - Mixed enrollment status (active, transferred, etc.)

Sample Staff:
  - 1 Admin
  - 1 Counselor per school
  - 3-4 Teachers per school
```

**Deliverable**: `test_data_phase1.sql`

### Documentation

- **SCHEMA_BASELINE.md**: Current database structure
- **SCHEMA_MAPPING.md**: OpenSIS table-to-California requirement mapping
- **WORKFLOW_BASELINE.md**: How current OpenSIS handles core operations
- **INSTALLATION_GUIDE.md**: Setup steps for developers

### Regression Testing

- Verify all Phase 1 tests still pass
- Check baseline data integrity
- Confirm no data loss during environment changes

### Success Criteria

✓ OpenSIS running on dev server  
✓ Schema documented with ER diagram  
✓ Sample TK–8 data loaded  
✓ Core workflows tested and documented  
✓ All baseline tests passing  

---

## Phase 2: Data Model Design (California Extension Tables)

**Duration**: 2-3 weeks  
**Goal**: Design and create California extension layer without modifying OpenSIS core tables

### Development Tasks

1. **Design California Extension Schema**
   - Create `ca_code_set` and `ca_code_value` tables for all California code domains
   - Design `ca_code_crosswalk` for mapping OpenSIS values to California values
   - Design student extension tables:
     - `ca_student_profile`
     - `ca_student_demographic_history`
     - `ca_student_language`
     - `ca_student_guardian_education`
   - **Deliverable**: `SCHEMA_DESIGN_CA.md` with DDL scripts

2. **Design Enrollment Extension Tables**
   - `ca_student_enrollment`
   - `ca_enrollment_status_history`
   - `ca_enrollment_code_map`
   - `ca_school_year_calendar`
   - **Deliverable**: DDL scripts, ER diagram

3. **Design Program Participation Tables**
   - `ca_program`
   - `ca_student_program`
   - `ca_student_program_history`
   - `ca_program_code_map`
   - **Deliverable**: DDL scripts

4. **Design Program Support Tables**
   - `ca_attendance_code_map`
   - `ca_discipline_code_map`
   - `ca_course_code_map`
   - **Deliverable**: DDL scripts

5. **Design CALPADS Infrastructure Tables**
   - `ca_calpads_extract_batch`
   - `ca_calpads_extract_file`
   - `ca_calpads_validation_rule`
   - `ca_calpads_validation_result`
   - `ca_calpads_submission_error`
   - **Deliverable**: DDL scripts

6. **Create Migration & Rollback Scripts**
   - Forward migration script: `migrate_phase2_up.sql`
   - Rollback script: `migrate_phase2_down.sql`
   - **Deliverable**: Versioned migration scripts

7. **Create Database Initialization Scripts**
   - Populate `ca_code_set` with California code domains
   - Populate initial code values (TK, K, 1–8, etc.)
   - **Deliverable**: `init_ca_codes_phase2.sql`

### Test Cases

| Test Case | Input | Expected Output | Pass/Fail |
|-----------|-------|-----------------|-----------|
| Create ca_code_set | INSERT code_set record | Record created, ID returned | |
| Add code value | INSERT code_value with set_id FK | Value created with correct FK | |
| Add code crosswalk | INSERT crosswalk mapping | Mapping stored, queryable | |
| Create student profile | INSERT ca_student_profile | Record created, linked to student | |
| Create enrollment | INSERT ca_student_enrollment | Enrollment created with dates | |
| Query enrollment history | SELECT from ca_enrollment_status_history | Returns ordered history by date | |
| Test foreign keys | INSERT invalid FK | Database rejects with FK error | |
| Rollback migration | Run rollback script | All Phase 2 tables dropped | |

### Test Data

- 10 code sets (Grade Levels, Gender, Race/Ethnicity, etc.)
- 200+ code values across all sets
- Sample crosswalks for common mappings
- Sample student profiles linked to test students
- Sample enrollment records with effective dates

**Deliverable**: `test_data_phase2.sql`

### Documentation

- **SCHEMA_DESIGN_CA.md**: Full schema design with rationale
- **TABLE_SPECIFICATIONS.md**: Column specifications, constraints, indexes
- **DATA_DICTIONARY.md**: Field definitions, valid values, business rules
- **MIGRATION_GUIDE.md**: How to run forward/rollback migrations

### Regression Testing

- Phase 1 baseline tests still pass
- OpenSIS core tables unchanged
- No data loss in baseline OpenSIS data
- Sample student data still queryable

### Success Criteria

✓ All California extension tables created  
✓ Foreign keys properly configured  
✓ Migration scripts work forward and backward  
✓ Code sets populated with initial values  
✓ All schema tests passing  
✓ No OpenSIS core table modifications  

---

## Phase 3: California Code Sets & Crosswalks

**Duration**: 2 weeks  
**Goal**: Build configuration framework for California code domains

### Development Tasks

1. **Populate All California Code Domains**
   - Grade levels: TK, K, 1–8
   - Gender codes (per California standards)
   - Race/ethnicity codes (per California standards)
   - Primary language codes (per CALPADS)
   - Enrollment status codes
   - Entry/exit reason codes
   - Program codes (EL, SPED, FY, etc.)
   - EL status codes (EL, IFEP, RFEP, EO)
   - Attendance codes (Present, Unexcused, etc.)
   - Discipline codes
   - Course state codes
   - **Deliverable**: `ca_code_values_complete.sql`

2. **Create Code Administration UI**
   - Module: `/modules/schoolsetup/CACodeSets.php`
   - View/manage code sets
   - Add/edit/delete code values
   - Display code hierarchy
   - **Deliverable**: UI module PHP files

3. **Create Crosswalk Administration UI**
   - Module: `/modules/schoolsetup/CACodeCrosswalk.php`
   - Map OpenSIS values → California values
   - Test/validate mappings
   - Display coverage/gaps
   - **Deliverable**: UI module PHP files

4. **Build Code Lookup Utilities**
   - Function: `GetCACodeValue($code_set_name, $local_value)`
   - Function: `ValidateCACodeSet($code_set_name, $value)`
   - Function: `GetCrosswalkedValue($opensis_value, $mapping_type)`
   - **Deliverable**: `/functions/CACodeFnc.php`

5. **Create Code Set Administration API**
   - REST endpoints for programmatic code management
   - `/api/CACodeSets.php`
   - GET, POST, PUT, DELETE operations
   - **Deliverable**: API PHP file

### Test Cases

| Test Case | Input | Expected Output | Pass/Fail |
|-----------|-------|-----------------|-----------|
| View code set list | Navigate to code admin | All code sets displayed | |
| Add new code value | Form submit + code data | New value in list, ID assigned | |
| Edit code value | Edit form + changes | Changes persisted | |
| Delete code value | Delete action | Code value removed, no FK violations | |
| Map crosswalk | OpenSIS value → CA value | Mapping stored, retrievable | |
| Test code lookup | Call GetCACodeValue() | Returns correct CA value | |
| Validate code set | Call ValidateCACodeSet() | Returns true/false correctly | |
| Get crosswalked value | Call GetCrosswalkedValue() | Returns mapped CA value | |
| API GET codes | GET /api/CACodeSets.php?set=GradeLevel | JSON array of grade levels | |
| API POST new code | POST with code data | New code created, 201 returned | |

### Test Data

- All California code domains fully populated
- Sample crosswalks for common OpenSIS → CA mappings
- Test code values for validation

**Deliverable**: `test_data_phase3.sql`

### Documentation

- **CODE_ADMINISTRATION_GUIDE.md**: How to manage code sets
- **CODE_SETS_REFERENCE.md**: All code domains, values, definitions
- **API_DOCUMENTATION.md**: Code API endpoints and examples
- **CROSSWALK_GUIDE.md**: How to map OpenSIS values to California

### Regression Testing

- Phase 1–2 tests still pass
- Code tables query correctly
- No impact on OpenSIS core

### Success Criteria

✓ All California code domains populated  
✓ Code admin UI functional  
✓ Crosswalk admin UI functional  
✓ Code lookup functions working  
✓ API endpoints operational  
✓ Code validation tests passing  

---

## Phase 4: Student Demographics Extension

**Duration**: 3 weeks  
**Goal**: Extend student profile with California demographic requirements

### Development Tasks

1. **Create California Student Profile Extension UI**
   - Extend OpenSIS student form: `/modules/students/Student.php`
   - Add California demographic fields
   - Link to `ca_student_profile` table
   - **Deliverable**: Modified `Student.php` with new form sections

2. **Build Student Profile Management**
   - Create/edit California student profile
   - Track demographic history (effective dating)
   - Store in `ca_student_demographic_history`
   - **Deliverable**: `/modules/students/CAStudentProfile.php`

3. **Build Language/Acquisition Status UI**
   - Primary language selection
   - Language acquisition status
   - Track changes with effective dates
   - **Deliverable**: `/modules/students/CAStudentLanguage.php`

4. **Build Guardian Education UI**
   - Parent/guardian education level
   - Support multiple parents/guardians
   - **Deliverable**: `/modules/students/CAGuardianEducation.php`

5. **Create Database Sync Functions**
   - Function: `SyncCAStudentProfile($student_id, $profile_data)`
   - Function: `GetCAStudentProfile($student_id, $effective_date)`
   - Function: `GetStudentDemographicHistory($student_id)`
   - **Deliverable**: `/functions/CAStudentFnc.php`

6. **Build Data Validation Functions**
   - Validate required demographic fields
   - Validate code set values against CA codes
   - Validate effective dates
   - **Deliverable**: Validation functions in `CAStudentFnc.php`

7. **Create Import/Sync from OpenSIS**
   - Script to populate `ca_student_profile` from OpenSIS student table
   - Script to sync ongoing student changes
   - **Deliverable**: `/modules/students/CAStudentSync.php`

### Test Cases

| Test Case | Input | Expected Output | Pass/Fail |
|-----------|-------|-----------------|-----------|
| Display CA demographics | Student detail page | CA demographic section visible | |
| Add CA demographic | Form data submit | Profile saved to ca_student_profile | |
| Edit CA demographic | Updated data + date | Change recorded with effective date | |
| View demographic history | History link | All changes displayed with dates | |
| Add language record | Language + date | Record in ca_student_language | |
| Validate required field | Missing required field | Form rejection with error message | |
| Validate code value | Invalid code value | Validation error displayed | |
| Query profile by date | Student + effective date | Returns profile effective on that date | |
| Sync from OpenSIS | Run sync script | ca_student_profile populated | |
| Check for duplicates | Sync script | No duplicate demographic records | |

### Test Data

- 50 sample students with complete California demographics
- Multiple demographic changes per student (history)
- Various language statuses (EL, RFEP, etc.)
- Multiple guardian education records
- Mix of valid and invalid test values

**Deliverable**: `test_data_phase4.sql`

### Documentation

- **STUDENT_DEMOGRAPHICS_GUIDE.md**: How to manage California student demographics
- **DEMOGRAPHIC_FIELDS_REFERENCE.md**: All fields, codes, requirements
- **DEMOGRAPHIC_VALIDATION_RULES.md**: Business rules, cross-field validations
- **IMPORT_PROCEDURES.md**: How to sync OpenSIS students

### Regression Testing

- Phase 1–3 tests still pass
- OpenSIS student data unmodified
- Existing student workflows unchanged
- CA extension does not break core student functionality

### Success Criteria

✓ Student demographics UI operational  
✓ Profile data persisted correctly  
✓ Demographic history tracked with effective dates  
✓ Language/acquisition status captured  
✓ Guardian education tracked  
✓ Validation prevents invalid data  
✓ Data sync script works  
✓ All demographic tests passing  

---

## Phase 5: Enrollment History Extension

**Duration**: 3 weeks  
**Goal**: Track enrollment lifecycle with California requirements

### Development Tasks

1. **Create Enrollment Management UI**
   - Extend OpenSIS enrollment: `/modules/students/Enrollment.php`
   - Add California enrollment fields
   - Link to `ca_student_enrollment` table
   - **Deliverable**: Modified enrollment UI

2. **Build Enrollment Status History Tracking**
   - Track status changes (Active → Transferred, etc.)
   - Store in `ca_enrollment_status_history`
   - Maintain effective dates
   - **Deliverable**: `/modules/scheduling/CAEnrollmentHistory.php`

3. **Build School Year Calendar UI**
   - Manage school years, terms, marking periods
   - Link to `ca_school_year_calendar`
   - **Deliverable**: `/modules/schoolsetup/CASchoolYear.php`

4. **Create Enrollment Code Mapping**
   - Map entry reasons, exit reasons, enrollment statuses
   - Create UI for code maintenance
   - **Deliverable**: `/modules/schoolsetup/CAEnrollmentCodes.php`

5. **Build Enrollment Lifecycle Validation**
   - Validate enrollment dates
   - Validate entry/exit reason combinations
   - Check for overlapping enrollments
   - Validate grade level consistency
   - **Deliverable**: Validation functions in `/functions/CAEnrollmentFnc.php`

6. **Create Enrollment Data Sync**
   - Import OpenSIS enrollment into CA tables
   - Map OpenSIS entry/exit codes to California codes
   - Detect enrollment changes
   - **Deliverable**: `/modules/scheduling/CAEnrollmentSync.php`

7. **Build Enrollment Reports**
   - Active enrollments by grade/school
   - Transfer audit report
   - Exit reason analysis
   - **Deliverable**: `/modules/scheduling/CAEnrollmentReports.php`

### Test Cases

| Test Case | Input | Expected Output | Pass/Fail |
|-----------|-------|-----------------|-----------|
| Create enrollment | Student + school + date | Enrollment in ca_student_enrollment | |
| Change enrollment status | New status + date | Change in history table with date | |
| Set entry reason | Entry reason code | Code validated, stored | |
| Set exit reason | Exit reason code | Code validated, stored | |
| Query enrollment on date | Student + date | Active enrollment for that date | |
| Detect overlap | Overlapping enrollments | Validation error | |
| Validate dates | Invalid date range | Validation error | |
| View enrollment history | Student detail | All status changes listed | |
| School year setup | Create school year | Record in ca_school_year_calendar | |
| Generate enroll report | Report parameters | Report shows active enrollments | |
| Sync enrollments | Run sync script | ca_student_enrollment populated | |

### Test Data

- 50 students with complete enrollment history
- Multiple schools, years
- Various entry/exit reasons
- Transfer scenarios
- Grade level changes

**Deliverable**: `test_data_phase5.sql`

### Documentation

- **ENROLLMENT_MANAGEMENT_GUIDE.md**: How to manage California enrollment
- **ENROLLMENT_LIFECYCLE.md**: Enrollment states, transitions, validations
- **ENROLLMENT_CODES_REFERENCE.md**: Entry/exit reasons, status codes
- **ENROLLMENT_REPORTS_GUIDE.md**: Available reports

### Regression Testing

- Phase 1–4 tests still pass
- OpenSIS core enrollment unmodified
- Student demographics still work
- Code sets still functional

### Success Criteria

✓ Enrollment UI operational  
✓ Enrollment history tracked with dates  
✓ Entry/exit codes captured and validated  
✓ Enrollment lifecycle validations working  
✓ Overlapping enrollment detection active  
✓ Reports generate correctly  
✓ Data sync script works  
✓ All enrollment tests passing  

---

## Phase 6: Program Participation

**Duration**: 3 weeks  
**Goal**: Track student program participation (EL, SPED, FY, Homeless, etc.)

### Development Tasks

1. **Create Program Catalog UI**
   - Manage program definitions
   - Map to California codes
   - **Deliverable**: `/modules/schoolsetup/CAProgram.php`

2. **Build Student Program Enrollment UI**
   - Enroll student in programs
   - Track effective dates
   - **Deliverable**: `/modules/students/CAProgramEnrollment.php`

3. **Build Program History UI**
   - View program participation timeline
   - Track program start/end dates
   - **Deliverable**: `/modules/students/CAProgramHistory.php`

4. **Create Program Eligibility Tracking**
   - Flag eligibility status
   - Track determination source
   - Store eligibility date
   - **Deliverable**: Functions in `/functions/CAProgramFnc.php`

5. **Build Program Validation**
   - Validate program dates
   - Check for conflicting programs
   - Validate entry/exit reasons for programs
   - **Deliverable**: Validation functions

6. **Create Program Reporting**
   - Students by program
   - Program participation timeline
   - Eligibility status reports
   - **Deliverable**: `/modules/tools/CAProgramReports.php`

### Test Cases

| Test Case | Input | Expected Output | Pass/Fail |
|-----------|-------|-----------------|-----------|
| Create program | Program data | Program in ca_program table | |
| Enroll student | Student + program + date | Enrollment in ca_student_program | |
| End program | Program + end date | Record marked inactive | |
| View program history | Student detail | All programs listed with dates | |
| Validate dates | Invalid date range | Validation error | |
| Detect conflict | Conflicting programs | Validation warning/error | |
| Query programs | Student + date | Return programs active on that date | |
| Generate report | Report parameters | Program participation list | |

### Test Data

- 6–8 program definitions (EL, SPED, FY, Homeless, Migrant, Title I, 504, GT)
- 50 students with varied program enrollments
- Multiple start/end dates per student

**Deliverable**: `test_data_phase6.sql`

### Documentation

- **PROGRAM_MANAGEMENT_GUIDE.md**
- **PROGRAM_CODES_REFERENCE.md**
- **PROGRAM_PARTICIPATION_RULES.md**

### Regression Testing

- Phase 1–5 tests pass
- Student demographics/enrollment unaffected
- Code sets functional

### Success Criteria

✓ Program catalog created  
✓ Student program enrollment working  
✓ Program history tracked  
✓ Validation rules enforce  
✓ Reports operational  
✓ All program tests passing  

---

## Phase 7: English Learner (EL) Status

**Duration**: 2 weeks  
**Goal**: Track California English Language Acquisition (ELA) status

### Development Tasks

1. **Create EL Status UI**
   - Record EL status (EL, IFEP, RFEP, EO)
   - Track status change dates
   - **Deliverable**: `/modules/students/CAELStatus.php`

2. **Build SELA History Tracking**
   - Store in `ca_student_ela_status` and `ca_student_ela_history`
   - Maintain effective dates
   - **Deliverable**: EL status management module

3. **Create ELPAC Tracking**
   - Record initial ELPAC date/status
   - Track proficiency level changes
   - **Deliverable**: Functions for ELPAC tracking

4. **Build EL Status Validation**
   - Validate status transitions
   - Validate reclassification dates
   - Check consistency with program enrollment
   - **Deliverable**: Validation functions

5. **Create EL Reporting**
   - EL student list
   - Reclassification candidates
   - ELPAC status report
   - **Deliverable**: `/modules/tools/CAELReports.php`

### Test Cases

| Test Case | Input | Expected Output | Pass/Fail |
|-----------|-------|-----------------|-----------|
| Record EL status | Student + status + date | Status in ca_student_ela_status | |
| Change status | Status change + date | Change in history table | |
| Record ELPAC date | ELPAC assessment date | Date stored | |
| Query status | Student + date | Return EL status effective on date | |
| Validate transition | Invalid status change | Validation error | |
| Generate EL report | Report parameters | List of EL students | |

### Test Data

- 50 students with varied EL statuses
- ELPAC assessment dates
- Reclassification dates for RFEP students

**Deliverable**: `test_data_phase7.sql`

### Documentation

- **EL_STATUS_MANAGEMENT_GUIDE.md**
- **EL_CODES_REFERENCE.md**
- **ELPAC_TRACKING_GUIDE.md**

### Regression Testing

- Phase 1–6 tests pass
- Program enrollment unaffected (EL is a program)

### Success Criteria

✓ EL status UI operational  
✓ Status history tracked  
✓ ELPAC data captured  
✓ Validation working  
✓ Reports generated  
✓ All EL tests passing  

---

## Phase 8: Attendance Extension

**Duration**: 2 weeks  
**Goal**: Align attendance with California code requirements

### Development Tasks

1. **Create California Attendance Code Mapping**
   - Map OpenSIS attendance status to CA codes
   - **Deliverable**: `/modules/schoolsetup/CAAttendanceCode.php`

2. **Extend Attendance Recording**
   - Capture California-aligned attendance codes
   - Ensure daily attendance captured
   - **Deliverable**: Modifications to attendance UI

3. **Build Attendance Aggregation**
   - Create `ca_daily_attendance_summary`
   - Daily counts by status
   - **Deliverable**: `/functions/CAAttendanceFnc.php`

4. **Create Attendance Validation**
   - Validate attendance status codes
   - Check for missing daily attendance
   - **Deliverable**: Validation functions

5. **Build Attendance Reports**
   - Daily attendance roster
   - Attendance summary by student
   - Chronic absence alerts
   - **Deliverable**: `/modules/tools/CAAttendanceReports.php`

### Test Cases

| Test Case | Input | Expected Output | Pass/Fail |
|-----------|-------|-----------------|-----------|
| Record attendance | Student + date + status | ca_daily_attendance record | |
| Map to CA code | OpenSIS status | CA code assigned | |
| Validate code | Invalid code | Validation error | |
| Query attendance | Student + date range | Attendance records returned | |
| Generate report | Report parameters | Attendance summary | |

### Test Data

- Daily attendance records for all students over period
- Various attendance statuses

**Deliverable**: `test_data_phase8.sql`

### Documentation

- **ATTENDANCE_MANAGEMENT_GUIDE.md**
- **ATTENDANCE_CODES_REFERENCE.md**

### Regression Testing

- Phase 1–7 tests pass
- OpenSIS attendance unmodified
- EL status unaffected

### Success Criteria

✓ CA attendance codes mapped  
✓ Daily attendance tracked  
✓ Validation working  
✓ Reports generated  
✓ All attendance tests passing  

---

## Phase 9: Discipline Module

**Duration**: 3 weeks  
**Goal**: Track student discipline with California STAS alignment

### Development Tasks

1. **Create Discipline Incident UI**
   - Record discipline incidents
   - Capture incident details
   - **Deliverable**: `/modules/tools/CADisciplineIncident.php`

2. **Build Discipline Action Tracking**
   - Record actions taken (suspension, expulsion, etc.)
   - Track start/end dates
   - **Deliverable**: Discipline action module

3. **Create Discipline Code Mapping**
   - Map incident types to California codes
   - Map actions to CA action codes
   - **Deliverable**: `/modules/schoolsetup/CADisciplineCode.php`

4. **Build Discipline Validation**
   - Validate incident/action codes
   - Validate date ranges
   - Check for legal compliance rules
   - **Deliverable**: Validation functions

5. **Create Discipline Reporting**
   - Student discipline history
   - Incident/action summary
   - Suspension/expulsion tracking
   - **Deliverable**: `/modules/tools/CADisciplineReports.php`

### Test Cases

| Test Case | Input | Expected Output | Pass/Fail |
|-----------|-------|-----------------|-----------|
| Record incident | Incident data | Incident in ca_discipline_incident | |
| Record action | Action data | Action in ca_discipline_action | |
| Link student | Student + incident | Student linked in ca_discipline_student_incident | |
| Validate codes | Invalid code | Validation error | |
| Query history | Student | All discipline records listed | |
| Generate report | Report parameters | Discipline summary report | |

### Test Data

- Sample discipline incidents and actions
- Various incident types and action types

**Deliverable**: `test_data_phase9.sql`

### Documentation

- **DISCIPLINE_MANAGEMENT_GUIDE.md**
- **DISCIPLINE_CODES_REFERENCE.md**

### Regression Testing

- Phase 1–8 tests pass
- No impact on other modules

### Success Criteria

✓ Discipline incident/action UI operational  
✓ CA codes mapped  
✓ Validation working  
✓ Reports generated  
✓ All discipline tests passing  

---

## Phase 10: Validation Layer

**Duration**: 4 weeks  
**Goal**: Build comprehensive validation framework

### Development Tasks

1. **Create Validation Rule Engine**
   - Load validation rules from database
   - Execute rules before data save
   - Store validation results
   - **Deliverable**: `/functions/CAValidationFnc.php`

2. **Implement Field-Level Validation**
   - Required field validation
   - Code set validation
   - Date format validation
   - **Deliverable**: Validation functions

3. **Implement Cross-Field Validation**
   - Enrollment status + date logic
   - Program participation consistency
   - EL status + program logic
   - **Deliverable**: Cross-field validators

4. **Build Validation Result Storage**
   - Store validation errors in `ca_calpads_validation_result`
   - Link errors to student/batch/file
   - **Deliverable**: Result storage functions

5. **Create Validation Dashboard**
   - View validation errors by student
   - View errors by error type
   - Drill down to detail
   - **Deliverable**: `/modules/tools/CAValidationDashboard.php`

6. **Build Bulk Validation**
   - Validate all students in batch
   - Generate validation report
   - **Deliverable**: `/modules/tools/CABulkValidation.php`

### Test Cases

| Test Case | Input | Expected Output | Pass/Fail |
|-----------|-------|-----------------|-----------|
| Validate required field | Missing required field | Validation error recorded | |
| Validate code value | Invalid code | Validation error recorded | |
| Validate dates | Invalid date range | Validation error recorded | |
| Validate cross-field | Invalid combination | Validation error recorded | |
| Valid data | Complete valid data | No validation errors | |
| View errors | Student with errors | Error list displayed | |
| Bulk validate | All students | Validation report generated | |

### Test Data

- Students with valid and invalid data
- Various validation error scenarios

**Deliverable**: `test_data_phase10.sql`

### Documentation

- **VALIDATION_RULES_REFERENCE.md**
- **VALIDATION_DASHBOARD_GUIDE.md**

### Regression Testing

- Phase 1–9 tests pass
- Validation does not prevent valid data save
- Error storage functional

### Success Criteria

✓ Validation rule engine working  
✓ All validation types implemented  
✓ Error storage functional  
✓ Dashboard operational  
✓ All validation tests passing  

---

## Phase 11: CALPADS Extraction

**Duration**: 4 weeks  
**Goal**: Generate California CALPADS extract files

### Development Tasks

1. **Build SENR Extract (Enrollment)**
   - Extract enrollment data from CA tables
   - Format per CALPADS SENR spec
   - **Deliverable**: `/modules/tools/CAExtractSENR.php`

2. **Build SINF Extract (Student Info)**
   - Extract student demographics
   - Format per CALPADS SINF spec
   - **Deliverable**: `/modules/tools/CAExtractSINF.php`

3. **Build SPRG Extract (Programs)**
   - Extract program participation
   - Format per CALPADS SPRG spec
   - **Deliverable**: `/modules/tools/CAExtractSPRG.php`

4. **Build SELA Extract (EL Status)**
   - Extract EL status
   - Format per CALPADS SELA spec
   - **Deliverable**: `/modules/tools/CAExtractSELA.php`

5. **Build STAS Extract (Discipline)**
   - Extract discipline incidents/actions
   - Format per CALPADS STAS spec
   - **Deliverable**: `/modules/tools/CAExtractSTAS.php`

6. **Create Extract Batch Management**
   - Create batch records in `ca_calpads_extract_batch`
   - Track file generation
   - Store file metadata
   - **Deliverable**: `/modules/tools/CAExtractBatch.php`

7. **Build Extract Validation**
   - Validate extract data before generation
   - Run all Phase 10 validations
   - Report validation errors
   - **Deliverable**: Pre-extract validation

8. **Create Extract File Management**
   - Generate CSV files per CALPADS spec
   - Store files in archive
   - Implement file versioning
   - **Deliverable**: File generation functions

### Test Cases

| Test Case | Input | Expected Output | Pass/Fail |
|-----------|-------|-----------------|-----------|
| Extract SENR | Parameters | SENR CSV with correct columns/format | |
| Extract SINF | Parameters | SINF CSV with correct columns/format | |
| Extract SPRG | Parameters | SPRG CSV with correct columns/format | |
| Extract SELA | Parameters | SELA CSV with correct columns/format | |
| Extract STAS | Parameters | STAS CSV with correct columns/format | |
| Validate before extract | Data with errors | Validation errors shown, extract blocked | |
| Generate batch | Batch parameters | Batch record created, files generated | |
| Download file | File selection | CSV file downloads | |

### Test Data

- Complete student data set with all extensions
- Valid and invalid scenarios for testing
- Multiple schools, years

**Deliverable**: `test_data_phase11.sql`

### Documentation

- **CALPADS_EXTRACT_GUIDE.md**
- **SENR_EXTRACT_SPEC.md**
- **SINF_EXTRACT_SPEC.md**
- **SPRG_EXTRACT_SPEC.md**
- **SELA_EXTRACT_SPEC.md**
- **STAS_EXTRACT_SPEC.md**

### Regression Testing

- Phase 1–10 tests pass
- Extract does not modify data
- Batch tracking functional

### Success Criteria

✓ All 5 CALPADS extracts generating  
✓ Correct CSV format per spec  
✓ Validation prevents bad extracts  
✓ Batch tracking functional  
✓ Files archivable and downloadable  
✓ All extract tests passing  

---

## Phase 12: Data Export & Reporting

**Duration**: 2 weeks  
**Goal**: Enable data export for external systems

### Development Tasks

1. **Create CSV Export Module**
   - Export by entity (students, enrollments, programs, etc.)
   - Configurable column selection
   - **Deliverable**: `/modules/tools/CADataExport.php`

2. **Build Scheduled Exports**
   - Configure scheduled data extracts
   - Email export files to administrators
   - **Deliverable**: Scheduled export functions

3. **Create Data API**
   - REST API for California data
   - `/api/CAStudents.php`, `/api/CAEnrollments.php`, etc.
   - JSON/XML response formats
   - **Deliverable**: API modules

4. **Build Export Template System**
   - Predefined export templates
   - Custom template creation
   - Template scheduling
   - **Deliverable**: Export template management

5. **Create Data Dictionary Export**
   - Document all fields, codes, definitions
   - Generate for external consumers
   - **Deliverable**: Data dictionary generation

### Test Cases

| Test Case | Input | Expected Output | Pass/Fail |
|-----------|-------|-----------------|-----------|
| Export students CSV | Parameters | CSV file with student data | |
| Select columns | Column selection | Export includes only selected columns | |
| Schedule export | Schedule config | Export runs at scheduled time | |
| API GET students | Parameters | JSON array of student data | |
| API GET enrollments | Parameters | JSON array of enrollments | |
| Generate data dictionary | Parameters | Data dictionary document | |

### Test Data

- Complete data set for export testing

**Deliverable**: `test_data_phase12.sql`

### Documentation

- **DATA_EXPORT_GUIDE.md**
- **API_REFERENCE.md**
- **EXPORT_TEMPLATES_GUIDE.md**

### Regression Testing

- Phase 1–11 tests pass
- Export does not modify data

### Success Criteria

✓ CSV export working  
✓ API endpoints functional  
✓ Scheduled exports operational  
✓ Data dictionary generated  
✓ All export tests passing  

---

## Phase 13: Testing & Pilot

**Duration**: 4-6 weeks  
**Goal**: Comprehensive testing and pilot with real data

### Development Tasks

1. **Unit Test Suite**
   - Create PHPUnit tests for all functions
   - Test coverage: >80%
   - **Deliverable**: `/tests/` directory with test files

2. **Integration Test Suite**
   - Test workflows across modules
   - Test data consistency
   - **Deliverable**: Integration test files

3. **Data Validation Test Suite**
   - Test all validation rules
   - Test with edge cases
   - **Deliverable**: Validation test files

4. **CALPADS Compliance Test**
   - Extract format compliance
   - Data accuracy verification
   - **Deliverable**: Compliance test checklist

5. **Performance Testing**
   - Test with 10K+ students
   - Measure query performance
   - Identify bottlenecks
   - **Deliverable**: Performance report

6. **Security Testing**
   - SQL injection testing
   - XSS prevention testing
   - Authorization testing
   - **Deliverable**: Security review report

7. **Pilot with Real Data**
   - Partner with TK–8 district
   - Load real student data
   - Run real workflows
   - Generate real CALPADS extracts
   - Collect feedback
   - **Deliverable**: Pilot report

8. **Documentation Finalization**
   - Update all user guides
   - Create administrator guides
   - Create training materials
   - **Deliverable**: Complete documentation set

### Test Cases

| Test Case | Input | Expected Output | Pass/Fail |
|-----------|-------|-----------------|-----------|
| Unit tests | Code + unit tests | >80% coverage, all pass | |
| Integration tests | Workflows | All workflows functional | |
| Validation tests | Various data | All rules validated correctly | |
| CALPADS compliance | Extract files | Format validated against spec | |
| Performance | 10K students | Query response <2s | |
| Security | Injection attempts | Attempts blocked | |
| Pilot data load | 5K real students | Data loads, no errors | |
| Pilot workflows | Real workflows | Workflows complete successfully | |

### Test Data

- Sample and real (pilot) data sets

**Deliverable**: `test_data_phase13.sql` + real pilot data

### Documentation

- **USER_GUIDE.md**
- **ADMINISTRATOR_GUIDE.md**
- **TRAINING_MATERIALS.md**
- **TROUBLESHOOTING_GUIDE.md**
- **PILOT_REPORT.md**

### Regression Testing

- All Phase 1–12 tests still pass
- Full system integration test

### Success Criteria

✓ >80% code coverage in unit tests  
✓ All integration tests passing  
✓ Performance acceptable (sub-2s queries)  
✓ CALPADS extract validates against spec  
✓ Security scan passes  
✓ Pilot with real district data successful  
✓ All feedback addressed  
✓ Documentation complete  

---

## Regression Testing Across All Phases

### Regression Test Checklist

Run this checklist after completing each phase:

**Phase 1–2: Schema & Infrastructure**
- [ ] OpenSIS core tables unchanged
- [ ] All CA tables created successfully
- [ ] Foreign keys enforced
- [ ] Migration forward/backward works
- [ ] Sample data loads without error

**Phase 3–4: Code Sets & Demographics**
- [ ] Code set queries return correct values
- [ ] Code validation functions work
- [ ] Student form displays new fields
- [ ] Demographic data persists
- [ ] History tracked with effective dates

**Phase 5–6: Enrollment & Programs**
- [ ] Enrollment records create/update
- [ ] Status history tracked
- [ ] Program enrollment works
- [ ] Program history tracked
- [ ] Overlapping enrollment detection works

**Phase 7–9: EL, Attendance, Discipline**
- [ ] EL status UI functional
- [ ] Attendance codes map correctly
- [ ] Discipline incidents record
- [ ] All data persists
- [ ] Reports generate

**Phase 10–11: Validation & CALPADS**
- [ ] Validation rules execute before save
- [ ] Invalid data blocked
- [ ] Valid data passes
- [ ] CALPADS extracts generate
- [ ] Extract format correct

**Phase 12–13: Export, Testing, Pilot**
- [ ] CSV export works
- [ ] API endpoints functional
- [ ] Unit tests >80% coverage
- [ ] Performance acceptable
- [ ] Real data pilot successful

### Full Regression Test Suite

```bash
# Run all unit tests
phpunit tests/

# Run integration tests
phpunit tests/integration/

# Load test data
mysql < test_data_phase1.sql
mysql < test_data_phase2.sql
... (etc)

# Run baseline workflows
# - Student enrollment
# - Attendance entry
# - Grade entry
# - CALPADS extract generation
# - CSV export

# Verify no data loss
# - Check OpenSIS core tables unchanged
# - Check CA extension tables populated
# - Verify referential integrity
```

---

## Test Data Management

### Test Data Locations

- **Phase 1**: `test_data_phase1.sql` (baseline OpenSIS data)
- **Phase 2**: `test_data_phase2.sql` (schema infrastructure)
- **Phase 3**: `test_data_phase3.sql` (code sets)
- **Phase 4**: `test_data_phase4.sql` (student demographics)
- **Phase 5**: `test_data_phase5.sql` (enrollment)
- **Phase 6**: `test_data_phase6.sql` (programs)
- **Phase 7**: `test_data_phase7.sql` (EL status)
- **Phase 8**: `test_data_phase8.sql` (attendance)
- **Phase 9**: `test_data_phase9.sql` (discipline)
- **Phase 10**: `test_data_phase10.sql` (validation scenarios)
- **Phase 11**: `test_data_phase11.sql` (CALPADS test)
- **Phase 12**: `test_data_phase12.sql` (export test)
- **Phase 13**: Real pilot data (subject to confidentiality)

### Test Data Characteristics

- **Sample Size**: 50–100 students per phase (scales to 10K+ in Phase 13)
- **Coverage**: All code values, all workflows, edge cases
- **Validity**: Mix of valid and invalid data for validation testing
- **Diversity**: Varied demographics, programs, enrollment patterns

### Cleanup

After each phase, preserve test data in version control, but clear from development databases before next phase to avoid conflicts.

---

## Success Metrics

### By Phase

| Phase | Success Metric |
|-------|----------------|
| 1 | OpenSIS baseline documented, sample data loaded |
| 2 | All CA extension tables created, migrated |
| 3 | Code sets populated, admin UI functional |
| 4 | Student demographics captured, history tracked |
| 5 | Enrollment lifecycle tracked, validation working |
| 6 | Program participation tracked |
| 7 | EL status captured and validated |
| 8 | Attendance aligned to CA codes |
| 9 | Discipline incidents/actions recorded |
| 10 | Validation rules enforced, errors tracked |
| 11 | All 5 CALPADS extracts generating |
| 12 | Data export and API operational |
| 13 | Pilot complete, documentation finalized |

### Overall Success Criteria

✓ OpenSIS extended without core modification  
✓ All California TK–8 requirements addressed  
✓ CALPADS extracts generate correctly  
✓ Data validation prevents invalid data  
✓ System validated with real pilot data  
✓ Documentation complete and validated  
✓ Team trained and ready for production  

---

## Documentation Deliverables by Phase

### By Phase

| Phase | Documentation | Location |
|-------|---------------|----------|
| 1 | SCHEMA_BASELINE.md, WORKFLOW_BASELINE.md | `/docs/` |
| 2 | SCHEMA_DESIGN_CA.md, TABLE_SPECIFICATIONS.md | `/docs/` |
| 3 | CODE_ADMINISTRATION_GUIDE.md, CODE_SETS_REFERENCE.md | `/docs/` |
| 4 | STUDENT_DEMOGRAPHICS_GUIDE.md, DEMOGRAPHIC_FIELDS_REFERENCE.md | `/docs/` |
| 5 | ENROLLMENT_MANAGEMENT_GUIDE.md, ENROLLMENT_LIFECYCLE.md | `/docs/` |
| 6 | PROGRAM_MANAGEMENT_GUIDE.md, PROGRAM_CODES_REFERENCE.md | `/docs/` |
| 7 | EL_STATUS_MANAGEMENT_GUIDE.md, EL_CODES_REFERENCE.md | `/docs/` |
| 8 | ATTENDANCE_MANAGEMENT_GUIDE.md, ATTENDANCE_CODES_REFERENCE.md | `/docs/` |
| 9 | DISCIPLINE_MANAGEMENT_GUIDE.md, DISCIPLINE_CODES_REFERENCE.md | `/docs/` |
| 10 | VALIDATION_RULES_REFERENCE.md, VALIDATION_DASHBOARD_GUIDE.md | `/docs/` |
| 11 | CALPADS_EXTRACT_GUIDE.md, Extract spec docs (SENR, SINF, etc.) | `/docs/` |
| 12 | DATA_EXPORT_GUIDE.md, API_REFERENCE.md | `/docs/` |
| 13 | USER_GUIDE.md, ADMINISTRATOR_GUIDE.md, TRAINING_MATERIALS.md | `/docs/` |

### Master Documentation Index

Create `/docs/INDEX.md` that links all documentation with:
- Purpose
- Audience (developer, administrator, end-user)
- Phase created
- Related documents

---

## Version Control & Branch Strategy

### Branch Naming

```
main (production-ready)
  ↓
develop (integration branch)
  ├─ feature/phase-1-baseline
  ├─ feature/phase-2-schema
  ├─ feature/phase-3-codes
  └─ ...
```

### Commit Message Convention

```
[Phase X] Component: Brief description

Detailed explanation of changes.

Related: Context.MD Section X.Y
Closes: #issue-number (if applicable)
```

### Tag Convention

```
v0.1.0-phase-1 (baseline complete)
v0.2.0-phase-2 (schema complete)
v0.3.0-phase-3 (codes complete)
...
v1.0.0 (pilot complete, production-ready)
```

---

## Risk & Mitigation

### Known Risks

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|-----------|
| OpenSIS modifications lose during upgrades | Medium | High | Separate extension tables, not core mods |
| Data validation misses cases | Medium | High | Comprehensive test suite, pilot feedback |
| CALPADS spec interpretation errors | Medium | High | Validate extracts with district, CDE |
| Performance degradation with large dataset | Medium | Medium | Performance testing in Phase 13, optimize queries |
| Pilot district feedback requires rework | High | Medium | Early engagement, pilot preparation |

### Mitigation Strategies

1. **Separation of Concerns**: Keep OpenSIS core untouched; all extensions in CA-prefixed tables
2. **Rigorous Testing**: >80% code coverage, comprehensive validation tests
3. **External Validation**: Pilot with real district, validate extracts with CDE
4. **Performance Monitoring**: Regular performance testing, query optimization
5. **Early Feedback**: Engage pilot district early, iterative improvements

---

## Kickoff Checklist

- [ ] Context.MD and PROJECT_PLAN.md reviewed and approved
- [ ] Development environment set up (PHP 8.x, MySQL 8.0, Apache 2.4+)
- [ ] Version control configured (Git, branching strategy)
- [ ] Documentation structure set up (`/docs/`, `CLAUDE.md`, etc.)
- [ ] Team trained on architecture and development guidelines
- [ ] Phase 1 development starts

---

## Questions & Next Steps

1. **Do we need to support 9–12 later?** → Design allows for extensibility
2. **Is there a preferred CALPADS validation reference?** → Identified in Phase 11 scope
3. **Should we integrate with external CALPADS submission system?** → Deferred to Phase 14
4. **What is the pilot district?** → Identified in Phase 13 planning

---

**This PROJECT_PLAN.md should be reviewed and updated as each phase completes.**

Last Updated: 2026-05-07
