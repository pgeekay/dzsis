# LOCAL_SETUP_GUIDE.md

## Local Development Environment Setup

**For**: Developers working on OpenSIS Classic California TK–8 Extension  
**Database**: MySQL 8.0 (or MariaDB 10.4.x) on Local Machine  
**Web Server**: Apache 2.4+ or PHP Built-in Server  
**PHP**: 8.x

---

## Prerequisites

Before starting, ensure you have:

- [ ] Git installed and configured
- [ ] PHP 8.x installed (`php --version` should show 8.x)
- [ ] MySQL 8.0 or MariaDB 10.4.x installed
- [ ] Apache 2.4+ (optional if using PHP built-in server)
- [ ] Composer (optional, for package management)
- [ ] Text editor/IDE (VS Code, PhpStorm, etc.)
- [ ] Terminal/command line access

---

## Step 1: Clone Project Repository

### Get the Project-Specific Branch

```bash
# Clone the repository
git clone https://github.com/OS4ED/openSIS-Classic.git opensis-dzsis
cd opensis-dzsis

# Check available branches
git branch -a

# Checkout the project branch (replace "ca-extension" with your branch name)
git checkout ca-extension

# Verify you're on the correct branch
git branch --show-current
```

**Expected Output:**
```
ca-extension
```

---

## Step 2: Set Up Local MySQL Database

### Option A: MySQL Command Line

```bash
# Start MySQL service (if not already running)
# macOS:
brew services start mysql

# Linux (Ubuntu/Debian):
sudo systemctl start mysql

# Windows:
# Start MySQL from Services or use MySQL Shell
```

### Option B: Use Database GUI

- **MySQL Workbench**: Free GUI tool for MySQL management
- **TablePlus**: Fast database client (macOS/Windows/Linux)
- **DBeaver**: Open-source database tool

### Create OpenSIS Database

```sql
-- Login to MySQL
mysql -u root -p

-- Create database
CREATE DATABASE opensis_classic CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

-- Create database user
CREATE USER 'opensis'@'localhost' IDENTIFIED BY 'openSIS_DevPass123!';

-- Grant privileges
GRANT ALL PRIVILEGES ON opensis_classic.* TO 'opensis'@'localhost';

-- Flush privileges
FLUSH PRIVILEGES;

-- Verify
SHOW DATABASES;
```

**Expected Output:**
```
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| opensis_classic    |
| performance_schema |
| sys                |
+--------------------+
```

---

## Step 3: Set Up OpenSIS Web Directory

### Configure Web Root

**For Apache:**

```bash
# macOS (using Homebrew Apache)
# Link project to Apache web root
sudo ln -s ~/path/to/opensis-dzsis /Library/WebServer/Documents/opensis

# Linux (Ubuntu/Debian)
sudo ln -s ~/path/to/opensis-dzsis /var/www/html/opensis

# Create/modify Apache vhost (optional)
# Edit /etc/apache2/sites-available/opensis.conf
<VirtualHost *:80>
    ServerName opensis.local
    DocumentRoot /path/to/opensis-dzsis
    <Directory /path/to/opensis-dzsis>
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>

# Enable site
sudo a2ensite opensis

# Restart Apache
sudo systemctl restart apache2
```

**For PHP Built-in Server (Simpler):**

```bash
# Navigate to project root
cd ~/path/to/opensis-dzsis

# Start PHP server (runs on http://localhost:8000)
php -S localhost:8000

# If port 8000 is in use, try another:
php -S localhost:8888
```

### Add hosts entry (Optional, for vhost):

```bash
# Edit /etc/hosts (macOS/Linux) or C:\Windows\System32\drivers\etc\hosts (Windows)
127.0.0.1 opensis.local
```

---

## Step 4: OpenSIS Installation

### Run Web Installer

1. **Open browser** and navigate to:
   - PHP Built-in: `http://localhost:8000`
   - Apache: `http://opensis.local` or `http://localhost/opensis`

2. **Follow OpenSIS Installer**:
   - Select language
   - Accept license
   - Enter database credentials:
     - **Server**: `localhost`
     - **Username**: `opensis`
     - **Password**: `openSIS_DevPass123!`
     - **Database Name**: `opensis_classic`
     - **Port**: `3306`
   - Create admin user
   - Create school and marking period
   - Complete installation

3. **Delete install directory** (as instructed by installer):
   ```bash
   rm -rf /path/to/opensis-dzsis/install/
   ```

4. **Verify Login**:
   - Admin username/password created during setup
   - Dashboard should load without errors

---

## Step 5: Create Data.php Configuration

The installer creates `Data.php`, but verify it exists and contains:

**File location**: `/path/to/opensis-dzsis/Data.php`

**Expected contents**:
```php
<?php
$DatabaseServer = "localhost";
$DatabaseUsername = "opensis";
$DatabasePassword = "openSIS_DevPass123!";
$DatabaseName = "opensis_classic";
$DatabasePort = "3306";
$DatabaseType = "mysqli";
?>
```

**Important**: 
- `Data.php` is created during installation
- It contains sensitive credentials, so it's in `.gitignore` (not version controlled)
- Each developer has their own `Data.php` with local credentials

---

## Step 6: Verify Installation

### Check Core Functionality

1. **Login**:
   - Use admin credentials created during setup
   - Dashboard should load

2. **Test Core Modules**:
   - Navigate to Students module → Add test student
   - Navigate to Staff module → View staff
   - Navigate to Scheduling → Create test course
   - Navigate to Grades → View gradebook
   - Navigate to Attendance → Record attendance

3. **Check Database**:
   ```bash
   mysql -u opensis -p opensis_classic

   # Show tables
   SHOW TABLES;

   # Should see tables like: users, students, staff, courses, etc.
   ```

### Check PHP Configuration

```bash
# Verify PHP version
php --version
# Should show PHP 8.x

# Check PHP CLI modules
php -m | grep -i mysql
# Should show: mysqli

# Check web PHP modules
# Create test file: test.php
echo '<?php phpinfo(); ?>' > /path/to/opensis-dzsis/test.php
# Open http://localhost:8000/test.php
# Look for "mysqli" section
# Then delete: rm test.php
```

---

## Step 7: Set Up Local Git Workflow

### Configure Git Locally

```bash
cd ~/path/to/opensis-dzsis

# Verify remote
git remote -v
# Should show origin pointing to GitHub repo

# Create local tracking branch from remote
git fetch origin
git checkout --track origin/ca-extension

# Verify branch
git branch --show-current
# Should output: ca-extension
```

### Create Feature Branches for Phases

```bash
# Before starting Phase 1, create feature branch
git checkout -b feature/phase-1-baseline

# Your changes go here...

# When done, push to remote
git push -u origin feature/phase-1-baseline

# Create pull request on GitHub to merge into ca-extension
```

---

## Step 8: Set Up Sample TK–8 Data (Phase 1)

### Create Test School

**Via UI:**
1. Login as admin
2. Navigate to School Setup → Schools
3. Create school: "Lincoln Elementary" (TK–Grade 3)
4. Create school: "Washington Middle" (Grade 4–8)
5. Set marking periods

**Via SQL (Faster for Phase 1):**
```bash
# Create SQL file with sample data
cat > sample_data_phase1.sql << 'EOF'
-- Sample schools
INSERT INTO schools (SCHOOL_NAME, SCHOOL_NUMBER, DISTRICT_ID, SYEAR) 
VALUES ('Lincoln Elementary', '1001', 1, 2025);

INSERT INTO schools (SCHOOL_NAME, SCHOOL_NUMBER, DISTRICT_ID, SYEAR) 
VALUES ('Washington Middle', '1002', 1, 2025);

-- Sample students (you'll create more in Phase 1)
-- Add more as needed...
EOF

# Load sample data
mysql -u opensis -p opensis_classic < sample_data_phase1.sql
```

---

## Step 9: Database Backup Strategy

### Manual Backups

```bash
# Backup database
mysqldump -u opensis -p opensis_classic > ~/backups/opensis_backup_$(date +%Y%m%d_%H%M%S).sql

# Restore from backup
mysql -u opensis -p opensis_classic < ~/backups/opensis_backup_YYYYMMDD_HHMMSS.sql
```

### Automated Backups (Optional)

**Create backup script** (`backup_opensis.sh`):
```bash
#!/bin/bash
BACKUP_DIR="$HOME/backups/opensis"
mkdir -p "$BACKUP_DIR"

mysqldump -u opensis -p opensis_classic > "$BACKUP_DIR/opensis_$(date +\%Y\%m\%d_\%H\%M\%S).sql"

# Keep only last 7 backups
find "$BACKUP_DIR" -name "opensis_*.sql" -mtime +7 -delete
```

**Schedule with cron** (macOS/Linux):
```bash
# Edit crontab
crontab -e

# Add line to backup daily at 2 AM
0 2 * * * /path/to/backup_opensis.sh
```

---

## Step 10: Verify All Prerequisites Met

### Pre-Development Checklist

- [ ] Git repository cloned, on `ca-extension` branch
- [ ] PHP 8.x installed and verified
- [ ] MySQL 8.0 running with `opensis_classic` database
- [ ] `Data.php` created with local credentials
- [ ] OpenSIS installer completed successfully
- [ ] Admin login works, dashboard loads
- [ ] Core modules accessible (Students, Staff, Scheduling, etc.)
- [ ] Database tables visible in MySQL
- [ ] Sample TK–8 data loaded (2 schools at minimum)
- [ ] Git feature branch created for Phase 1

---

## Directory Structure

After setup, your local project should look like:

```
~/opensis-dzsis/
├── install/                 (remove after setup)
├── modules/                 (feature modules)
├── functions/               (utility functions, auto-loaded)
├── assets/                  (CSS, JS, images)
├── api/                      (API endpoints)
├── libraries/               (external libraries)
├── docs/                    (documentation)
├── index.php                (entry point)
├── Warehouse.php            (bootstrapper)
├── ConfigInc.php            (configuration)
├── DatabaseInc.php          (database layer)
├── Data.php                 (credentials - .gitignore'd)
├── CLAUDE.md                (developer guide)
├── Context.MD               (requirements)
├── PROJECT_PLAN.md          (phase plan)
├── LOCAL_SETUP_GUIDE.md     (this file)
├── GIT_WORKFLOW.md          (git strategy)
└── .gitignore               (excludes Data.php, etc.)
```

---

## Troubleshooting

### Issue: "Could not connect to database"

**Solution:**
1. Verify MySQL is running: `mysql -u opensis -p`
2. Check `Data.php` credentials match MySQL setup
3. Verify database exists: `SHOW DATABASES;`
4. Restart MySQL and Apache

### Issue: "Permission denied" on web files

**Solution:**
```bash
# Fix permissions
chmod -R 755 /path/to/opensis-dzsis
chmod 644 /path/to/opensis-dzsis/*.php
```

### Issue: "PHP Fatal error: Class not found"

**Solution:**
1. Check `Warehouse.php` is running: Look for auto-loading in logs
2. Verify all `/functions/` files are included
3. Check for syntax errors: `php -l /path/to/file.php`

### Issue: "MySQL connection timeout"

**Solution:**
```bash
# Restart MySQL
sudo systemctl restart mysql

# Or increase timeout in Data.php:
# Add after connection: mysqli_options($connection, MYSQLI_OPT_CONNECT_TIMEOUT, 5);
```

### Issue: "installer/ still exists"

**Solution:**
```bash
# Delete installer as instructed
rm -rf /path/to/opensis-dzsis/install/

# If you need it again, restore from git
git checkout HEAD -- install/
```

---

## Phase 1 Setup Validation

Before starting Phase 1 development:

```bash
# 1. Verify branch
git branch --show-current
# Should output: ca-extension

# 2. Check database
mysql -u opensis -p opensis_classic
> SHOW TABLES;  # Verify tables exist

# 3. Verify web access
curl http://localhost:8000/index.php | head -20  # Check for HTML

# 4. Check logs for errors
tail -f /var/log/apache2/error.log  # Apache errors
# or
php -S localhost:8000 -t /path/to/opensis-dzsis  # Watch PHP server
```

---

## Next: Phase 1 Kickoff

Once all prerequisites are met:

1. Read `PROJECT_PLAN.md` Phase 1 section
2. Create feature branch: `git checkout -b feature/phase-1-baseline`
3. Document baseline schema
4. Create test data script
5. See `PHASE_1_KICKOFF.md` for detailed tasks

---

## Quick Reference Commands

```bash
# Start development
cd ~/opensis-dzsis
git checkout ca-extension
php -S localhost:8000

# MySQL access
mysql -u opensis -p opensis_classic

# Backup database
mysqldump -u opensis -p opensis_classic > backup.sql

# Git workflow
git checkout -b feature/phase-X-name
git add .
git commit -m "[Phase X] Component: Description"
git push -u origin feature/phase-X-name
# Create PR on GitHub

# View git log
git log --oneline --graph --all
```

---

## Support & Documentation

- **CLAUDE.md** - Architecture and development guidelines
- **PROJECT_PLAN.md** - Phase 1 tasks and test cases
- **Context.MD** - Requirements reference
- **GIT_WORKFLOW.md** - Detailed git branching strategy

---

**Last Updated**: 2026-05-08  
**Status**: Ready for Phase 1 Setup
