# Rudderstack Tracking Plan Pipeline

This GitLab CI pipeline helps manage Rudderstack tracking plans by automating validation and deployment processes. It provides a secure workflow for reviewing tracking plan changes in merge requests and applying them to production.

## Features

- Automatic validation of tracking plans on merge requests
- Dry run capability to preview changes
- Protected production deployments
- Secure credential handling
- Automated CLI setup

## Prerequisites

- GitLab repository
- Rudderstack account and access token
- Tracking plan files in your repository

## Setup

1. Add the `.gitlab-ci.yml` file to your repository's root directory

2. Configure GitLab CI/CD Variables:
   - Go to Settings > CI/CD > Variables
   - Add the following variables:
     ```
     RUDDER_ACCESS_TOKEN: [Your Rudderstack Access Token]
     - Mask variable: Yes
     - Protect variable: Yes

     TRACKING_PLANS_PATH: [Path to your tracking plans]
     - Example: "tracking-plans"
     - Mask variable: No
     - Protect variable: No
     ```

3. (Optional) Customize CLI version:
   - Change the `CLI_VERSION` variable in `.gitlab-ci.yml` if needed
   - Default is "v0.2.0"

## Usage

### For Developers

1. Create a new branch for your changes
   ```bash
   git checkout -b feature/update-tracking-plan
   ```

2. Make changes to your tracking plans

3. Create a Merge Request
   - Pipeline automatically runs validation
   - Review the dry run output in the pipeline logs

### For Maintainers

1. Review the Merge Request and pipeline results

2. After merging to main:
   - Go to CI/CD > Pipelines
   - Find the pipeline for the main branch
   - Click the "play" button on the apply job
   - Review the deployment logs

## Pipeline Stages

### Review Stage (Automatic)
- Triggers on: Merge Requests
- Actions:
  - Validates tracking plan syntax
  - Performs dry run
  - Shows potential changes

### Apply Stage (Manual)
- Triggers on: Main branch only
- Requires: Manual approval
- Actions:
  - Applies tracking plan changes to Rudderstack
  - Updates production configuration

## Folder Structure

```
your-repo/
├── .gitlab-ci.yml
├── tracking-plans/
│   ├── plan1.yml
│   ├── plan2.yml
│   └── ...
└── README.md
```

## Error Handling

Common pipeline errors and solutions:

1. **Validation Failures**
   - Check tracking plan syntax
   - Verify schema compliance
   - Review error messages in pipeline logs

2. **CLI Installation Issues**
   - Verify internet connectivity in pipeline
   - Check CLI version compatibility
   - Review runner configuration

3. **Authentication Errors**
   - Verify RUDDER_ACCESS_TOKEN is set correctly
   - Check token permissions
   - Ensure token is not expired

## Security Considerations

- Access token is protected and masked
- Production deployments require manual approval
- Only maintainers can trigger apply job
- Configuration changes are tracked in version control

## Contributing

1. Clone the repository
2. Create a feature branch
3. Make your changes
4. Create a merge request
5. Wait for pipeline validation
6. Address any review comments

## License

MIT License - see the [LICENSE](LICENSE) file for details

## Support

For issues with:
- Pipeline configuration: Create a GitLab issue
- Rudderstack integration: Contact Rudderstack support
- Tracking plans: Consult Rudderstack documentation
