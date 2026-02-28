# GitHub Actions Logging Guide

This repository includes comprehensive logging capabilities for GitHub Actions workflows. This guide explains how to use and customize the logging features.

## Available Workflows

### 1. CI with Comprehensive Logging (`ci-with-logging.yml`)

A production-ready CI workflow that demonstrates logging best practices:

- **Automatic logging** on push/pull requests
- **Manual trigger** with optional debug mode
- **Structured log files** saved as artifacts
- **Job summaries** visible in GitHub UI
- **Environment information** logging
- **Build and test logging** with error handling

#### Usage

**Automatic Triggers:**
```bash
# Automatically runs on:
# - Push to main or devspaces-3-rhel-9 branches
# - Pull requests to main or devspaces-3-rhel-9 branches
```

**Manual Trigger with Debug:**
1. Go to Actions tab in GitHub
2. Select "CI with Comprehensive Logging"
3. Click "Run workflow"
4. Enable "Enable debug logging" checkbox
5. Click "Run workflow"

### 2. Advanced Logging Examples (`advanced-logging-example.yml`)

An educational workflow demonstrating advanced logging techniques:

- **Multiple log levels** (debug, info, warning, error)
- **Log grouping** for better organization
- **Sensitive data masking**
- **Structured logging** (JSON format)
- **Conditional logging** based on log level
- **Performance metrics** and timing
- **Multiline logging** with formatting

#### Usage

1. Go to Actions tab in GitHub
2. Select "Advanced Logging Examples"
3. Click "Run workflow"
4. Select desired log level
5. Click "Run workflow"

## Logging Features

### 1. Log Levels

GitHub Actions supports several log levels:

```bash
# Debug (only visible when debug mode is enabled)
echo "::debug::This is a debug message"

# Notice (highlighted in blue)
echo "::notice::This is important information"

# Warning (highlighted in yellow)
echo "::warning::This is a warning"

# Error (highlighted in red)
echo "::error::This is an error"
```

### 2. Enabling Debug Logging

**Method 1: Environment Variables (Repository Secrets)**

Add these secrets to your repository:
- `ACTIONS_STEP_DEBUG`: `true`
- `ACTIONS_RUNNER_DEBUG`: `true`

**Method 2: Workflow Dispatch Input**

Use the manual trigger with debug checkbox enabled.

**Method 3: Re-run with Debug**

In GitHub UI, click "Re-run all jobs" → "Enable debug logging"

### 3. Log Grouping

Organize logs into collapsible groups:

```yaml
- name: Example with grouping
  run: |
    echo "::group::Building Application"
    npm install
    npm run build
    echo "::endgroup::"
    
    echo "::group::Running Tests"
    npm test
    echo "::endgroup::"
```

### 4. File Annotations

Add annotations to specific files and lines:

```bash
# Warning on specific file/line
echo "::warning file=app.js,line=10,col=5::Deprecated API usage"

# Error on specific file/line
echo "::error file=config.json,line=15::Invalid configuration"
```

### 5. Masking Sensitive Data

Prevent sensitive data from appearing in logs:

```yaml
- name: Mask sensitive data
  env:
    API_KEY: ${{ secrets.API_KEY }}
  run: |
    echo "::add-mask::$API_KEY"
    echo "Using API key: $API_KEY"  # Will show as ***
```

### 6. Setting Step Outputs

Share data between steps with logging:

```yaml
- name: Build application
  id: build
  run: |
    VERSION="1.0.0"
    echo "Building version $VERSION"
    echo "version=$VERSION" >> $GITHUB_OUTPUT

- name: Use version
  run: |
    echo "Version from previous step: ${{ steps.build.outputs.version }}"
```

### 7. Job Summaries

Create rich summaries that appear in GitHub UI:

```yaml
- name: Create summary
  run: |
    cat >> $GITHUB_STEP_SUMMARY <<EOF
    ## Build Results
    
    - ✅ Build: Success
    - ✅ Tests: Passed
    - 📦 Artifacts: 5 files
    EOF
```

### 8. Uploading Log Artifacts

Save logs for later analysis:

```yaml
- name: Save logs
  if: always()
  uses: actions/upload-artifact@v4
  with:
    name: build-logs
    path: logs/
    retention-days: 30
```

## Log File Structure

The CI workflow creates the following log files:

```
logs/
├── workflow-info.log           # Workflow metadata and context
├── structure-validation.log    # Repository structure validation
├── build-YYYYMMDD-HHMMSS.log  # Build process logs
├── test-YYYYMMDD-HHMMSS.log   # Test execution logs
└── summary-report.log          # Overall summary
```

## Best Practices

### 1. Use Appropriate Log Levels

- `debug`: Detailed diagnostic information
- `notice`: Important informational messages
- `warning`: Warning messages that don't stop execution
- `error`: Error messages for failures

### 2. Group Related Operations

Group logically related commands to keep logs clean and organized:

```yaml
run: |
  echo "::group::Setup Phase"
  # setup commands
  echo "::endgroup::"
  
  echo "::group::Build Phase"
  # build commands
  echo "::endgroup::"
```

### 3. Always Upload Logs on Failure

```yaml
- name: Upload failure logs
  if: failure()
  uses: actions/upload-artifact@v4
  with:
    name: failure-logs
    path: logs/
```

### 4. Create Meaningful Job Summaries

Use job summaries to provide quick insights:

```yaml
- name: Summary
  if: always()
  run: |
    echo "## Results" >> $GITHUB_STEP_SUMMARY
    echo "- Status: ${{ job.status }}" >> $GITHUB_STEP_SUMMARY
```

### 5. Mask All Secrets

Always mask sensitive data before using it:

```yaml
- run: |
    echo "::add-mask::${{ secrets.API_TOKEN }}"
    echo "::add-mask::${{ secrets.PASSWORD }}"
```

### 6. Include Timestamps

Add timestamps to logs for better debugging:

```bash
echo "[$(date '+%Y-%m-%d %H:%M:%S')] Starting process..."
```

### 7. Log Environment Information

Include context about the execution environment:

```yaml
- name: Log environment
  run: |
    echo "Runner: ${{ runner.os }}"
    echo "Branch: ${{ github.ref }}"
    echo "Commit: ${{ github.sha }}"
    echo "Actor: ${{ github.actor }}"
```

## Troubleshooting

### Logs Not Appearing

1. Check if the step actually executed
2. Verify the log level is appropriate
3. Enable debug mode for more details

### Debug Logs Not Visible

1. Ensure `ACTIONS_STEP_DEBUG` is set to `true`
2. Re-run the workflow with debug enabled
3. Check repository secrets configuration

### Artifacts Not Uploaded

1. Verify the path exists: `path: logs/`
2. Use `if: always()` to upload even on failure
3. Check `if-no-files-found` setting

## Examples

### Basic Logging

```yaml
- name: Simple logging
  run: |
    echo "Starting task..."
    echo "::notice::Task in progress"
    echo "Task complete!"
```

### Advanced Logging with Error Handling

```yaml
- name: Advanced logging
  id: advanced
  continue-on-error: true
  run: |
    echo "::group::Task Execution"
    
    if ! command -v node &> /dev/null; then
      echo "::error::Node.js not found"
      exit 1
    fi
    
    echo "::notice::Running command..."
    node --version
    
    echo "::endgroup::"
```

### Performance Logging

```yaml
- name: Performance tracking
  run: |
    START=$(date +%s)
    
    # Your command here
    sleep 2
    
    END=$(date +%s)
    DURATION=$((END - START))
    
    echo "::notice::Completed in ${DURATION}s"
```

## Additional Resources

- [GitHub Actions Workflow Commands](https://docs.github.com/en/actions/using-workflows/workflow-commands-for-github-actions)
- [GitHub Actions Workflow Syntax](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions)
- [Using Job Summaries](https://github.blog/2022-05-09-supercharging-github-actions-with-job-summaries/)

## Contributing

To improve these workflows:

1. Test your changes locally using [act](https://github.com/nektos/act)
2. Add appropriate logging to new steps
3. Update this documentation
4. Submit a pull request
