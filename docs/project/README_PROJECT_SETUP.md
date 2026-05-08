# OpenSIS Classic - California TK–8 Extension Project

## Project Setup Complete ✓

This directory contains comprehensive project documentation for extending OpenSIS Classic Community Edition to support California TK–8 Student Information System requirements.

---

## What's Included

### 📋 Core Documentation Files

1. **CLAUDE.md** - Developer's Technical Guide
   - Architecture overview of OpenSIS Classic
   - Module system design
   - Database layer and function auto-loading
   - Development guidelines for working with the codebase
   - **Read this first** if you're a developer

2. **Context.MD** - Requirements & Feasibility Analysis
   - California TK–8 SIS requirements by domain (demographics, enrollment, attendance, programs, EL status, discipline)
   - OpenSIS feature mapping
   - Gap analysis
   - Recommended extension architecture (extension layer pattern)
   - Proposed database tables
   - Feasibility assessment
   - **Read this** to understand what needs to be built

3. **PROJECT_PLAN.md** - Operational Development Plan
   - 13 detailed phases with tasks, test cases, and deliverables
   - For each phase:
     - Development tasks
     - Test cases with inputs/outputs
     - Test data specifications
     - Documentation requirements
     - Regression testing checklist
     - Success criteria
   - Regression testing strategy across all phases
   - Test data management
   - Risk mitigation
   - **Use this** to plan sprints and track progress

4. **PROJECT_SETUP_SUMMARY.txt** - Quick Reference
   - 1-page overview of project structure
   - Phase timeline
   - Quick start guides for different roles
   - Key principles
   - Architecture summary
   - **Use this** for quick reference

---

## Quick Start by Role

### For Development Team
1. Read **CLAUDE.md** (understand architecture)
2. Read **Context.MD** (understand requirements)
3. Review **PROJECT_PLAN.md** Phase 1
4. Begin Phase 1: Install & Baseline
5. Create test data and documentation per phase

### For Project Managers
1. Review **Context.MD** (Section 11 - Sequential Action Plan)
2. Review **PROJECT_PLAN.md** (phases, durations, success criteria)
3. Schedule sprints based on phase timelines
4. Track deliverables per phase
5. Use success criteria to measure completion

### For QA/Testing
1. Review **PROJECT_PLAN.md** (Test Cases in each phase)
2. Review Regression Testing Checklist
3. Review Test Data Management section
4. Prepare test environments for each phase
5. Track test results per phase

### For Documentation Team
1. Review **PROJECT_PLAN.md** (Documentation Deliverables by Phase)
2. Use phase-specific documentation templates
3. Update documentation with each phase completion
4. Maintain /docs/ directory with all phase docs

---

## Phase Timeline

```
Phase 1:  Install & Baseline              [1-2 weeks]
Phase 2:  Data Model Design               [2-3 weeks]
Phase 3:  California Code Sets            [2 weeks]
Phase 4:  Student Demographics            [3 weeks]
Phase 5:  Enrollment History              [3 weeks]
Phase 6:  Program Participation           [3 weeks]
Phase 7:  English Learner Status          [2 weeks]
Phase 8:  Attendance Extension            [2 weeks]
Phase 9:  Discipline Module               [3 weeks]
Phase 10: Validation Layer                [4 weeks]
Phase 11: CALPADS Extraction              [4 weeks]
Phase 12: Data Export & Reporting         [2 weeks]
Phase 13: Testing & Pilot                 [4-6 weeks]

Total: ~35-41 weeks (8-10 months)
```

---

## Architecture Summary

### Design Principle: Extension Layer

All California enhancements are implemented as **extension tables** (prefixed with `ca_`), not modifications to OpenSIS core tables. This preserves the upgrade path and keeps the system maintainable.

```
OpenSIS Core Tables (unchanged)
         ↓
California Extension Tables (ca_* prefix)
         ↓
Validation Layer (rules + result storage)
         ↓
CALPADS Extract Generator (SENR, SINF, SPRG, SELA, STAS)
         ↓
CSV/API Export (external systems)
```

### California Extension Tables

**Student Domain:**
- `ca_student_profile` - Extended student demographics
- `ca_student_demographic_history` - Effective-dated demographic history
- `ca_student_language` - Primary language and acquisition status
- `ca_student_guardian_education` - Parent/guardian education level

**Enrollment Domain:**
- `ca_student_enrollment` - California-aligned enrollment records
- `ca_enrollment_status_history` - Status changes with effective dates
- `ca_enrollment_code_map` - Mapping of entry/exit codes
- `ca_school_year_calendar` - School years and terms

**Programs Domain:**
- `ca_program` - Program definitions (EL, SPED, FY, Homeless, etc.)
- `ca_student_program` - Student program enrollment
- `ca_student_program_history` - Program participation history

**Attendance Domain:**
- `ca_attendance_code_map` - Mapping to California attendance codes
- `ca_daily_attendance_summary` - Daily attendance aggregation

**Discipline Domain:**
- `ca_discipline_incident` - Discipline incident records
- `ca_discipline_student_incident` - Student-incident links
- `ca_discipline_action` - Actions taken (suspension, expulsion, etc.)

**Code Sets Framework:**
- `ca_code_set` - Code domain definitions
- `ca_code_value` - Valid values for each domain
- `ca_code_crosswalk` - Mappings from OpenSIS to California codes

**CALPADS Support:**
- `ca_calpads_extract_batch` - Extract batch tracking
- `ca_calpads_extract_file` - Generated file records
- `ca_calpads_validation_rule` - Validation rules catalog
- `ca_calpads_validation_result` - Validation results per student/batch

---

## Key Principles

1. **Extension Layer**: All CA enhancements are extension tables, not core modifications
2. **Incremental Development**: 13 phases, each with testable deliverables
3. **Data Separation**: OpenSIS core data separate from CA extension data
4. **Validation First**: Comprehensive validation before data save and before CALPADS extract generation
5. **Effective Dating**: All California records support effective-dated history

---

## Document Navigation

### By Purpose

| Need | Read | Location |
|------|------|----------|
| Understand architecture | CLAUDE.md | In repository |
| Understand requirements | Context.MD | In repository |
| Plan development | PROJECT_PLAN.md | In repository |
| Quick reference | PROJECT_SETUP_SUMMARY.txt | In repository |
| Phase-specific docs | /docs/*.md | Created per phase |

### By Audience

| Role | Start With | Then Read |
|------|-----------|-----------|
| Developer | CLAUDE.md | Context.MD, PROJECT_PLAN.md Phase 1 |
| Project Manager | Context.MD + PROJECT_SETUP_SUMMARY.txt | PROJECT_PLAN.md phases |
| QA/Tester | PROJECT_PLAN.md (Test Cases) | Phase-specific test docs |
| Documentation | PROJECT_PLAN.md (Doc Deliverables) | /docs/ and phase docs |

---

## Next Steps

### Immediate (This Week)

- [ ] All stakeholders review CLAUDE.md
- [ ] All stakeholders review PROJECT_SETUP_SUMMARY.txt
- [ ] Development team reviews Context.MD
- [ ] Project manager approves PROJECT_PLAN.md
- [ ] Schedule Phase 1 kickoff

### Phase 1 Preparation (Week 1)

- [ ] Set up development environment (PHP 8.x, MySQL 8.0, Apache 2.4+)
- [ ] Install OpenSIS Classic Community Edition
- [ ] Load sample TK–8 test data
- [ ] Document baseline schema
- [ ] Create SCHEMA_BASELINE.md and WORKFLOW_BASELINE.md

### Phase 1 Completion (End of Week 2)

- [ ] OpenSIS running on dev server
- [ ] Schema documented with ER diagram
- [ ] Sample data loaded
- [ ] Core workflows tested
- [ ] Phase 1 success criteria met

---

## Success Criteria

### By Phase

Each phase has specific success criteria in PROJECT_PLAN.md. Example:

**Phase 1 Success:**
- ✓ OpenSIS running on dev server
- ✓ Schema documented with ER diagram
- ✓ Sample TK–8 data loaded
- ✓ Core workflows tested and documented
- ✓ All baseline tests passing

**Phase 2 Success:**
- ✓ All California extension tables created
- ✓ Foreign keys properly configured
- ✓ Migration scripts work forward and backward
- ✓ Code sets populated with initial values
- ✓ All schema tests passing

### Overall Project Success

- ✓ OpenSIS extended without core modification
- ✓ All California TK–8 requirements addressed
- ✓ CALPADS extracts generate correctly
- ✓ Data validation prevents invalid data
- ✓ System validated with real pilot data
- ✓ Documentation complete
- ✓ Team trained and ready for production

---

## Regression Testing

After each phase, verify that:
- All prior phase tests still pass
- OpenSIS core functionality unchanged
- California extension tables functional
- No data loss or corruption
- Performance acceptable

See PROJECT_PLAN.md "Regression Testing Across All Phases" for complete checklist.

---

## Test Data Management

Each phase includes test data SQL scripts:
- `test_data_phase1.sql` - Baseline OpenSIS data
- `test_data_phase2.sql` - Schema infrastructure
- ... (one per phase)
- Real pilot data in Phase 13

Test data is version-controlled but cleared between phases to avoid conflicts.

---

## Version Control Strategy

### Branching

```
main (production-ready)
  ↓
develop (integration)
  ├─ feature/phase-1-baseline
  ├─ feature/phase-2-schema
  ├─ feature/phase-3-codes
  └─ ...
```

### Tagging

```
v0.1.0-phase-1 (baseline complete)
v0.2.0-phase-2 (schema complete)
...
v1.0.0 (pilot complete, production-ready)
```

### Commit Messages

```
[Phase X] Component: Brief description

Detailed explanation of changes.

Related: Context.MD Section X.Y
Closes: #issue-number (if applicable)
```

---

## Risk Management

### Known Risks & Mitigations

| Risk | Mitigation |
|------|-----------|
| OpenSIS upgrades overwrite modifications | Keep extension tables separate from core |
| Validation misses cases | Comprehensive test suite, pilot feedback |
| CALPADS spec misinterpretation | Validate with district and CDE |
| Performance issues at scale | Performance testing in Phase 13 |
| Pilot feedback requires rework | Early engagement, iterative improvements |

See PROJECT_PLAN.md "Risk & Mitigation" for details.

---

## Resources & References

### Documentation

- **CLAUDE.md** - Developer technical guide
- **Context.MD** - Requirements and feasibility
- **PROJECT_PLAN.md** - Detailed implementation plan
- **PROJECT_SETUP_SUMMARY.txt** - Quick reference

### External References

- [OpenSIS Classic GitHub](https://github.com/OS4ED/openSIS-Classic)
- California Department of Education (for CALPADS specifications)
- Sample CALPADS file specifications (obtained from CDE or pilot district)

### Tools & Technology

- **Web Server**: Apache 2.4+
- **Database**: MySQL 8.0 or MariaDB 10.4.x
- **Language**: PHP 8.x
- **Version Control**: Git
- **Testing**: PHPUnit

---

## Questions?

Refer to:
1. **Context.MD** Section 13 - "Questions & Next Steps"
2. **PROJECT_PLAN.md** Section on "Questions & Next Steps"
3. Project team and stakeholders

---

## Document Versions

| Version | Date | Status | Changes |
|---------|------|--------|---------|
| 1.0 | 2026-05-07 | Initial | Initial project setup complete |

---

## Summary

This project extends OpenSIS Classic with California TK–8 compliance in 13 testable phases, estimated at 8–10 months. The extension layer approach preserves the upgrade path while enabling comprehensive CALPADS support.

**Status**: Ready for Phase 1 Kickoff

**Next Step**: Review documents and schedule Phase 1 sprint planning.

---

**For detailed phase-by-phase tasks, tests, and deliverables, see PROJECT_PLAN.md**

**For technical architecture guidance, see CLAUDE.md**

**For requirements and feasibility, see Context.MD**

Generated: 2026-05-07
