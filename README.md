# Playwright QA Automation

A GitHub Actions-based automation project demonstrating continuous integration, scheduled workflows, and automated testing with Playwright.

## Overview

This repository showcases a robust QA automation pipeline that:
- Runs continuous integration checks on code pushes
- Executes scheduled daily automated tasks
- Maintains detailed logs of automated runs
- Demonstrates GitHub Actions best practices
- Integrates with Playwright for end-to-end testing

## Features

- **CI/CD Pipeline**: Automated testing on every push to main branch
- **Scheduled Automation**: Daily automated commits and logging
- **Workflow Management**: Multiple GitHub Actions workflows for different tasks
- **Testing Framework**: Playwright for cross-browser testing and automation
- **Activity Logging**: Comprehensive daily update logs with timestamps

## Tech Stack

- **Playwright**: End-to-end testing framework
- **GitHub Actions**: CI/CD automation platform
- **YAML**: Workflow configuration

## Project Structure

```
playwright-qa-automation/
├── .github/
│   └── workflows/
│       ├── ci.yml              # Continuous integration workflow
│       └── daily-update.yml    # Scheduled daily automation
├── daily-log.txt               # Log file with automated run history
└── README.md                   # This file
```

## GitHub Actions Workflows

### 1. CI Workflow (`ci.yml`)

**Trigger**: Push to main branch or manual trigger

**Purpose**: Runs continuous integration checks

```yaml
on:
  push:
    branches:
      - main
  workflow_dispatch:
```

**Jobs**:
- Checks out the repository
- Runs simple test verification
- Confirms CI pipeline is functional

### 2. DevSync Daily Automation (`daily-update.yml`)

**Trigger**: Scheduled daily at 02:30 UTC or manual trigger

**Purpose**: Automated daily commits and logging

```yaml
on:
  schedule:
    - cron: '30 2 * * *'   # Runs daily at 02:30 UTC
  workflow_dispatch:
```

**Jobs**:
- Checks out the repository with persistent credentials
- Configures Git user identity (DevSync Bot)
- Appends current timestamp to daily-log.txt
- Commits and pushes changes automatically
- Handles cases where no changes exist

## Daily Log

The `daily-log.txt` file maintains a comprehensive history of every automated run, including:
- Date and time of execution
- Timezone (UTC)
- Sequential record of all scheduled executions

This file grows with each daily run, providing an audit trail of automation activity.

## Installation & Setup

### Prerequisites

- GitHub account and repository access
- Basic understanding of GitHub Actions
- Playwright knowledge for test automation

### Local Setup

1. Clone the repository:
```bash
git clone https://github.com/DS24F3004981/playwright-qa-automation.git
cd playwright-qa-automation
```

2. Install Playwright:
```bash
npm install -D @playwright/test
```

3. Create your test files in the root directory or organize in a `tests/` folder

### Running Tests Locally

```bash
# Run all tests
npx playwright test

# Run tests in headed mode (see browser)
npx playwright test --headed

# Run specific test file
npx playwright test tests/example.spec.ts
```

## GitHub Actions Workflow Setup

### Prerequisites for Workflows

1. **Push Workflow** - Automatically triggers on push to main
2. **Scheduled Workflow** - Runs on the configured cron schedule
3. **Manual Workflow** - Manually trigger via GitHub Actions UI

### Triggering Workflows Manually

1. Go to your repository on GitHub
2. Click on "Actions" tab
3. Select the workflow you want to run
4. Click "Run workflow" button
5. Confirm execution

## Creating Test Automation

### Example Playwright Test

```javascript
// tests/example.spec.ts
import { test, expect } from '@playwright/test';

test('has title', async ({ page }) => {
  await page.goto('https://example.com');
  await expect(page).toHaveTitle(/Example Domain/);
});
```

### Integrating with CI Workflow

Add test execution to the CI workflow:

```yaml
- name: Run Playwright tests
  run: npx playwright test
```

## Best Practices

1. **Daily Log Management**: Monitor the growth of `daily-log.txt` and archive periodically
2. **Workflow Monitoring**: Check GitHub Actions UI for workflow status and logs
3. **Error Handling**: The daily-update workflow handles commit failures gracefully
4. **Credentials**: Uses workflow permissions to safely write to repository
5. **Scheduling**: Current schedule (02:30 UTC) can be adjusted in `daily-update.yml`

## Customization

### Changing the Daily Schedule

Edit `.github/workflows/daily-update.yml`:

```yaml
schedule:
  - cron: '30 2 * * *'  # Change this cron expression
```

Cron format: `minute hour day month day-of-week`

Common schedules:
- `0 0 * * *` - Midnight daily
- `0 12 * * *` - Noon daily
- `0 */6 * * *` - Every 6 hours
- `0 9 * * 1-5` - 9 AM on weekdays

### Adding Environment Variables

```yaml
env:
  NODE_ENV: test
  DEBUG: true
```

## Troubleshooting

### Workflow Not Triggering

- Verify the main branch is protected and workflows have permission to run
- Check GitHub Actions is enabled in repository settings
- Review workflow syntax for errors

### Daily Commit Failing

- Ensure the `contents: write` permission is set
- Verify Git configuration in the workflow

### Test Failures

- Check Playwright installation is correct
- Verify test file syntax
- Review workflow logs in GitHub Actions UI

## Monitoring & Logs

### View Workflow Logs

1. Go to repository → Actions tab
2. Click on the workflow run
3. Click on the job to see detailed logs
4. Download logs if needed

### Daily-Log.txt Analysis

```bash
# Count number of automated runs
wc -l daily-log.txt

# View latest updates
tail daily-log.txt

# View specific date range
grep "Mar" daily-log.txt
```

## Contributing

To add new automation tests:

1. Create test files following Playwright conventions
2. Ensure tests run locally before committing
3. Update CI workflow if needed for new test dependencies
4. Document your tests in this README

## License

This project is open source and available under the MIT License.

## Author

Created by DS24F3004981

## Resources

- [Playwright Documentation](https://playwright.dev/)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Cron Expression Generator](https://crontab.guru/)
