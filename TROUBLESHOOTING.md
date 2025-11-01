# Troubleshooting GitHub Pages Not Loading

## ✅ Fixed Issues

1. **Deployment Workflow**: Added missing `id: deployment` to deploy step
2. **Pushed to GitHub**: Latest workflow changes are now in your repo

## 🔍 Check These Steps

### 1. Verify GitHub Actions Ran
- Go to: https://github.com/nhanna95/personal-website/actions
- Check if the latest workflow completed successfully
- If it failed, click on it to see error messages

### 2. Check Your Site URL
Your site URL depends on how GitHub Pages is configured:

- **User site**: `https://nhanna95.github.io` (needs repo named `nhanna95.github.io`)
- **Project site**: `https://nhanna95.github.io/personal-website` (current repo name)

If your repo is named `personal-website`, your site is at the **project site** URL.

### 3. Update baseurl if Needed

If you're using a project site (not `username.github.io` repo), edit `_config.yml`:

```yaml
url: https://nhanna95.github.io
baseurl: "/personal-website"  # Add this!
```

Then commit and push:
```bash
git add _config.yml
git commit -m "Fix baseurl for project site"
git push
```

### 4. Check GitHub Pages Settings
- Go to: Settings → Pages
- Under "Build and deployment":
  - Source should be: **GitHub Actions**
  - Branch should be: **main** (though this doesn't matter when using Actions)

### 5. Wait for Deployment
After a successful workflow run, it can take 1-5 minutes for the site to be available.

### 6. Clear Browser Cache
- Try incognito/private browsing mode
- Or hard refresh: Cmd+Shift+R (Mac) or Ctrl+Shift+R (Windows)

### 7. Check for Build Errors
If the workflow failed, common issues:
- Jekyll syntax errors in markdown
- Missing layout files
- Plugin issues

## 🐛 Common Issues & Fixes

### Issue: "Page build failed"
**Fix**: Check Actions logs for specific error. Usually a YAML syntax error or missing file.

### Issue: "404 Not Found"
**Fixes**:
1. Wait a few minutes (deployment is slow)
2. Check the correct URL (project vs user site)
3. Verify `baseurl` is set correctly

### Issue: "Content not showing"
**Fix**: Likely `baseurl` issue. Make sure assets use `{{ '/asset.png' | relative_url }}`.

### Issue: Workflow not running
**Fix**: Make sure you have Actions enabled:
- Settings → Actions → General
- Allow all actions and reusable workflows

## 📋 What to Check Now

1. ✅ Actions tab shows successful workflow run
2. ✅ Wait 2-3 minutes after successful workflow
3. ✅ Try the correct URL (project site)
4. ✅ Update baseurl in _config.yml if needed
5. ✅ Clear browser cache

## 🆘 Still Not Working?

Share:
1. Screenshot of Actions tab showing workflow status
2. Any error messages from failed workflows
3. The exact URL you're trying to access

