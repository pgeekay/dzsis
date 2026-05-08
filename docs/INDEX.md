# Documentation Index

**OpenSIS Classic - California TK–8 Extension Project**

All project documentation is organized in this `/docs` folder.

---

## 📖 Quick Navigation

### 🚀 **Start Here (If New)**
1. **[docs/setup/NEXT_STEPS.md](setup/NEXT_STEPS.md)** - Your action items for Phase 1 (READ FIRST!)
2. **[docs/setup/LOCAL_SETUP_GUIDE.md](setup/LOCAL_SETUP_GUIDE.md)** - Local development environment setup
3. **[docs/setup/GIT_WORKFLOW.md](setup/GIT_WORKFLOW.md)** - How to work with git in this project

### 📋 **Project Overview**
- **[docs/project/CLAUDE.md](project/CLAUDE.md)** - Architecture & development guide
- **[docs/project/Context.MD](project/Context.MD)** - California requirements & feasibility
- **[docs/project/PROJECT_PLAN.md](project/PROJECT_PLAN.md)** - 13-phase development plan
- **[docs/project/README_PROJECT_SETUP.md](project/README_PROJECT_SETUP.md)** - Documentation navigation
- **[docs/project/PROJECT_SETUP_SUMMARY.txt](project/PROJECT_SETUP_SUMMARY.txt)** - 1-page quick ref

### 🔨 **Phase Development**
- **[docs/phases/PHASE_1_KICKOFF.md](phases/PHASE_1_KICKOFF.md)** - Phase 1: Install & Baseline (detailed tasks)
- **[docs/phases/](phases/)** - Additional phase documentation (created as work progresses)

---

## 📂 Directory Structure

```
docs/
├── INDEX.md                          ← You are here
├── setup/                            ← Setup & development workflow
│   ├── NEXT_STEPS.md                 ← Start here!
│   ├── LOCAL_SETUP_GUIDE.md          ← Local environment setup
│   └── GIT_WORKFLOW.md               ← Git branching & workflow
├── project/                          ← Project-level documentation
│   ├── CLAUDE.md                     ← Architecture guide
│   ├── Context.MD                    ← Requirements document
│   ├── PROJECT_PLAN.md               ← 13-phase plan
│   ├── README_PROJECT_SETUP.md       ← Navigation guide
│   └── PROJECT_SETUP_SUMMARY.txt     ← Quick reference
└── phases/                           ← Phase-specific documentation
    ├── PHASE_1_KICKOFF.md            ← Phase 1 tasks
    ├── SCHEMA_BASELINE.md            ← (Created during Phase 1)
    ├── WORKFLOW_BASELINE.md          ← (Created during Phase 1)
    ├── PHASE_1_TEST_CASES.md         ← (Created during Phase 1)
    └── [More phase docs...]          ← Future phases
```

---

## 🎯 By Role

### **For Developers**
1. **New to this project?** → Read [setup/NEXT_STEPS.md](setup/NEXT_STEPS.md)
2. **Setting up locally?** → Read [setup/LOCAL_SETUP_GUIDE.md](setup/LOCAL_SETUP_GUIDE.md)
3. **Understanding git workflow?** → Read [setup/GIT_WORKFLOW.md](setup/GIT_WORKFLOW.md)
4. **Need architecture info?** → Read [project/CLAUDE.md](project/CLAUDE.md)
5. **Working on Phase 1?** → Read [phases/PHASE_1_KICKOFF.md](phases/PHASE_1_KICKOFF.md)

### **For Project Managers**
1. **Project overview?** → Read [project/PROJECT_SETUP_SUMMARY.txt](project/PROJECT_SETUP_SUMMARY.txt)
2. **Detailed requirements?** → Read [project/Context.MD](project/Context.MD)
3. **13-phase plan?** → Read [project/PROJECT_PLAN.md](project/PROJECT_PLAN.md)
4. **Phase 1 timeline?** → Read [phases/PHASE_1_KICKOFF.md](phases/PHASE_1_KICKOFF.md)

### **For QA/Testing**
1. **Test strategy?** → Read [project/PROJECT_PLAN.md](project/PROJECT_PLAN.md) (Test Cases section)
2. **Phase 1 tests?** → Read [phases/PHASE_1_KICKOFF.md](phases/PHASE_1_KICKOFF.md) (Test Cases)
3. **Regression testing?** → Read [project/PROJECT_PLAN.md](project/PROJECT_PLAN.md) (Regression Testing section)

### **For Documentation Team**
1. **What to document?** → Read [project/PROJECT_PLAN.md](project/PROJECT_PLAN.md) (Documentation Deliverables)
2. **Phase 1 docs?** → Create docs in [phases/](phases/) following PROJECT_PLAN.md

---

## 📖 Reading by Purpose

| Goal | Read This |
|------|-----------|
| Understand OpenSIS architecture | [project/CLAUDE.md](project/CLAUDE.md) |
| Understand California requirements | [project/Context.MD](project/Context.MD) |
| See 13-phase plan | [project/PROJECT_PLAN.md](project/PROJECT_PLAN.md) |
| Set up local environment | [setup/LOCAL_SETUP_GUIDE.md](setup/LOCAL_SETUP_GUIDE.md) |
| Learn git workflow | [setup/GIT_WORKFLOW.md](setup/GIT_WORKFLOW.md) |
| Get started on Phase 1 | [phases/PHASE_1_KICKOFF.md](phases/PHASE_1_KICKOFF.md) |
| Quick project overview | [project/PROJECT_SETUP_SUMMARY.txt](project/PROJECT_SETUP_SUMMARY.txt) |
| Next action items | [setup/NEXT_STEPS.md](setup/NEXT_STEPS.md) |

---

## 📅 Content Overview

### **docs/setup/** - Get Started (3 files)
- **NEXT_STEPS.md** - Your Phase 1 action items ⭐ START HERE
- **LOCAL_SETUP_GUIDE.md** - Step-by-step local MySQL + OpenSIS installation
- **GIT_WORKFLOW.md** - Git branching strategy, commit conventions, PR workflow

### **docs/project/** - Project Level (5 files)
- **CLAUDE.md** - OpenSIS architecture, modules, functions, development guidelines
- **Context.MD** - California TK-8 requirements, gap analysis, feasibility
- **PROJECT_PLAN.md** - 13 phases with tasks, tests, deliverables, timelines
- **README_PROJECT_SETUP.md** - How to navigate all documentation
- **PROJECT_SETUP_SUMMARY.txt** - 1-page executive summary

### **docs/phases/** - Phase Development (1+ files)
- **PHASE_1_KICKOFF.md** - Phase 1 detailed tasks, test cases, deliverables
- *More phase docs created during development*

---

## 🚀 First Steps

1. **Clone repository**:
   ```bash
   git clone https://github.com/OS4ED/openSIS-Classic.git opensis-dzsis
   cd opensis-dzsis
   git checkout ca-extension
   ```

2. **Read the docs** (in this order):
   - [setup/NEXT_STEPS.md](setup/NEXT_STEPS.md)
   - [setup/LOCAL_SETUP_GUIDE.md](setup/LOCAL_SETUP_GUIDE.md)
   - [setup/GIT_WORKFLOW.md](setup/GIT_WORKFLOW.md)

3. **Start Phase 1**:
   - [phases/PHASE_1_KICKOFF.md](phases/PHASE_1_KICKOFF.md)

---

## 📞 Getting Help

- **Installation help?** → See [setup/LOCAL_SETUP_GUIDE.md](setup/LOCAL_SETUP_GUIDE.md) Troubleshooting
- **Git help?** → See [setup/GIT_WORKFLOW.md](setup/GIT_WORKFLOW.md) Troubleshooting
- **Phase 1 help?** → See [phases/PHASE_1_KICKOFF.md](phases/PHASE_1_KICKOFF.md) Troubleshooting
- **Architecture help?** → See [project/CLAUDE.md](project/CLAUDE.md)
- **Requirements help?** → See [project/Context.MD](project/Context.MD)

---

## 📋 File Status

| File | Location | Status | Purpose |
|------|----------|--------|---------|
| CLAUDE.md | docs/project/ | ✅ Complete | Architecture guide |
| Context.MD | docs/project/ | ✅ Complete | Requirements doc |
| PROJECT_PLAN.md | docs/project/ | ✅ Complete | 13-phase plan |
| LOCAL_SETUP_GUIDE.md | docs/setup/ | ✅ Complete | Setup instructions |
| GIT_WORKFLOW.md | docs/setup/ | ✅ Complete | Git strategy |
| PHASE_1_KICKOFF.md | docs/phases/ | ✅ Complete | Phase 1 tasks |
| NEXT_STEPS.md | docs/setup/ | ✅ Complete | Action items |

---

## 🎯 Key Documents by Phase

**Phase 1**: docs/phases/PHASE_1_KICKOFF.md  
**Phase 2+**: Will be added in docs/phases/ as work progresses

---

## 📂 Repository Structure

```
opensis-dzsis/
├── docs/                           ← ALL DOCUMENTATION (you are here)
│   ├── INDEX.md                    ← This file
│   ├── setup/                      ← Setup & workflow guides
│   ├── project/                    ← Project overview docs
│   └── phases/                     ← Phase-specific documentation
├── modules/                        ← OpenSIS feature modules
├── functions/                      ← Auto-loaded utility functions
├── libraries/                      ← External libraries
├── assets/                         ← CSS, JS, images
├── api/                            ← API endpoints
├── install/                        ← Installation files
├── index.php                       ← Main entry point
├── Warehouse.php                   ← Bootstrap/setup
├── ConfigInc.php                   ← Configuration
├── DatabaseInc.php                 ← Database layer
├── Data.php                        ← Database credentials (local only)
├── .gitignore                      ← Ignore rules (Data.php, etc.)
├── README.md                       ← Original OpenSIS readme
└── [Other OpenSIS files...]
```

---

## 💡 Pro Tips

- **Bookmark this page** for quick navigation
- **Print or bookmark** [setup/NEXT_STEPS.md](setup/NEXT_STEPS.md) for quick reference
- **Keep git-related docs** ([setup/GIT_WORKFLOW.md](setup/GIT_WORKFLOW.md)) handy during development
- **Reference** [project/PROJECT_PLAN.md](project/PROJECT_PLAN.md) for phase timelines and success criteria

---

## 📝 Updating Documentation

As you work through phases:
1. Create new files in `docs/phases/` for each phase
2. Update relevant sections in project-level docs
3. Keep this INDEX.md updated with new files
4. Maintain clear, semantic file naming

---

**Last Updated**: 2026-05-08  
**Status**: Documentation organized and indexed  
**Next**: Read [setup/NEXT_STEPS.md](setup/NEXT_STEPS.md)
