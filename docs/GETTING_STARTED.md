# Getting Started with CI/CD Pipeline

This guide will help you set up and start using the CI/CD pipeline for the MauiStoreInfo project.

## Prerequisites

Before you begin, ensure you have:

1. **Development Environment:**
   - .NET 10.0 SDK or later
   - Visual Studio 2022 or Visual Studio Code
   - Git installed and configured

2. **Access:**
   - Write access to the GitHub repository
   - (Optional) NuGet.org API key for publishing

3. **Local Tools:**
   ```bash
   # Verify .NET installation
   dotnet --version
   
   # Install MAUI workload
   dotnet workload install maui
   
   # Verify git
   git --version
   ```

## Initial Setup

### 1. Clone the Repository

```bash
git clone https://github.com/sunilsolanki-alight/MauiStoreInfo.git
cd MauiStoreInfo
```

### 2. Restore Dependencies

```bash
# Restore all project dependencies
dotnet restore src/StoreInfoPlugin.sln
```

### 3. Build Locally

```bash
# Build the solution to ensure everything works
dotnet build src/StoreInfoPlugin.sln -c Release
```

If the build succeeds, your environment is ready!

## Understanding the Workflows

The pipeline includes 5 main workflows:

### 1. CI Build (`ci.yml`)
**Purpose:** Build and validate code on all platforms
**When it runs:** On every push to main/master/develop branches
**What it does:**
- Builds for Android, iOS, Windows, and Xamarin
- Runs code quality checks
- Creates build artifacts

### 2. PR Validation (`pr-validation.yml`)
**Purpose:** Quick validation for pull requests
**When it runs:** When a PR is opened or updated
**What it does:**
- Shows PR information
- Validates changed files
- Performs quick build check
- Runs code analysis

### 3. Release & Publish (`release.yml`)
**Purpose:** Create releases and publish packages
**When it runs:** When a version tag is pushed (e.g., v1.0.5)
**What it does:**
- Builds NuGet packages
- Creates GitHub release
- Publishes to NuGet.org and GitHub Packages

### 4. Security Scan (`security.yml`)
**Purpose:** Scan for security vulnerabilities
**When it runs:** On commits, PRs, and weekly schedule
**What it does:**
- Scans dependencies for vulnerabilities
- Runs CodeQL static analysis
- Checks for leaked secrets

### 5. Code Quality (`code-quality.yml`)
**Purpose:** Enforce code quality standards
**When it runs:** When C# files are modified
**What it does:**
- Checks code formatting
- Runs linting
- Reports code metrics

## Your First Contribution

### Step 1: Create a Feature Branch

```bash
git checkout -b feature/my-first-feature
```

### Step 2: Make Changes

Edit some code, for example:
```bash
# Edit a file
code src/StoreInfoMaui/StoreInfoImplementation.cs
```

### Step 3: Format and Build

```bash
# Format code to match style guidelines
dotnet format src/StoreInfoPlugin.sln

# Build to check for errors
dotnet build src/StoreInfoPlugin.sln -c Release
```

### Step 4: Commit Changes

```bash
git add .
git commit -m "feat: add my first feature"
```

### Step 5: Push and Create PR

```bash
git push origin feature/my-first-feature
```

Then go to GitHub and create a Pull Request:
1. Navigate to the repository
2. Click "Pull requests" tab
3. Click "New pull request"
4. Select your branch
5. Fill in PR details
6. Create PR

### Step 6: Wait for CI Checks

The following workflows will run automatically:
- ✅ PR Validation (2-5 minutes)
- ✅ Code Quality (1-3 minutes)
- ✅ Security Scan (3-5 minutes)

Watch the "Checks" section of your PR for status.

### Step 7: Address Feedback

If checks fail:
1. Review the error messages in the workflow logs
2. Fix issues locally
3. Commit and push again:
   ```bash
   git add .
   git commit -m "fix: address CI feedback"
   git push
   ```

## Creating Your First Release

### Preparation

1. **Update Version Numbers**
   - Edit `src/StoreInfo/StoreInfo.csproj` - Update `<Version>` and `<PackageVersion>`
   - Update `CHANGELOG.md` with release notes

2. **Commit Changes**
   ```bash
   git add .
   git commit -m "chore: prepare for v1.0.5 release"
   git push
   ```

3. **Wait for CI to Pass**
   - Ensure all builds on main branch succeed
   - Check Actions tab for green status

### Creating the Release

**Method 1: Using Git Tags (Recommended)**

```bash
# Create annotated tag
git tag -a v1.0.5 -m "Release version 1.0.5"

# Push tag to trigger release workflow
git push origin v1.0.5
```

**Method 2: Manual Trigger via GitHub UI**

1. Go to repository on GitHub
2. Click "Actions" tab
3. Select "Release and Publish" workflow
4. Click "Run workflow" button
5. Enter version number (e.g., `1.0.5`)
6. Optionally check "Is this a pre-release?"
7. Click "Run workflow"

### What Happens Next

The release workflow will:
1. ⏱️ Build packages (5-10 minutes)
2. ⏱️ Create GitHub release (1 minute)
3. ⏱️ Publish to NuGet.org (2-5 minutes)
4. ⏱️ Publish to GitHub Packages (1-2 minutes)

Total time: 10-20 minutes

### Verifying the Release

1. **Check GitHub Release:**
   - Go to repository
   - Click "Releases" section
   - Find your version
   - Verify packages are attached

2. **Check NuGet.org:**
   - Visit https://www.nuget.org/packages/Plugin.StoreInfo/
   - Verify new version appears

3. **Test Package:**
   ```bash
   # In a test project
   dotnet add package Plugin.StoreInfo --version 1.0.5
   ```

## Configuring Secrets (Maintainers Only)

To publish to NuGet.org, you need to configure a secret:

### Getting a NuGet API Key

1. Go to https://www.nuget.org/
2. Sign in to your account
3. Click your username → API Keys
4. Click "Create"
5. Configure:
   - Key Name: `MauiStoreInfo CI/CD`
   - Glob Pattern: `Plugin.StoreInfo*`
   - Scopes: `Push`, `Push new packages and package versions`
6. Click "Create"
7. **Copy the key** (you won't see it again!)

### Adding the Secret to GitHub

1. Go to repository on GitHub
2. Click "Settings" tab
3. In left sidebar, click "Secrets and variables" → "Actions"
4. Click "New repository secret"
5. Name: `NUGET_API_KEY`
6. Value: Paste your NuGet API key
7. Click "Add secret"

## Monitoring Your Workflows

### Via GitHub UI

1. **Actions Tab:**
   - Go to repository
   - Click "Actions" tab
   - See all workflow runs
   - Click any run for details

2. **PR Checks:**
   - Open your pull request
   - Scroll to "Checks" section
   - View status of each workflow

3. **Commit Status:**
   - View any commit
   - See status indicators next to commit message
   - Click for details

### Via CLI (Optional)

Install GitHub CLI:
```bash
# Install gh CLI
# macOS: brew install gh
# Windows: winget install GitHub.cli
# Linux: See https://github.com/cli/cli#installation

# Authenticate
gh auth login

# View workflow runs
gh run list

# View specific run
gh run view [run-id]

# Watch a run in real-time
gh run watch
```

## Troubleshooting

### Build Fails Locally

```bash
# Clean everything
dotnet clean
rm -rf **/bin **/obj

# Restore and rebuild
dotnet restore
dotnet build
```

### Format Check Fails in CI

```bash
# Check what's wrong
dotnet format src/StoreInfoPlugin.sln --verify-no-changes --verbosity diagnostic

# Fix formatting
dotnet format src/StoreInfoPlugin.sln

# Commit the changes
git add .
git commit -m "style: fix code formatting"
git push
```

### Security Scan Finds Issues

```bash
# Check for vulnerable packages
dotnet list src/StoreInfoPlugin.sln package --vulnerable --include-transitive

# Update a specific package
dotnet add src/StoreInfoMaui/StoreInfoMaui.csproj package System.Text.Json --version 10.0.0

# Or update all packages (carefully)
# Review each update individually
```

### iOS Build Fails

iOS builds require macOS runners and can fail due to:
- Xcode version mismatches
- Provisioning profile issues
- Certificate problems

For now, you can:
1. Check the error in workflow logs
2. Update target framework if needed
3. Ensure Xcode version matches requirements

### Release Workflow Fails

Common causes:
- Tag format incorrect (must be `v1.0.0`)
- NuGet API key not set or expired
- Version number already exists

Fix:
```bash
# Delete bad tag
git tag -d v1.0.5
git push origin :refs/tags/v1.0.5

# Create correct tag
git tag -a v1.0.5 -m "Release 1.0.5"
git push origin v1.0.5
```

## Best Practices

1. **Always format before committing:**
   ```bash
   dotnet format src/StoreInfoPlugin.sln
   ```

2. **Build locally before pushing:**
   ```bash
   dotnet build src/StoreInfoPlugin.sln -c Release
   ```

3. **Write clear commit messages:**
   - `feat: add new feature`
   - `fix: correct bug in iOS implementation`
   - `docs: update README with examples`
   - `chore: update dependencies`

4. **Keep PRs focused:**
   - One feature or fix per PR
   - Small, reviewable changes
   - Clear description

5. **Update CHANGELOG:**
   - Document all user-facing changes
   - Follow format of existing entries

## Getting Help

1. **Read the Documentation:**
   - [CI/CD Pipeline Docs](CICD_PIPELINE.md)
   - [Architecture Docs](PIPELINE_ARCHITECTURE.md)
   - [Quick Reference](QUICK_REFERENCE.md)

2. **Check Workflow Logs:**
   - Actions tab → Select workflow → View run → Check logs
   - Look for error messages
   - Note the failing step

3. **Common Resources:**
   - [GitHub Actions Documentation](https://docs.github.com/en/actions)
   - [.NET MAUI Documentation](https://learn.microsoft.com/en-us/dotnet/maui/)
   - [NuGet Documentation](https://learn.microsoft.com/en-us/nuget/)

4. **Ask for Help:**
   - Open an issue in the repository
   - Tag maintainers in PR comments
   - Reach out to the team

## Next Steps

Now that you understand the basics:

1. **Explore the Workflows:**
   - Look at `.github/workflows/*.yml` files
   - Understand the structure
   - See how jobs are configured

2. **Try a Full Cycle:**
   - Create a branch
   - Make a change
   - Create a PR
   - Get it merged
   - Create a release

3. **Customize for Your Needs:**
   - Adjust build configurations
   - Add new platforms
   - Extend workflows
   - Add notifications

4. **Contribute to the Pipeline:**
   - Suggest improvements
   - Fix issues
   - Add new features
   - Update documentation

Happy coding! 🚀
