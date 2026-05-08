# PHASE_1_KICKOFF.md

## Phase 1: Install & Baseline - Detailed Kickoff Plan

**Phase**: 1 of 13  
**Duration**: 1–2 weeks  
**Goal**: Establish working OpenSIS environment and document baseline  
**Reference**: PROJECT_PLAN.md Phase 1 section

---

## Pre-Kickoff Checklist

Before starting Phase 1 work:

- [ ] Read `CLAUDE.md` (understand architecture)
- [ ] Read `Context.MD` (understand requirements)
- [ ] Read `PROJECT_PLAN.md` Phase 1 section
- [ ] Read `LOCAL_SETUP_GUIDE.md` (local environment)
- [ ] Read `GIT_WORKFLOW.md` (git strategy)
- [ ] Local MySQL/MariaDB installed and running
- [ ] PHP 8.x installed
- [ ] Git repository cloned, on `ca-extension` branch
- [ ] Assigned to Phase 1 work in your team

---

## Phase 1 Development Tasks

### Task 1.1: Install OpenSIS Classic

**Objective**: Get OpenSIS running on local machine  
**Estimated Time**: 1–2 hours

#### Steps

1. **Clone Repository**
   ```bash
   cd ~/projects
   git clone https://github.com/OS4ED/openSIS-Classic.git opensis-dzsis
   cd opensis-dzsis
   git checkout ca-extension
   git pull origin ca-extension
   ```

2. **Create Feature Branch**
   ```bash
   git checkout -b feature/phase-1-baseline
   ```

3. **Create Local MySQL Database**
   ```sql
   mysql -u root -p
   
   CREATE DATABASE opensis_classic CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
   CREATE USER 'opensis'@'localhost' IDENTIFIED BY 'openSIS_DevPass123!';
   GRANT ALL PRIVILEGES ON opensis_classic.* TO 'opensis'@'localhost';
   FLUSH PRIVILEGES;
   ```

4. **Start Web Server**
   ```bash
   cd ~/projects/opensis-dzsis
   php -S localhost:8000
   # Navigate to http://localhost:8000
   ```

5. **Run OpenSIS Installer**
   - Access: http://localhost:8000
   - Follow installer steps
   - Database: localhost, opensis, openSIS_DevPass123!, opensis_classic
   - Create admin user (e.g., admin / AdminPassword123!)
   - Create sample school: "Lincoln Elementary (TK-3)" for year 2025
   - Complete installation
   - Delete `install/` directory

6. **Verify Installation**
   - Login with admin credentials
   - Dashboard loads without errors
   - All modules visible in menu

**Deliverable**: Working OpenSIS installation on localhost:8000

**Test Cases to Pass**:
- [ ] Installation completes without errors
- [ ] Admin login successful
- [ ] Dashboard loads
- [ ] No fatal PHP errors in browser console
- [ ] `Data.php` created with correct credentials

---

### Task 1.2: Load Sample TK–8 Data

**Objective**: Create realistic test data for development  
**Estimated Time**: 2–3 hours

#### Create Sample Schools and Grades

1. **Via OpenSIS UI** (Optional):
   - Navigate to School Setup → Schools
   - Create additional schools if needed
   - Set marking periods for 2025-2026 school year

2. **Via SQL** (Recommended for Phase 1):
   - Create file: `sample_data_phase1.sql`
   - See code below

#### SQL Script: sample_data_phase1.sql

```sql
-- Sample TK-8 Data for Phase 1

-- Create sample schools (if not already created)
-- Note: Adjust IDs/values based on your OpenSIS setup

-- Create sample staff
INSERT INTO staff (STAFF_ID, STAFF_NUMBER, FIRST_NAME, LAST_NAME, MIDDLE_NAME, EMAIL, SYEAR)
VALUES 
  (1001, 'T001', 'John', 'Smith', 'M', 'jsmith@school.local', 2025),
  (1002, 'T002', 'Jane', 'Doe', 'A', 'jdoe@school.local', 2025),
  (1003, 'A001', 'Robert', 'Johnson', 'L', 'rjohnson@school.local', 2025);

-- Create sample students (TK-8 grades)
-- Kindergarten students (Grade K)
INSERT INTO students (STUDENT_ID, STUDENT_NUMBER, FIRST_NAME, LAST_NAME, MIDDLE_NAME, DOB, SEX, SYEAR)
VALUES
  (100001, 'S001', 'Alice', 'Anderson', 'Anne', '2018-06-15', 'F', 2025),
  (100002, 'S002', 'Bob', 'Brown', 'Benjamin', '2018-07-20', 'M', 2025),
  (100003, 'S003', 'Carol', 'Carter', 'Catherine', '2018-08-10', 'F', 2025),
  (100004, 'S004', 'David', 'Davis', 'Daniel', '2018-05-25', 'M', 2025),
  (100005, 'S005', 'Emma', 'Evans', 'Elizabeth', '2018-09-05', 'F', 2025);

-- Grade 1 students (5 students)
INSERT INTO students (STUDENT_ID, STUDENT_NUMBER, FIRST_NAME, LAST_NAME, MIDDLE_NAME, DOB, SEX, SYEAR)
VALUES
  (100006, 'S006', 'Frank', 'Franklin', 'Frederick', '2017-06-15', 'M', 2025),
  (100007, 'S007', 'Grace', 'Garcia', 'Gabrielle', '2017-07-20', 'F', 2025),
  (100008, 'S008', 'Henry', 'Harris', 'Hugh', '2017-08-10', 'M', 2025),
  (100009, 'S009', 'Iris', 'Ingram', 'Isabella', '2017-05-25', 'F', 2025),
  (100010, 'S010', 'Jack', 'Jackson', 'James', '2017-09-05', 'M', 2025);

-- Grade 2 students (5 students)
INSERT INTO students (STUDENT_ID, STUDENT_NUMBER, FIRST_NAME, LAST_NAME, MIDDLE_NAME, DOB, SEX, SYEAR)
VALUES
  (100011, 'S011', 'Kate', 'Kelly', 'Katherine', '2016-06-15', 'F', 2025),
  (100012, 'S012', 'Leo', 'Lewis', 'Leonard', '2016-07-20', 'M', 2025),
  (100013, 'S013', 'Mia', 'Miller', 'Michelle', '2016-08-10', 'F', 2025),
  (100014, 'S014', 'Noah', 'Nelson', 'Nicholas', '2016-05-25', 'M', 2025),
  (100015, 'S015', 'Olivia', 'Ortiz', 'Ophelia', '2016-09-05', 'F', 2025);

-- Grade 3 students (5 students)
INSERT INTO students (STUDENT_ID, STUDENT_NUMBER, FIRST_NAME, LAST_NAME, MIDDLE_NAME, DOB, SEX, SYEAR)
VALUES
  (100016, 'S016', 'Paul', 'Parker', 'Peter', '2015-06-15', 'M', 2025),
  (100017, 'S017', 'Quinn', 'Quinn', 'Quinton', '2015-07-20', 'M', 2025),
  (100018, 'S018', 'Rachel', 'Roberts', 'Rose', '2015-08-10', 'F', 2025),
  (100019, 'S019', 'Sam', 'Smith', 'Samuel', '2015-05-25', 'M', 2025),
  (100020, 'S020', 'Tina', 'Taylor', 'Theresa', '2015-09-05', 'F', 2025);

-- Grade 4 students (5 students) - Washington Middle
INSERT INTO students (STUDENT_ID, STUDENT_NUMBER, FIRST_NAME, LAST_NAME, MIDDLE_NAME, DOB, SEX, SYEAR)
VALUES
  (100021, 'S021', 'Uma', 'Underwood', 'Ursula', '2014-06-15', 'F', 2025),
  (100022, 'S022', 'Victor', 'Valdez', 'Vincent', '2014-07-20', 'M', 2025),
  (100023, 'S023', 'Wendy', 'Watson', 'Wanda', '2014-08-10', 'F', 2025),
  (100024, 'S024', 'Xavier', 'Xavier', 'Xander', '2014-05-25', 'M', 2025),
  (100025, 'S025', 'Yara', 'Young', 'Yasmine', '2014-09-05', 'F', 2025);

-- Grade 5 students (5 students)
INSERT INTO students (STUDENT_ID, STUDENT_NUMBER, FIRST_NAME, LAST_NAME, MIDDLE_NAME, DOB, SEX, SYEAR)
VALUES
  (100026, 'S026', 'Zachary', 'Zimmerman', 'Zane', '2013-06-15', 'M', 2025),
  (100027, 'S027', 'Ava', 'Adams', 'Aurora', '2013-07-20', 'F', 2025),
  (100028, 'S028', 'Benjamin', 'Baker', 'Blake', '2013-08-10', 'M', 2025),
  (100029, 'S029', 'Chloe', 'Campbell', 'Charlotte', '2013-05-25', 'F', 2025),
  (100030, 'S030', 'Daniel', 'Cooper', 'Derek', '2013-09-05', 'M', 2025);

-- Grade 6 students (5 students)
INSERT INTO students (STUDENT_ID, STUDENT_NUMBER, FIRST_NAME, LAST_NAME, MIDDLE_NAME, DOB, SEX, SYEAR)
VALUES
  (100031, 'S031', 'Ella', 'Edwards', 'Eleanor', '2012-06-15', 'F', 2025),
  (100032, 'S032', 'Ethan', 'Evans', 'Evan', '2012-07-20', 'M', 2025),
  (100033, 'S033', 'Fiona', 'Fisher', 'Faith', '2012-08-10', 'F', 2025),
  (100034, 'S034', 'Gabriel', 'Garcia', 'Grayson', '2012-05-25', 'M', 2025),
  (100035, 'S035', 'Hannah', 'Harris', 'Harriet', '2012-09-05', 'F', 2025);

-- Grade 7 students (5 students)
INSERT INTO students (STUDENT_ID, STUDENT_NUMBER, FIRST_NAME, LAST_NAME, MIDDLE_NAME, DOB, SEX, SYEAR)
VALUES
  (100036, 'S036', 'Isaac', 'Ingram', 'Ian', '2011-06-15', 'M', 2025),
  (100037, 'S037', 'Julia', 'Jackson', 'Juliet', '2011-07-20', 'F', 2025),
  (100038, 'S038', 'Kevin', 'Kelly', 'Keith', '2011-08-10', 'M', 2025),
  (100039, 'S039', 'Lisa', 'Lewis', 'Lillian', '2011-05-25', 'F', 2025),
  (100040, 'S040', 'Michael', 'Miller', 'Matthew', '2011-09-05', 'M', 2025);

-- Grade 8 students (5 students)
INSERT INTO students (STUDENT_ID, STUDENT_NUMBER, FIRST_NAME, LAST_NAME, MIDDLE_NAME, DOB, SEX, SYEAR)
VALUES
  (100041, 'S041', 'Natalie', 'Nelson', 'Naomi', '2010-06-15', 'F', 2025),
  (100042, 'S042', 'Oliver', 'Ortiz', 'Owen', '2010-07-20', 'M', 2025),
  (100043, 'S043', 'Penelope', 'Parker', 'Patricia', '2010-08-10', 'F', 2025),
  (100044, 'S044', 'Quentin', 'Quinn', 'Quinn', '2010-05-25', 'M', 2025),
  (100045, 'S045', 'Rose', 'Roberts', 'Rebecca', '2010-09-05', 'F', 2025);

-- Note: You may need to adjust IDs and school assignments based on your OpenSIS instance
-- Use OpenSIS UI to enroll students in schools after data loads
```

#### Load Data

```bash
# Load sample data into database
mysql -u opensis -p opensis_classic < sample_data_phase1.sql

# Verify
mysql -u opensis -p opensis_classic
> SELECT COUNT(*) FROM students;
# Should show: 45 (students)
```

#### Verify in OpenSIS UI

- Navigate to Students module
- Search for a student (e.g., "Alice Anderson")
- Verify student appears in list with correct info

**Deliverable**: `sample_data_phase1.sql` with 45 TK–8 students

**Test Cases to Pass**:
- [ ] SQL script loads without errors
- [ ] Students visible in OpenSIS Students module
- [ ] Student search works
- [ ] Student detail page displays correctly
- [ ] 45 students created (5 per grade, K–8)

---

### Task 1.3: Document Database Schema

**Objective**: Create baseline schema documentation  
**Estimated Time**: 2–3 hours

#### Generate Schema Documentation

1. **Export Schema**
   ```bash
   # Generate schema dump (structure only, no data)
   mysqldump -u opensis -p opensis_classic --no-data > schema_baseline.sql
   ```

2. **Create Schema Documentation** (file: `docs/SCHEMA_BASELINE.md`)
   ```markdown
   # OpenSIS Classic - Baseline Schema Documentation

   ## Overview
   This document describes the OpenSIS Classic Community Edition database schema
   as of Phase 1 baseline (2026-05-08).

   ## Core Tables

   ### students
   - **Purpose**: Student master record
   - **Key Columns**:
     - STUDENT_ID: Primary key
     - STUDENT_NUMBER: District-assigned ID
     - FIRST_NAME, LAST_NAME, MIDDLE_NAME: Student name
     - DOB: Date of birth
     - SEX: Gender (M/F)
     - SYEAR: School year

   ### staff
   - **Purpose**: Staff master record
   - **Key Columns**:
     - STAFF_ID: Primary key
     - STAFF_NUMBER: District ID
     - FIRST_NAME, LAST_NAME: Staff name
     - EMAIL: Contact email

   ### schools
   - **Purpose**: School master record
   - **Key Columns**:
     - SCHOOL_ID: Primary key
     - SCHOOL_NAME: School name
     - SCHOOL_NUMBER: District ID

   ### student_enrollment (if exists)
   - **Purpose**: Student enrollment records
   - **Key Columns**:
     - STUDENT_ID: Foreign key to students
     - SCHOOL_ID: Foreign key to schools
     - GRADE_LEVEL: Grade (K, 1-8, etc.)
     - ENROLLMENT_DATE: When student enrolled

   ### courses
   - **Purpose**: Course catalog
   - **Key Columns**:
     - COURSE_ID: Primary key
     - COURSE_NAME: Course name
     - COURSE_NUMBER: District code

   ### course_periods (or similar)
   - **Purpose**: Course sections/periods
   - **Key Columns**:
     - COURSE_PERIOD_ID: Primary key
     - COURSE_ID: Foreign key to courses
     - PERIOD_ID: Marking period

   ### marking_periods
   - **Purpose**: School year marking periods
   - **Key Columns**:
     - MARKING_PERIOD_ID: Primary key
     - MARKING_PERIOD_NAME: Name (Q1, Semester 1, etc.)
     - START_DATE, END_DATE: Period dates

   ### attendance
   - **Purpose**: Daily attendance records
   - **Key Columns**:
     - ATTENDANCE_ID: Primary key
     - STUDENT_ID: Foreign key to students
     - ATTENDANCE_DATE: Date of attendance
     - ATTENDANCE_CODE: Type (P=Present, A=Absent, etc.)

   ## Relationships
   
   ```
   students (1) ──────→ (M) student_enrollment
       ↓
   [Optional: enrollment to schools]
   
   courses (1) ──────→ (M) course_periods
   
   students (1) ──────→ (M) attendance
   ```

   ## Key Observations
   - No effective-dated history tables (will add in Phase 2)
   - No California-specific code tables (will add in Phase 3)
   - Minimal demographic fields (will extend in Phase 4)

   ## Total Tables: [COUNT]
   (Run: SELECT COUNT(*) FROM information_schema.TABLES WHERE TABLE_SCHEMA='opensis_classic';)
   ```

3. **Create ER Diagram** (use tool like Lucidchart, draw.io, or ASCII)
   - Save as `docs/ER_DIAGRAM_PHASE1.md` or `.png`

**Deliverable**: `docs/SCHEMA_BASELINE.md` with table descriptions

**Test Cases to Pass**:
- [ ] Schema document created and readable
- [ ] All major tables documented (students, staff, schools, courses, etc.)
- [ ] Key columns identified
- [ ] Relationships mapped
- [ ] ER diagram created

---

### Task 1.4: Document Workflow Baseline

**Objective**: Document how OpenSIS handles core operations  
**Estimated Time**: 2–3 hours

#### Test and Document Workflows

Create file: `docs/WORKFLOW_BASELINE.md`

**Workflows to Document**:

1. **Student Enrollment**
   - UI Path: How to enroll a student
   - Database Changes: What tables are updated
   - Limitations: What OpenSIS can't do yet

2. **Attendance Entry**
   - UI Path: How to record attendance
   - Database Changes: Attendance table updates
   - Current Codes: What attendance codes exist

3. **Grade Entry**
   - UI Path: How teacher enters grades
   - Database Changes: What grade table exists
   - Current Features: Gradebook capabilities

4. **Report Generation**
   - Report types available
   - Export formats (PDF, CSV, Excel)

#### Template for docs/WORKFLOW_BASELINE.md

```markdown
# OpenSIS Classic - Baseline Workflows

## 1. Student Enrollment Workflow

### UI Path
1. Navigate to Students module
2. Click "Add Student" (if new) or select existing student
3. Fill in student information
4. Assign to school and grade level
5. Set enrollment date
6. Click Save

### Database Updates
- `students` table updated/created
- Enrollment recorded in [enrollment table name]

### Gaps for California
- [ ] No entry reason code
- [ ] No exit reason code
- [ ] No enrollment history tracking
- [ ] No effective dating

## 2. Attendance Workflow

### Current Codes
- P: Present
- [Other codes...]

### Gaps for California
- [ ] No CA-specific absence categories
- [ ] No suspension-related absence
- [ ] No daily aggregation

## [Continue for other workflows...]
```

**Deliverable**: `docs/WORKFLOW_BASELINE.md` with all core workflows

**Test Cases to Pass**:
- [ ] All major workflows documented
- [ ] UI paths clear and accurate
- [ ] Database changes identified
- [ ] Gaps listed for later phases

---

### Task 1.5: Create Test Cases Document

**Objective**: Record baseline test cases for regression testing  
**Estimated Time**: 1–2 hours

#### Create docs/PHASE_1_TEST_CASES.md

```markdown
# Phase 1 Test Cases - Baseline

## Installation Tests
- [x] Installation completes without errors
- [x] Database created with all tables
- [x] Admin user created
- [x] Login works with admin credentials
- [x] install/ directory deleted

## Basic Module Tests
- [x] Students module loads
- [x] Staff module loads
- [x] Schools module loads
- [x] Scheduling module loads
- [x] Grades module loads
- [x] Attendance module loads

## Data Load Tests
- [x] 45 sample students created
- [x] Students appear in Students module
- [x] Student search works
- [x] Student detail page loads

## Regression Test Results
| Test Case | Date | Result | Notes |
|-----------|------|--------|-------|
| Install | 2026-05-08 | PASS | |
| Login | 2026-05-08 | PASS | |
| Student List | 2026-05-08 | PASS | 45 students visible |
| [Continue...] | | | |
```

**Deliverable**: `docs/PHASE_1_TEST_CASES.md` with test results

---

## Phase 1 Deliverables Checklist

Before committing, ensure you have:

- [ ] `Data.php` with correct database credentials (local)
- [ ] Working OpenSIS installation on localhost:8000
- [ ] `sample_data_phase1.sql` with 45 students loaded
- [ ] `docs/SCHEMA_BASELINE.md` documenting baseline tables
- [ ] `docs/ER_DIAGRAM_PHASE1.md` or `.png` with ER diagram
- [ ] `docs/WORKFLOW_BASELINE.md` with core workflows
- [ ] `docs/PHASE_1_TEST_CASES.md` with test results
- [ ] All files committed to git feature branch

---

## Git Workflow for Phase 1

### Commit Strategy

Make commits as you complete each task:

```bash
# Task 1.1 complete
git add .
git commit -m "[Phase 1] Install: Complete OpenSIS installation

Installed OpenSIS Classic on localhost with:
- MySQL 8.0 database (opensis_classic)
- PHP 8.0 development server
- Admin user created
- Install directory removed

Test: Installation wizard completed, admin login successful"

# Task 1.2 complete
git add .
git commit -m "[Phase 1] Sample Data: Load 45 TK-8 students

Created sample_data_phase1.sql with test data:
- 45 students (5 per grade, K-8)
- Realistic names and birthdates
- Ready for Phase 2+ testing

Test: All students loaded, searchable in UI"

# Task 1.3 complete
git add .
git commit -m "[Phase 1] Schema: Document baseline database

Created docs/SCHEMA_BASELINE.md documenting:
- Core tables (students, staff, schools, courses, etc.)
- Key columns and relationships
- ER diagram

Test: All tables documented, diagram created"

# Task 1.4 complete
git add .
git commit -m "[Phase 1] Workflows: Document core operations

Created docs/WORKFLOW_BASELINE.md for:
- Student enrollment workflow
- Attendance entry workflow
- Grade entry workflow
- Report generation

Identified gaps for California extension"

# Task 1.5 complete
git add .
git commit -m "[Phase 1] Tests: Record baseline test cases

Created docs/PHASE_1_TEST_CASES.md with:
- Installation tests (all passing)
- Module tests (all passing)
- Data load tests (all passing)

Ready for regression testing in Phase 2+"
```

### Create Pull Request

```bash
# Push to remote
git push -u origin feature/phase-1-baseline

# Create PR on GitHub with description:
# Title: [Phase 1] Install & Baseline - Setup OpenSIS and Document Schema
# 
# Description:
# ## Phase 1: Install & Baseline
#
# ### Completed Tasks
# - [x] OpenSIS Classic installed locally
# - [x] Sample TK-8 data loaded (45 students)
# - [x] Baseline schema documented
# - [x] Core workflows documented
# - [x] Test cases recorded
#
# ### Deliverables
# - Working OpenSIS installation on localhost:8000
# - sample_data_phase1.sql (45 students)
# - docs/SCHEMA_BASELINE.md
# - docs/WORKFLOW_BASELINE.md
# - docs/PHASE_1_TEST_CASES.md
# - ER diagram
#
# ### Test Results
# - [x] Installation successful
# - [x] Admin login works
# - [x] All modules accessible
# - [x] Students module works with sample data
#
# Related: PROJECT_PLAN.md Phase 1
```

---

## Phase 1 Success Criteria

Confirm all are met before closing Phase 1:

- [ ] OpenSIS running on localhost:8000
- [ ] Admin login successful
- [ ] 45 sample TK–8 students loaded
- [ ] All students visible and searchable in UI
- [ ] Schema documented (SCHEMA_BASELINE.md)
- [ ] ER diagram created
- [ ] Core workflows documented (WORKFLOW_BASELINE.md)
- [ ] Test cases documented and passing (PHASE_1_TEST_CASES.md)
- [ ] All code committed to `feature/phase-1-baseline` branch
- [ ] PR created and approved
- [ ] Merged to `ca-extension` branch
- [ ] Tag created: `v0.1.0-phase-1`

---

## Estimated Timeline

- **Day 1**: Install OpenSIS + Load Sample Data (Tasks 1.1–1.2)
- **Day 2**: Document Schema + Workflows (Tasks 1.3–1.4)
- **Day 3**: Test Cases + PR + Review (Task 1.5 + merge)

**Total**: 3 working days (or 1 week part-time)

---

## Troubleshooting Phase 1

### Issue: MySQL connection fails during install

**Solution**:
1. Verify MySQL running: `mysql -u root -p`
2. Verify database exists: `SHOW DATABASES;`
3. Verify user created: `SELECT User, Host FROM mysql.user WHERE User='opensis';`
4. Check Data.php credentials

### Issue: PHP errors on homepage

**Solution**:
1. Check error log: Look in browser console
2. Verify Warehouse.php loads: Check if functions auto-loaded
3. Enable debug: `error_reporting(E_ALL);` in index.php
4. Check file permissions: `chmod 755` on directories

### Issue: Sample data doesn't load

**Solution**:
1. Verify SQL syntax: `mysql -u opensis -p < sample_data_phase1.sql`
2. Check for FK errors: May need to adjust ID values
3. Verify student_enrollment table exists
4. Try loading part of data to find problem row

---

## Next Phase

After Phase 1 completes:

1. Merge PR to `ca-extension`
2. Create tag: `v0.1.0-phase-1`
3. **Start Phase 2**: Data Model Design
   - Design California extension tables
   - Create migration scripts
   - See PROJECT_PLAN.md Phase 2

---

**Last Updated**: 2026-05-08  
**Status**: Ready for Phase 1 Kickoff  
**Next**: Begin Task 1.1
