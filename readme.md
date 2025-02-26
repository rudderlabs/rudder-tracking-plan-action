# Rudderstack Tracking Plan Manager Action

This GitHub Action allows you to validate and manage Rudderstack tracking plans directly through your GitHub workflows.

## Features

- Validate tracking plans
- Perform dry-run of tracking plan changes
- Apply tracking plan changes
- Secure handling of Rudderstack access tokens

## Usage

### Prerequisites

- A Rudderstack account and access token
- Tracking plan files in your repository

### Basic Usage

```yaml
name: Manage Tracking Plans

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Validate Tracking Plans
        uses: your-username/rudderstack-tracking-plan-action@v1
        env:
          RUDDERSTACK_ACCESS_TOKEN: ${{ secrets.RUDDER_ACCESS_TOKEN }}
        with:
          location: "tracking-plans/"
          mode: "validate"

  apply:
    runs-on: ubuntu-latest
    needs: validate
    if: github.ref == 'refs/heads/main'
    steps:
      - uses: actions/checkout@v4
      - name: Apply Tracking Plans
        uses: your-username/rudderstack-tracking-plan-action@v1
        env:
          RUDDERSTACK_ACCESS_TOKEN: ${{ secrets.RUDDER_ACCESS_TOKEN }}
        with:
          location: "tracking-plans/"
          mode: "apply"
```

### Inputs

| Input         | Description                                        | Required | Default |
| ------------- | -------------------------------------------------- | -------- | ------- |
| `location`    | Path to the folder containing tracking plan files  | Yes      | N/A     |
| `mode`        | Operation mode (`validate`, `dry-run`, or `apply`) | Yes      | N/A     |
| `cli_version` | Version of rudder-cli to use                       | No       | v0.2.0  |

### Environment Variables

| Variable                   | Description              | Required |
| -------------------------- | ------------------------ | -------- |
| `RUDDERSTACK_ACCESS_TOKEN` | Rudderstack access token | Yes      |

### Modes

1. **validate**:

   - Validates tracking plan syntax and structure
   - No changes are applied to your configuration

2. **dry-run**:

   - Simulates the application of changes
   - Shows what would be modified without making actual changes

3. **apply**:
   - Applies the tracking plan changes to your Rudderstack configuration

## Security

The action requires your Rudderstack access token to be provided via the `RUDDERSTACK_ACCESS_TOKEN` environment variable. Always store this token in GitHub Secrets and reference it in your workflow using `${{ secrets.YOUR_SECRET_NAME }}`. Never expose the token directly in your workflow files.

## License

MIT License - see the [LICENSE](LICENSE) file for details

## Contributing

Contributions are welcome! Please see our [Contributing Guidelines](CONTRIBUTING.md) for more details.
