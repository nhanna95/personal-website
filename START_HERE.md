# 🚀 Quick Start Guide

Your website has been converted to Jekyll! Here's the fastest way to get it live:

## ⚡ Deploy to GitHub (Recommended)

**This is the easiest way - no local setup needed!**

```bash
# 1. Initialize git
git init

# 2. Add all files
git add .

# 3. Commit
git commit -m "Convert to Jekyll"

# 4. Create a repo on GitHub.com, then:
git remote add origin https://github.com/YOUR_USERNAME/REPO_NAME.git

# 5. Push
git push -u origin main
```

**Then:**
1. Go to GitHub → Your Repo → Settings → Pages
2. Under "Source", select "GitHub Actions"
3. The site will build automatically!
4. Check the Actions tab to watch progress
5. Your site will be live at `https://YOUR_USERNAME.github.io/REPO_NAME`

---

## 🏠 Test Locally (Optional)

Only do this if you want to test changes before pushing.

**You'll need Ruby 3.1+** (you currently have 2.6)

### Install Ruby via rbenv:

```bash
# Install Homebrew (if needed)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install rbenv
brew install rbenv ruby-build

# Add to ~/.zshrc
echo 'eval "$(rbenv init - zsh)"' >> ~/.zshrc
source ~/.zshrc

# Install Ruby 3.3.0
rbenv install 3.3.0
rbenv global 3.3.0

# Verify
ruby -v  # Should show 3.3.0

# Install Jekyll
gem install bundler
bundle install

# Run local server
bundle exec jekyll serve
```

Then visit http://localhost:4000

---

## 📝 Need More Help?

- **Testing Guide**: See `TESTING_GUIDE.md` for all testing options
- **Local Setup**: See `LOCAL_SETUP.md` for detailed Ruby installation
- **Migration Details**: See `JEKYLL_SETUP.md` for what changed

---

## ✨ What's New?

✅ Blog posts are now in `_posts/*.md` (Markdown!)  
✅ Pages are in `_pages/*.md`  
✅ GitHub Actions builds & deploys automatically  
✅ Just add new Markdown files to blog  

**That's it! Your site is ready to go.** 🎉

