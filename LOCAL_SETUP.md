# Local Jekyll Setup Guide

To test your Jekyll site locally, you need Ruby 3.1+ installed. Here are the options:

## Option 1: Install rbenv (Recommended for macOS)

### Step 1: Install Homebrew (if not installed)
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### Step 2: Install rbenv via Homebrew
```bash
brew install rbenv ruby-build
```

### Step 3: Initialize rbenv
Add to your `~/.zshrc`:
```bash
eval "$(rbenv init - zsh)"
```

Then reload:
```bash
source ~/.zshrc
```

### Step 4: Install Ruby 3.1+
```bash
rbenv install 3.3.0
rbenv global 3.3.0
```

### Step 5: Install Bundler and Jekyll
```bash
gem install bundler
cd /Users/noxin/Desktop/person_website
bundle install
```

### Step 6: Run Jekyll
```bash
bundle exec jekyll serve
```

Visit http://localhost:4000

---

## Option 2: Use Docker (Alternative)

If you prefer Docker:

### Step 1: Install Docker Desktop
Download from: https://www.docker.com/products/docker-desktop/

### Step 2: Create Dockerfile
Already created for you in the repo.

### Step 3: Build and Run
```bash
docker build -t jekyll-site .
docker run -p 4000:4000 -v $(pwd):/srv/jekyll jekyll-site
```

Visit http://localhost:4000

---

## Option 3: Skip Local Testing (Easiest)

Just push to GitHub and let GitHub Actions handle it:

```bash
git init
git add .
git commit -m "Initial Jekyll setup"
git remote add origin YOUR_REPO_URL
git push -u origin main
```

Then check the Actions tab for build status.

---

## Troubleshooting

### "Command not found" errors
- Make sure you've installed Ruby 3.1+ and reloaded your shell
- Check `ruby -v` shows 3.1 or higher

### Bundle install fails
- Make sure you have Xcode Command Line Tools: `xcode-select --install`
- Try running without sudo (shouldn't need it with rbenv)

### Port 4000 already in use
- Change the port: `bundle exec jekyll serve -p 4001`
- Or kill the process using port 4000

---

## Quick Reference

Once set up:
```bash
# Start server
bundle exec jekyll serve

# Build only (no server)
bundle exec jekyll build

# Watch for changes (auto-rebuild)
bundle exec jekyll serve --watch

# View at different port
bundle exec jekyll serve -p 4001
```

