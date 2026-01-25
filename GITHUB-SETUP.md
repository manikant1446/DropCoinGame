# 📤 How to Push CoinDrop to GitHub

## Step 1: Install Git

Git is not installed on your system. You need to install it first.

### Download Git for Windows:
1. Go to: https://git-scm.com/download/win
2. Download the installer
3. Run the installer (use default settings)
4. Restart your terminal/PowerShell

### Verify Installation:
```bash
git --version
```

## Step 2: Create GitHub Repository

1. Go to https://github.com
2. Click the **"+"** icon → **"New repository"**
3. Repository name: `coindrop` (or your preferred name)
4. Description: "Skill-based Web3 physics game on Monad blockchain"
5. Choose **Public** or **Private**
6. **DO NOT** check "Initialize with README" (we already have one)
7. Click **"Create repository"**

## Step 3: Initialize Git and Push

Open PowerShell in `d:\New folder2` and run:

```bash
# Initialize Git repository
git init

# Add all files
git add .

# Create first commit
git commit -m "Initial commit: CoinDrop Web3 game"

# Add your GitHub repository as remote
# Replace YOUR_USERNAME and REPO_NAME with your actual values
git remote add origin https://github.com/YOUR_USERNAME/REPO_NAME.git

# Push to GitHub
git branch -M main
git push -u origin main
```

## Step 4: Alternative - GitHub Desktop (Easier)

If you prefer a GUI:

1. **Download GitHub Desktop**: https://desktop.github.com
2. **Install and sign in** to your GitHub account
3. Click **"Add"** → **"Add existing repository"**
4. Browse to `d:\New folder2`
5. Click **"Publish repository"**
6. Choose name and visibility
7. Click **"Publish"**

## Step 5: Verify Upload

1. Go to your GitHub repository URL
2. You should see all your files
3. The README.md will be displayed on the main page

## 📋 What Will Be Uploaded

✅ **Included:**
- All game files (HTML, CSS, JS)
- Smart contract (CoinDrop.sol)
- Backend code
- Documentation files
- Configuration files
- README.md

❌ **Excluded** (via .gitignore):
- node_modules/
- .env files
- Database files
- Log files
- OS-specific files

## 🔒 Important Security Notes

Before pushing, make sure:
- ✅ `.env` files are in `.gitignore`
- ✅ No private keys or secrets in code
- ✅ Contract address is placeholder (`0x000...`)
- ✅ API keys are not hardcoded

## 🎯 Quick Commands Reference

```bash
# Check status
git status

# Add specific files
git add filename.js

# Commit changes
git commit -m "Your message"

# Push changes
git push

# Pull latest changes
git pull

# View commit history
git log
```

## 🆘 Troubleshooting

### "git is not recognized"
→ Git is not installed. Follow Step 1.

### "Permission denied"
→ You need to authenticate with GitHub. Use GitHub Desktop or set up SSH keys.

### "Repository not found"
→ Check the remote URL: `git remote -v`

### Large files error
→ Some files might be too large. Check `.gitignore`.

## 📞 Need Help?

1. **GitHub Docs**: https://docs.github.com
2. **Git Basics**: https://git-scm.com/doc
3. **GitHub Desktop**: https://docs.github.com/en/desktop

---

**After pushing, share your repository URL with others!** 🚀
