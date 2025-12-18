# CI/CD Pipeline Documentation

This repository uses GitHub Actions for Continuous Integration and Continuous Deployment (CI/CD). The pipeline is designed to build, test, and deploy the .NET MAUI StoreInfo plugin across multiple platforms.

## Overview

The CI/CD pipeline consists of multiple workflows that handle different aspects of the development lifecycle:

1. **CI - Build and Test** (`ci.yml`) - Main build pipeline for all platforms
2. **PR Validation** (`pr-validation.yml`) - Quick validation for pull requests
3. **Release and Publish** (`release.yml`) - Release management and NuGet publishing
4. **Security Scan** (`security.yml`) - Security vulnerability scanning
5. **Code Quality** (`code-quality.yml`) - Code formatting and linting

## Workflows

### 1. CI - Build and Test (`ci.yml`)

**Triggers:**
- Push to `main`, `master`, or `develop` branches
- Pull requests to these branches
- Manual workflow dispatch

**Jobs:**
- **Build Android**: Builds the MAUI plugin for Android platform
- **Build iOS**: Builds the MAUI plugin for iOS platform (runs on macOS)
- **Build Windows**: Builds the MAUI plugin for Windows platform
- **Build Xamarin**: Builds the legacy Xamarin.Forms plugin
- **Code Quality**: Runs code formatting checks
- **Summary**: Aggregates results from all build jobs

**Artifacts:**
- Build outputs for each platform (retained for 7 days)

### 2. PR Validation (`pr-validation.yml`)

**Triggers:**
- Pull request opened, synchronized, or reopened

**Jobs:**
- **PR Info**: Displays pull request information
- **Validate Files**: Lists changed files in the PR
- **Quick Build**: Performs a quick debug build
- **Code Analysis**: Runs code analysis on changes
- **PR Summary**: Aggregates validation results

### 3. Release and Publish (`release.yml`)

**Triggers:**
- Push tags matching `v*.*.*` pattern (e.g., `v1.0.5`)
- Manual workflow dispatch with version input

**Jobs:**
- **Build Packages**: 
  - Builds NuGet packages for both Xamarin and MAUI
  - Versions packages based on tag or input
  - Uploads packages as artifacts (retained for 90 days)
  
- **Create Release**:
  - Creates a GitHub release
  - Attaches NuGet packages to the release
  - Generates release notes
  
- **Publish to NuGet.org**:
  - Publishes packages to NuGet.org (for non-prerelease versions)
  - Requires `NUGET_API_KEY` secret
  
- **Publish to GitHub Packages**:
  - Publishes packages to GitHub Package Registry
  - Uses `GITHUB_TOKEN` for authentication

**Required Secrets:**
- `NUGET_API_KEY`: API key for publishing to NuGet.org

### 4. Security Scan (`security.yml`)

**Triggers:**
- Push to `main`, `master`, or `develop` branches
- Pull requests to these branches
- Weekly schedule (Mondays at 00:00 UTC)
- Manual workflow dispatch

**Jobs:**
- **Dependency Scan**: Checks for vulnerable NuGet packages
- **CodeQL Analysis**: Performs static code analysis for security issues
- **Secret Scan**: Scans for accidentally committed secrets using TruffleHog
- **Security Summary**: Aggregates security scan results

### 5. Code Quality (`code-quality.yml`)

**Triggers:**
- Push to `main`, `master`, or `develop` branches (when C# files change)
- Pull requests (when C# files change)
- Manual workflow dispatch

**Jobs:**
- **Format Check**: Verifies code formatting standards
- **Lint Analysis**: Runs code analyzers
- **Code Metrics**: Generates code statistics
- **Quality Summary**: Aggregates quality check results

## Usage Guide

### For Developers

#### Running Builds Locally

```bash
# Restore dependencies
dotnet restore src/StoreInfoPlugin.sln

# Build for a specific platform
dotnet build src/StoreInfoMaui/StoreInfoMaui.csproj -c Release -f net10.0-android
dotnet build src/StoreInfoMaui/StoreInfoMaui.csproj -c Release -f net10.0-ios

# Format code
dotnet format src/StoreInfoPlugin.sln

# Check for vulnerable packages
dotnet list src/StoreInfoPlugin.sln package --vulnerable --include-transitive
```

#### Creating a Pull Request

1. Create a feature branch from `develop`
2. Make your changes
3. Push your branch - PR validation workflow will run automatically
4. Wait for all checks to pass
5. Request review

#### Working with Pre-commit Checks

Before committing, ensure:
- Code is formatted: `dotnet format`
- Code builds: `dotnet build`
- No security issues: Check dependency vulnerabilities

### For Maintainers

#### Creating a Release

**Option 1: Using Git Tags (Recommended)**

```bash
# Create and push a version tag
git tag v1.0.5
git push origin v1.0.5
```

This will:
1. Trigger the release workflow
2. Build NuGet packages
3. Create a GitHub release
4. Publish to NuGet.org and GitHub Packages

**Option 2: Manual Release**

1. Go to Actions → Release and Publish
2. Click "Run workflow"
3. Enter version number (e.g., `1.0.5`)
4. Check "Is this a pre-release?" if needed
5. Click "Run workflow"

#### Managing Secrets

Required secrets in repository settings:
- `NUGET_API_KEY`: Get from https://www.nuget.org/account/apikeys

### For DevOps Engineers

#### Workflow Dependencies

```
ci.yml (Main CI)
├── Runs on: push, PR
└── Dependencies: None

pr-validation.yml
├── Runs on: PR only
└── Dependencies: None

release.yml
├── Runs on: tag push, manual
└── Dependencies: ci.yml (implicit)

security.yml
├── Runs on: push, PR, schedule
└── Dependencies: None

code-quality.yml
├── Runs on: push, PR
└── Dependencies: None
```

#### Caching Strategy

The workflows use default .NET caching provided by `setup-dotnet` action:
- NuGet packages are cached automatically
- Workload installations are cached per runner

#### Runner Configuration

- **Linux (ubuntu-latest)**: Android builds, quick checks, security scans
- **macOS (macos-13)**: iOS builds (requires Xcode)
- **Windows (windows-latest)**: Windows builds, Xamarin builds

#### Optimization Tips

1. **Reduce build times**:
   - Use matrix builds for multi-platform scenarios
   - Enable NuGet package caching
   - Use `--no-restore` flag after initial restore

2. **Reduce workflow runs**:
   - Use `paths` filters to run only when relevant files change
   - Combine related jobs where possible

3. **Cost optimization**:
   - iOS builds on macOS runners are more expensive
   - Consider using self-hosted runners for high-frequency builds

## Monitoring and Troubleshooting

### Viewing Workflow Status

1. Go to the "Actions" tab in GitHub
2. Select the workflow you want to view
3. Click on a specific run to see details

### Common Issues

**Issue: iOS build fails**
- **Cause**: Xcode or iOS SDK version mismatch
- **Solution**: Update `macos-13` runner or adjust target framework

**Issue: NuGet publish fails**
- **Cause**: Invalid or expired API key
- **Solution**: Update `NUGET_API_KEY` secret

**Issue: Code format check fails**
- **Cause**: Code not formatted according to .editorconfig
- **Solution**: Run `dotnet format` locally and commit changes

**Issue: Security scan finds vulnerabilities**
- **Cause**: Outdated dependencies with known vulnerabilities
- **Solution**: Update affected packages to patched versions

### Build Status Badges

Add these to your README.md:

```markdown
[![CI Build](https://github.com/sunilsolanki-alight/MauiStoreInfo/actions/workflows/ci.yml/badge.svg)](https://github.com/sunilsolanki-alight/MauiStoreInfo/actions/workflows/ci.yml)
[![Security Scan](https://github.com/sunilsolanki-alight/MauiStoreInfo/actions/workflows/security.yml/badge.svg)](https://github.com/sunilsolanki-alight/MauiStoreInfo/actions/workflows/security.yml)
[![Code Quality](https://github.com/sunilsolanki-alight/MauiStoreInfo/actions/workflows/code-quality.yml/badge.svg)](https://github.com/sunilsolanki-alight/MauiStoreInfo/actions/workflows/code-quality.yml)
```

## Best Practices

1. **Version Management**:
   - Use semantic versioning (MAJOR.MINOR.PATCH)
   - Tag releases in git
   - Update CHANGELOG.md for each release

2. **Branch Strategy**:
   - `main`/`master`: Production-ready code
   - `develop`: Integration branch
   - `feature/*`: Feature branches
   - `hotfix/*`: Emergency fixes

3. **Security**:
   - Never commit secrets or API keys
   - Review security scan results weekly
   - Keep dependencies up to date

4. **Code Quality**:
   - Run formatters before committing
   - Address analyzer warnings
   - Maintain test coverage (when tests are added)

## Future Enhancements

Potential improvements to consider:

1. **Testing**:
   - Add unit tests and integration tests
   - Include test coverage reporting
   - Add UI tests for sample apps

2. **Performance**:
   - Add performance benchmarking
   - Monitor build times and optimize

3. **Documentation**:
   - Auto-generate API documentation
   - Include in releases

4. **Deployment**:
   - Add staging environment deployments
   - Include rollback mechanisms

5. **Monitoring**:
   - Add workflow analytics
   - Set up notifications for failures

## Support

For issues or questions about the CI/CD pipeline:
1. Check the workflow logs in the Actions tab
2. Review this documentation
3. Open an issue in the repository
4. Contact the DevOps team

## References

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [.NET MAUI Documentation](https://learn.microsoft.com/en-us/dotnet/maui/)
- [NuGet Publishing Guide](https://learn.microsoft.com/en-us/nuget/nuget-org/publish-a-package)
- [CodeQL Documentation](https://codeql.github.com/docs/)
