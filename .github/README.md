# GitHub Actions Workflows

This directory contains GitHub Actions workflows for the DevSpaces Images repository.

## 📋 Available Workflows

### 1. CI with Comprehensive Logging
**File:** `workflows/ci-with-logging.yml`

A production-ready continuous integration workflow with comprehensive logging features.

**Features:**
- ✅ Automatic execution on push/PR
- ✅ Manual trigger with optional debug mode
- ✅ Environment information logging
- ✅ Build and test validation with detailed logs
- ✅ Log files saved as artifacts (30-day retention)
- ✅ Job summaries in GitHub UI
- ✅ Error handling and notifications

**Triggers:**
- Push to `main` or `devspaces-3-rhel-9` branches
- Pull requests to `main` or `devspaces-3-rhel-9` branches
- Manual workflow dispatch

### 2. Advanced Logging Examples
**File:** `workflows/advanced-logging-example.yml`

Educational workflow demonstrating advanced GitHub Actions logging techniques.

**Features:**
- ✅ Multiple log levels (debug, info, warning, error)
- ✅ Log grouping and organization
- ✅ Sensitive data masking
- ✅ Structured logging (JSON)
- ✅ Conditional logging based on level
- ✅ Performance timing metrics
- ✅ Rich job summaries

**Triggers:**
- Manual workflow dispatch only
- Configurable log level selection

## 🚀 Quick Start

### Running Workflows

**Option 1: Automatic (CI Workflow)**
Push to main branch or create a pull request - the CI workflow runs automatically.

**Option 2: Manual with Debug**
1. Go to the **Actions** tab in GitHub
2. Select a workflow from the left sidebar
3. Click **Run workflow** button
4. Configure options (e.g., enable debug logging)
5. Click **Run workflow** to start

### Enabling Debug Logging

**Method 1: Use Workflow Dispatch**
- Select "CI with Comprehensive Logging" workflow
- Check "Enable debug logging"
- Run workflow

**Method 2: Re-run with Debug**
- Open a completed workflow run
- Click "Re-run all jobs"
- Select "Enable debug logging"

**Method 3: Repository Secrets (Global)**
Add these secrets to enable debug logging for all runs:
- `ACTIONS_STEP_DEBUG` = `true`
- `ACTIONS_RUNNER_DEBUG` = `true`

### Viewing Logs

**In GitHub UI:**
1. Go to **Actions** tab
2. Select a workflow run
3. Click on a job to see logs
4. Click on a step to see detailed output
5. Expand groups by clicking the arrows

**Download Log Artifacts:**
1. Scroll to bottom of workflow run page
2. Find "Artifacts" section
3. Download `workflow-logs-{run-id}`
4. Extract and view log files

## 📚 Documentation

For detailed information about logging features and best practices, see:
- **[Logging Guide](workflows/LOGGING_GUIDE.md)** - Comprehensive guide to all logging features

## 🛠️ Customization

### Adapting the CI Workflow

The `ci-with-logging.yml` workflow is designed as a template. To adapt it for your specific needs:

1. **Modify build commands** in the "Build example" step
2. **Add actual tests** in the "Run tests" step
3. **Configure triggers** in the `on:` section
4. **Adjust log retention** in the upload artifact step
5. **Add notifications** in the "Report failures" step

### Creating Custom Workflows

Use the existing workflows as templates:

```yaml
name: My Custom Workflow

on:
  push:
    branches: [ main ]

jobs:
  my-job:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: My step with logging
        run: |
          echo "::group::My Task"
          # Your commands here
          echo "::notice::Task completed successfully"
          echo "::endgroup::"
      
      - name: Upload logs
        if: always()
        uses: actions/upload-artifact@v4
        with:
          name: my-logs
          path: logs/
```

## 📊 Logging Features Summary

| Feature | CI Workflow | Advanced Example |
|---------|-------------|------------------|
| Debug logging | ✅ | ✅ |
| Log grouping | ✅ | ✅ |
| File annotations | ✅ | ✅ |
| Job summaries | ✅ | ✅ |
| Log artifacts | ✅ | ❌ |
| Sensitive data masking | ❌ | ✅ |
| Structured logging | ❌ | ✅ |
| Performance metrics | ❌ | ✅ |

## 🔧 Troubleshooting

### Workflow Not Running
- Check trigger conditions in `on:` section
- Verify branch names match
- Check workflow permissions

### Logs Not Visible
- Enable debug mode for detailed logs
- Check if step executed successfully
- Verify log level is appropriate

### Artifacts Not Uploaded
- Check if `logs/` directory was created
- Verify upload path is correct
- Use `if: always()` to upload on failure

## 📖 Additional Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Workflow Syntax](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions)
- [Workflow Commands](https://docs.github.com/en/actions/using-workflows/workflow-commands-for-github-actions)

## 🤝 Contributing

To improve these workflows:

1. Test changes locally using [act](https://github.com/nektos/act)
2. Follow existing logging patterns
3. Update documentation
4. Submit a pull request
