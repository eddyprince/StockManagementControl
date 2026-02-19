# GitLab Setup Instructions

## Setting Up GitLab Repository

### Step 1: Create GitLab Repository

1. **Go to GitLab**
   - Visit: https://gitlab.com
   - Sign in to your account

2. **Create New Project**
   - Click "New project" or "+" button
   - Select "Create blank project"

3. **Configure Project**
   - **Project name**: `stock-management-control-system`
   - **Project slug**: `stock-management-control-system` (auto-generated)
   - **Visibility Level**: Choose Public or Private
   - **Initialize repository with a README**: ❌ **DO NOT CHECK** (we already have files)
   - Click "Create project"

### Step 2: Add GitLab Remote

After creating the repository, GitLab will show you the repository URL. Use it to add the remote:

```powershell
# Navigate to your project directory
cd "C:\Users\cyuba\Desktop\WebTech Project\StockManagementControl"

# Remove existing GitHub remote (if you want to switch)
git remote remove origin

# Add GitLab remote (replace YOUR_USERNAME with your GitLab username)
git remote add origin https://gitlab.com/YOUR_USERNAME/stock-management-control-system.git

# Verify remote
git remote -v
```

**Alternative: Add GitLab as additional remote**
```powershell
# Keep GitHub and add GitLab
git remote add gitlab https://gitlab.com/YOUR_USERNAME/stock-management-control-system.git

# Push to GitLab
git push -u gitlab main
```

### Step 3: Push to GitLab

```powershell
# Push all branches and tags
git push -u origin main

# Or if using separate remote name
git push -u gitlab main
```

### Step 4: Verify Push

1. Go to your GitLab project page
2. Verify all files are present
3. Check that documentation files are visible

---

## GitLab Repository URL Format

Your GitLab repository URL will be:
```
https://gitlab.com/YOUR_USERNAME/stock-management-control-system.git
```

**HTTPS (Recommended for beginners):**
```powershell
git remote add origin https://gitlab.com/YOUR_USERNAME/stock-management-control-system.git
```

**SSH (If you have SSH keys set up):**
```powershell
git remote add origin git@gitlab.com:YOUR_USERNAME/stock-management-control-system.git
```

---

## Quick Commands Summary

```powershell
# Check current remotes
git remote -v

# Add GitLab remote
git remote add gitlab https://gitlab.com/YOUR_USERNAME/stock-management-control-system.git

# Push to GitLab
git push -u gitlab main

# Future pushes
git push gitlab main
```

---

## Troubleshooting

### Issue: Authentication Required

**Solution:** GitLab may require authentication. Use one of these methods:

1. **Personal Access Token:**
   - Go to GitLab → Settings → Access Tokens
   - Create token with `write_repository` scope
   - Use token as password when pushing

2. **Git Credential Manager:**
   ```powershell
   git config --global credential.helper manager-core
   ```

### Issue: Repository Not Found

**Solution:** 
- Verify repository name is correct
- Check you have access to the repository
- Ensure repository exists on GitLab

### Issue: Push Rejected

**Solution:**
```powershell
# If remote has content, pull first
git pull gitlab main --allow-unrelated-histories

# Then push
git push -u gitlab main
```

---

## Next Steps After Setup

1. ✅ Repository created on GitLab
2. ✅ Remote added
3. ✅ Code pushed
4. 📝 Update README with GitLab badges (optional)
5. 📝 Set up CI/CD pipeline (optional)
6. 📝 Configure project settings on GitLab

---

**Need Help?** 
- GitLab Documentation: https://docs.gitlab.com/
- Git Basics: https://docs.gitlab.com/ee/gitlab-basics/
