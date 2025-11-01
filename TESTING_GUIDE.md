# Testing Your Jekyll Site

Since you don't have Ruby 3.1+ installed locally, here are the best ways to test:

## Option A: Push to GitHub (Recommended) ⭐

The easiest way is to just push your code to GitHub and let GitHub Actions build it automatically.

### Quick Start:
```bash
# Initialize git (if not done)
git init

# Create .github repo (if not exists)
# Then add and commit
git add .
git commit -m "Migrate to Jekyll"

# Push to GitHub
git remote add origin YOUR_GITHUB_REPO_URL
git push -u origin main
```

### Then:
1. Go to your GitHub repo
2. Click "Actions" tab
3. Watch the build progress
4. Once it succeeds, your site is live!
5. Check the deployment URL in the "Environments" section

---

## Option B: Test in a Branch

If you want to test before merging:

```bash
# Create test branch
git checkout -b test-jekyll

# Push to GitHub
git push -u origin test-jekyll
```

Then GitHub Actions will build the test branch too.

---

## Option C: Set Up Local Environment

If you really want to test locally, you'll need to install Ruby 3.1+.

See `LOCAL_SETUP.md` for detailed instructions.

**Quick summary:**
1. Install Homebrew (if needed): `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`
2. Install rbenv: `brew install rbenv ruby-build`
3. Add to `~/.zshrc`: `eval "$(rbenv init - zsh)"` then `source ~/.zshrc`
4. Install Ruby: `rbenv install 3.3.0 && rbenv global 3.3.0`
5. Install gems: `gem install bundler && bundle install`
6. Run Jekyll: `bundle exec jekyll serve`
7. Open http://localhost:4000

---

## Option D: Use GitHub Codespaces

GitHub offers online development environments:

1. Go to your repo on GitHub
2. Click the green "Code" button
3. Select "Codespaces" tab
4. Click "Create codespace on main"
5. Wait for it to set up
6. In the terminal: `bundle install && bundle exec jekyll serve`
7. Forward port 4000 to your browser

---

## My Recommendation

**Just push to GitHub!** 🚀

Since your site is configured to build automatically via GitHub Actions, there's no need to test locally unless you're making frequent small changes. The GitHub Actions workflow will:

- Build your site
- Show any errors
- Deploy it automatically
- Preview the result

Much simpler than setting up a local environment just to test once.

---

## What to Check After Deployment

1. ✅ Home page loads correctly
2. ✅ Navigation works (sidebar/menu)
3. ✅ Blog page shows all posts
4. ✅ Tags filter posts correctly
5. ✅ Search works
6. ✅ Read times display properly
7. ✅ Welcome post is pinned
8. ✅ Links work
9. ✅ Styling looks good on mobile and desktop

If anything's broken, GitHub Actions will show you the error in the build log.

