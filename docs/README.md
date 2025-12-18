# CI/CD Pipeline Documentation Index

Welcome to the CI/CD pipeline documentation for MauiStoreInfo. This index will help you find the information you need.

## 📚 Documentation Overview

### 1. [Getting Started](GETTING_STARTED.md) - **START HERE** ⭐
**For:** New contributors and developers
**Content:**
- Prerequisites and setup
- Understanding the workflows
- Your first contribution
- Creating your first release
- Troubleshooting basics

**When to read:** Before making your first contribution

### 2. [Quick Reference](QUICK_REFERENCE.md)
**For:** Developers who need quick answers
**Content:**
- Quick commands for common tasks
- Workflow triggers
- Common issues and fixes
- Emergency procedures
- Versioning strategy

**When to read:** When you need to quickly find a command or solve an issue

### 3. [CI/CD Pipeline Documentation](CICD_PIPELINE.md)
**For:** Developers and DevOps engineers
**Content:**
- Detailed workflow explanations
- Usage guide for developers
- Usage guide for maintainers
- Monitoring and troubleshooting
- Best practices
- Future enhancements

**When to read:** When you need to understand how the pipeline works in detail

### 4. [Pipeline Architecture](PIPELINE_ARCHITECTURE.md)
**For:** DevOps engineers and technical leads
**Content:**
- Architecture diagrams
- Workflow interdependencies
- Platform matrix
- Artifact flow
- Security pipeline
- Resource requirements
- Cost optimization

**When to read:** When planning changes to the pipeline or understanding the architecture

## 🎯 Find What You Need

### I want to...

#### Make a Code Change
→ Read: [Getting Started](GETTING_STARTED.md) - "Your First Contribution"
→ Reference: [Quick Reference](QUICK_REFERENCE.md) - "Quick Commands"

#### Create a Pull Request
→ Read: [Getting Started](GETTING_STARTED.md) - "Creating a Pull Request"
→ Check: [CI/CD Pipeline](CICD_PIPELINE.md) - "PR Validation Workflow"

#### Create a Release
→ Read: [Getting Started](GETTING_STARTED.md) - "Creating Your First Release"
→ Reference: [Quick Reference](QUICK_REFERENCE.md) - "Creating a Release"

#### Fix a Build Failure
→ Reference: [Quick Reference](QUICK_REFERENCE.md) - "Common Issues"
→ Details: [CI/CD Pipeline](CICD_PIPELINE.md) - "Monitoring and Troubleshooting"

#### Understand Workflow Architecture
→ Read: [Pipeline Architecture](PIPELINE_ARCHITECTURE.md) - "Pipeline Architecture Diagram"

#### Set Up Secrets
→ Read: [Getting Started](GETTING_STARTED.md) - "Configuring Secrets"
→ Details: [CI/CD Pipeline](CICD_PIPELINE.md) - "For Maintainers"

#### Optimize Build Performance
→ Read: [Pipeline Architecture](PIPELINE_ARCHITECTURE.md) - "Cost Optimization"
→ Check: [CI/CD Pipeline](CICD_PIPELINE.md) - "For DevOps Engineers"

## 📋 Workflow Quick Links

### Active Workflows

| Workflow | File | Purpose | Documentation |
|----------|------|---------|---------------|
| CI Build | `ci.yml` | Build all platforms | [Details](CICD_PIPELINE.md#1-ci---build-and-test-ciyml) |
| PR Validation | `pr-validation.yml` | Validate pull requests | [Details](CICD_PIPELINE.md#2-pr-validation-pr-validationyml) |
| Release | `release.yml` | Create and publish releases | [Details](CICD_PIPELINE.md#3-release-and-publish-releaseyml) |
| Security Scan | `security.yml` | Security vulnerability scanning | [Details](CICD_PIPELINE.md#4-security-scan-securityyml) |
| Code Quality | `code-quality.yml` | Code formatting and linting | [Details](CICD_PIPELINE.md#5-code-quality-code-qualityyml) |

### Legacy Workflows

| Workflow | File | Status | Notes |
|----------|------|--------|-------|
| .NET 10 Android | `dotnet10-android.yml` | Legacy | Experimental .NET 10 build |

## 🚀 Common Tasks

### Developer Tasks

```bash
# Format code
dotnet format src/StoreInfoPlugin.sln

# Build locally
dotnet build src/StoreInfoPlugin.sln -c Release

# Create feature branch
git checkout -b feature/my-feature

# Push and create PR
git push origin feature/my-feature
```

### Maintainer Tasks

```bash
# Create release tag
git tag -a v1.0.5 -m "Release 1.0.5"
git push origin v1.0.5

# Check workflow status
gh run list

# View workflow logs
gh run view [run-id]
```

## 🔧 Configuration Files

### Workflow Files
Location: `.github/workflows/`
- `ci.yml` - Main CI build
- `pr-validation.yml` - PR checks
- `release.yml` - Release automation
- `security.yml` - Security scanning
- `code-quality.yml` - Code quality checks

### Project Files
Location: `src/`
- `StoreInfoPlugin.sln` - Main solution
- `StoreInfo/StoreInfo.csproj` - Xamarin plugin
- `StoreInfoMaui/StoreInfoMaui.csproj` - MAUI plugin
- `StoreInfoCore/StoreInfoCore.csproj` - Core library

### Documentation Files
Location: `docs/`
- `README.md` - This index
- `GETTING_STARTED.md` - Beginner guide
- `QUICK_REFERENCE.md` - Command reference
- `CICD_PIPELINE.md` - Detailed pipeline docs
- `PIPELINE_ARCHITECTURE.md` - Architecture details

## 📊 Pipeline Status

Check current pipeline status:
- [CI Build Status](https://github.com/sunilsolanki-alight/MauiStoreInfo/actions/workflows/ci.yml)
- [Security Scan Status](https://github.com/sunilsolanki-alight/MauiStoreInfo/actions/workflows/security.yml)
- [Code Quality Status](https://github.com/sunilsolanki-alight/MauiStoreInfo/actions/workflows/code-quality.yml)

## 🆘 Need Help?

1. **Check documentation** (you're here!)
2. **View workflow logs** in Actions tab
3. **Open an issue** if you find a problem
4. **Ask the team** in your organization's channel

## 📝 Contributing to Documentation

Found an error or want to improve the docs?

1. Edit the relevant markdown file
2. Submit a PR with your changes
3. Tag documentation in the title: `docs: improve getting started guide`

## 🔗 External Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [.NET MAUI Documentation](https://learn.microsoft.com/en-us/dotnet/maui/)
- [NuGet Publishing Guide](https://learn.microsoft.com/en-us/nuget/nuget-org/publish-a-package)
- [CodeQL Documentation](https://codeql.github.com/docs/)
- [Semantic Versioning](https://semver.org/)

## 📅 Documentation Updates

| Date | Document | Change |
|------|----------|--------|
| 2024-12-18 | All | Initial CI/CD pipeline documentation created |

---

**Last Updated:** December 18, 2024
**Version:** 1.0.0
