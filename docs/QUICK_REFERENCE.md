# CI/CD Quick Reference Guide

## Quick Commands

### Local Development

```bash
# Restore dependencies
dotnet restore src/StoreInfoPlugin.sln

# Build all projects
dotnet build src/StoreInfoPlugin.sln -c Release

# Build specific platform
dotnet build src/StoreInfoMaui/StoreInfoMaui.csproj -c Release -f net7.0-android
dotnet build src/StoreInfoMaui/StoreInfoMaui.csproj -c Release -f net7.0-ios

# Format code
dotnet format src/StoreInfoPlugin.sln

# Check formatting without making changes
dotnet format src/StoreInfoPlugin.sln --verify-no-changes

# Check for vulnerable packages
dotnet list src/StoreInfoPlugin.sln package --vulnerable --include-transitive
```

### Creating a Pull Request

```bash
# Create feature branch
git checkout -b feature/my-feature

# Make changes and commit
git add .
git commit -m "feat: add new feature"

# Push branch
git push origin feature/my-feature
```

Then create PR on GitHub. The following checks will run automatically:
- ✅ PR Validation
- ✅ Code Quality
- ✅ Security Scan

### Creating a Release

**Method 1: Git Tag (Recommended)**
```bash
# Tag the commit
git tag v1.0.5

# Push tag to trigger release
git push origin v1.0.5
```

**Method 2: Manual Trigger**
1. Go to GitHub Actions
2. Select "Release and Publish" workflow
3. Click "Run workflow"
4. Enter version (e.g., 1.0.5)
5. Run

## Workflow Triggers

| Workflow | Trigger | When |
|----------|---------|------|
| CI Build | Push to main/master/develop | Every commit |
| PR Validation | Pull request | When PR opened/updated |
| Release | Tag push `v*.*.*` | Version tag created |
| Security Scan | Push, PR, Schedule | Commits + Weekly |
| Code Quality | Push, PR | When .cs files change |

## Build Status

Check build status:
1. Go to repository on GitHub
2. Click "Actions" tab
3. Select workflow to view
4. Click on specific run for details

## Common Issues

### Build Fails

**Check:**
1. Workflow logs in Actions tab
2. Error messages in build output
3. Dependencies are restored

**Fix:**
```bash
# Clean and rebuild
dotnet clean
dotnet restore
dotnet build
```

### Format Check Fails

**Check:**
```bash
dotnet format src/StoreInfoPlugin.sln --verify-no-changes --verbosity diagnostic
```

**Fix:**
```bash
dotnet format src/StoreInfoPlugin.sln
git add .
git commit -m "style: apply code formatting"
```

### Security Scan Finds Issues

**Check:**
1. Review security alerts in Actions tab
2. Check dependency vulnerabilities

**Fix:**
```bash
# Update vulnerable package
dotnet add package <PackageName> --version <SafeVersion>

# Or update all packages
dotnet restore
```

### Release Fails

**Check:**
1. Version number is valid (semantic versioning)
2. NUGET_API_KEY secret is set
3. Tag format is correct (v1.0.0)

**Fix:**
```bash
# Delete bad tag locally and remotely
git tag -d v1.0.5
git push origin :refs/tags/v1.0.5

# Create correct tag
git tag v1.0.5
git push origin v1.0.5
```

## Secrets Configuration

Required secrets in repository settings:

| Secret | Purpose | How to Get |
|--------|---------|------------|
| NUGET_API_KEY | Publish to NuGet.org | https://www.nuget.org/account/apikeys |
| GITHUB_TOKEN | Automatic | Provided by GitHub |

## Artifact Locations

After successful builds:

| Artifact | Location | Retention |
|----------|----------|-----------|
| Build Outputs | Actions → Workflow → Artifacts | 7 days |
| NuGet Packages | Actions → Release → Artifacts | 90 days |
| Release Packages | GitHub Releases | Permanent |
| Published Packages | NuGet.org | Permanent |

## Platform-Specific Notes

### Android
- Builds on Ubuntu runners
- Target: net7.0-android
- Fast build times (~5 min)

### iOS
- Builds on macOS-13 runners
- Target: net7.0-ios
- Requires Xcode
- Slower build times (~10-15 min)
- Higher runner costs

### Windows
- Builds on Windows runners
- Target: net7.0-windows10.0.19041.0
- Moderate build times (~5-10 min)

### Xamarin
- Builds on Windows runners
- Uses MSBuild
- Legacy support

## Versioning Strategy

Semantic Versioning (MAJOR.MINOR.PATCH):
- **MAJOR**: Breaking changes
- **MINOR**: New features (backward compatible)
- **PATCH**: Bug fixes

Examples:
- `v1.0.0` - Major release
- `v1.1.0` - Minor update with new features
- `v1.0.1` - Patch/bugfix
- `v1.0.0-beta.1` - Pre-release (beta)
- `v1.0.0-rc.1` - Release candidate

## Notifications

Workflow results are visible:
1. In GitHub Actions tab
2. As PR checks (for PRs)
3. As commit status (for pushes)

Configure additional notifications:
- Email: GitHub notification settings
- Slack: Add GitHub app to Slack
- Teams: Add GitHub connector

## Best Practices

1. **Before Committing:**
   - Format code: `dotnet format`
   - Build locally: `dotnet build`
   - Check for warnings

2. **Pull Requests:**
   - Create from feature branches
   - Keep changes focused
   - Wait for CI checks to pass
   - Address review comments

3. **Releases:**
   - Update CHANGELOG.md
   - Use semantic versioning
   - Test before tagging
   - Verify NuGet packages

4. **Security:**
   - Review security scan results
   - Update vulnerable dependencies
   - Never commit secrets

## Getting Help

1. **Check Documentation:**
   - [CI/CD Pipeline Docs](CICD_PIPELINE.md)
   - [Architecture Docs](PIPELINE_ARCHITECTURE.md)

2. **View Logs:**
   - GitHub Actions tab → Select workflow → View logs

3. **Common Resources:**
   - [GitHub Actions Docs](https://docs.github.com/en/actions)
   - [.NET MAUI Docs](https://learn.microsoft.com/en-us/dotnet/maui/)
   - [NuGet Docs](https://learn.microsoft.com/en-us/nuget/)

## Emergency Procedures

### Rollback a Release

```bash
# Unpublish from NuGet.org
# (Must be done within 72 hours via NuGet.org website)

# Create hotfix
git checkout v1.0.4  # Last good version
git checkout -b hotfix/critical-fix
# Make fix
git commit -m "fix: critical issue"
git tag v1.0.5
git push origin v1.0.5
```

### Stop a Running Workflow

1. Go to Actions tab
2. Click on running workflow
3. Click "Cancel workflow"

### Disable a Workflow

1. Go to Actions tab
2. Select workflow
3. Click "..." menu
4. Select "Disable workflow"

## Monitoring

Key metrics to watch:
- Build success rate
- Build duration trends
- Security scan results
- Code quality scores

Access:
- Actions tab → Select workflow → View runs
- Insights tab → Workflow insights (if available)
