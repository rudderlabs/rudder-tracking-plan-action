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
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Validate Tracking Plans
        uses: your-username/rudderstack-tracking-plan-action@v1
        with:
          folder_path: 'tracking-plans/'
          mode: 'review'
          access_token: ${{ secrets.RUDDER_ACCESS_TOKEN }}

  apply:
    runs-on: ubuntu-latest
    needs: validate
    if: github.ref == 'refs/heads/main'
    steps:
      - uses: actions/checkout@v4
      - name: Apply Tracking Plans
        uses: your-username/rudderstack-tracking-plan-action@v1
        with:
          folder_path: 'tracking-plans/'
          mode: 'apply'
          access_token: ${{ secrets.RUDDER_ACCESS_TOKEN }}
```

### Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `folder_path` | Path to the folder containing tracking plan files | Yes | N/A |
| `mode` | Operation mode (`review` or `apply`) | Yes | N/A |
| `access_token` | Rudderstack access token | Yes | N/A |
| `cli_version` | Version of rudder-cli to use | No | v0.2.0 |

### Modes

1. **review**: 
   - Validates tracking plan syntax
   - Performs a dry run of changes
   - No actual changes are applied

2. **apply**:
   - Applies the tracking plan changes to your Rudderstack configuration

## Security

The action securely handles your Rudderstack access token. Always store your access token in GitHub Secrets and never expose it in your workflow files.

## License

MIT License - see the [LICENSE](LICENSE) file for details

## Contributing

Contributions are welcome! Please see our [Contributing Guidelines](CONTRIBUTING.md) for more details.
