# 📦 Git Push Guide - What to Include/Exclude

## ✅ **INCLUDE (Commit These)**

### Source Code
- ✅ All `.py` files (Python source code)
- ✅ All `.js`, `.jsx` files (React/JavaScript source)
- ✅ All `.sql` files (database queries)
- ✅ All `.json` files **EXCEPT** credentials (schemas, configs, package.json)
- ✅ All `.md` files (documentation)
- ✅ All `.yml`, `.yaml` files **EXCEPT** secrets.yaml
- ✅ All `.sh` files (shell scripts)
- ✅ All `.txt` files (requirements, apt.txt, etc.)

### Configuration Files (Non-sensitive)
- ✅ `package.json` and `package-lock.json` (dependency definitions)
- ✅ `requirements.txt` (Python dependencies)
- ✅ `Dockerfile` files
- ✅ `docker-compose.yml`
- ✅ `render.yaml`
- ✅ Kubernetes configs (except secrets.yaml)
- ✅ `.github/workflows/` (CI/CD workflows)

### Documentation
- ✅ All `.md` files
- ✅ `README.md`
- ✅ `LICENSE`

### Project Structure
- ✅ `src/` directories (source code)
- ✅ `public/` directories (public assets)
- ✅ `schemas/` (data schemas)
- ✅ `scripts/` (utility scripts)

---

## ❌ **EXCLUDE (Never Commit These)**

### 🔐 **CRITICAL: Secrets & Credentials**
- ❌ `config/gcp-credentials.json` - **CONTAINS SENSITIVE GCP CREDENTIALS**
- ❌ `secrets.yaml` and `secrets.yaml.backup` - **CONTAINS API KEYS**
- ❌ Any `.env` files
- ❌ Any files with `credentials`, `secret`, `key`, `.pem` in name

### 📦 **Dependencies (Regenerated on Install)**
- ❌ `node_modules/` - Install with `npm install`
- ❌ `venv/`, `env/`, `.venv/` - Python virtual environments
- ❌ `__pycache__/` - Python bytecode cache

### 🏗️ **Build Artifacts (Generated)**
- ❌ `build/` directories (React build output)
- ❌ `dist/` directories
- ❌ `*.pyc`, `*.pyo` files

### 💻 **IDE & Editor Files**
- ❌ `.vscode/`, `.idea/`
- ❌ `*.sublime-*`
- ❌ `*.swp`, `*.swo`

### 🖥️ **OS Files**
- ❌ `.DS_Store` (macOS)
- ❌ `Thumbs.db` (Windows)
- ❌ `*.lnk` (Windows shortcuts)

### 📝 **Temporary & Log Files**
- ❌ `*.log` files
- ❌ `*.tmp`, `*.temp`
- ❌ `*.bak`, `*.backup`

### 🎬 **Large Media Files (Optional)**
- ⚠️ `logistic.mov` - Large video file (consider excluding if >100MB)
- ⚠️ `logistic.png` - Image file (usually OK to include)

---

## 🚀 **Quick Start: Push to GitHub**

### Step 1: Enable Long Path Support (Windows)
```bash
cd Logistics-Network-Real-Time-Intelligent-Dispatch-System
git config core.longpaths true
```

### Step 2: Remove Already-Tracked Files (if needed)
If you've already committed files that should be ignored:

```bash
# Remove credentials from Git (but keep local file)
git rm --cached config/gcp-credentials.json

# Remove node_modules if tracked
git rm -r --cached applications/dashboard/node_modules/

# Remove build artifacts
git rm -r --cached applications/dashboard/build/

# Remove Python cache
find . -type d -name __pycache__ -exec git rm -r --cached {} \;
```

### Step 3: Verify .gitignore is Working
```bash
# Check what Git will track
git status

# Should NOT see:
# - node_modules/
# - config/gcp-credentials.json
# - build/
# - __pycache__/
```

### Step 4: Add and Commit
```bash
# Add all files (respecting .gitignore)
git add .

# Review what will be committed
git status

# Commit
git commit -m "Initial commit: Enterprise logistics dispatch system"

# Push to GitHub
git push origin main
```

---

## ⚠️ **Security Checklist Before Pushing**

Before pushing to GitHub, verify:

- [ ] No `.env` files are tracked
- [ ] No `*credentials*.json` files are tracked
- [ ] No `secrets.yaml` files are tracked
- [ ] No API keys in source code (check for hardcoded keys)
- [ ] No database passwords in config files
- [ ] `node_modules/` is excluded
- [ ] Build artifacts are excluded

### Check for Secrets in Code
```bash
# Search for potential secrets
grep -r "api_key\|API_KEY\|password\|PASSWORD\|secret\|SECRET" --include="*.py" --include="*.js" --include="*.json" .
```

---

## 📊 **What Gets Pushed: Summary**

| Category | Include? | Reason |
|----------|----------|--------|
| Source Code (.py, .js, .jsx) | ✅ Yes | Core application code |
| Dependencies (node_modules, venv) | ❌ No | Can be regenerated |
| Build Outputs (build/, dist/) | ❌ No | Generated artifacts |
| Credentials & Secrets | ❌ **NEVER** | Security risk |
| Config Files (non-sensitive) | ✅ Yes | Needed for deployment |
| Documentation (.md) | ✅ Yes | Project documentation |
| Docker Files | ✅ Yes | Containerization configs |
| CI/CD Workflows | ✅ Yes | Automation scripts |

---

## 🔧 **Troubleshooting**

### "Filename too long" Error
```bash
# Enable long paths in Git
git config core.longpaths true

# If still issues, enable in Windows (requires admin):
# Run PowerShell as admin:
New-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\FileSystem" -Name "LongPathsEnabled" -Value 1 -PropertyType DWORD -Force
```

### Already Committed Sensitive Files?
```bash
# Remove from Git history (use BFG Repo-Cleaner or git filter-branch)
# Or create new repo and push clean version
```

---

## 📝 **Notes**

- The `.gitignore` file is already configured for this project
- Always review `git status` before committing
- Use GitHub Secrets for sensitive values in CI/CD
- Consider using environment variables instead of config files for secrets

